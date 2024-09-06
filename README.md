# EOF
```
class EOF(dataset: tuple, n_components: int, field: str = "2D", **svd_args)
```
This module EOF provides EOFs and combined EOFs of either 1-dimensional or 2-dimensional meteorological field.

If the dataset only contains one array, it will give single variable EOFs. If the dataset contains more than one array, it will give combined EOFs.

## A simple introduction to EOFs
Considering a data matrix $X$ with n timesteps and m spacegrids

$$
X =
\begin{pmatrix}
x_{1,1} & x_{1,2} & ... & x_{1,m}\\
x_{2,1} & x_{2,2} & ... & x_{2,m}\\
. & . & & .\\
. & . & & .\\
x_{n,1} & x_{n,2} & ... & x_{n,m}
\end{pmatrix}
$$

We want to find a series of vectors, or EOFs, of matrix $X$ that maximize the temporal variance.

After a bunch of derivation and proof, we find that calculating EOFs is equivalent to solving an eigenvalue problem

$$
Le = \lambda e
$$

where $e$ is the eigenvector, $L$ is the covariance matrix of $X$ and is given by

$$
L = X^T X =
\begin{pmatrix}
\Sigma_{1}^{n}\ x_{n,1}x_{n,1} & \Sigma_{1}^{n}\ x_{n,1}x_{n,2} & ... & \Sigma_{1}^{n}\ x_{n,1}x_{n,m}\\
\Sigma_{1}^{n}\ x_{n,2}x_{n,1} & \Sigma_{1}^{n}\ x_{n,2}x_{n,2} & ... & \Sigma_{1}^{n}\ x_{n,2}x_{n,m}\\
. & . & & .\\
. & . & & .\\
\Sigma_{1}^{n}\ x_{n,m}x_{n,1} & \Sigma_{1}^{n}\ x_{n,m}x_{n,2} & ... & \Sigma_{1}^{n}\ x_{n,m}x_{n,m}\\
\end{pmatrix}
$$

The eigenvectors $e_m$ give the empirical orthogonal functions. The first eigenvetor $e_1$ is EOF1, the second eigenvector $e_2$ is EOF2 and so on.

More details in derivation can be seen in:
https://doi.org/10.1016/B978-0-12-800066-3.00006-1.

## Usage
For the single 1-dimensional variable EOF case:
```
import numpy as np
from EOF import EOF

ds     = ncfile.variable["var"][:]
ds_new = (ds - ds.mean()) / ds.std()

single_EOF = EOF((ds_new,), n_components=40, field = "1D")
single_EOF.get()

# EOF modes
single_EOF.EOF

# Principle components
single_EOF.PC

# Explainable ratio
single_EOF.explained
```

For the multi 2-dimensional variable combined EOF case:

```
import numpy as np
from EOF import EOF

ds1     = ncfile.variable["var1"][:]
ds1_new = (ds1 - ds1.mean()) / ds1.std()
ds2     = ncfile.variable["var2"][:]
ds2_new = (ds2 - ds2.mean()) / ds2.std()
ds3     = ncfile.variable["var3"][:]
ds3_new = (ds3 - ds3.mean()) / ds3.std()

combined_EOF = EOF((ds1_new, ds2_new, ds3_new,), n_components=40)
combined_EOF.get()

# EOF modes
combined_EOF.EOF

# Principle components
combined_EOF.PC

# Explainable ratio
combined_EOF.explained
```

## Notes
Thanks to Kai-Chi Tseng for the advising.

Kai-Chih's github: https://kuiper2000.github.io/

This package uses sklearn.decomposition.PCA to calculate EOF. Please make sure you've already had sklearn.decomposition.PCA in your environment before using.

sklearn.decomposition.PCA: https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html
