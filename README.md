## Conversion between quaternions and other rotation parametrizations
This Python module provides conversion functions between quaternions and other rotation parameterizations (axis-angle, rotation matrix, Euler angles).
> See also the pure-python package [quaternionic](https://github.com/moble/quaternionic).

## Parameterizations of rotations
Choosing how to represent the orientation of a solid in three-dimensional space is a fairly complex problem. The aim is to arrive at a compact (no dependent elements) and unique way of describing the orientation of the solid, while guaranteeing numerical stability. The scientific literature presents several choices for parameterizing the orientation of a rigid body: Cardan angles, Euler angles, Cartesian rotation vectors, rotation matrix, Rodrigues parameters, etc.

To avoid the gimbal lock phenomenon (representation singularity) that occurs for some of the above parameterizations, quaternions can be used as rotation parameters.

## Quaternions as rotations
Introduced by Irish mathematician William R. Hamilton (1843), quaternions are hypercomplex numbers using 4 variables (a scalar real part and 3 imaginary components) to represent the orientation of a rigid body.
the orientation of a rigid body. This representation introduces a holonomic constraint equation related to the normalization of the quaternion.

Quaternion is given by the quadruplet :

$$
\underline{p}=\left\langle\begin{array}{llll} e_0 & e_1 & e_2 & e_3 \end{array}\right\rangle^t=e_0+\underline{e}
$$

- Axis of rotation:

$$
\underline{e}^t=\left\langle e_1, e_2, e_3\right\rangle^t
$$

- Angle of rotation $\theta$ :
  
$$
\left\{
    \begin{aligned}
        e_0 & = \cos \frac{\theta}{2}\\
        |\underline{e}| & = \sin \frac{\theta}{2}
    \end{aligned}
    \right.
$$

- Quaternion normalization equation:

$$
    e_0^2+e_1^2+e_2^2+e_3^2=1
$$

### Advantages
Compact writing compared to the rotation matrix ● Numerical stability ● No singularity (Cardan blocking)

### Combining rotations
Let $q_1 = \left\langle e_0, \underline{e}\right\rangle$ and $q_2 = \left\langle u_0, \underline{u}\right\rangle$, two quaternions each representing a given rotation, the product defines the sequence of rotations $q_1$ then $q_2$.

$$
q_2 \cdot q_1 = \left\langle u_0, \underline{u}\right\rangle \left\langle e_0, \underline{e}\right\rangle = \left\langle u_0 e_0 - \underline{u}^{T} \underline{e} \quad, \quad u_0 \underline{e} + e_0 \underline{u} + \underline{u} \times \underline{e}\right\rangle
$$

## Converting from one representation of a solid's orientation to another
### Axis-angle to quaternion 
Knowing the axis and angle of a rotation $\left\langle \theta, x, y, z\right\rangle$, we can convert it to a quaternion $q = \left\langle q_0, q_1, q_2, q_3\right\rangle$ defining the same rotation as follows:

$$
\underline{q}=\cos \frac{\theta}{2}+\sin \frac{\theta}{2} \underline{u} \quad \text{avec}  \quad \underline{u} = \left\langle x, y, z\right\rangle 
$$

The `axisAngleToQuaternion()` function in the `quaternion.py` module performs this conversion.

### Quaternion to axis-angle 
Given a quaternion p = ⟨e0, e1, e2, e3⟩, the axis of rotation u and the angle of rotation θ can be obtained from the following equations:

$$
\left\{
        \begin{aligned}
                \underline{u} & =\frac{\underline{e}}{|\underline{e}|} \\
                \theta & =2 \arctan \left(\frac{|\underline{e}|}{e_0}\right) \quad \text { with } \quad \underline{e}^t=\left\langle e_1, e_2, e_3\right\rangle^t 
        \end{aligned}
        \label{quaternionToAxisAngle-Equation}
\right.
$$

The `quaternionToAxisAngle()` function in the `quaternion.py` module performs this conversion.

### Quaternion to rotation matrix 
Given a quaternion $p = \left\langle e_0, e_1, e_2, e_3\right\rangle$ representing the orientation of a given solid, the 3x3 matrix corresponding to this orientation is given by :

$$
\underline{\underline{R}}(\underline{p})
    =
\left[\begin{array}{ccc}
        e_0^2+e_1^2-e_2^2-e_3^2 & 2 e_1 e_2-2 e_0 e_3 & 2 e_0 e_2+2 e_1 e_3 \\
        2 e_0 e_3+2 e_1 e_2 & e_0^2-e_1^2+e_2^2-e_3^2 & 2 e_2 e_3-2 e_0 e_1 \\
        2 e_1 e_3-2 e_0 e_2 & 2 e_0 e_1+2 e_2 e_3 & e_0^2-e_1^2-e_2^2+e_3^2
        \end{array}
\right]
$$

The `quaternionToRotationMatrix()` function in the `quaternion.py` module performs this conversion.

### Rotation matrix to quaternion 
Consider the following rotation matrix R:

$$
R
    =
\left[\begin{array}{lll}
        r_{11} & r_{12} & r_{13} \\
        r_{21} & r_{22} & r_{23} \\
        r_{31} & r_{32} & r_{33}
    \end{array}
\right]
$$

The quaternion defining the orientation equivalent to this rotation matrix can be defined in 2 steps.

**Step 1:** We first compute $\left|q_0\right|, \left|q_1\right|, \left|q_2\right|, \left|q_3\right|$ but not their signs.

$$
\begin{aligned}
        &\left|q_0\right|=\sqrt{\frac{1+r_{11}+r_{22}+r_{33}}{4}}\\
        &\left|q_1\right|=\sqrt{\frac{1+r_{11}-r_{22}-r_{33}}{4}}\\
        &\left|q_2\right|=\sqrt{\frac{1-r_{11}+r_{22}-r_{33}}{4}}\\
        &\left|q_3\right|=\sqrt{\frac{1-r_{11}-r_{22}+r_{33}}{4}}
\end{aligned}
$$

**Step 2:** Identify the signs by finding the largest absolute value of $q_0, q_1, q_2, q_3$ and assuming its sign is positive. We then calculate the remaining components of the quaternion as table below. Division by the largest amplitude reduces errors in numerical numerical accuracy.

<div align="center">
<img align="center" src="https://github.com/joycesudi/quaternion/blob/main/images/rotation-matrix-to-quaternion-table.png" width="80%">
</div>

The reason for this ambiguity over the signs of the quaternion components is that the quaternions $\left(q_0, q_1, q_2, q_3\right)$ and $\left(-q_0, -q_1, -q_2, -q_3\right)$ define the same rotation.
The `rotationMatrixToQuaternion()` function in the `quaternion.py` module performs this conversion.

### Euler angles to quaternion
Introduced by Swiss mathematician and physicist Leonhard Euler, the three Euler angles (roll, pitch, yaw) are used to define the orientation of a rigid body. Roll can be defined as the orientation with respect to the X axis, pitch as the orientation with respect to the Y axis, and yaw as the orientation with respect to the Z axis.

There are dozens of mutually exclusive ways of defining Euler angles (Conventions XYX, XYZ, XZX, XZY, YXY, YXZ, YZX, YZY, ZXY, ZXZ, ZYX, ZYZ). We therefore need to define which convention is used in our code.

We have used the following definition of Euler angles in the developed software.
- Variant of Tait-Bryan angles
- Yaw-pitch-roll rotation order (ZYX convention), rotating respectively around axes
Z, Y and X axes
- Intrinsic rotation (axes move with each rotation)
- Active rotation (the point is rotated, not the coordinate system)
- Direct coordinate system (right-hand rule for vector product)

Based on this definition, let u (roll angle), v (pitch angle) and w (yaw angle) define a given orientation, the equivalent quaternion is determined as follows:

$$
\begin{aligned}
        & q_0=\cos\left(\frac{u}{2}\right) \cos\left(\frac{v}{2}\right) \cos\left(\frac{w}{2}\right)+\sin\left(\frac{u}{2}\right) \sin\left(\frac{v}{2}\right) \sin\left(\frac{w}{2}\right) \\
        & q_1=\sin\left(\frac{u}{2}\right) \cos\left(\frac{v}{2}\right) \cos\left(\frac{w}{2}\right)-\cos\left(\frac{u}{2}\right) \sin\left(\frac{v}{2}\right) \sin\left(\frac{w}{2}\right) \\
        & q_2=\cos\left(\frac{u}{2}\right) \sin\left(\frac{v}{2}\right) \cos\left(\frac{w}{2}\right)+\sin\left(\frac{u}{2}\right) \cos\left(\frac{v}{2}\right) \sin\left(\frac{w}{2}\right) \\
        & q_3=\cos\left(\frac{u}{2}\right) \cos\left(\frac{v}{2}\right) \sin\left(\frac{w}{2}\right)-\sin\left(\frac{u}{2}\right) \sin\left(\frac{v}{2}\right) \cos\left(\frac{w}{2}\right)
\end{aligned}
$$

The `eulerAnglesToQuaternion()` function in the `quaternion.py` module performs this conversion.

### Quaternion to Euler angles
The conversion of a quaternion into Euler angles (using the definition of Euler angles given in the previous paragraph) is given by the following equations:

$$
\begin{aligned}
        & \text {roll}=u=\tan ^{-1}\left(\frac{2\left(q_0 q_1+q_2 q_3\right)}{q_0^2-q_1^2-q_2^2+q_3^2}\right)=\operatorname{atan} 2\left[2\left(q_0 q_1+q_2 q_3\right), q_0^2-q_1^2-q_2^2+q_3^2\right] \\
        & \text {pitch}=v=\sin ^{-1}\left(2\left(q_0 q_2-q_1 q_3\right)\right)=\operatorname{asin}\left[2\left(q_0 q_2-q_1 q_3\right)\right] \\
        & \text {yaw}=w=\tan ^{-1}\left(\frac{2\left(q_0 q_3+q_1 q_2\right)}{q_0^2+q_1^2-q_2^2-q_2^2}\right)=\operatorname{atan} 2\left[2\left(q_0 q_3+q_1 q_2\right), q_0^2+q_1^2-q_2^2-q_3^2\right]
\end{aligned}
$$

#### Gimbal lock
Euler angles are susceptible to Cardan blocking (singularity of representation). The preceding equations provide a general solution for determining Euler angles. In the special case where the pitch angle v is equal to +90° or -90°, it becomes impossible to calculate the other 2 angles (roll and yaw), as the function `atan2()` is not defined for the two null arguments. This is because, for a pitch angle of +90° or -90°, the roll and yaw axes are aligned. There is no single solution in this configuration: any orientation can be described using an infinite number of combinations of yaw and roll angles.

To manage the Cardan lock, we first determine the value of the pitch angle. Depending on whether it is +90° or -90°, we calculate the other 2 angles.


The `quaternionToEulerAngles()` function in the `quaternion.py` module performs this conversion.
`Of the rotation parameterizations presented, only Euler angles are susceptible to Cardan blocking, quaternions and rotation matrices are not.`












