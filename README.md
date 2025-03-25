[README.md](https://github.com/user-attachments/files/19213372/README.md)# Dual-Pull : NURBS-Based Fitting of Dual Point Clouds for Fast Surface Reconstruction Using Thick-slice Scans

## A. Supplementary Material

Here, we provide supplementary material for the proposed NURBS fitting of dual point clouds and the pull-back operation to guide implicit neural representations of shapes. In Section A.1, we discuss the NURBS theory, the NURBS fitting process. In Section A.2, we discuss the dual point clouds pullback operation. In Section A.3, we provide more experimental results, including 3D reconstruction visualisation of different organs and different modal data. Finally, we provide a Ablation Study.

### A.1. NURBS  theory and the NURBS fitting process

#### A.1.1 NURBS  theory

NURBS (Non-Uniform Rational B-Splines) is a powerful method for representing curves and surfaces, widely used in computer graphics, CAD (Computer-Aided Design), and medical imaging reconstruction. Its features include:

- **Non-Uniform**: Allows precise control over local shapes.
- **Rational**: Uses weights to control the shape of the curve, enabling precise fitting.
- **B-Splines**: Ensures smooth connections and avoids abrupt changes.

# NURBS Curve Mathematical Expression

The mathematical expression of NURBS curves is as follows:

$$
C(u) = \frac{\sum_{i=0}^{n} N_{i,p}(u) w_i P_i}{\sum_{i=0}^{n} N_{i,p}(u) w_i}
$$

Where:

- $C(u)$ is a point on the curve;
- $P_i$ are the control points;
- $N_{i,p}(u)$ are the rational B-spline basis functions;
- $p$ is the degree of the curve;
- $u$ is the parameter that controls the shape of the curve.
  
#### A.1.2 the NURBS fitting process
Algorithm 1 is the organ contour processing procedure, while Algorithm 2 is the dual point cloud fitting process.
# Algorithm 1: NURBS Fitting for Initial Contours

## **Input**: 
- **P** = Extracted contour points from MRI slices

## **Output**: 
- NURBS-fitted contour curves

| **Step** | **Description** |
|----------|----------------|
| **1** | Sort contour points in a counterclockwise order using `sort_contour_point(P)`. |
| **2** | Initialize NURBS curve **N**: |
| **2.1** | Set degree **p = 3**. |
| **2.2** | Define control points. |
| **2.3** | Append the first **p + 1** control points to **P'** for closure. |
| **2.4** | Set the knot vector **T** as a uniform distribution. |
| **3** | Compute NURBS curve points **C** using NURBS interpolation. |
| **4** | Reconstruct 3D points: |
| **4.1** | For each point, assign **z** from the original contour layer. |
| **4.2** | Store the final 3D contour. |
| **5** | Return the fitted NURBS curve. |

# Algorithm 2: Dual Contour Construction

## **Input**: 
- **NURBS-fitted contours** from different MRI slices

## **Output**: 
- Dual contour curves for smooth 3D reconstruction

| **Step** | **Description** |
|----------|----------------|
| **1** | For each index **i** in \([1, m]\) (each corresponding contour point along the layers): |
| **1.1** | Extract corresponding points from all slices. |
| **2** | Construct a new NURBS curve **N_d**: |
| **2.1** | Set degree **p = 3**. |
| **2.2** | Define control points. |
| **2.3** | Generate a non-uniform knot vector. |
| **3** | Compute the dual contour curve. |
| **4** | Store the result in the dual contour set. |
| **5** | Return the dual contour curves. |



   ## A.2.pullback operation
   The figure 1 represents the pull-back process of the dual point cloud to the initial point cloud.
### **Figure 1: pullback operation**   
![Figure 1: pullback operation](https://github.com/TianTianZhao/AnonyDual/blob/main/images/pullLoss.png)


### A.3. Additional experiments and detail

#### A.3.1 Evaluation metrics

#### A.3.2 Experimental setup and timing performance

#### A.3.3 Surface reconstruction on ACDC(MRI)

### **Figure 2: ACDC Dataset Surface Reconstruction**
![Figure 2: ACDC Dataset Surface Reconstruction](https://github.com/TianTianZhao/AnonyDual/blob/main/images/ACDC_com.png)
### **Figure 3: 3D-Slicer compare with ours on ACDC Dataset**
![Figure 3: 3D-Slicer compare with ours on ACDC Dataset Surface Reconstruction](https://github.com/TianTianZhao/AnonyDual/blob/main/images/ACDC_0701.png)

#### A.3.3 Surface reconstruction on CHAOS(CT)
below are the results of our method applied to the ACDC CT dataset to demonstrate the generalisability ability of our method across domains.
### **Figure 4:  ours on CT Dataset**
![CHAOS-CT-LIVER](https://github.com/TianTianZhao/AnonyDual/blob/main/images/CT10_OUR00.png)

## 🔍 Ablation Study

To validate the contribution of each proposed component in improving 3D reconstruction performance, we conduct a detailed ablation study on the **right ventricle** masks of the **ACDC dataset**. The study evaluates the effectiveness of each core module in our proposed **Dual-Pull** framework, including:

- Dual-contour point cloud generation  
- Pull-back loss  
- Hessian smoothness regularization  
- Minimal surface (area) loss  

The baseline is set to **IGR**, and the modules are incrementally added to analyze performance improvements. Results are summarized in the table below.

### Key Observations:

1. **📌 Dual-contour point cloud module (Base model) shows significant gains:**  
Compared to the IGR baseline, adding the dual-contour point cloud module (Base) reduces Chamfer Distance (CD), Hausdorff Distance (HD), and Average Symmetric Surface Distance (ASSD) by **42.49%**, **35.95%**, and **23.26%** respectively. This shows the module effectively reduces reconstruction errors caused by sparse slice intervals and enhances continuity across anatomical layers.

2. **🔧 Individual loss functions contribute differently:**  
Adding **Pull-back**, **Hessian**, and **Area** losses on top of the base model each improve different aspects:
   - **Pull-back loss** leads to the most substantial improvements, with HD reduced by **51.94%**.
   - **Hessian regularization** improves local surface smoothness, reflected in ASSD (↑23.56%).
   - **Minimal area loss** improves global consistency, with HD improved by **41.51%**.

3. **🔗 Combined loss functions exhibit synergy:**  
When two losses are combined:
   - **Area + Hessian** performs best overall, achieving the highest improvement across all three metrics (CD: ↑59.54%, HD: ↑64.66%, ASSD: ↑30.27%).
   - **Pull + Area** ranks second with comparable gains, indicating synergistic effects across loss types.

### 📊 Quantitative Results

| Method                            | Chamfer Distance ↓ | Hausdorff Distance ↓ | ASSD ↓         |
|----------------------------------|---------------------|-----------------------|----------------|
| **IGR (Baseline)**               | 0.006187 (0.00%)    | 0.401772 (0.00%)      | 0.040283 (0.00%) |
| + Dual-contour (Base)            | 0.003558 (**↑42.49%**) | 0.257340 (**↑35.95%**) | 0.030915 (**↑23.26%**) |
| + Base + Pull-back               | 0.002773 (**↑55.18%**) | 0.193097 (**↑51.94%**) | 0.028979 (**↑28.06%**) |
| + Base + Hessian                 | 0.003198 (**↑48.31%**) | 0.257882 (**↑35.81%**) | 0.030793 (**↑23.56%**) |
| + Base + Area                    | 0.003516 (**↑43.17%**) | 0.235006 (**↑41.51%**) | 0.031351 (**↑22.17%**) |
| + Base + Area + Hessian          | 0.002503 (**↑59.54%**) | 0.142005 (**↑64.66%**) | 0.028089 (**↑30.27%**) |
| + Base + Pull-back + Hessian     | 0.003317 (**↑46.39%**) | 0.272452 (**↑32.19%**) | 0.029886 (**↑25.81%**) |
| + Base + Pull-back + Area        | 0.002578 (**↑58.33%**) | 0.146873 (**↑63.44%**) | 0.027947 (**↑30.62%**) |

> 📌 All metrics are "the lower the better". Improvement percentages are relative to IGR.

---

### 🖼 Visual Comparison

The figure below shows visual reconstruction comparisons of the **right ventricle** on the ACDC dataset. It is evident that the **minimal surface loss** provides more natural extrapolation in regions with missing slices.

<p align="center">
  <img src="pic/rv_noarea01.png" alt="No Area" width="300"/>
  <img src="pic/rv_+area02.png" alt="With Area" width="300"/>
</p>

**Figure:** Visual comparison of right ventricle 3D reconstruction on ACDC dataset  
Left: *Ours without Area loss* Right: *Ours with Area loss*






