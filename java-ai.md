---
title: Artificial Intelligence (AI) for Java
layout: default
---

# Artificial Intelligence (AI) for Java

## Deep Learning & Neural Networks

Deep learning usually refers to deep artificial neural networks. [Neural networks](https://deeplearning4j.org/neuralnet-overview) are a type of machine learning algorithm loosely modeled on the neurons in the human brain. Deep neural nets involve stacking several neural nets on top of each other to enable a feature hierarchy for more accurate classification and prediction. Deep learning is the state of the art in most tasks or machine perception, involved classification, clustering and prediction applied to raw sensory data. 

### Deeplearning4j

[Deeplearning4j](deeplearning4j.org) is the most widely used open source deep learning library for Java and the JVM. It also has a Scala API and uses Keras as its Python API for neural network configuration. The official website provides many tutorials and simple theoretical explanations for deep learning and neural networks.

<p align="center">
<a href="https://docs.skymind.ai/docs/welcome" type="button" class="btn btn-lg btn-success" onClick="ga('send', 'event', ‘quickstart', 'click');">GET STARTED WITH DEEP LEARNING</a>
</p>

### Neuroph

[Neuroph](http://neuroph.sourceforge.net/) is an open-source Java framework for neural networks. Developers can create neural nets with the Neuroph GUI. The Neuroph API documentation also explains how neural networks work.

### Arbiter (Hyperparameter Optimization for Java)

[Eclipse Arbiter](https://github.com/deeplearning4j/arbiter) is a hyperparameter optimization library designed to automate hyperparameter tuning for deep neural net training. It is the equivalent of Google Tensorflow's Vizier, or the Python library Spearmint. Arbiter is part of the Deeplearning4j framework. 

## Expert Systems

An expert system is also called a rules-based system. The rules are typically if-then statements; i.e. if this condition is met, then perform this action. An expert system usually comprises hundreds or thousands of nested if-then statements. Expert systems were a popular form of AI in the 1980s. They are good at modeling static and deterministic relationships; e.g. the tax code. However, they are also brittle and they require manual modification, which can be slow and expensive. Unlike, machine-learning algorithms, they do not adapt as they are exposed to more data. They can be a useful complement to a machine-learning algorithm, codifying the things that should always happen a certain way. 

### Drools

[Drools](https://www.drools.org/) is a business rules management system backed by Red Hat. 

## Natural-Language Processing

Natural language processing (NLP) refers to applications that use computer science, AI and computational linguistics to enable interactions between computers and human languages, both spoken and written. It involves programming computers to process large natural language corpora (sets of documents). 

Challenges in natural language processing frequently involve natural language understanding (NLU) and natural language generation  (NLG), as well as connecting language, machine perception and dialog systems.

### OpenNLP

[Apache OpenNLP](https://opennlp.apache.org/) is a machine-learning toolkit for processing natural language; i.e. text. The official website provides API documentation with information on how to use the library.

### Stanford CoreNLP

[Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/) is the most popular Java natural-language processing framework. It provides various tools for NLP tasks. The official website provides tutorials and documentation with information on how to use this framework.

## Machine Learning

Machine learning encompasses a wide range of algorithms that are able to adapt themselves when exposed to data, this includes random forests, gradient boosted machines, support-vector machines and others. 

### SMILE

[SMILE](https://github.com/haifengl/smile) stands for Statistical and Machine Intelligence Learning Engine. SMILE was create by Haifeng Lee, and provides fast, scalable machine learning for Java. 

### SINGA

[Apache SINGA](https://singa.incubator.apache.org/en/index.html) is an open-source machine-learning library capable of distributed training, with a focus on healthcare applications. 

### Java Machine Learning Library (Java-ML)

[Java-ML](http://java-ml.sourceforge.net/) is an open source Java framework which provides various machine learning algorithms specifically for programmers. The official website provides API documentation with many code samples and tutorials.

### RapidMiner

[RapidMiner](https://rapidminer.com/) is a data science platform that supports various machine- and deep-learning algorithms through its GUI and Java API. It has a very big community, many available tutorials, and an extensive documentation.

### Weka 

[Weka](http://www.cs.waikato.ac.nz/ml/weka/) is a collection of machine learning algorithms that can be applied directly to a dataset, through the Weka GUI or API. The WEKA community is large, providing various tutorials for Weka and machine learning itself.

### MOA (Massive On-line Analysis)
[MOA (Massive On-line Analysis)](https://moa.cms.waikato.ac.nz/) is for mining data streams. 

### Encog Machine Learning Framework

[Encog](http://www.heatonresearch.com/encog/) is a Java machine learning framework that supports many machine learning algorithms. It was developed by Jeff Heaton, of Heaton Research. The official website provides documentation and examples.

## Reinforcement Learning

### Eclipse RL4J

[RL4J](https://github.com/deeplearning4j/rl4j) is a reinforcement learning library for Java that is part of the Eclipse Deeplearning4j framework. [RL4J examples for A3C and Deep-Q learning are here](https://github.com/deeplearning4j/dl4j-examples/tree/master/rl4j-examples). 

### Burlap

The [Brown-UMBC Reinforcement Learning and Planning](http://burlap.cs.brown.edu/) is for the use and development of single or multi-agent planning and learning algorithms and domains to accompany them.

## Scientific Computing/Numerical Computing for Java

All machine learning libraries depend on some form of scientific computing. 

### Eclipse ND4J: Numpy for the JVM

[ND4J](https://nd4j.org/) stands for n-dimensional arrays for Java. An n-dimensional array is also called a tensor. ND4J is Numpy for the JVM. It is the most powerful and flexible scientific computing framework available for JVM languages such as Java and Scala. ND4J uses [JavaCPP](https://github.com/bytedeco/javacpp) and [libnd4j](https://github.com/deeplearning4j/libnd4j) (a C++ library) to perform large matrix manipulations efficiently. [ND4J Github Repository](https://github.com/deeplearning4j/nd4j). Unlike [jblas](http://jblas.org/), ND4J is under active development. 

## <a name="intro">Other Introductory Resources</a>

For people just getting started with deep learning, the following tutorials and videos provide an easy entrance to the fundamental ideas of feedforward networks:

* [Introduction to Deep Neural Networks](./neuralnet-overview.html)
* [Convolutional Networks for Image Recognition](./convolutionalnets.html)
* [Recurrent Networks and LSTMs](./lstm.html)
* [Generative Adversarial Networks (GANs)](/generative-adversarial-network.html)
* [Deep Reinforcement Learning](./deepreinforcementlearning.html)
* [Symbolic Reasoning and Deep Learning](./symbolicreasoning.html)
* [Graph Data and Deep Learning](./graphdata.html)
* [Word2vec and Natural-Language Processing](./word2vec.html)
* [MNIST for Beginners](./mnist-for-beginners.html)
* [Restricted Boltzmann Machines](./restrictedboltzmannmachine.html)
* [Eigenvectors, PCA, Covariance and Entropy](./eigenvector.html)
* [Glossary of Deep-Learning and Neural-Net Terms](./glossary.html)
* [Deeplearning4j Examples via Quickstart](./quickstart.html)
* [Artificial Intelligence (AI) for Scala](./scala-ai.html)
* [Inference: Machine Learning Model Server](./modelserver.html)
* [Multilayer Perceptron (MLPs) for Classification](./multilayerperceptron.html)
