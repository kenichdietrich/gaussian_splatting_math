Frobenius inner product

$$ \braket{A|B} = 
A_{ij}B_{ij} = 
\sum_{i}\sum_{j} A_{ij}B_{ij} \in \mathbb{R} 
$$

$$ A^T = A^T_{ij} = A_{ji} $$


$$ \frac{\partial(A\textbf{\textit{x}})}{\partial \textbf{\textit{x}}} = 
\frac{\partial (A_{ij}x_j)}{\partial x_k} =
A_{ij}\frac{\partial x_j}{\partial x_k} = A_{ij}\delta_{jk} =
A_{ik} = 
A
$$

$$ \frac{\partial(A B A^T)}{\partial B} =
\frac{\partial (A_{ik}B_{kr}A^T_{rj})}{\partial B_{nm}} =
A_{ik}\frac{\partial B_{kr}}{\partial B_{nm}}A_{jr} =
A_{ik}\delta_{kn}\delta_{rm}A_{jr} =
A_{in}A_{jm}
$$

$$ \frac{\partial (A B A^T)}{\partial A} = 
    \frac{\partial (A_{ik} B_{kr} A^T_{rj})}{\partial A_{nm}} = 
    A_{ik} B_{kr} \frac{\partial A_{jr}}{\partial A_{nm}} + \frac{\partial A_{ik}}{\partial A_{nm}} B_{kr} A_{jr} =
    A_{ik} B_{kr} \delta_{jn} \delta_{rm} + \delta_{in} \delta_{km} B_{kr} A_{jr} =
    A_{ik} B_{km} \delta_{jn} + \delta_{in} B_{mr} A_{jr}
$$

$$ \frac{\partial (AA^T)}{\partial A} = 
\frac{\partial (A_{ik} A^T_{kj})}{\partial A_{nm}} = 
A_{ik} \frac{\partial A_{jk}}{\partial A_{nm}} + \frac{\partial A_{ik}}{\partial A_{nm}} A_{jk} = 
A_{ik} \delta_{jn} \delta_{km} + \delta_{in} \delta_{km} A_{jk} = 
A_{im} \delta_{jn} + \delta_{in} A_{jm}
$$

$$ \frac{\partial (AB)}{\partial B} = 
\frac{\partial A_{ik} B_{kj}}{\partial B_{nm}} = 
A_{ik} \delta_{kn} \delta_{jm} = 
A_{in} \delta_{jm}
$$

$$ \frac{\partial (AB)}{\partial A} = 
\frac{\partial A_{ik} B_{kj}}{\partial A_{nm}} = 
\delta_{in} \delta_{km} B_{kj} = 
\delta_{in} B_{mj}
$$

$$
\frac{\partial (A \textbf{\textit{x}})}{\partial A} =
\frac{\partial (A_{ij} x_j)}{\partial A_{nm}} =
\frac{\partial A_{ij}}{\partial A_{nm}} x_j =
\delta_{in} \delta_{jm} x_j =
\delta_{in} x_m
$$

