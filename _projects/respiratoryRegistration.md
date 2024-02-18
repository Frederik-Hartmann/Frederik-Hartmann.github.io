---
layout: page
title: Respiratory Registration
description: Image registration of chest CT volumes; 4DCT DIR-Lab Challenge
img: assets/projectImages/respiratoryRegistration/Dalle3_lung_reg.png
importance: 2
category: Registration
toc:
  sidebar: left
---
---
Authors: [Frederik Hartmann](https://github.com/Frederik-Hartmann), [Xavier Beltran Urbano](https://xavibeltranurbano.github.io/)
\
Code: [Github](https://github.com/Frederik-Hartmann/4DCT_DIR-Lab_Challenge)
\
Report: [Github](https://github.com/Frederik-Hartmann/4DCT_DIR-Lab_Challenge/blob/main/Manuscript/Report.pdf)


*This project was carried out within the scope of the Medical Image Registration course taught by [Prof. Dr. Robert Martí](https://scholar.google.com/citations?user=M_sM6x8AAAAJ&hl=en). You can reach a detailed report [here]().* 


December 2023

---
---

## Dataset

To implement this project we have utilized a data set consisting of 4 thoracic 4DCT images acquired at the University of Texas M. D. Anderson Cancer Center in Houston TX. Each CT image in the dataset corresponds to different respiratory-binned phases ranging from T00 to T90. The T00 phase represented end-inhalation while the T50 phase represented end-exhalation. Expert manual annotation was conducted to identify 300 landmarks on each patient’s CT images of T00 and T50. In the Figure 1 we can observe an example of the inhalation and exhalation phases of patient 1 with their corresponding landmarks.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/respiratoryRegistration/Dataset.png" title="Example of the 4DCT DIR-LAB Dataset" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
 An illustration of the respiratory phases in the COPD1 case. A) Inhalation phase with highlighted landmarks (red points). B) Exhalation phase, also
featuring corresponding landmarks (red points).</div>

## Methodology
Our approach can be divided into 2 sections:
### · Preprocessing

Three preprocessing techniques have been employed and tested (see Fig. 2):

1. **Segmentation:** Two approaches were tested. The first approach relied on classical image processing techniques, focusing on the intensity range and position of the lung. It involved coarse segmentation to remove the table, contour tracking for refinement, and post-processing to close holes. The second approach used a pretrained UNet model, trained on various datasets for lung segmentation.
   
2. **Normalization:** After segmentation, min-max normalization was applied using the formula:
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/respiratoryRegistration/Normalization_Equation.png" title="Example of the Normalization formula" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

1. **Histogram Equalization:** Contrast-limited adaptive histogram equalization (CLAHE) was applied to each axial slice.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/respiratoryRegistration/Preprocessing.png" title="Example of the preprocessing employed" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
Sequential overview of preprocessing steps in this approach. A) Original image B) Segmented Image C) Normalized and contrast-enhanced (CLAHE)
image.</div>

### · Training

1. **Registration-Elastix:** This method included an affine transformation and a bspline registration for non-rigid transformations. A custom parameter set adapted for the challenge was used.

2. **Registration-VoxelMorph:** A deep learning approach using a U-Net architecture, the 'VxmDense' model in TensorFlow, was implemented. The model was trained over 100 epochs, combining Mean Squared Error (MSE) and a gradient loss in the loss function.

## Results

Both quantitative and qualitative results are presented in this section.

### · Quantitative Results

<table border="1" align="center">
  <caption><em>Table 1: Results of the registration comparing different approaches. ¹: Bspline file 1 and 2; ²: Bspline file 1</em></caption>
  <tr>
    <th>Parameter File</th>
    <th>Affine</th>
    <th>Bspline</th>
    <th>Segmentation</th>
    <th>CLAHE</th>
    <th>Time (min)</th>
    <th>COPD1</th>
    <th>COPD2</th>
    <th>COPD3</th>
    <th>COPD4</th>
    <th>Mean</th>
    <th>Std</th>
  </tr>
  <tr>
    <td>No Registration</td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>✓</td>
    <td>0:36</td>
    <td>-</td>
    <td>26.33</td>
    <td>21.79</td>
    <td>12.64</td>
    <td>29.58</td>
    <td>22.59</td>
    <td>6.38</td>
  </tr>
  <tr>
    <td>Par11</td>
    <td>✓</td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>0:34 ± 0:04</td>
    <td>25.98</td>
    <td>26.26</td>
    <td>7.50</td>
    <td>25.75</td>
    <td>21.38</td>
    <td>8.01</td>
  </tr>
  <tr>
    <td>Par11</td>
    <td>✓</td>
    <td>✓</td>
    <td>-</td>
    <td>-</td>
    <td>0:51 ± 0:11</td>
    <td>25.37</td>
    <td>26.03</td>
    <td>7.56</td>
    <td>23.02</td>
    <td>20.50</td>
    <td>7.55</td>
  </tr>
  <tr>
    <td>Par11</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
    <td>-</td>
    <td>0:40 ± 0:15</td>
    <td>15.12</td>
    <td>14.68</td>
    <td>5.63</td>
    <td>12.90</td>
    <td>12.08</td>
    <td>3.82</td>
  </tr>
  <tr>
    <td>Par11</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
    <td>10:07 ± 3:24</td>
    <td>14.16</td>
    <td>12.84</td>
    <td>4.87</td>
    <td>10.16</td>
    <td>10.51</td>
    <td>3.56</td>
  </tr>
  <tr>
    <td>Par11</td>
    <td>✓</td>
    <td>✓¹</td>
    <td>-</td>
    <td>✓</td>
    <td>23:43 ± 5:58</td>
    <td>6.88</td>
    <td>6.73</td>
    <td>1.90</td>
    <td>10.91</td>
    <td>6.60</td>
    <td>3.19</td>
  </tr>
  <tr>
    <td>Par11</td>
    <td>✓</td>
    <td>✓²</td>
    <td>✓</td>
    <td>✓</td>
    <td>-</td>
    <td>8.11</td>
    <td>7.75</td>
    <td>2.36</td>
    <td>10.07</td>
    <td>7.07</td>
    <td>2.86</td>
  </tr>
  <tr>
    <td>custom</td>
    <td>✓</td>
    <td>-</td>
    <td>✓</td>
    <td>-</td>
    <td>1:25 ± 0:24</td>
    <td>14.17</td>
    <td>12.88</td>
    <td>4.79</td>
    <td>10.18</td>
    <td>10.50</td>
    <td>3.60</td>
  </tr>
  <tr>
    <td>custom</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
    <td>-</td>
    <td>4:39 ± 0:33</td>
    <td>2.39</td>
    <td>5.60</td>
    <td>1.95</td>
    <td>3.71</td>
    <td>3.41</td>
    <td>1.42</td>
  </tr>
  <tr>
    <td>custom</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
    <td>4:57 ± 0:48</td>
    <td>1.39</td>
    <td>4.86</td>
    <td>1.34</td>
    <td>2.63</td>
    <td>2.55</td>
    <td>1.43</td>
  </tr>
  <tr>
    <td>Voxelmorph</td>
    <td>-</td>
    <td>-</td>
    <td>✓</td>
    <td>✓</td>
    <td>46:37 ± 5:12</td>
    <td>39.76</td>
    <td>11.28</td>
    <td>31.50</td>
    <td>33.12</td>
    <td>28.92</td>
    <td>10.53</td>
  </tr>
</table>



### · Qualitative Results

The image below showcases the outcomes of the final registration for each method employed in this project, providing a visual comparison of their performance:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projectImages/respiratoryRegistration/Results.png" title="Results" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
Example of the registration obtained with the different approaches implemented in this project. A) Moving Image B) Fixed Image C) Affine +
Normalization + CLAHE, D) Only Affine, E) Affine + Segmentation + Normalization + CLAHE, F) Affine + Segmentation , G) Affine + Bspline set 1 +
Bspline set 2 + Segmentation + Normalization + CLAHE, H) Affine + Bspline set 1 + Segmentation + Normalization + CLAHE, I) Custom parameter set
affine only, J) Custom parameter set affine + Bspline + Segmentation, K) Custom parameter set affine + Segmentation, L) Voxelmorph .</div>

## Conclusion

In conclusion, the variety of methods tested led to several findings:

1. Classical approaches can still outperform deep learning methods if fine-tuned properly.
2. Normalization and contrast enhancement did not lead to improved results but rather worse ones.
3. Segmentation improved results significantly by targeting registration only at relevant regions.
4. The parameter set choice was crucial, with improvements observed when adapting it for the dataset's specific requirements.

