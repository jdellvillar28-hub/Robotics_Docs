
# Transform Nomenclature
In robotics, transformations are often represented using a combination of rotation and translation. The common nomenclature for these transformations includes:

- **Rotation Matrix \( R \)**: A 3x3 matrix that represents the rotation of a frame in 3D space.

- **Translation Vector \( T \)**: A 3x1 vector that represents the translation of a frame in 3D space.

- **Homogeneous Transformation Matrix \( H \)**: A 4x4 matrix that combines

![TEXT](recursos/imgs/act2/a%20(1).png){ width="600" }
---
# Exercise 1: Rotation of a Vector
1. **Rotation around the \( Y \) axis (45°)**  
   Rotate the vector around the \( Y \) axis by 45°.

2. **Rotation around the \( X \) axis (60°)**  
   Then, rotate the resulting vector around the \( X \) axis by 60°.

3. **Final result**  
   The final vector is the one that has undergone both rotations.

![TEXT](recursos/imgs/act2/a%20(2).png){ width="600" }

---

# Exercise 2: Translation and Rotation
1. **Rotation of \( B \) with respect to \( A \)**  
   Rotate the frame \( B \) by 30° around the \( X \) axis of \( A \).

2. **Translation of \( B \) with respect to \( A \)**  
   Translate frame \( B \) by a vector of \( [5, 10, 0] \) with respect to \( A \).

3. **Final transformation**  
   Combine both rotation and translation to obtain the new position of \( B \) with respect to \( A \).

![TEXT](recursos/imgs/act2/a%20(3).png){ width="600" }

---

# Exercise 3: Transformation Between Frames
1. **Rotation from \( A \) to \( B \)**  
   Apply the necessary rotation to convert the frame \( A \) to frame \( B \).

2. **Translation from \( A \) to \( B \)**  
   Translate the frame \( A \) to the new location of frame \( B \).

3. **Rotation from \( A \) to \( C \)**  
   Apply the necessary rotation to convert the frame \( A \) to frame \( C \).

4. **Translation from \( A \) to \( C \)**  
   Translate the frame \( A \) to the new location of frame \( C \).

5. **Complete transformation**  
   Combine the rotation and translation to get the final transformation from \( A \) to \( C \).
![TEXT](recursos/imgs/act2/a%20(4).png){ width="600" }
![TEXT](recursos/imgs/act2/a%20(5).png){ width="600" }


