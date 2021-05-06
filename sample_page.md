## Deep Learning - Set Classification Based on A Histopathology Dataset 

**Task:** Given a set of lung tissue images generating from one whole slide, can you predict which lung cancer (LUAD, LUSC, MESO) this patient have? 

### 1. Motivation - Why set classification? 

Classification of high-resolution histopathology slides is one of the most popular yet challenging problems in the field of medical image analysis! Since the size of microscopic images is usually gigapixel, it is computationally infeasible to feed an image like that directly into a deep neural network. 

Since it is hard to directly feed a whole slide into a neural net, why don't we cut a slide into many pieces, call it a set, and then do a set classification?


### 2. Inspring Ideas!

PointNet: PointNet is a deep architecture that can classify a 3D object. Isn't our problem right now kinda similar to a 3D classfication problem? We have a set of order-invariant, permutation-invariant set of elements of an object to classify!

Densely Connected Convolutional Networks (DenseNet): DenseNet can draw discrimination power from images, and it is also reliable candidate solution for image representation in histopathology!

KimiaNet: A modification of DenseNet! Researchers in Kimia Lab @ UWaterloo have proposed to use a modified DenseNet as a feature extractor to extract features of histopathology images!

### 3. Our models!

#### 3.1 Patch-based Convolutional Neural Network (CNN)
<img src="images/set classificaiton/model1.png?raw=true"/>

#### 3.2 DenseNet + modified PointNet
<img src="images/set classificaiton/model2.png?raw=true"/>

### 4. Result!
<img src="images/set classificaiton/model1 kimia net.png?raw=true"/>

<img src="images/set classificaiton/model1 aggregate.png?raw=true"/>

<img src="images/set classificaiton/model2 accuracy.png?raw=true"/>

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
