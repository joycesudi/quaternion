## Conversion between quaternions and other rotation parametrizations
This Python module provides conversion functions between quaternions and other rotation parametrizations.
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
Let $q_1 = \left\langle e_0, \underline{e}\right\rangle$ and q2 = ⟨u0, u⟩, two quaternions each representing a given rotation, the product defines the sequence of rotations q1 then q2.

## Converting from one representation of a solid's orientation to another






