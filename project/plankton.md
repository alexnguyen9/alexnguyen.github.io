---
layout: page
title: Plankton Image Classification
subtitle: using convolutional neural networks
---



This was a project I did for school using data from [kaggle](https://www.kaggle.com/c/datasciencebowl).  The goal of the project was to classify plankton images to their correct species type.  There were about 30,000 plankton images from about 100 different species. Some examples of the images are shown below.

![Various Plankton](/img/plankton.png)

This was one of my first forays into using neural networks, specifically utilizing convolutional neural networks.  This project was done primary using the Pytorch package to build the neural networks.  I build various neural networks of various depths to see how accuracy varied with depth.  I also utilized data augmentation; using various transforms on the images to artificially inflate the training set and improve accuracy.

The project code can be found [here](https://github.com/alexnguyen9/Plankton-CNN) which also contains a more thorough <a href="https://alexnguyen9.github.io/misc/Final%20Project%20495.pdf" target="_blank">report</a> and also my <a href="http://alexnguyen9.github.io/misc/Final%20Presentation%20495.pptx" target="_blank">final presentation.</a>



