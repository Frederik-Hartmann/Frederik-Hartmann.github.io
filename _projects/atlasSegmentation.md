---
layout: page
title: atlas based segmentation
description: A Hybrid Approach for Brain Tissue Segmentation - Integrating Gaussian Mixture Models with Atlas-based and Tissue Modeling Techniques
img: assets/projectImages/atlasSegmentation/Dalle_3.png
importance: 2
category: Segmentation
toc:
  sidebar: left
---
---
Authors: [Frederik Hartmann](https://github.com/Frederik-Hartmann), [Xavier Beltran Urbano](https://xavibeltranurbano.github.io/)
\
Code: [Github](https://github.com/Frederik-Hartmann/Atlas-Based-Brain-Tissue-Segmentation)
\
Report: [Github](https://github.com/Frederik-Hartmann/Atlas-Based-Brain-Tissue-Segmentation/blob/main/Report/LAB3_Report.pdf)


*This project was carried out within the scope of the Medical Image Segmentation course taught by [Prof. Dr. Xavier Lladó](http://atc.udg.edu/~llado/).

November 2023

---
---
## Dataset
The dataset used for this laboratory contains 20 cases, each with a T1-weighted scan and a ground truth (GT) consisting of segmentation masks for white matter (WM), grey matter (GM), and cerebrospinal fluid (CSF). A registration step was performed prior to the segmentation to register the atlas, i.e., the mean image, to the target. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/atlasSegmentation/dataset.png" title="Example of the different modalities of the dataset" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Fig.1. Example of the different modalities of the dataset. A) T1-weighted C) GT
</div>

## Methodology
Our approach involved a hybrid technique integrating Gaussian Mixture Models (GMM) with Atlas-based and Tissue Modeling techniques. The methodology encompassed various stages:
1. **Atlas Registration**: Employing a multi-step registration process using rigid, affine, and multi-resolution b-spline registration.
2. **Segmentation Techniques**: We experimented with segmentation using tissue models, probabilistic atlas, and combinations of both, with and without GMM. We also explored different GMM initialization methods and integration of the probabilistic atlas at various stages of the process.

## Results
The effectiveness of the methods was evaluated using the Dice Score and Balanced Accuracy metrics. We found that the combination of tissue models and probabilistic atlas yielded the best results. Below are detailed tables of our findings:

### Results of Different Segmentation Approaches without Using GMM Algorithm

<table border="1">
  <tr>
    <th>Initialization</th>
    <th>CSF (Dice: mean ± std)</th>
    <th>WM (Dice: mean ± std)</th>
    <th>GM (Dice: mean ± std)</th>
    <th>BA (mean ± std)</th>
    <th>Time [s] (mean ± std)</th>
  </tr>
  <tr>
    <td>Tissue Models</td>
    <td>0.247 ± 0.164</td>
    <td>0.780 ± 0.087</td>
    <td>0.816 ± 0.111</td>
    <td>0.614 ± 0.100</td>
    <td><strong>0.644 ± 0.101</strong></td>
  </tr>
  <tr>
    <td>Probabilistic Atlas</td>
    <td>0.790 ± 0.055</td>
    <td>0.846 ± 0.015</td>
    <td>0.771 ± 0.019</td>
    <td>0.802 ± 0.025</td>
    <td>1.478 ± 0.531</td>
  </tr>
  <tr>
    <td>Combination of Both</td>
    <td><strong>0.856 ± 0.050</strong></td>
    <td><strong>0.915 ± 0.012</strong></td>
    <td><strong>0.867 ± 0.018</strong></td>
    <td><strong>0.879 ± 0.021</strong></td>
    <td>108.320 ± 19.230</td>
  </tr>
</table>


### Results of Different Initialization Types

<table border="1">
  <tr>
    <th>Initialization</th>
    <th>CSF (Dice: mean ± std)</th>
    <th>WM (Dice: mean ± std)</th>
    <th>GM (Dice: mean ± std)</th>
    <th>BA (mean ± std)</th>
    <th>Time [s] (mean ± std)</th>
  </tr>
  <tr>
    <td>K-means</td>
    <td>0.196 ± 0.289</td>
    <td><strong>0.879 ± 0.089</strong></td>
    <td>0.792 ± 0.053</td>
    <td>0.703 ± 0.106</td>
    <td>40.163 ± 21.664</td>
  </tr>
  <tr>
    <td>Tissue Model</td>
    <td>0.300 ± 0.313</td>
    <td>0.854 ± 0.096</td>
    <td>0.839 ± 0.049</td>
    <td><strong>0.746 ± 0.087</strong></td>
    <td><strong>18.277 ± 14.510</strong></td>
  </tr>
  <tr>
    <td>Probabilistic Atlas</td>
    <td><strong>0.307 ± 0.319</strong></td>
    <td>0.831 ± 0.108</td>
    <td><strong>0.840 ± 0.044</strong></td>
    <td>0.737 ± 0.082</td>
    <td>21.220 ± 11.963</td>
  </tr>
</table>


### Results of Different Initializations and Atlas Integration Points

<table border="1">
  <tr>
    <th>Initialization</th>
    <th>Atlas Integration</th>
    <th>CSF (Dice: mean ± std)</th>
    <th>WM (Dice: mean ± std)</th>
    <th>GM (Dice: mean ± std)</th>
    <th>BA (mean ± std)</th>
    <th>Time [s] (mean ± std)</th>
  </tr>
  <tr>
    <td rowspan="2">Best initialization*</td>
    <td>After</td>
    <td><strong>0.729 ± 0.092</strong></td>
    <td>0.935 ± 0.013</td>
    <td>0.883 ± 0.026</td>
    <td><strong>0.870 ± 0.047</strong></td>
    <td>16.5 ± 14.2</td>
  </tr>
  <tr>
    <td>Into</td>
    <td>0.623 ± 0.139</td>
    <td><strong>0.948 ± 0.010</strong></td>
    <td><strong>0.920 ± 0.014</strong></td>
    <td>0.830 ± 0.044</td>
    <td>29.9 ± 23.0</td>
  </tr>
  <tr>
    <td rowspan="2">Tissue Model & Probabilistic Atlas</td>
    <td>After</td>
    <td>0.354 ± 0.282</td>
    <td>0.850 ± 0.108</td>
    <td>0.848 ± 0.049</td>
    <td>0.756 ± 0.068</td>
    <td>21.4 ± 18.1</td>
  </tr>
  <tr>
    <td>Into</td>
    <td>0.337 ± 0.252</td>
    <td>0.876 ± 0.066</td>
    <td>0.661 ± 0.332</td>
    <td>0.625 ± 0.123</td>
    <td><strong>11.5 ± 6.0</strong></td>
  </tr>
  <tr>
    <td>MNI Atlas & Best Initialization*</td>
    <td>Into</td>
    <td>0.374 ± 0.152</td>
    <td>0.884 ± 0.017</td>
    <td>0.792 ± 0.040</td>
    <td>0.756 ± 0.068</td>
    <td>29.9 ± 23.0</td>
  </tr>
</table>

_*Best initialization refers to the "Tissue Model" method in this context._

In addition, some qualitative results are presented. In Figure 2 we can observe an example of the different segmentations techniques presented in this project. Additionally, the Figure 3 represents a visual comparison of the segmentation obtained by the best approach using our atlas, and the segmentation obtained using the MNI atlas.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/atlasSegmentation/results.png" title="results" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
Fig. 2: Example of the different results obtained in this project. A) Original Image B) GT C) Tissue Models Segmentation D)
Probabilistic Atlas Segmentation E) Combination of C and D F) KMeans Initialization for GMM G) Tissue Models Initialization for
GMM H) Probabilistic Atlas Initialization for GMM I) Best Approach (Tissue Models) including Probabilistic Atlas Into GMM algorithm
J) Best Approach (Tissue Models) including Probabilistic Atlas After EM algorithm K) Tissue Model & Propabalistic Atlas Initialization
For GMM including Probabilistic Atlas After GMM algorithm L) Tissue Model & Propabalistic Atlas Initialization for GMM including
Probabilistic Atlas Into GMM algorithm
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/atlasSegmentation/MNI.png" title="MNI" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
Fig. 3: Example of the segmentation by employing the MNI atlas. A) Original Image, B) Ground Truth, C) Best segmentation by
employing our atlas, D) MNI segmentation
</div>


## Conclusion
In this study, we explored various brain tissue segmentation techniques, focusing on the integration of probabilistic atlases and tissue models. Our experiments revealed that combining these two methods significantly enhances the accuracy of segmentation, as evidenced by the improved dice scores. The standout finding was the superior performance of the tissue model, both as a standalone approach and when used in conjunction with the probabilistic atlas.

Further, we observed that the timing of atlas integration plays a crucial role in the segmentation process. The integration of the probabilistic atlas post-GMM computation emerged as the most effective strategy, suggesting a nuanced interplay between atlas information and GMM initialization. These insights pave the way for more sophisticated approaches in medical imaging analysis.

Moving forward, we aim to incorporate these findings into a deep learning framework. We believe that leveraging the strengths of both traditional and modern techniques will revolutionize the accuracy and efficiency of brain tissue segmentation. The potential for improved diagnostic tools and treatment strategies in neurology is immense, and our study marks a significant step toward realizing this goal.
