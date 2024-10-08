## Gaussian Projection - Forward

### Frustum culling

Not all gaussians in scene contribute to image renderization. The frustum culling consists of filtering the gaussians which lie between the $z$ limits of the frustum to reduce computation overhead.

Thanks to the projection matrix $P$, it is possible to project the gaussian centers to the NDC space, i.e., the warped space where the camera frustum becomes a normalized cube between coordinates $[-1, 1]$.

$$ \textbf{\textit{x}}_f = P \textbf{\textit{x}} $$

The positions in frustum space $\textbf{\textit{x}}_f = (x_f, y_f, z_f, z_c)$ are not normalized, the last component of the 4-vector is just the $z$ coordinate in camera space. Dividing by this coordinate is needed to get the NDC coordinates

$$ \textbf{\textit{x}}_{ndc} = \textbf{\textit{x}}_f / z_c $$

Any gaussian with depth greater than $z_{far}$ or lower than $z_{near}$ limits of the frustum will have $z_{ndc}$ coordinate greater than $+1$ or lower than $-1$, so the culling is as easy as querying the NDC $z$ coordinate. Then, the subset $G$ of preserved gaussians will be

$$ G = \{g \ \forall \ z_{ndc} \in [-1, 1] \} $$

### 3D covariance construction

The covariance matrix $\Sigma$ of the gaussians are decomposed in a $3\times3$ rotation $R$ and a diagonal scale matrix $S$ as

$$ M = RS $$
$$ \Sigma = M M^T = R S S^T R^T $$

The scale matrix is constructed from scale vectors $s = (s_x, s_y, s_z)$ as

$$ S = 
\begin{pmatrix}
    s_x & 0 & 0 \\
    0 & s_y & 0 \\
    0 & 0 & s_z
\end{pmatrix} 
$$

The rotation matrix $R$, in turn, is built from $\textbf{\textit{q}} = (q_r, q_x, q_y, q_z)$ quaternions as

$$
    R = 
    \begin{pmatrix}
        1 - 2(q_y^2 + q_z^2) & 2(q_x q_y - q_rq_z) & 2(q_xq_z+q_rq_y) \\
        2(q_xq_y + q_rq_z) & 1 - 2(q_x^2 + q_z^2) & 2(q_yq_z-q_rq_x) \\
        2(q_xq_z - q_rq_y) & 2(q_yq_z+q_rq_x) & 1-2(q_x^2+q_y^2)
    \end{pmatrix} 
$$

### Covariance projection

The world-to-camera matrix $W$ allows to transform world points to camera space coordinates

$$ \textbf{\textit{x}}_c = W \textbf{\textit{x}} $$

The $z$ component will define the depth $\delta = z_c$ in the rasterization step. The EWA approximation projects the 3D covariance to a planar image using the relation

$$
    \Sigma_p = T \Sigma T^T = J W \Sigma W^T J^T
$$

where $T = J W_{3 \times 3} = J W$ and $J$ is the jacobian of the affine approximation of the projective transformation, given by

$$
    J = 
    \begin{pmatrix}
        f_x / z_c & 0 & -f_x x_c / z_c^2 \\
        0 & f_y / z_c & -f_y y_c / z_c^2 \\
        0 & 0 & 0
    \end{pmatrix}
$$

The $\Sigma_i$ $2\times2$ upper-left submatrix (i.e. removing 3rd row and column) provides the 2D covariance matrix $\sigma$ of the gaussians projected on the image plane. Moreover, the center of the gaussians can be obtained from NDC coordinates as

```math
    \textbf{\textit{x}}_p = 
    \left(\frac{(x_{ndc} + 1)w - 1}{2}, \frac{(y_{ndc} + 1)h - 1}{2} \right) 
```

### Conic

The $\sigma$ covariance will be necessary to evaluate gaussians in the rasterization step. However, what will actually be needed is the inverse matrix, better known as the conic $c$

$$
    C = 
    \sigma^{-1} = 
    \frac{1}{|\sigma|}\textrm{adj}(\sigma) = 
    \frac{1}{\sigma_{xx}\sigma_{yy} - \sigma_{xy}^2} 
        \begin{pmatrix}
            \sigma_{yy} & -\sigma_{xy} \\
            -\sigma_{xy} & \sigma_{xx} \\
        \end{pmatrix} 
$$

It is even possible to compute a characteristic radius $r$ for the gaussians

$$ 
    r = 
    3\sqrt{var}, \hspace{0.5cm} var = 
    \overline{var} + \sqrt{\max(0.1, \overline{var}^2-|\sigma|)}, \hspace{0.5cm} \overline{var} = 
    \frac{\sigma_{xx} + \sigma_{yy}}{2} 
$$