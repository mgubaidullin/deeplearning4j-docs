---
title: DL4J核心概念简介
layout: cn-default
---

# DL4J核心概念简介

> 在进一步深入了解DL4J之前，请先阅读[快速入门指南](http://deeplearning4j.org/cn/quickstart)。该指南将确保您能正确安装并顺利运行DL4J。

> 本页内容假设您已安装最新版本的DL4J。如果您不确定最新的版本是什么，请克隆快速入门指南中的[示例](https://github.com/deeplearning4j/dl4j-examples)，然后查看其中的`pom.xml`文件。


## 概述

机器学习应用程序由两个部分组成。一个部分负责加载数据并对其进行预加工，以供网络学习。我们将这一部分称为ETL（提取、转换、加载）流程。 我们建立了名为[DataVec](http://deeplearning4j.org/cn/simple-image-load-transform) 的库来简化这一流程。另一个部分则是实际的机器学习系统——这是DL4J的核心。

所有机器学习都以向量的数学运算为基础，因此DL4J需要依赖名为[ND4J](http://nd4j.org/)的计算库。ND4J让我们能进行任意N维数组（亦称张量）的运算，而在各类后端的支持下，这一计算库甚至能同时使用CPU和GPU资源。

使用DL4J时，通常需要上述所有部分才能确保工作过程既快速又可靠。 


## 学习和预测前的数据预加工

与其他机器学习和深度学习框架不同，DL4J将数据加载和网络定型分为两套不同的流程。您不能直接将模型指向磁盘上某处存储的数据，而是需要使用DataVec来加载数据。这种模式更加灵活，同时也让数据加载流程保持简单，为用户带来方便。

在开始学习之前，您必须对数据进行预加工，即使模型已定型完毕也一样。数据预加工即加载数据并将其转换至合适的形态和取值区间。用户自行实现的预加工流程非常容易出现错误，所以请尽可能使用DataVec。

深度学习兼容多种数据类型，包括图像、csv、arff、纯文本等；而未来与[Apache Camel](https://camel.apache.org/)集成后，几乎所有已知的数据类型都可以用于深度学习。

使用DataVec时，您需要选择[RecordReader](http://deeplearning4j.org/datavecdoc/org/datavec/api/records/reader/RecordReader.html)接口的一种实现，还需要[RecordReaderDataSetIterator](http://deeplearning4j.org/doc/org/deeplearning4j/datasets/datavec/RecordReaderDataSetIterator.html)（更详细的说明请参见[简易的图像加载与转换](http://deeplearning4j.org/cn/simple-image-load-transform)）。 

有了[DataSetIterator](http://deeplearning4j.org/doc/org/deeplearning4j/datasets/iterator/DataSetIterator.html)之后，您就可以用它来提取数据，确保数据格式符合模型定型要求。 


### 数据标准化

神经网络使用取值范围限于-1到1之间的数据时表现最佳。原因在于网络的定型采用[梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)，而此类算法的激活函数的取值区间通常在-1到1的范围之内。即便您采用的是不会迅速饱和的激活函数，理想的做法仍然是将数据的取值区间限制于这一范围；而这样做通常可以提高网络性能。

DL4J中的数据标准化很容易操作。您只需要决定数据标准化的方式，将相应的[DataNormalization](http://nd4j.org/doc/org/nd4j/linalg/dataset/api/preprocessor/DataNormalization.html)类设置为DataSetIterator的预处理器即可。目前您可以选择的标准化方法包括[ImagePreProcessingScaler](http://nd4j.org/doc/org/nd4j/linalg/dataset/api/preprocessor/ImagePreProcessingScaler.html)、 [NormalizerMinMaxScaler](http://nd4j.org/doc/org/nd4j/linalg/dataset/api/preprocessor/NormalizerMinMaxScaler.html)和[NormalizerStandardize](http://nd4j.org/doc/org/nd4j/linalg/dataset/api/preprocessor/NormalizerStandardize.html)。 ImagePreProcessingScaler显然适合用于图像数据；对其他数据而言，如果输入数据在所有维度上的范围相同，那么NormalizerMinMaxScaler是比较好的选择，其他情形下通常可以使用NormalizerStandardize。

假如您需要进行其他类型的标准化，也可以自行实现DataNormalization接口。

若您最终使用的是NormalizerStandardize，那么应当会发现这一标准化器依靠从数据中提取的统计信息。所以我们必须把这些统计信息与模型一同保存，以便在还原模型时也一并还原这些信息。


## DataSet、INDArray和微批次

DataSetIterator即数据集迭代器；顾名思义，它返回的是[DataSet](http://nd4j.org/doc/org/nd4j/linalg/dataset/DataSet.html)（数据集）对象。DataSet对象只是容纳数据的特征和标签的容器，但它并非每次只能存放单个样例。一个DataSet可以按需要容纳任何数量的样例。

实现方法是将数据的值保存到多个[INDArray](http://nd4j.org/doc/org/nd4j/linalg/api/ndarray/INDArray.html)实例中：一个用于保存样例的特征，一个用于标签，另外两个用于时间序列数据的掩膜（详情参见[使用循环网络/掩膜](http://deeplearning4j.org/cn/usingrnns#masking)）。 
 
INDArray是ND4J提供的N维数组（或称张量）之一。存放特征的INDArray是一个大小为`样例数量 x 特征数量`的矩阵。即便只有一个样例，矩阵的形状也是如此。

为什么不把所有样例都放在一个DataSet内呢？这就要引出深度学习的另一个重要概念：微批次（mini-batch）的划分。为了产生准确率较高的结果，网络经常需要借助大量实际数据来定型。而这样的数据量经常会超过可用的内存，所以有时不可能将其存储在单个DataSet中。但即使有足够的数据存储空间，还有另一个重要原因导致我们不会一次使用全部数据。划分微批次后，可以增加每个epoch周期内模型更新的次数。

那为什么不让每个DataSet只包含一个样例呢？因为模型定型采用的是[梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)算法，需要良好的梯度才能正常进行。一次只使用一个样例，产生的梯度就只会考虑当前样例中的误差。这会导致学习行为出现错误，大幅降低学习速度，甚至可能无法收敛至有用的结果。

微批次应当足够大，能够形成可代表实际数据情况的样本（或者至少能代表您使用的数据）。这意味着微批次应该始终包含所有您希望预测的类别，而且各个类别的样例数量应和总体数据中的分布情况大体保持一致。


## 构建模型

DL4J让您能在非常高的级别上构建深度学习模型。它采用一种构建器（builder）模式来以声明性方式建立模型，如以下（简化的）示例所示：

~~~ java
MultiLayerConfiguration conf = 
	new NeuralNetConfiguration.Builder()
		.optimizationAlgo(OptimizationAlgorithm.STOCHASTIC_GRADIENT_DESCENT)
		.updater(Updater.NESTEROVS).momentum(0.9)
		.learningRate(learningRate)
		.list(
			new DenseLayer.Builder().nIn(numInputs).nOut(numHiddenNodes).activation("relu").build(),
			new OutputLayer.Builder(LossFunction.NEGATIVELOGLIKELIHOOD).activation("softmax").nIn(numHiddenNodes).nOut(numOutputs).build()
		).backprop(true).build();
~~~

如果您熟悉其他深度学习框架，您会发现这与基于Python的Keras有些相似。

与许多其他框架不同，DL4J决定将优化算法和更新器算法分离开来。这样您就可以灵活选择，找到最适合您的数据和课题的算法组合。

除了您在上述示例中看到的[DenseLayer](http://deeplearning4j.org/doc/org/deeplearning4j/nn/conf/layers/DenseLayer.html)（稠密层）和[OutputLayer](http://deeplearning4j.org/doc/org/deeplearning4j/nn/conf/layers/OutputLayer.html)（输出层）之外，还有几种[其他类型的层](http://deeplearning4j.org/doc/org/deeplearning4j/nn/conf/layers/package-summary.html)，例如GravesLSTM、ConvolutionLayer（卷积层）、RBM（受限玻尔兹曼机）、EmbeddingLayer（嵌入层）等。使用这些层，您不仅能搭建简单的神经网络，还可以构建[循环网络](http://deeplearning4j.org/cn/usingrnns)和[卷积网络](http://deeplearning4j.org/convolutionalnets)。 


## 模型定型

模型构建完毕后，您需要对其进行定型。最简单的方式是对模型配置调用`.fit()`方法，将您的DataSetIterator设为参数之一。这将会用您的所有数据来进行一次模型定型。这样遍历所有的数据一次称为一个Epoch周期。DL4J有多种不同方法可以让网络不止一次地遍历数据。

最简单的方式是重置DataSetIterator并循环调用fit方法，次数可任意设定。您可以用这种方法来进行任意个epoch周期的定型，直至达到满意的效果。

另一种方式是`.iterations(N)`配置参数。该参数决定网络应当如何对单个微批次进行迭代（即如何用该微批次定型）。如果您有A、B、C三个微批次，设置`.iterations(3)`会使网络以`AAABBBCCC`的顺序学习数据，而采用3个epoch周期的`.iterations(1)`则会将数据以`ABCABCABC`的顺序输入网络。

还有一种方式是使用[EarlyStoppingTrainer](http://deeplearning4j.org/doc/org/deeplearning4j/earlystopping/trainer/EarlyStoppingTrainer.html)（早停定型器）。您可以任意配置该定型器运行的epoch周期数和运行时间。它会在每个epoch周期结束后（或按您设置的方式）评估网络表现，将表现最佳的版本保存以供之后使用。 

另外请注意，DL4J不仅支持MultiLayerNetwork（多层网络）的定型，也支持更为灵活的[ComputationGraph](http://deeplearning4j.org/compgraph)（计算图）。

### 评估模型表现

模型定型完毕后，您会需要了解它的表现情况。为此，您应当事先分出一部分数据，不在定型中使用，而是仅仅用于模型表现的评估。这部分数据应当和模型在未来实际应用中遇到的数据具有相同的分布形式。不能直接用定型数据来进行评估的原因是机器学习方法在大到一定程度后会倾向于出现过拟合的问题。

模型评估通过[Evaluation](http://deeplearning4j.org/doc/org/deeplearning4j/eval/Evaluation.html)类来实现。普通前馈网络采用的评估方法和循环网络略有不同。更多使用方法的细节参见相应的[示例](https://github.com/deeplearning4j/dl4j-examples)。


## 模型问题排查

用神经网络来解决问题是一个非常依赖经验的过程。所以您必须尝试不同的设置和架构，才能找到效果最让您满意的组合。

DL4J提供一项网络侦听功能来帮助您进行这方面的尝试。您可以为模型设置侦听器，在每个微批次之后调用。DL4J所附带的两种最常用的侦听器是[ScoreIterationListener](http://deeplearning4j.org/doc/org/deeplearning4j/optimize/listeners/ScoreIterationListener.html)（得分迭代侦听器）和[HistogramIterationListener](http://deeplearning4j.org/doc/org/deeplearning4j/ui/weights/HistogramIterationListener.html)（柱状图迭代侦听器）。ScoreIterationListener会直接显示网络当前的误差得分，而HistogramIterationListener则会启动一项网页UI，为您提供一系列不同的信息，帮助您调试网络配置。[神经网络学习的可视化、监测及调试方法](http://deeplearning4j.org/cn/visualization)中介绍了这些数据的解读方法。

您也可参阅[神经网络问题排查](http://deeplearning4j.org/troubleshootingneuralnets)，了解如何提高网络学习效果。
