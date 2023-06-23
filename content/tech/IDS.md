---
title: "IDS-Extract:Downsizing Deep Learning Model For Question and Answering"
date: 2022-12-23T22:11:35+09:00
draft: true
---

Published in: 2023 International Conference on Electronics, Information, and Communication (ICEIC)

## Abstract:
In recent years, Question-answering systems are extensively used in human-computer systems, and the accuracy rate on a large scale is increasing. However, in actual deployment, a large number of parameters are often accompanied by a large amount of memory and long-term processing requirements. Therefore, compressing the data of the model, reducing training time, memory, becomes more and more urgent. we aim to resolve issues: IDS-Extract dynamically sized data to support models and devices with different memory. The proposed technique does efficient data extraction, segments that are not meaningful for model learning on the original dataset and output multiple datasets of adaptive size followed by target training based on model size. We leverage techniques in IG(Integration Gradient), DPR, and SBERT to improve localization performance for answer positions. We compare the model performance of SQuAD and the data set reduced by the IDS extraction technique, and the results prove that our technique can train the model more targeted and obtain higher performance evaluation. We prove that this method has successfully passed the sanity check, and can be directly applied to emotion recognition, two-classification, and multi-classification fields.

## SECTION I.Introduction
Pre-trained model such as based language models such as Transformer[1] BERT[2], T5[3] have achieved impressive performance on various downstream NLP tasks. These models are the current state of art research, using self-supervised methods to conduct and train in the corpus, and then fine-tune in specific tasks, which can ensure the semantic understanding ability of the model, and reduce the training cost of downstream tasks. Although they are effective, These models are difficult to target for various downstream tasks and resource-constrained scenarios and are therefore very expensive in terms of computational and memory costs, making them to train for this reason. Therefore, it becomes increasingly important to compress the resources required for pretrained models. There are many studies using different methods to compress pretrained models, such as pruning [4], weight decomposition [5], resistance [6] and knowledge distillation [7]. However, from a training perspective, if we use a lower memory device for pre-training or fine-tuning, the training time is often very long due to the low device performance. For example, if an embedded device is used instead of an online server, it is often faced with a low-memory training device. Another convenience is that when using the old method to train the model to adapt to the next task, the effect of a professional training server is often higher than that of a model trained by a computer or an embedded device. But compressing this operation for all models is laborious and cannot be passed from one task to another.In this paper, we investigate different information extraction settings: the extracted datasets need to cover different sizes to support devices with different types of memory; the extraction is in the fine-tuning phase, independent of training. In this work we propose IDS-Extraction, which utilizes dense passage retrieval (DPR), SBERT, and Integration Gradient to automatically extract context. We are concerned with fully designing with a space of search and separable extraction models. For example, in classification models, this approach can clearly tell us what is more important in deep networks, The advantage of this is that developers not only know the deep network's output, and can help them debug. Axiom - Sensitivity and implementation form the working method of the integral gradient, which is a derivative design based on the sensitivity axiom, which has the advantage of not requiring modification or optimization for a certain class of models and unifying deep model attribution Importance analysis under inference performance. After inputting sequence information into the model to get predictions, the standard method of gradient integration does not have an impact on the model work province and can quickly help developers or researchers to obtain the rule extraction features and progress of deep models, whether it is multi-image processing, text processing Or any form of data, such as tabular data, can demonstrate the attribution inference ability of deep network model， and enable users to better interact with the model. In order to make the model adaptive sizes to meet different embedded devices or mobile phones with different memory and different performance, we design a weighted extraction architecture that includes all candidate operations. In order to improve the pertinence in SQuAD[8], we directly train and specify different datasets to train the model. However, training directly in large networks is extremely expensive on pre-training tasks with large memory and long-running requirements. Improve the convergence efficiency and accuracy of the model, we use block-by-block training to divide the data into blocks in order to classify large, medium and small datasets.

## SECTION II.Related Work
Recently, compressing pretrained models has been extensively studied and several techniques have been proposed, such as knowledge distillation, pruning, weight decomposition, quantization, etc. Existing works aim to compress pretrained models into fixed-size models and strike a balance between small parameter sizes (usually no more than 66M) and good performance. However, these compression models cannot be deployed in devices with different memory and latency constraints. Recent works can provide adaptive models for each specific downstream task and demonstrate the effectiveness of task-oriented compression. For actual deployment, from each task. Other works are available in a pre-training stage that can generalize directly to downstream tasks and allow for efficient pruning at inference time. Different from existing work, IDS-Extraction aims at the compression of the fine-tuning stage independent of the compression of the model itself, eliminates the heavy compression of the model structure, and carefully designs the training architecture that can explore the potential and provide different models given different data require. This session can be compressed more.

A. DPR(Dense Paragraph Retrieval)
Dense Passage Retrieval [9] an unsupervised pre-training model for retrieval, different pre-retrieval training tasks, there are a high-efficiency general-propose technology has been proposed the Inverse Cloze Task [10], According to the experimental results of previous research, the application of dense paragraph retrieval can be applied in daily life such as Q&A Bot [11], information retrieval [12], dialogue [13], and entity linking [14] can effectively improve the performance of the model and propose a new method that using a pre-train method with trains a masked sequence data model and a retriever.

However, the lack of lexical overlap between questions and answers in many question answering datasets makes standard IR methods that rely on strict lexical matching less applicable. Some IR systems have been modified to use BM25 [15] similarity to align query words with the most similar document words for various tasks including document matching, short text similarity, and answer selection. Also passages in low-dimensional semantic space have significantly outperformed traditional term-based techniques [16]. In this study, we practically achieve context extraction for retrieval and Integration Gradient [17] using dense representations, where the embeddings are learned from a small number of questions and passages via a simple two-encoder framework. The encoded information processed based on SBERT [18] is extracted from it. At the same time we use as a tool to elucidate important aspects of the learned model is extracted from it. At the same time we use as a tool to elucidate important aspects of the learned model, The purpose is to highlight the part of the input that positively contributes to the result. Although research has shown that the method has played a role, the definition and methodology of interpretability are still largely lacking. The lack of principled guidance confuses practitioners when deciding between a multitude of competing approaches.

B. SBERT(Sentence-BERT)
SBERT, a modification of the pretrained BERT network that use Siamese and triplet network structures to derive semantically meaningful sentence embeddings that can be Compared using cosine-similarity. This reduces the effort for finding the most similar pair from 65 hours with BERT / RoBERTa [19] to about 5 seconds with SBERT, while maintaining the accuracy from BERT.

C. IG(Integration Gradient)
while query-centric similarity relationships can enforce the position of the searched answer, the length of the sequence data is not something that all models can handle. When issuing a new query, once the length exceeds the maximum length that the model can handle: this often happens, especially in the OpenQA field, it is difficult to distinguish the performance bottleneck of the model itself or the data mismatch problem. The goal of explanation of the deep neural network is to Increase the model’s transparency to humans. The reasoning behind it can be explained in a way that humans can understand. In the effort to provide explanations, visualizing a certain amount of interest, such as the importance of input features or learning weights, becomes the most direct way to gain user trust. A novel Hindsight explanation method called Score-CAM based on class activation mapping. Increasing attention has been well-studied.

To address this issue, We use an encoder structure for paragraph similarity relations for reinforcement The basic idea is shown in Figure 2 shown, where we set an additional constraint s(p, q) : query the value between the similarity between q and p. In this way, the similarity relationship between sentences can be better handled in the dual encoder between queries, affirmative paragraphs and negative paragraphs. While the idea is appealing, it's not easy to implement due to two main problems. First, the sequence information output in the BERT model is positively correlated with non-semantic similarity, which means that the tensor distance of the output of SBERT and DPR is often messy. Second, manually labeling data is costly. And there is no open source dataset like this at present, so we made our own multi-threshold extraction information based on the Stanford University question and answer dataset. Aims for targeted training on the features of the model.

SECTION III.Methodology
A. Overview
In this section, we describe IDS-Extract, which performs semantic search to compress the dataset passed to the model for training. Through multi-batch deletion, input constraints of different memory models and training requirements across different downstream tasks can be met. Firstly we train BERT models with different sizes, including Tiny, Mini, Small, Base and subsequent T5 models. Then by using the IDS-extraction method directly, they can smoothly retain the original semantic environment in the stream and shorten the length to meet the length requirements of the model. The method can be divided into three steps: 1) IDS-extraction; 2) Multiple sequence length training; 3) Model evaluation. Due to the huge cost of training a large superset on a heavy fine-tuning task and choosing a compressed model under specific constraints, we introduce a technique that can reduce the time and improve the efficiency of fine-tuning.

![20230423221412](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423221412.png)
The confirmation of the answer index can be confirmed by the full text or part of the sentence. The first sentence can determine the position of the answer. Other information (Half content, Content) can be omitted. Where S(q, p)represents the similarity of paragraphs under SBERT, D(q, p)represents the similarity of paragraphs in the DPR, D1, D2 to Dn representations to make BERT datasets for various downstream tasks, our input representations are able to unambiguously represent a sentence and a pair of sentences (e.g, Question, Content) in a sequence of tokens. In the whole work, the training data is continuously refined during the process of different extraction thresholds. The D1 data set is the result of the IDS extraction of the threshold s1, so it has long sequence data. Therefore, for the Base model that can process long data, all texts can be considered. With the improvement of the extraction degree, the length of the sequence data is further reduced until Dn. Therefore, the tiny model is used for learning, even if the tiny can process the sequence data. It is very short, but since the text has been extracted, the small model can still process it without ignoring some parts under the premise of guaranteeing the text information.

B. Calculatie
The SBERT network uses a triplet network structure model to calculate the semantic similarity of two sentences. Among them, this method can reduce the huge computational overhead, so in this task, we use SBERT to calculate the semantic correlation between questions and texts: as we all know, generally speaking, the semantic similarity between a question Q and a text P should be small, Because the question needs to dig out the position of the answer from the text, but if it is compared with other blocks that do not even have an answer, in the contribution score of the first semantic similarity, the block of the text containing the answer often has a higher similarity.
![20230423221448](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423221448.png)
Dense encoder will map this paragraph to a D-dimensional real vector. We can use him to form an index of M text segments to facilitate retrieval. Another Encoder E(x) maps the question into a D-dimensional vector, and then retrieves k text segments whose vectors are the closest to the question vector. The vector dot product used by the approximation function, as:
![20230423221504](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423221504.png)
We consider the straight line path Rn from the baseline x′ to the input x and compute the gradients at all points along the path. Integrated gradients are obtained by cumulating these gradients. Specifically, integrated gradients are defined as the path integral of the gradients along the straight line path from the baseline x′ to the input x. the gradients along the path γ(α) for α ∈ [0, 1]
![20230423221518](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423221518.png)
For many deep networks, it is possible to choose a baseline with predictions close to 0, x′ →0 In this way, the baseline can be ignored when interpreting the attribution results and only the attribution results are assigned to the input without considering the baseline. Path Methods: The integrated gradient method accumulates the gradient along the line between the baseline input and our IDS retriever uses the Dense Paragraph Retriever (DPR) and SBERT fusion together to perform semantic similarity detection on text blocks, which maps any text paragraph to a d-dimensional real-valued vector, and provides all M paragraphs that we will use for retrieval Build an index. At runtime, IDS applies different encoders to map the input text to d-dimensional vectors and retrieves the k paragraphs of which vectors are closest to the question vector. We define the similarity between question and text as:
![20230423221535](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423221535.png)
According to different extraction degrees, We use these datasets to measure the size of SBERT respectively. And the large, medium and small models of T5 are trained and evaluated.

## SECTION IV.Experimental
A. Multi-model performance
The main task studied in this paper is to test models of different sizes on the same task with different levels of difficulty, so we conduct experiments on the Stanford University question answering dataset: SQuAD Stanford question answering dataset is a reading comprehension dataset, which is developed by crowdsourced staff Consists of questions posed in a set of Wikipedia articles, where the answer to each question is a piece of text or span that may not be answered by the corresponding reading article or question. In our experiments, we reused the NQ version authored by DPR. We use DPR, SBERT and Integration Gradient to perform decimation operations on the question answering dataset, where the score for each option. Query, and IG(q,p) represents the contribution of each token in the gradient integration algorithm The average of the sums is shown in Figure 3.

Before using the gradient integral to process the model, the prime minister performs an independent interpretation experiment with the model: In order to show that the interpretation result is independent of the model, I use the bert-large-uncased-whole-word-masking-finetuned-squad model, Perform independent and cascaded independent experiments on it.
![20230423221614](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423221614.png)
![20230423221629](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423221629.png)
B. Data verification
Formally, we introduce the extraction relationship between paragraphs, and create a new extraction data set in the order of relevance. The sequence data with high scores are the candidate paragraphs for relabeling, in order to prove that the difficulty index of our books is from hard to hard. Simple and incremental, we directly apply it to the training model for evaluation, and the results are shown in Figure 2. We can see from the figure that as the threshold increases from 0 to 100%, the performance of the model is also improving. Since the degree of text extraction increases with the threshold, the ratio of ground truth , and also increased to 42.85 with the improvement of the extraction level, all the examples included the answer.
## SECTION VI.Conclusion
This paper proposes a new model training method, which can reduce the amount of data required and improve the performance of the model on the original basis. Since the size determines the performance of the model. I consider using difficulty reduction to improve model performance. I extract significant parts of the text, reduce text complexity, and reduce text length. Improved performance for small models that cannot handle long sequences of data. And has a higher effect on small size models. The method exploits the retrieval ability of dense passage retrieval, the semantic understanding ability of SBERT, and the Integral Gradient method based on attribution method to capture more comprehensive semantic relations. Thereby, abstracting and summarizing text semantics from different degrees can be realized. Implementing our approach, I make important technical contributions to compress training data procedures and propose a dataset that can be manually adjusted in size and complexity. Therefore, it can be used to fine-tune models of different sizes for better performance. Extensive results demonstrate the effectiveness of our method. This is the first time the model has been trained on compressed data. Has been considered for question answering systems. I believe that such an idea itself is worth exploring when designing new training mechanisms. In future work, we will design more data compression techniques and apply current retrieval methods to downstream tasks such as question answering.

## ACKNOWLEDGMENT
This work was partly supported by the National Research Foundation of Korea(NRF) grant funded by the Korea government(MSIT) (No. NRF-2021R1A2C3011169)[50%] and the National Research Foundation of Korea(NRF) grant funded by the Korea government (MSIT) (No. 2022R1A5A7026673)[50%]
