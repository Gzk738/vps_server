---
title: "用nltk对squad数据集进行自动拆分和标记"
date: 2022-04-06T15:16:43+08:00
draft: true
---

最近使用可解释ai翻译bert模型，但是数据集太多导致的速度太慢，三台服务器要跑一周才能
出结果，于是乎自己精简了一下squad数据集。记录一下防止忘记：

squad加载+洗牌
洗牌是因为训练的时候需要随机化标签位置，不然会训练出不平衡的模型

```python
from datasets import load_dataset
squad = load_dataset("squad")
shuffled_squad = squad.shuffle()
```

大力出奇迹的数据拆分哈哈哈，暴力的列表拆分，如果不想可以使用api，我这么做仅仅是懒

```python
train_contexts = []
train_questions = []
train_answers = []
train_ids = []


for i in range(len(shuffled_squad['train'])):
    train_contexts.append(shuffled_squad['train'][i]['context'])
    train_questions.append(shuffled_squad['train'][i]['question'])
    train_answers.append(shuffled_squad['train'][i]['answers'])
    train_ids.append(shuffled_squad['train'][i]['id'])

validation_contexts = train_contexts[77599:87599]
train_contexts = train_contexts[:77599]
validation_question = train_questions[77599:87599]
train_questions = train_questions[:77599]
validation_answers = train_answers[77599:87599]
train_answers = train_answers[:77599]
validation_ids = train_ids[77599:87599]
train_ids = train_ids[:77599]

```

核心代码，不解释，远离很简单。就是扫描答案位置，然后重定向到新数据集里

```python
import nltk as tk
import re
sep_train_contexts = []
sep_train_questions = []
sep_train_answers = []
error_index = []
null_answer = {'text': '[NULL]', 'answer_start': 0}
for i in range(len(train_contexts)):
    tokens = tk.sent_tokenize(train_contexts[i])
    for token in tokens:
        if train_answers[i]['text'][0] in token:
            try:
                answer_start = re.search(train_answers[i]['text'][0], token)
                answer = {'text': train_answers[i]['text'], 'answer_start':  answer_start.span()[0]}
                sep_train_contexts.append(token)

            
                sep_train_answers.append(answer)
                sep_train_questions.append(train_questions[i])
            except:
                error_index.append(i)

                # print(i)
        # else:
        #     sep_train_contexts.append('[NULL]' + token)
        #     sep_train_answers.append(null_answer)
        #     sep_train_questions.append(train_questions[i])
print("error_index : ", len(error_index))
            

```
添加索引index
```python
def add_end_idx(answers, contexts):
    for answer, context in zip(answers, contexts):
        gold_text = answer['text'][0]
        if type(answer['answer_start']) == list:
            temp = answer['answer_start'][0]
            answer['answer_start'] = temp
        else:
            start_idx = answer['answer_start']
        end_idx = start_idx + len(gold_text)

        # sometimes squad answers are off by a character or two – fix this
        if context[start_idx:end_idx] == gold_text:
            answer['answer_end'] = end_idx
        elif context[start_idx-1:end_idx-1] == gold_text:
            answer['answer_start'] = start_idx - 1
            answer['answer_end'] = end_idx - 1     # When the gold label is off by one character
        elif context[start_idx-2:end_idx-2] == gold_text:
            answer['answer_start'] = start_idx - 2
            answer['answer_end'] = end_idx - 2     # When the gold label is off by two characters
        else:
            answer['answer_end'] = end_idx

add_end_idx(sep_train_answers, sep_train_contexts)
add_end_idx(train_answers, train_contexts)

```
加载模型和Tokenizer
```python
from transformers import DistilBertTokenizerFast
tokenizer = DistilBertTokenizerFast.from_pretrained('distilbert-base-uncased')

sep_train_encodings = tokenizer(sep_train_contexts, sep_train_questions, truncation=True, padding=True)
from transformers import DistilBertForQuestionAnswering
model = DistilBertForQuestionAnswering.from_pretrained("distilbert-base-uncased")
```

映射到word_embeding

```python
train_encodings = tokenizer(train_contexts, train_questions, truncation=True, padding=True)
```

添加答案的起始索引
```python
def add_token_positions(encodings, answers):
    start_positions = []
    end_positions = []
    print("len len(answers) : ",len(answers))
    for i in range(len(answers)):
        start_positions.append(encodings.char_to_token(i, answers[i]['answer_start']))

        
        end_positions.append(encodings.char_to_token(i, answers[i]['answer_end'] - 1))
        # if None, the answer passage has been truncated
        if start_positions[-1] is None:
            start_positions[-1] = tokenizer.model_max_length
        if end_positions[-1] is None:
            end_positions[-1] = tokenizer.model_max_length
    encodings.update({'start_positions': start_positions, 'end_positions': end_positions})

add_token_positions(sep_train_encodings, sep_train_answers)
add_token_positions(train_encodings, train_answers)
import torch

class SquadDataset(torch.utils.data.Dataset):
    def __init__(self, encodings):
        self.encodings = encodings

    def __getitem__(self, idx):
        return {key: torch.tensor(val[idx]) for key, val in self.encodings.items()}

    def __len__(self):
        return len(self.encodings.input_ids)

sep_train_dataset = SquadDataset(sep_train_encodings)
train_dataset = SquadDataset(train_encodings)


```

定义下测试的代码

```python
import datetime
import json
ref_token_id = tokenizer.pad_token_id # A token used for generating token reference
sep_token_id = tokenizer.sep_token_id # A token used as a separator between question and text and it is also added to the end of the text.
cls_token_id = tokenizer.cls_token_id
print (datetime.datetime.now())
def predict(inputs):
    output = model(inputs)
    return output.start_logits, output.end_logits


def construct_input_ref_pair(question, text, ref_token_id, sep_token_id, cls_token_id):
    question_ids = tokenizer.encode(question, add_special_tokens=False)
    text_ids = tokenizer.encode(text, add_special_tokens=False)

    # construct input token ids
    input_ids = [cls_token_id] + question_ids + [sep_token_id] + text_ids + [sep_token_id]

    # construct reference token ids
    ref_input_ids = [cls_token_id] + [ref_token_id] * len(question_ids) + [sep_token_id] + \
                    [ref_token_id] * len(text_ids) + [sep_token_id]

    return torch.tensor([input_ids], device=device), torch.tensor([ref_input_ids], device=device), len(question_ids)

def predict_qt(question, text):
    input_ids, ref_input_ids, sep_id = construct_input_ref_pair(question, text, ref_token_id, sep_token_id, cls_token_id)

    indices = input_ids[0].detach().tolist()
    all_tokens = tokenizer.convert_ids_to_tokens(indices)

    ground_truth = '13'


    start_scores, end_scores = predict(input_ids)


    return (' '.join(all_tokens[torch.argmax(start_scores) : torch.argmax(end_scores)+1]))

def test_valisdation():
    evl_dick = {}

    for i in range(len(validation_contexts)):
        question = validation_question[i]
        text = validation_contexts[i]
        if len(text) <= 512:
            answer = predict_qt(question, text)
            evl_dick[str(validation_ids[i])] = answer
    time = str(datetime.datetime.now().strftime('%Y-%m-%d-%H-%M-%S'))
    json_dick = json.dumps(evl_dick)
    filname = time + "answers.txt"

    fo = open("./results/"+filname, "w",encoding='utf-8')
    fo.write(json_dick)
    fo.close()
```


开始训练~~~~每一个epoch会存储到model文件夹
```python
from torch.utils.data import DataLoader
from transformers import AdamW
import datetime

print (datetime.datetime.now())

device = torch.device('cuda') if torch.cuda.is_available() else torch.device('cpu')

model.to(device)
model.train()

train_loader = DataLoader(sep_train_dataset, batch_size=16, shuffle=True)

optim = AdamW(model.parameters(), lr=5e-5)
list_loss = []
for epoch in range(10):
    for batch in train_loader:
        optim.zero_grad()
        input_ids = batch['input_ids'].to(device)
        attention_mask = batch['attention_mask'].to(device)
        start_positions = batch['start_positions'].to(device)
        end_positions = batch['end_positions'].to(device)
        outputs = model(input_ids, attention_mask=attention_mask, start_positions=start_positions, end_positions=end_positions)
        loss = outputs[0]
        loss.backward()
        optim.step()
    test_valisdation()
    list_loss.append(float(loss))
    model_name = "sep_data_trained_epoch" + str(epoch) + str(datetime.datetime.now().strftime('%Y-%m-%d-%H-%M-%S')) + '.pth'
    torch.save(model, './models/' + model_name)

model.eval()

print (datetime.datetime.now())
print("loss : ", list_loss)
```

