---
layout: page
title: A new brain atlas
description: Deep learning ensemble for brain tissue segmentation
img: assets/projectImages/DL_brain_seg/Dalle3_brain_dl.png
importance: 1
category: Segmentation
toc:
  sidebar: left
---
---
Authors: [Frederik Hartmann](https://github.com/Frederik-Hartmann), [Xavier Beltran Urbano](https://xavibeltranurbano.github.io/)
\
Code: [Github](https://github.com/Frederik-Hartmann/DL-Ensemble-Brain-Tissue-Segmentation)
\
Report: [Github](https://github.com/Frederik-Hartmann/DL-Ensemble-Brain-Tissue-Segmentation/blob/main/Manuscript/Report.pdf)


*This project was carried out within the scope of the Medical Image Segmentation course taught by [Prof. Dr. Xavier Lladó](http://atc.udg.edu/~llado/).


December 2023

---
---
## Dataset
The dataset used in this study is the IBSR18, containing 18 T1-weighted scans of normal subjects from the Internet Brain Segmentation Repository (IBSR). It includes preprocessed scans with a 1.5 mm slice thickness and ground truth segmentation for white matter (WM), gray matter (GM), and cerebrospinal fluid (CSF).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/DL_brain_seg/ISBR18_Dataset.png" title="Example of the IBSR18 Dataset" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
Example of the IBSR18 Dataset
</div>


## Methodology

Our approach can be divided into 2 sections:
### · Preprocessing
First, normalization was implemented using a robust z-normalization technique, chosen due to the non-Gaussian distribution and the presence of outliers in some data. This involves adjusting the intensity values by subtracting the mean (calculated using the 25th and 75th quantiles) and dividing by the standard deviation, calculated over the same quantile range. Additionally, data augmentation was performed through random flips and rotations of the original images to enhance algorithm reliability. Finally, selective slice selection was employed for multi-class segmentation, prioritizing slices containing cerebrospinal fluid (CSF) to address class imbalance.
### · Training
For the training of this approach, several architectures, such as U-Net, Res-U-Net, Dense-U-Net, and SegResNet, have been used. We have also investigated the effect of utilizing distinct image planes (axial and coronal). Additionally, both 2D and 3D approaches have been analyzed. In Table 1, you can observe all the networks that we have trained independently.
After the single trainings, we have ensembled the networks in different configurations (see Table 2).

## Results
Both quantitative and qualitative results are presented in this section. They are as follows:
### · Quantitative Results
In the following tables, we can observe the results obtained for the single trainings and the different ensemble methods carried out in this project. The metrics utilized to evaluate the segmentations are the Dice Coefficient (DSC) and the Hausdorff Distance (HD).

#### Single Model Results
<table border="1" align="center">
  <caption><em>Table 1: Single model results on the validation set.</em></caption>
  <tr>
    <th>Model</th>
    <th>CSF Dice</th>
    <th>GM Dice</th>
    <th>WM Dice</th>
    <th>Mean Dice</th>
    <th>CSF HD</th>
    <th>GM HD</th>
    <th>WM HD</th>
    <th>Mean HD</th>
  </tr>
  <tr>
    <td>2D Coronal U-Net</td>
    <td>0.878</td>
    <td>0.937</td>
    <td>0.933</td>
    <td>0.917</td>
    <td>39.352</td>
    <td>11.344</td>
    <td>10.443</td>
    <td>20.380</td>
  </tr>
  <tr>
    <td>2D Coronal Dense U-Net</td>
    <td><strong>0.899</strong></td>
    <td>0.937</td>
    <td>0.938</td>
    <td>0.925</td>
    <td>17.168</td>
    <td>12.199</td>
    <td><strong>8.149</strong></td>
    <td>12.502</td>
  </tr>
  <tr>
    <td>2D Coronal Multi-U-Net</td>
    <td>0.890</td>
    <td>0.935</td>
    <td>0.936</td>
    <td>0.920</td>
    <td>26.234</td>
    <td>13.391</td>
    <td>8.422</td>
    <td>16.016</td>
  </tr>
  <tr>
    <td>2D Coronal Res-U-Net</td>
    <td>0.882</td>
    <td>0.931</td>
    <td>0.931</td>
    <td>0.915</td>
    <td>21.894</td>
    <td>12.000</td>
    <td>10.905</td>
    <td>14.933</td>
  </tr>
  <tr>
    <td>2D Axial U-Net</td>
    <td>0.868</td>
    <td>0.929</td>
    <td>0.922</td>
    <td>0.906</td>
    <td>26.598</td>
    <td>9.876</td>
    <td>9.887</td>
    <td>15.454</td>
  </tr>
  <tr>
    <td>2D Axial Dense-U-Net</td>
    <td>0.868</td>
    <td>0.920</td>
    <td>0.920</td>
    <td>0.902</td>
    <td>27.137</td>
    <td>11.281</td>
    <td>10.580</td>
    <td>16.333</td>
  </tr>
  <tr>
    <td>2D Axial Multi-U-Net</td>
    <td>0.876</td>
    <td>0.923</td>
    <td>0.926</td>
    <td>0.908</td>
    <td>30.938</td>
    <td>10.546</td>
    <td>9.872</td>
    <td>17.119</td>
  </tr>
  <tr>
    <td>2D Axial Res-U-Net</td>
    <td>0.866</td>
    <td>0.925</td>
    <td>0.921</td>
    <td>0.904</td>
    <td>23.733</td>
    <td>21.277</td>
    <td>10.113</td>
    <td>18.375</td>
  </tr>
  <tr>
    <td>2D Seg-Res-Net</td>
    <td>0.877</td>
    <td>0.933</td>
    <td>0.935</td>
    <td>0.915</td>
    <td><strong>13.540</strong></td>
    <td>9.977</td>
    <td>9.449</td>
    <td><strong>10.989</strong></td>
  </tr>
  <tr>
    <td>3D U-Net</td>
    <td>0.882</td>
    <td><strong>0.942</strong></td>
    <td><strong>0.942</strong></td>
    <td><strong>0.922</strong></td>
    <td>16.202</td>
    <td>12.864</td>
    <td>11.574</td>
    <td>13.486</td>
  </tr>
  <tr>
    <td>3D Seg-Res-Net</td>
    <td>0.888</td>
    <td>0.935</td>
    <td>0.937</td>
    <td>0.921</td>
    <td>15.198</td>
    <td>10.367</td>
    <td>9.541</td>
    <td>11.702</td>
  </tr>
  <tr>
    <td>SynthSeg</td>
    <td>0.812</td>
    <td>0.829</td>
    <td>0.888</td>
    <td>0.843</td>
    <td>29.822</td>
    <td><strong>8.353</strong></td>
    <td>12.066</td>
    <td>16.747</td>
  </tr>
</table>


#### Ensemble Results

<table border="1" align="center">
  <caption><em>Table 2: Ensemble results on the validation set.</em></caption>
  <tr>
    <th>Model</th>
    <th>CSF Dice</th>
    <th>GM Dice</th>
    <th>WM Dice</th>
    <th>Mean Dice</th>
    <th>CSF HD</th>
    <th>GM HD</th>
    <th>WM HD</th>
    <th>Mean HD</th>
  </tr>
  <tr>
    <td>The Coronal Ensemble Mean</td>
    <td>0.895</td>
    <td>0.939</td>
    <td>0.939</td>
    <td>0.925</td>
    <td>18.508</td>
    <td>9.630</td>
    <td>7.783</td>
    <td>11.974</td>
  </tr>
  <tr>
    <td>The Coronal Ensemble Maximum</td>
    <td>0.893</td>
    <td>0.939</td>
    <td>0.939</td>
    <td>0.923</td>
    <td>23.860</td>
    <td>9.811</td>
    <td>8.843</td>
    <td>14.171</td>
  </tr>
  <tr>
    <td>The Coronal Ensemble Majority</td>
    <td>0.890</td>
    <td>0.939</td>
    <td>0.937</td>
    <td>0.922</td>
    <td>19.123</td>
    <td>11.465</td>
    <td>7.564</td>
    <td>12.717</td>
  </tr>
  <tr>
    <td>The Axial Ensemble Mean</td>
    <td>0.884</td>
    <td>0.930</td>
    <td>0.928</td>
    <td>0.914</td>
    <td>17.121</td>
    <td>10.704</td>
    <td>9.127</td>
    <td>12.317</td>
  </tr>
  <tr>
    <td>The Axial Ensemble Maximum</td>
    <td>0.881</td>
    <td>0.930</td>
    <td>0.927</td>
    <td>0.913</td>
    <td>23.055</td>
    <td>10.655</td>
    <td>9.782</td>
    <td>14.498</td>
  </tr>
  <tr>
    <td>The Axial Ensemble Majority</td>
    <td>0.877</td>
    <td>0.930</td>
    <td>0.925</td>
    <td>0.911</td>
    <td>22.114</td>
    <td>10.946</td>
    <td>9.277</td>
    <td>14.112</td>
  </tr>
  <tr>
    <td>The Coronal + Axial Mean</td>
    <td>0.897</td>
    <td>0.939</td>
    <td>0.938</td>
    <td>0.925</td>
    <td>16.410</td>
    <td>8.902</td>
    <td>9.095</td>
    <td>11.469</td>
  </tr>
  <tr>
    <td>The Coronal + Axial Maximum</td>
    <td>0.893</td>
    <td>0.938</td>
    <td>0.937</td>
    <td>0.923</td>
    <td>21.901</td>
    <td>10.270</td>
    <td>9.370</td>
    <td>13.847</td>
  </tr>
  <tr>
    <td>The Coronal + Axial Majority</td>
    <td>0.894</td>
    <td>0.940</td>
    <td>0.938</td>
    <td>0.924</td>
    <td>16.611</td>
    <td>9.811</td>
    <td>8.653</td>
    <td>11.692</td>
  </tr>
  <tr>
    <td>The Multidimensional Ensemble Mean</td>
    <td><strong>0.904</strong></td>
    <td><strong>0.945</strong></td>
    <td><strong>0.948</strong></td>
    <td><strong>0.932</strong></td>
    <td><strong>11.918</strong></td>
    <td><strong>8.730</strong></td>
    <td><strong>7.660</strong></td>
    <td><strong>9.436</strong></td>
  </tr>
</table>


### · Qualitative Results
Here, an example of the segmentation obtained from the ensemble with the best performance (the Multidimensional Ensemble Mean) is presented in Fig 2.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/DL_brain_seg/Result.png" title="Results on the IBSR18 Dataset" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
Comparison of segmentation results of the best-performing ensemble: The Multidimensional Ensemble. Displayed are axial slices
(left), sagittal slices (middle), and coronal slices (right)</div>

## Conclusion
The study clearly illustrates the efficacy of an ensemble methodology that synergizes 2D and 3D convolutional neural networks (CNNs) for segmenting brain tissue. This innovative approach benefits significantly from leveraging various orientations of 2D slices in combination with both 2D and 3D models. Among the various techniques explored, the ensemble method, especially the mean of probabilities technique, stands out for its exceptional robustness and precision in results.
Future scope would include working on dealing with the imbalance problem since, even after obtaining excellent performance for all tissues, it can be appreciated that the results obtained for CSF are slightly lower than the WM and GM ones.
