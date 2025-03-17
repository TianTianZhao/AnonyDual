[README.md](https://github.com/user-attachments/files/19213372/README.md)# Dual-Pull : NURBS-Based Fitting of Dual Point Clouds for Fast Surface Reconstruction Using Thick-slice Scans

## A. Supplementary Material

Here, we provide supplementary material for the proposed NURBS fitting of dual point clouds and the pull-back operation to guide implicit neural representations of shapes. In Section A.1, we discuss the NURBS theory, the NURBS fitting process. In Section A.2, we discuss the dual point clouds pullback operation. In Section A.3, we provide more experimental results, including 3D reconstruction visualisation of different organs and different modal data. Finally, we provide a high-definition video that demonstrates the performance of our method in multiple scenarios at the URL.

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

   ![pullLoss](https://github.com/TianTianZhao/AnonyDual/blob/main/images/pullLoss.png)



### A.3. Additional experiments and detail

#### A.3.1 Evaluation metrics

#### A.3.2 Experimental setup and timing performance

#### A.3.3 Surface reconstruction on ACDC(MRI)

![ACDC_0701](https://github.com/TianTianZhao/AnonyDual/blob/main/images/ACDC_0701.png)

#### A.3.3 Surface reconstruction on CHAOS(CT)

![CHAOS-CT-LIVER](https://github.com/TianTianZhao/AnonyDual/blob/main/images/CT10_OUR00.png)






