---
title: "外部exp模型以及平均值的梯度积分算法"
date: 2022-01-11T11:07:30Z
draft: true
---
# review
## Average cascading & average sliding windows
last week I proposed a new method to extern the text and delete text:
Calulate the average of the contribution of sentences rather than sum of all 
tokenziers. 


# Result

It will take a long time for test all data so I just test half test (1000 data)

Here are results:
| Method |Test Data|Number of cycle for explain| Original model | exp model |Improved accuracy|
| ------ |---------| --------------|---------- | --------- |-----------------|
|Cascading|     4142   | 10|       79.50%      | 79.48%    |        -0.02%   | 
|Sliding windows| 4142 | 10| 79.50% |    79.54%  |         +0.047  |
|low frequency|  1245 |   2         | 83.22%     |  81.56%   | -1.66%  |
|AVG frequency|  - |   2         | 79.50%     |  -    | -     |
| AVG cascading | 4142   |10| 79.50%   |    79.40%  |     -0.10%      |
| AVG Sliding windows | -   | 10|    79.50% | - |  -  |
|Remove individual tokenizers|-     |10| 79.50%   |-       |-      |

## Conclusion
According to the current results(Cascading, Sliding windows, low frequency, AVG cascading)
Not very helpful for model performance. 


The remaining few methods are not expected to improve much
# Extern Seletion model
Through the test records of the model, we can obtain a large number of test records, including: changes in probability and changes in f1. Among them, the label of the data we manually
 add the highest f1 value index:
 ```python
 """f1_score = 
 [[1,1,1,1,1,1,1,1,1,1],
 [0.8,0.8,0.8,1,1,1,0.5,0.5,0.5],
 [0,0,0,0,0,0.5,1,1,1,1,1]]
 ..."""
 for i in f1_score:
    label.append(f1_score.index(max(f1_score)))
    
 ```
 Training data : Probability changes in the interpretation process

 Label : index of highest f1 score

 model type : Multiclass Model (10 categories, since the number of explanations is 10, predict the index with the highest f1 value based on the probability change)

 ## MODEL
 ```python
 net = torch.nn.Sequential(
    torch.nn.Linear(2, 500),
    torch.nn.ReLU(),
    torch.nn.Linear(500, 11),
)
 ```

## Unbalanced Data

![20220111213059](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220111213059.png)
<center>initial state, accuracy = 0.95%</center>

![20220111213219](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220111213219.png)
<center>initial state, accuracy = 92.7592%</center>

Although the accuracy is high, the test has no effect because 99% of the labels in the data are 0
```python
print(f1)
$[[1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [0.6666666666666666,
  0.6666666666666666,
  0.6666666666666666,
  0.6666666666666666,
  0.6666666666666666,
  0.6666666666666666,
  0.6666666666666666,
  0.6666666666666666,
  0.6666666666666666,
  0.6666666666666666,
  0.6666666666666666],
 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5],
 [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0],
 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5],
 [0.2222222222222222, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5]...
 
```
## Balanced Data

![20220112112417](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220112112417.png)

<center>initial state, accuracy = 0.95%</center>

![20220112112402](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220112112402.png)

<center>data : 300, accuracy : 0.95->27.8%</center>

## Possible reasons for this situation
1.Not enough data  (only 300data) 

solution : test more dataset and collect more data

2.The exp data  random data without features, and the model cannot find useful features.

solution : We need to prove that explainable AI is a regular technique or use some mathematical method 
to prove that the data is not random data

![20220112112505](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220112112505.png)