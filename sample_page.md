## Deep Learning - Set Classification Based on A Histopathology Dataset 

**Task:** Given a set of lung tissue images generating from one whole slide, can you predict which lung cancer this patient have? 

### 1. Motivation 

Classification of high-resolution histopathology slides is one of the most popular yet challenging problems in the field of medical image analysis. Since the size of microscopic images is usually gigapixel, it is computationally infeasible to feed such a high-resolution histopathology slide directly into a deep neural network. 

To tackle these issues, previous literature has proposed to process the input images as a set of smaller patches of a
histopathology slide. In this way, a conventional image classification problem is converted to a set classification problem, and high-resolution histopathology slides can be
classified efficiently without usage of too many computing resources!


### 2. Inspring Idea!

PointNet: PointNet is a deep architecture proposed to classify a 3D object that is formed by a point cloud. A point cloud is a set of points from an Euclidean space and has the three main properties: unordered, interaction among points, and invariant under transformations. A PointNet can classify the object by directly consuming the whole set of points, learning the deep
representation of a 3D object through blocks of T-Net, and extracting their global features through max pooling.

Densely Connected Convolutional Networks (DenseNet): Pre-trained DenseNet can draw discrimination power from intensive training with millions of non-medical images. Also, DenseNet topology is also referred by recent work as a reliable candidate solution for image representation in histopathology. Researchers in Kimia Lab have proposed to use a modified DenseNet as a feature extractor to extract features of histopathology images.

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
