# MDistil

-----------

This is the documentation to the implementation of Distillation [Peardon et al., 2009, 0905.2160] and stochastic LapH [Morningstar et al., 2011, 1104.3870] in grid.

## LapEvec

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Calculates the lowest $ n_\mathrm{vec} $ `Laplacian eigenvectors` $v$ and `eigenvalues` $\lambda$ by solving the Laplacian eigenvalue equation

$$\sum_{b,\vec{y}}-\nabla^2_{ab}(\vec{x},\vec{y};t) |v(\vec{y};t)\rangle_{kb} =  \lambda_k(t) |v(\vec{x};t)\rangle_{ka}$$

on each timeslice $ t $ with the 3-dimensional lattice `Laplacian` 

$$-\nabla^2_{ab}(\vec{x},\vec{y};t) = 6 \delta_{xy} - \sum_{j=1}^3 (\tilde{U}^{ab}_j(\vec{x},t) \delta_{x+\hat{j},y} + \tilde{U}^{ba}_j(\vec{y},t)^* \delta_{x-\hat{j},y} )$$

where $ \tilde{U} $ are Stout-smeared [Morningstar, Peardon: Phys.Rev. D69 (2004) 054501] gauge links using $ n_\mathrm{step} $ smearing steps with a weight of $ \rho $ (3D smearing, only on the spatial directions).

These eigenvectors define the hermitian smearing matrix

$$ S_{xy}(t) = \sum_{k=1}^{N_\mathrm{vec}} |v(\vec{x};t)\rangle_{ka} \langle v(\vec{y};t)|_{ka} $$

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `gauge field`         | `FermionField`             | $U_{\mu}$              |
| `StoutParameters`     | `{int,double}`             | steps, rho             |
| `ChebyshevParameters` | `{int,double,double}`      | PolyOrder, alpha, beta |
| `LanczosParameters`   | `{int,int,int,int,double,int}` | Nvec,Nk,Np,MaxIt,resid,IRLLog |

### Dependencies

- gauge field

### Products

Laplacian eigenvectors, named `eigenPack` (Note: The eigenvectors are 4d-eigenvectors, which are reinterpreted from the 3d-Lapalcian eigenvectors. The eigenvalues of the eigenpack are those of $t=0$.)

-----------

## DistilPar

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Sets the parameters used within distillation and returns them for other modules to use.

Exact distillation is used when the dilution interlacing parameters (TI,LI,SI) are set to ($N_t$,$N_{\mathrm{vec}}$,$N_s$). In this case, distillation sources have support only on timeslice $t_{\mathrm{src}}$ and inversions are only computed on this timeslice.

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `nvec`                | `int`             | $N_{\mathrm{vec}}$              |
| `nnoise`              | `int`             | $N_{\mathrm{noise}}$              |
| `tsrc`                | `int`             | $t_{\mathrm{src}}$   (only used for exact distillation)           |
| `TI`                  | `int`             | time-interlacing for dilution             |
| `LI`                  | `int`             | eigenvector-interlacing for dilution               |
| `SI`                  | `int`             | spin-interlacing for dilution              |

### Dependencies

### Products

Distillation Parameters, used as input in other MDistil modules.

-----------


## Noises

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Computes  $N_\mathrm{noise}$ noise vectors

$$  \rho^{[n]}_{k\alpha}(t)  \, , $$

which have the expectation value

$$ \langle \rho^{[n_1]}  \rho^{[n_2]} \rangle = \delta^{n_1, n_2} \, ,$$

needed for stochastic perambulator, seeded from a Unique Identifier (string). If exact distillation is chosen ($N_\mathrm{noise}=1$,$TI=N_t$,$SI=4$,$LI=N_\mathrm{vec}$, the noise vector object contains only values of 1.

If `NoiseFileName` is unspecified, the noises are not saved to disk.


### Parameters

| Parameter          | Type                         | Description                            |
|--------------------|------------------------------|----------------------------------------|
| `DistilParams` | `std::string`           | module name for `DistilPar` |
| `NoiseFileName` | `std::string`           | file name for the noises.  |

### Dependencies

- Distillation parameters `DistilParams`

### Products

- `noises` $\rho$


-----------

## Perambulator

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Computes the `(stochastic) perambulator` $\tau$ from the laplacian eigenevctors

$$ \tau_{k\alpha}^{[n,d]}(t') = \sum_{a,b,\beta,t,\vec{x}',\vec{x}} \langle v(\vec{x}';t')|_{ka} D^{-1}_{a\alpha,b\beta}(\vec{x}',t';\vec{x},t) |\varrho^{[n,d]} (\vec{x},t)\rangle_{b \beta} $$

defined via the quark source vectors

$$  |\varrho^{[n,d]} (\vec{x},t)\rangle_{a \alpha}  = \sum_{k,l,t',\beta} |v(\vec{x};t')\rangle_{ka} P_{k\alpha,l\beta}^{[d]}(t,t') \rho_{l \beta}^{[n]}(t') \, , $$

which are constructed from the Laplacian eigenevectors from the LapEvec module, $N_\mathrm{noise}$ noise vectors $  \rho^{[n]}_{k\alpha}(t)  \, , $ and $N_\mathrm{dil}$ dilution projectors

$$ P_{k\alpha,l\beta}^{[d]}(t,t') $$

which have the properties

$$ (P^{[d]})^2 = P^{[d]} \ , \ \sum_d P^{[d]} = 1 $$

The solver should be provided by another Hadrons module as an input.

The perambulators can in turn be used to define the quark sink vectors

$$ | \varphi^{[n,d]} (\vec{x},t)\rangle_{a \alpha}  = \sum_{k} |v(\vec{x};t)\rangle_{ka} \tau_{k\alpha}^{[n,d]}(t)  $$

using which a smeared-to-smeared quark propagator can be expressed via

$$ \langle \tilde{q}_{a \alpha} (\vec{x}',t') \bar{\tilde{q}}_{b \beta} (\vec{x},t) \rangle = \bigg\langle \sum_d \langle \varphi^{[n,d]}(\vec{x},t)|_{a \alpha}  \varrho^{[n,d]} (\vec{x},t)\rangle_{b \beta} \bigg\rangle $$

The code in grid allows for interlaced dilution in spin (interlace-$I_\alpha$), time (interlace-$I_t$) and laplacian eigenmodes (interlace-$I_k$) - the dilution index $d$ is therefore implemented as a compound index, $d = (d_\alpha,d_t,d_k)$. 

For $I_t=N_t$, $I_\alpha=N_s$, $I_k=N_\mathrm{vec}$ we reproduce the exact `perambulator` $\tau$ 

$$ \tau_{k\alpha, l\beta}(t',t) = \sum_{a,b,\vec{x},\vec{x}'} \langle v(\vec{x}';t')|_{ka} D^{-1}_{a\alpha,b\beta}(\vec{x}',t';\vec{x},t) |v(\vec{x};t)\rangle_{lb} \, . $$

In this case of exact distillation, the noise vectors are automatically replaced by vectors of ones, but $N_\mathrm{noise}$ has to be $=1$ ior the code will throw an error.

Additionally, this module gives access to unsmeared quark fields [Mastropas, Richards, 2014, 1403.5575, eq. (20)]

$$ | \phi^{[n,d]}(\vec{x}',t') \rangle_{a\alpha} = \sum_{b,\beta,t,\vec{x}} e^{-i\vec{p} \cdot \vec{x}} D^{-1}_{a\alpha,b\beta}(\vec{x}',t';\vec{x},t) |\varrho^{[n,d]} (\vec{x},t)\rangle_{b \beta} $$

Unlike in the case of the perambulators, the LapH smearing can not be used to project them into the LapH subspace and consequently these objects are much larger and more expensive to keep on disk.

### Parameters

| Parameter          | Type                         | Description                            |
|--------------------|------------------------------|----------------------------------------|
| `lapevec`          | `std::string`                | $v_k$                                  |
| `solver`           | `std::string`                | module name for solver                 |
| `noise`            | `std::string`                | module name for noises                 |
| `PerambFileName`           | `std::string`                | filestem for the perambulators saved to disk (not saved if empty)                 |
| `UnsmearedSinkFileName`           | `std::string`                | filestem for the unsmeared sinks saved to disk (not saved if empty)                 |
| `DistilParams` | `std::string`           | module name for `DistilPar` |

### Dependencies

- Laplacian eigenPack `lapevec` 

- `noises` $\rho$

- solver

- Distillation parameters `DistilParams`


### Products

- `perambulator` $\tau$

- `unsmeared_sink` $\phi$

-----------

## PerambFromSolve

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Computes the `(stochastic) perambulator` $\tau$ an already computed solve (unsmeared sink) $\phi$ (output from an earlier `MDistil::Perambulator` module).

As an option, theparameters $N_\mathrm{vec}$ and LI can be re-specified to be smaller than the ones used to create $\phi$. This can be useful if one wants to study the signals of correlation functions built from distillation smearings built of a different set of these parameters, without re-computing the inversions. If this is not wanted, `nvec_reduced` and `LI_reduced` should be set to their maximum values, $N_\mathrm{vec}$ and LI. In any case, `DistilParams` need to be the ones used to compute the `solve`.

### Parameters

| Parameter          | Type                         | Description                            |
|--------------------|------------------------------|----------------------------------------|
| `lapevec`          | `std::string`                    | $v_k$                                  |
| `PerambFileName`           | `std::string`                | filestem for the perambulators saved to disk (not saved if empty)                 |
| `solve`           | `std::string`                | unsmeared sink / solve                |
| `nvec_reduced`           | `int`                | $N_\mathrm{vec}$   of the output perambulator       |
| `LI_reduced`           | `int`                | LI   of the output perambulator       |
| `DistilParams` | `std::string`           | module name for `DistilPar`, paramaters of the input `solve` |

### Dependencies

- Laplacian eigenPack `eigenPack` 

- `solve` $\phi$

- Distillation parameters `DistilParams`


### Products

- `perambulator` $\tau$


-----------


## DistilVectors

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Calculates the quark source vectors

$$  |\varrho^{[n,d]} (\vec{x},t)\rangle_{a \alpha}  = \sum_{k,l,t',\beta} |v(\vec{x};t)\rangle_{ka} P_{k\alpha,l\beta}^{[d]}(t,t') \rho_{l \beta}^{[n]}(t') $$

from the eigenvectors and noises and the quark sink vectors

$$ |\varphi^{[n,d]} (\vec{x},t)\rangle_{a \alpha}  = \sum_{k} |v(\vec{x};t)\rangle_{ka} \tau_{k\alpha}^{[n,d]}(t)  $$

from the eigenvectors and perambulators.

### Parameters

| Parameter          | Type                       | Description                            |
|--------------------|----------------------------|----------------------------------------|
| `noise`            | `std::string`     | module name for noises                                 |
| `perambulator`     | `std::string` | module name for perambulators                                 |
| `lapevec`     | `std::string`                | module name for eigenvectors                                  |
| `rho`            | `std::string`     | $\varrho$ filename (don't compute if empty)                                 |
| `phi`            | `std::string`     | $\varphi$ filename (don't compute if empty)                                 |
| `DistilParams` | `std::string`           | module name for `DistilPar` |


### Dependencies

- Laplacian eigenPack

- `perambulator` $\tau$

- `noises` $\rho$

- Distillation parameters `DistilParams`


### Products

- `source vectors` $\varrho$

- `sink vectors` $\varphi$

-----------

## BContraction

### NOTE

In development, not yet part of Hadrons

### Description

Calculates the baryon fields

$$ B^{[n_1,d_1;n_2,d_2;n_3,d_3]}_\alpha(v^1,v^2,v^3;t,\vec{p}) = 
\sum_{\vec{x},a,b,c,\alpha',\beta,\gamma} e^{-i \vec{p} \cdot \vec{x}} \epsilon_{abc} (\Gamma P_\pm)_{\alpha \alpha'} v^{1;[n_1,d_1]}_{ a \alpha'}(\vec{x},t) \Big( v^{2;[n_2,d_2]}_{ b \beta}(\vec{x},t) (\Gamma P_\pm)_{\beta \gamma} v^{3;[n_3,d_3]}_{ c \gamma}(\vec{x},t) \Big)$$

where the vectors $v_1,v_2,v_3$ can be either a LapH source ($\varrho$) or sink vector ($\varphi$) or an unsmeared sink ($\phi$).

In the approach used in this module, a diquark 

$$ d^{[n_2,d_2;n_3,d_3]}(v_2,v_3;t,\vec{x}) = 
\sum_{a,b,c,\beta,\gamma} \epsilon_{abc} \Big( v^{2; [n_2,d_2]}_{ b \beta}(\vec{x},t) (\Gamma P_\pm)_{\beta \gamma} v^{3; [n_3,d_3]}_{ c \gamma}(\vec{x},t) \Big) $$

is computed first and then contracted with the third quark which gives the baryon field its spin component:

$$ B^{[n_1,d_1;n_2,d_2;n_3,d_3]}_\alpha(v^1,v^2,v^3;t,\vec{p}) = 
\sum_{\vec{x},\alpha'} e^{-i \vec{p} \cdot \vec{x}} (\Gamma P_\pm)_{\alpha \alpha'} v^{1; [n_1,d_1]}_{ a \alpha'}(\vec{x},t) d^{[n_2,d_2;n_3,d_3]}(v^2,v^3;t,\vec{x}) $$

The final baryon field has consequently one free spin component which is contracted along with the other open indices in the final contraction into a correlation function.

The matrices $\Gamma_{\alpha \alpha'}$ and $\Gamma_{\beta \gamma}$ have to be chosen according to the spin $J$ and irrep and polarisation of the baryonic interpolator. $P_\pm$ is the parity operator.

### Parameters

| Parameter          | Type                       | Description                            |
|--------------------|----------------------------|----------------------------------------|
| `one`              | `std::string`              | $v^1_{a \alpha}$ - this quark will give the spin to the baryon field!                                  |
| `two`              | `std::string`              | $v^2_{b \beta}$                        |
| `three`            | `std::string`              | $v^3_{c \gamma}$                       |
| `output`           | `std::string`              | output file name                       |
| `parity`           | `int`                      | Parity:  $1 \rightarrow +$,  $(-1) \rightarrow -$ |
| `mom`              | `std::vector<std::string>` |   list of momenta                       |

### Dependencies

- `source vectors` $\varrho$

- `sink vectors` $\varphi$

- `unsmeared sinks` $\phi$



### Products

- `baryon field` $B_\alpha$


-----------

## Baryon2pt

### NOTE

In development, not yet part of Hadrons

### Description

Contracts two baryon fields into a 2-point function

$$ C_2(t,t_0,\vec{p}) = \sum_{\alpha,n,d}  B^{[n_1,d_1;n_2,d_2;n_3,d_3]}_\alpha(v^{1,A},v^{2,B},v^{3,C};t,\vec{p}) $$
$$ \times  \Big( \delta_{ABC}^{\bar{A}\bar{B}\bar{C}} B^{[n_1,d_1;n_2,d_2;n_3,d_3]}_\alpha(w^{1,\bar{A}},w^{2,\bar{B}},w^{3,\bar{C}};t_0,\vec{p}) $$
$$  + \delta_{ABC}^{\bar{B}\bar{C}\bar{A}} B^{[n_2,d_2;n_3,d_3;n_1,d_1]}_\alpha(w^{2,\bar{B}},w^{3,\bar{C}},w^{1,\bar{A}};t_0,\vec{p}) $$
$$  + \delta_{ABC}^{\bar{C}\bar{A}\bar{B}} B^{[n_3,d_3;n_1,d_1;n_2,d_2]}_\alpha(w^{3,\bar{C}},w^{1,\bar{A}},w^{2,\bar{B}};t_0,\vec{p}) $$
$$  + \delta_{ABC}^{\bar{A}\bar{C}\bar{B}} B^{[n_1,d_1;n_3,d_3;n_2,d_2]}_\alpha(w^{1,\bar{A}},w^{3,\bar{C}},w^{2,\bar{B}};t_0,\vec{p}) $$
$$  + \delta_{ABC}^{\bar{C}\bar{B}\bar{A}} B^{[n_3,d_3;n_2,d_2;n_1,d_1]}_\alpha(w^{3,\bar{C}},w^{2,\bar{B}},w^{1,\bar{A}};t_0,\vec{p}) $$
$$  + \delta_{ABC}^{\bar{B}\bar{A}\bar{C}} B^{[n_2,d_2;n_1,d_1;n_3,d_3]}_\alpha(w^{2,\bar{B}},w^{1,\bar{A}},w^{3,\bar{C}};t_0,\vec{p}) \Big)$$

where the vectors $v_1,v_2,v_3$ and $w_1,w_2,w_3$ can be either a LapH source ($\varrho$) or sink vector ($\varphi$) or an unsmeared sink ($\phi$). The quark flavours $A,B,C$ and $\bar{A},\bar{B},\bar{C}$ are an input parameter to this modules and the deltas are evaluated automatically.

### Parameters

| Parameter          | Type                       | Description                            |
|--------------------|----------------------------|----------------------------------------|
| `inputL`           | `std::string`              | file for Baryon Field left                                  |
| `inputR`           | `std::string`              | file for Baryon Field right                                  |
| `output`           | `std::string`              | output file name                       |
| `quarksL`           | `std::string`                      | string of exactly 3 characters with the quark content of the left field |
| `quarksR`           | `std::string`                      | string of exactly 3 characters with the quark content of the right field |

### Dependencies

- `source vectors` $\varrho$

- `sink vectors` $\varphi$

- `unsmeared sinks` $\phi$



### Products

- `baryon field` $B_\alpha$


-----------




