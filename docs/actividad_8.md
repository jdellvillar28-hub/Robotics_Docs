# Assignment - Inverse Kinematics and Jacobian

## Objective

In this assignment, I solved the Inverse Kinematics and calculated the Jacobian matrix for a 3-DOF articulated robotic arm.

### What You Will Add:

- **Inverse Kinematics**: Geometric solutions for $\theta_1, \theta_2,$ and $\theta_3$.
- **Jacobian Matrix**: Analytical derivation to relate joint and linear velocities.

---

## 1. Objectives

### General
Analyze a 3-DOF articulated robotic mechanism by applying geometric methods to determine the Inverse Kinematics and computing its Jacobian matrix to relate joint velocities to the end effector's linear velocities.

---

## 2. 3-DOF Articulated Robot Exercise

### Explanation
This exercise focuses on a 3-Degree-of-Freedom (DOF) articulated robotic arm. First, the forward kinematics transformation matrix was established. Then, a geometric approach was used to find the inverse kinematics equations. Finally, the Jacobian matrix was computed by differentiating the position vector equations.

### Forward Kinematics Matrices
Before jumping into the inverse kinematics, the position equations from the transformation matrix are needed. The individual transformation matrices ($A_1, A_2, A_3$) were defined based on the Denavit-Hartenberg parameters:

**Link 1 Transformation Matrix ($A_1$):**
$$
A_1 = \begin{bmatrix} \cos(q_1) & 0 & -\sin(q_1) & 0 \\ \sin(q_1) & 0 & \cos(q_1) & 0 \\ 0 & -1 & 0 & a_1 \\ 0 & 0 & 0 & 1 \end{bmatrix}
$$

**Link 2 Transformation Matrix ($A_2$):**
$$
A_2 = \begin{bmatrix} \cos(q_2) & -\sin(q_2) & 0 & a_2\cos(q_2) \\ \sin(q_2) & \cos(q_2) & 0 & a_2\sin(q_2) \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}
$$

**Link 3 Transformation Matrix ($A_3$):**
$$
A_3 = \begin{bmatrix} \cos(q_3) & -\sin(q_3) & 0 & a_3\cos(q_3) \\ \sin(q_3) & \cos(q_3) & 0 & a_3\sin(q_3) \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}
$$



By multiplying these individual matrices ($T = A_1 \cdot A_2 \cdot A_3$), the complete forward kinematics transformation matrix is obtained:

$$
T_0^3 = \begin{bmatrix} C_1 C_{23} & -C_1 S_{23} & -S_1 & (a_2 C_2 + a_3 C_{23})C_1 \\ S_1 C_{23} & -S_1 S_{23} & C_1 & (a_2 C_2 + a_3 C_{23})S_1 \\ -S_{23} & -C_{23} & 0 & a_1 - a_2 S_2 - a_3 S_{23} \\ 0 & 0 & 0 & 1 \end{bmatrix}
$$

---

## 3. Inverse Kinematics Solution

Using geometric analysis (trigonometry and the law of cosines), the joint angles are solved based on a given end-effector position $(x, y, z)$:

- **$\theta_1$**: Solved using the $x$ and $y$ coordinates.
- **$\theta_3$**: Solved using the Law of Cosines based on the distance to the target.
- **$\theta_2$**: Solved using the elevation angle and the internal geometry of the arm.

---

## 4. Jacobian Matrix

The analytical Jacobian matrix was obtained by calculating the partial derivatives of the $X, Y,$ and $Z$ position equations with respect to the joint variables ($q_1, q_2, q_3$):

$$
J = \begin{bmatrix} 
\frac{\partial X}{\partial q_1} & \frac{\partial X}{\partial q_2} & \frac{\partial X}{\partial q_3} \\
\frac{\partial Y}{\partial q_1} & \frac{\partial Y}{\partial q_2} & \frac{\partial Y}{\partial q_3} \\
\frac{\partial Z}{\partial q_1} & \frac{\partial Z}{\partial q_2} & \frac{\partial Z}{\partial q_3}
\end{bmatrix}
$$