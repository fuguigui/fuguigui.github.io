---
title: A quick reflection on Master thesis 
tags: [Thesis, Capsule Network, Deep Learning, Graph]
categories: [Sharing]
date: 2021-04-18
---

This article is a quick review of the path of my master thesis until now. I hope this record could be some guide for my future learning path.

I started the thesis unofficially from the middle of October 2020. The topic was about simplicial complex. In one sentence, building neural networks for higher-order graphs via designing a new graph convolution method based on simplicial complex. My mentor gave me 3-4 papers for reference reading. In the beginning, I was a perfectionist. I hoped to figure out each concept I met in the paper reading. I did the paper survey and collected over 50 papers for reading. However, on one side, that field was quite new for me, and the terms I was unfamiliar with were in a huge amount. On the other side, the field itself had not been well studied. I got lost in the paper reading part. I read for around three weeks without clear ideas.

In November, I began to make my hands dirty.  I started coding by reproducing a paper's results. I tried to apply it on other datasets and also adjust its' structure in some way. The experiments results were not good. In December, we worked on putting forward to our own creative ideas in this filed by designing a training task. The name was triangle prediction. The task was not well designed from the following aspects. First, we couldn't find a wide range of useful applications for the task as well as not many suitable datasets for it. Besides, assuming that we have such a task, what was its contribution? We failed to figure out those problems. I did the experiments, more paper reading and datasets collection for around one month. In the end of January, we still couldn't find hopeful path for this topic and had to say goodbye to it.

From Feburary, I started the current topic on temporal knowledge graph. We hope to model temporal knowledge graph by capsule network. In this time, I gave up my perfection principle, and put more attention to efficiency under time pressure. According to experience from the last topic, the paper reading process was greatly accelerated. 

- Two weeks reading for capsule network and temporal knowledge graph.
- two weeks for code studying by downloading, running and understanding. I did this for 5 repositories.
- Two weeks for rewriting the baseline work from tensorflow to pytorch, and write a clear training and testing architecture for it.
- two weeks for testing the baseline models and reproducing the results in the original paper
- two week for comparison experiments on the baseline models and other baseline models.
- Two week for designing and implementing my own models.

This week, I work on adjusting my own model. Many things I would never realized until I created by my own. The biggest issues I met this week are 

- loss function choice, 
- initialization method and 
- the inproper model structure

How did I realize these issues? It came from an NaN loss. After several epochs of training, my model's loss became NaN. I looked for solutions. Many articles online, e.g. [this one](https://zhuanlan.zhihu.com/p/89588946), saied it could be a inproper initialization issue. Then I checked the intermediate embedding of my model. As expected, the norm of these embeddings became infinitely large, compared to a small value less than 1 when initialized. So, I realized it could be my parameters initialization issue as well as the match between parameters' and embeddings' initialization. [A very nice article from deeplearning-AI](https://www.deeplearning.ai/ai-notes/initialization/) explains and shows the power of Xavier initialization. My next step is to find a proper initialization method.

For the issue of loss function, I founded it by observing the gap between the achievable length range of my model's output and ideal length range of the loss function's input. In other words, given the output of my model, the loss couldn't be decreased under a certain bar. Thus, I need to figure out which loss to use in the following week.

I designed an inapproriate model structure for my task: temporal knowledge graph forecasting. Namely, given an incomplete triplet with one element missing, the task is to find the missing elements. For knowledge graph, we are interested in the missing subjects (the first element in the triplet) or objects (the third element), not the relation (the second relation). In the beginning, I modeled this as a multi-class classificaiton problem. Given a pair of subject and object, I hope the model will predict the real relation type and use a cross-entropy loss. This way doesn't work since the relation types could be huge, more than hundreds. Then, the true lable of 1 will be sparse and my model prefer to predicting every type with a low score to minize the loss. By the way, I tried to add negative sampling into this model and made things worse. Since all the labels of the negative samples are zero. 



To wrap up, the main issue of the following week is to find a proper initialization method, design an effective model structure and choose a loss function for it.

