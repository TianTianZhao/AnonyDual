# Anony
[README.md](https://github.com/user-attachments/files/19213372/README.md)# Dual-Pull : NURBS-Based Fitting of Dual Point Clouds for Fast Surface Reconstruction Using Thick-slice Scans

## A. Supplementary Material

Here, we provide supplementary material for the proposed NURBS fitting of dual point clouds and the pull-back operation to guide implicit neural representations of shapes. In Section A.1, we discuss the NURBS theory, the NURBS fitting process. In Section A.2, we discuss the dual point clouds pullback operation. In Section A.3, we provide more experimental results, including 3D reconstruction visualisation of different organs and different modal data. Finally, we provide a high-definition video that demonstrates the performance of our method in multiple scenarios at the URL.

### A.1. NURBS  theory and the NURBS fitting process

#### A.1.1 NURBS  theory

NURBS (Non-Uniform Rational B-Splines) is a powerful method for representing curves and surfaces, widely used in computer graphics, CAD (Computer-Aided Design), and medical imaging reconstruction. Its features include:

- **Non-Uniform**: Allows precise control over local shapes.
- **Rational**: Uses weights to control the shape of the curve, enabling precise fitting.
- **B-Splines**: Ensures smooth connections and avoids abrupt changes.

The mathematical expression of NURBS curves is as follows:

$$
C(u) = \frac{\sum_{i=0}^{n} N_{i,p}(u) w_i P_i}{\sum_{i=0}^{n} N_{i,p}(u) w_i}]
$$
Where:
- \( C(u) \) is a point on the curve;
- \( P_i \) are the control points;
- \( N_{i,p}(u) \) are the rational B-spline basis functions;
- \( p \) is the degree of the curve;
- \( u \) is the parameter that controls the shape of the curve.

NURBS curves are defined by control points, weights, and a knot vector:
1. **Control Points**: Determine the shape of the curve.

2. **Knot Vector**: Determines the domain and smoothness of the basis functions.

3. **Weights**: Control the influence of different points on the curve.

   #### A.1.1 NURBS fitting process

   $$
   ### Algorithm 1: NURBS Fitting for Initial Contours
   
   **Input:**  
   - `P` = Extracted contour points from MRI slices  
   
   **Output:**  
   - NURBS-fitted contour curves  
   
   ---
   
   1. **Sort contour points** in a counterclockwise order using `sort_contour_point(P)`.  
   2. **Initialize NURBS curve `N`:**  
      - Set degree `p = 3`.  
      - Define control points.  
      - Append the first `p + 1` control points to `P'` for closure.  
      - Set the knot vector `T` as a uniform distribution.  
   3. **Compute NURBS curve points `C` using:**  
      \[
      C(u) = \frac{\sum_{i=0}^{n} N_{i,p}(u) w_i P_i}{\sum_{i=0}^{n} N_{i,p}(u) w_i}
      \]  
   4. **Reconstruct 3D points:**  
      - For each point, assign `z` from the original contour layer.  
      - Store the final 3D contour.  
   5. **Return** the NURBS-fitted contour curves.
   $$

   

   

   ```markdown
   ### Algorithm 2: Dual Contour Construction
   
   **Input:**  
   - NURBS-fitted contours from different MRI slices  
   
   **Output:**  
   - Dual contour curves for smooth 3D reconstruction  
   
   ---
   
   1. **For each index `i` in `[1, m]`** (each corresponding contour point along the layers):  
      - Extract corresponding points from all slices:  
        \[
        P_i = \{P_{i,1}, P_{i,2}, \dots, P_{i,n}\}
        \]  
   2. **Construct a new NURBS curve `N_d`:**  
      - Set degree `p = 3`.  
      - Define control points.  
      - Generate a non-uniform knot vector `T`.  
   3. **Compute the dual contour curve:**  
      \[
      C_d(u) = \frac{\sum_{j=0}^{n} N_{j,p}(u) w_j P_j}{\sum_{j=0}^{n} N_{j,p}(u) w_j}
      \]  
   4. **Store `C_d` in the dual contour set `D`.**  
   5. **Return `D`.**  
   ```

   ## A.2.pullback operation

   ![pullLoss](C:\Users\70436\Desktop\小论文miccai\MICCAI2025-LaTeX-Template\MICCAI 2025 LaTeX Template\figure\pullLoss.png)



### A.3. Additional experiments and detail

#### A.3.1 Evaluation metrics

#### A.3.2 Experimental setup and timing performance

#### A.3.3 Surface reconstruction on ACDC(MRI)



#### A.3.3 Surface reconstruction on CHAOS(CT)

![CHAOS-CT-LIVER](C:\Users\70436\AppData\Roaming\Typora\typora-user-images\image-20250312231429889.png)






