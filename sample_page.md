## Deep Learning - Set Classification Based on A Histopathology Dataset 

**Task:** Given a set of lung tissue patches generating from one whole slide of a patient, can you identify which lung cancer (LUAD, LUSC, MESO) this patient has? 

### 1. Motivation - Why set classification? 

Classification of high-resolution histopathology slides is one of the most popular yet challenging problems in the field of medical image analysis! Since the size of microscopic images is usually gigapixel, it is computationally infeasible to feed an image like that directly into a deep neural network. 

Since it is hard to directly feed a whole slide into a neural net, why don't we cut a slide into many pieces, call it a set, and then do a set classification?


### 2. Inspring Ideas!

PointNet: PointNet is a deep architecture that can classify a 3D object. Isn't our problem right now kinda similar to a 3D classfication problem? We have a set of order-invariant, permutation-invariant set of elements of an object to classify!

Densely Connected Convolutional Networks (DenseNet): DenseNet can draw discrimination power from images, and it is also reliable candidate solution for image representation in histopathology!

KimiaNet: A modified DenseNet. Used as a feature extractor to extract features of histopathology images!

### 3. Our models!

#### 3.1 Patch-based Convolutional Neural Network (CNN)
Set classification sounds difficult. We don't know how to do that, but what we are familiar with is to use a CNN (e.g. KimiaNet) to classify objects! Why can't we use CNN to classify each patch and then aggregate the results for each set in the very end? 

Below is a model we proposed. For each patch in each set, a CNN will produce a set of probabilities of which class a patch belonging to. We can take average or maximum of all probs, take argmax, and see which class the set belongs to.

<img src="images/set classificaiton/model1.png?raw=true"/>

Below is the performance of KimiaNet on patch-level prediction. You see, the validation accuracy is not good, right?

<img src="images/set classificaiton/model1 kimia net.png?raw=true"/>

What if now we try aggregate the results and make predictions on set-level?

<img src="images/set classificaiton/model1 aggregate.png?raw=true"/>

Wow! A big improvement! We see when using average to aggregate the results, the accuracy achieves 85%!

#### 3.2 KimiaNet + modified PointNet
Now, we want to do something more creative!

We are very inspired by the idea of PointNet. Just recall, a PointNet is a deep architecture that can learn the deep representation of 3D object. In both of their case and our case, our target object (a set) consists of many elements that are order-invariant and permutation invariant! Why can't we borrow their idea and apply it to our case?

Alright! We first used KimiaNet to extract deep features of each patch for each set, and then pass the whole set of features to a PointNet-like deep neural network! The architecture is shown below!

<img src="images/set classificaiton/model2.png?raw=true"/>

Let's see the performance! 
<img src="images/set classificaiton/model2 accuracy.png?raw=true"/>

We see when the size of our network is 512, the accuracy is 90%! That's encouraging!
