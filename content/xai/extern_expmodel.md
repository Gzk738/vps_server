---
title: "Extern_expmodel & average method"
date: 2022-01-11T11:07:30Z
draft: true
---
# review
## Average cascading & average sliding windows
last week I proposed a new method to extern the text and delete text:
Calulate the average of the contribution of sentences rather than sum of all 
tokenziers. 

![20220111201534](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220111201534.png)
# Result

It will take a long time for test all data so I just test half test (1000 data)

Here are results:
| Method |Test Data|Number of cycle for explain| Original model | exp model |Improved accuracy|
| ------ |---------| --------------|---------- | --------- |-----------------|
|Cascading|     4142   | 10|       79.50%      | 79.48%    |        -0.02%   | 
|Sliding windows| 4142 | 10| 79.50% |    79.54%  |         +0.047  |
|low frequency|  1000 |   2         | 79.50%     |  79.50%   | 0.00%  |
|AVG frequency|  - |   2         | 79.50%     |  -    | -     |
| AVG cascading | 1000   |10| 79.50%   |    79.40%  |     -0.10%      |
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
 train data : Probability changes in the interpretation process

 label : index of highest f1 score

 model type : Multiclass Model (10 categories, since the number of explanations is 10, predict the index with the highest f1 value based on the probability change)

 ## MODEL
 ```python
 net = torch.nn.Sequential(
    torch.nn.Linear(2, 500),
    torch.nn.ReLU(),
    torch.nn.Linear(500, 11),
)
 ```
 ![20220111211450](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220111211450.png)
<center>initial state, accuracy = 0.95%</center>

 ![20220111211711](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220111211711.png)
<center>data : 300, accuracy : 0.95->27.8%</center>
