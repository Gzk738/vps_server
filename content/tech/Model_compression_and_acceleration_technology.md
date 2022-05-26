---
title: "Model compression and acceleration technology based on attention extraction mechanism"
date: 2022-05-26T21:06:54+08:00
draft: true
---
<h1 align = "center">Model compression and acceleration technology based on attention extraction mechanism
</h1>
<center>Artificial Brain Research Lab<center/>
<center>Kyungpook university<center/>
<center>Department of Artificial Intelligence<center/>
<center>Zikun Guo<center/>

# Introduction

In recent years, deep learning models have continuously refreshed the performance of traditional models in various fields such as computer vision, natural language processing, and search recommendation advertising, and have been widely used. With the continuous improvement of the computing power of mobile devices, the implementation of mobile AI has also become possible.
Model compression and acceleration can not only improve the performance of mobile models, but also greatly speed up inference response on the server.
Algorithm layer compression acceleration.
Framework layer acceleration
Hardware layer acceleration.

## Disadvantages
The pre-trained model Bert can handle a maximum sequence length of 512. 
When dealing with long text, the text truncation or sliding window method is usually used to make the sequence length of the input model conform to the preset value, but this processing method will Causes the loss of task-related global information.

## Experiment

Sentence splitting:
In the text, only one paragraph contains the answer
Use the tokenizer to tokenize sentences.
The attention and interpretation results of the model are different after the subsentence with and without the answer are entered into the model
![20220526221456](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220526221456.png)
![20220526221510](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220526221510.png)
![20220526221527](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220526221527.png)
![20220526221547](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220526221547.png)
 
# Datasets

## Crashed Dataset
This data set is obtained by further processing the Squad data set. By splitting sentences and comparing with the questions, if the answer is included, the label is the position of the answer, and if the answer is not included, the label is empty
Through the test records of the model, we can obtain a large number of test records, including: changes in probability and changes in f1. Among them, the label of the data we manually add the highest f1 value index
There is basically no difference in size, but there are more labels for each sentence: whether it contains the ground turth

## Data balance
On average, each text will be divided into 3-8 subsentence.
Only the subsentence containing the answer will mark the position of the answer
let the model learn directly, resulting in the model learning a lot of irrelevant information
Remove the empty label data to make the data label balanced
![20220526221632](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220526221632.png)
![20220526221642](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220526221642.png)

## SBERT:
The entire sentence is input into the pre-training model, and the sentence vector of the sentence is obtained, which is then represented as the sentence vector of the sentence.
For two similar sentences, the resulting sentence vectors may be very different.(huge expenses)
Use Siamese and triplet network structures to obtain semantically meaningful sentence embeddings, so as to obtain fixed-length sentence embeddings, and use cosine similarity or Manhattan/Euclidean distance to compare to find semantically similar sentences.
![20220526221735](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220526221735.png)

## Dense channel retrieval
Dense Channel Retrieval (DPR) - is a set of tools and models for state-of-the-art open-domain question answering research.
![20220526221800](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220526221800.png)
![20220526221808](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220526221808.png)

# Conclusion
We propose to use IR to filter useless information in question answering systems for small models to obtain the performance of large models. The intuition is that making filters not only eliminates the need for retraining.

At the same time, it can meet the ability to handle far beyond its own limitations. The problem of incomplete information caused by other algorithms such as sliding windows is prevented.