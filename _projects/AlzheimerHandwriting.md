---
layout: page
title: Alzheimer's Classification
description: Classification through Handwriting Analysis
img: assets/projectImages/AlzheimerWriting/DALLE3_Alzheimer_Handwriting.png
importance: 4
category: work
toc:
  sidebar: left
---
---
Authors: [Frederik Hartmann](https://github.com/Frederik-Hartmann), [Agustin Cartaya](https://github.com/AgustinCartaya), [Jesus Gonzalez](https://github.com/dagazrev), [Micaela Rivas](https://github.com/MicaelaRivas)
\
Code: [Github]()


*This project was carried out within the scope of the Machine and Deep Learning course taught by [Prof. Dr. Alessandra Scotto di Freca
](https://scholar.google.com/citations?user=1f_f-yAAAAAJ&hl=en). You can reach a detailed report [here]().* 


June 2023

---
---

## Dataset
The dataset consists of 166 observations from Alzheimer’s patients (n = 88) and healthy controls (n = 78), with 92 features per task, including patient demographics and health status. The participants perform 25 tasks in three groups (memory, copy/reverse copy, and graphic) using a graphic tablet to record pen movements. Both the pen features and the images are stored. The pen features are differentiated into in-air and on-paper. The pen features consist of features such as "mean velocity on paper" or "standard deviation of straightness error in the air". For this project, a subset of six tasks was used to classify Alzheimer's disease.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/AlzheimerWriting/task2_sample.png" title="task 2" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/AlzheimerWriting/task3_sample.png" title="task 3" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/AlzheimerWriting/task4_sample.png" title="task 4" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Example of graphic tasks. On the left, "Join two points with a horizontal line continuously for four times". Middle: "Join two points with a vertical line continuously for four times". Right, "Retrace a 6 cm-diameter circle continuously for four times".
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/AlzheimerWriting/task9_sample.png" title="task 9" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/AlzheimerWriting/task10_sample.png" title="task 10" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Example of copy tasks. On the left, "write the bigram ’le’ four times, continuously in cursive format". Right, "Copy the word "foglio" above a line".
</div>

## Analyzing the pen features with Machine Learning
In order to classify Alzheimer's disease, a fully automatic pipeline has been built. First, three different feature selection approaches have been performed. Second, an exhaustive search of different outlier detection techniques, feature scalers, and classifiers has been employed. Based on the cross-validation results (n = 5), the best pipeline for each classifier was chosen. Third, the hyperparameters of each classifier pipeline were tuned individually. This process was repeated for each task.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/AlzheimerWriting/Pipeline.svg" title="pipeline" class="img-fluid rounded z-depth-1" zoomable=true %}
   </div> 
</div>

## Analyzing the images with Deep Learning
For the classification based on the images, a transfer learning approach has been chosen. All backbone models were trained on ImageNet.
<ul>
    <li>VGG19</li>
    <li>ResNET50</li>
    <li>InceptionV3</li>
    <li>InceptionResnetV2</li>
</ul>

For each of the backbone models, four different heads have been tested.
<ul>
<li>GlobalAveragePooling2D layer, followed by a 200-dense layer with ReLU as the
activation function, and one final 2-dense layer classifier based on sigmoid
activation function.</li>
<li>GlobalAveragePooling2D layer followed by a 256-dense layer with ReLU acti-
vation function, then a 128-dense layer again with ReLU activation function,
then a 64-dense layer with the ReLU activation function, and finally a 2-dense
classifier layer using sigmoid as activation. </li>
<li>GlobalAveragePooling2D layer, followed by a 256-dense layer with ReLU
activation function, then a 0.5-dropout layer, and then a 2-dense classifier.
layer with the Softmax activation function.</li>
<li>Feature Extraction with Backbone Model. Classification with Machine Learning</li>
</ul>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/AlzheimerWriting/layers.png" title="pipeline" class="img-fluid rounded z-depth-1" zoomable=true %}
   </div> 
</div>
<div class="caption">
Visualisation of the third approach.
</div>
For each of the different heads, multiple variants of unfrozen and frozen layers were tested.

## Results
In the following, an example of the results is shown. For more detailed results, refer to the report.
<table>
  <caption>Classification using pen features results achieved with Random Forest</caption>
  <thead>
    <tr>
      <th></th>
      <th colspan="3"><strong>In Air</strong></th>
      <th colspan="3"><strong>On Paper</strong></th>
      <th colspan="3"><strong>All dataset</strong></th>
    </tr>
    <tr>
      <th>Task</th>
      <th>Full</th>
      <th>RF10</th>
      <th>RF400</th>
      <th>Full</th>
      <th>RF10</th>
      <th>RF400</th>
      <th>Full</th>
      <th>RF10</th>
      <th>RF400</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>69.8</td>
      <td>69.9</td>
      <td>72.9</td>
      <td>73.5</td>
      <td>73.5</td>
      <td><strong>75.3</strong></td>
      <td>71.7</td>
      <td>74.7</td>
      <td>71.6</td>
    </tr>
    <tr>
      <td>2</td>
      <td>72.3</td>
      <td>74.1</td>
      <td>72.3</td>
      <td>68.6</td>
      <td>71.0</td>
      <td>71.7</td>
      <td>71.1</td>
      <td><strong>74.7</strong></td>
      <td>72.3</td>
    </tr>
    <tr>
      <td>3</td>
      <td>70.5</td>
      <td>66.9</td>
      <td>71.7</td>
      <td>72.2</td>
      <td>74.0</td>
      <td><strong>74.1</strong></td>
      <td>71.1</td>
      <td>69.8</td>
      <td>70.4</td>
    </tr>
    <tr>
      <td>4</td>
      <td>68.1</td>
      <td>66.3</td>
      <td>67.4</td>
      <td>70.5</td>
      <td>71.1</td>
      <td>71.1</td>
      <td><strong>71.8</strong></td>
      <td>69.3</td>
      <td>71.7</td>
    </tr>
    <tr>
      <td>9</td>
      <td>63.2</td>
      <td>69.2</td>
      <td>68.0</td>
      <td>68.7</td>
      <td>68.7</td>
      <td><strong>72.9</strong></td>
      <td>66.2</td>
      <td>67.4</td>
      <td>69.2</td>
    </tr>
    <tr>
      <td>10</td>
      <td>68.0</td>
      <td>66.2</td>
      <td><strong>74.1</strong></td>
      <td>71.7</td>
      <td>69.8</td>
      <td>71.0</td>
      <td>70.4</td>
      <td>67.5</td>
      <td>73.5</td>
    </tr>
  </tbody>
</table>

<table>
  <caption>CNN results for tasks 3</caption>
  <thead>
    <tr>
      <th>Model</th>
      <th>Optimizer</th>
      <th>Unfrozen layers</th>
      <th>Acc</th>
      <th>AUC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>VGG19</td>
      <td>Adam</td>
      <td>2</td>
      <td><strong>84.0</strong></td>
      <td><strong>0.89</strong></td>
    </tr>
    <tr>
      <td>resnet50</td>
      <td>Adam</td>
      <td>3</td>
      <td>68.0</td>
      <td>0.84</td>
    </tr>
    <tr>
      <td>ir2</td>
      <td>SGD</td>
      <td>3</td>
      <td>74.0</td>
      <td>0.82</td>
    </tr>
    <tr>
      <td>inceptionv3</td>
      <td>SGD</td>
      <td>0</td>
      <td>74.0</td>
      <td>0.81</td>
    </tr>
  </tbody>
</table>


<table>
  <caption>Classifier Performance Metrics on Task 3</caption>
  <thead>
    <tr>
      <th><strong>Classifier</strong></th>
      <th><strong>RFT</strong></th>
      <th><strong>DTC</strong></th>
      <th><strong>SVM</strong></th>
      <th><strong>XGB</strong></th>
      <th><strong>MLP</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Accuracy</td>
      <td>0.8</td>
      <td>0.8</td>
      <td><strong>0.84</strong></td>
      <td>0.82</td>
      <td>0.76</td>
    </tr>
    <tr>
      <td>F1-Score</td>
      <td>0.814</td>
      <td>0.799</td>
      <td><strong>0.84</strong></td>
      <td>0.823</td>
      <td>0.799</td>
    </tr>
    <tr>
      <td>Precision</td>
      <td>0.814</td>
      <td>0.869</td>
      <td><strong>0.913</strong></td>
      <td>0.875</td>
      <td>0.727</td>
    </tr>
    <tr>
      <td>Recall</td>
      <td>0.814</td>
      <td>0.740</td>
      <td>0.777</td>
      <td>0.777</td>
      <td><strong>0.888</strong></td>
    </tr>
  </tbody>
</table>


## Conclusion
In the project, Task 1 proved challenging for comparison due to the unique nature of each sample. Writing dynamics analysis was more effective than image analysis using deep learning for this task. Signature tasks, which rely on memory, showed that learned motor patterns impact performance. For other copy tasks and graphic tasks, deep learning methods yielded better results. The study highlighted the importance of choosing the right classifier based on the task, with dynamic features suiting unique form copy tasks and deep learning features excelling in graphic and general copy tasks. 