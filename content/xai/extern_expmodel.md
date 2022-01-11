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