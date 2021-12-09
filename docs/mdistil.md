# MDistil

-----------
## Intoduction and Notation

This is the documentation to the implementation of Distillation [Peardon et al., 2009, 0905.2160] and stochastic LapH [Morningstar et al., 2011, 1104.3870] in grid. The smearing kernel of distillation is

$$ \mathcal{S}_{}(\pmb x,\pmb y, t)  = \sum_{l=1}^{N_{vec}} v_l (\pmb x,t)  v^{\dagger}_l (\pmb y,t), $$

where $v^{(i)}$ are the low-lying eigenvectors of the 3D Laplace operator, as in

$$ - \nabla^2 v_l(t) = \lambda_l(t) v_l(t), \qquad l=1, 2, \ldots, N_{vec}$$

with eigenvalues $\lambda_l$. Here, the color index was omitted in $v$. The 3D Laplace operator is

$$ -\nabla^2_{ab}(\pmb{x},\pmb{y},t) = 6 \delta_{xy} - \sum_{j=1}^3 (U^{ab}_j(\pmb{x},t) \delta_{x+\hat{j},y} + U^{ba}_j(\pmb{y},t)^* \delta_{x-\hat{j},y} ) $$

where $U$ is a gauge fields.

For generality purposes, the high-level language is that of stochastic-diluted distillation. We then define time-Laplacian-spin noises with the properties 

$$ \langle \eta_{\alpha l}^{r}(t)) \rangle_{\eta} = 0 \qquad , \qquad \langle \eta_{\alpha k}^{r}(t) \eta_{\beta l}^{s}(t') \rangle_{\eta} = \delta_{rs} \delta_{kl} \delta_{\alpha \beta} \delta(t-t') $$

where $r,s$ are noise indices (number of hits of noise vector). Furthermore, given an arbitrary dilution projector $P^{I_T I_L I_S}$ on the same degrees of freedom, the diluted noise vector is denoted as

$$ \eta^{r I} = P^I \eta^r .$$

where $I=(I_T,I_L,I_S)$ is a compound dilution index. Dilution indices will be denoted as capital letters. 

We can further introduce intermediate objects to facilitate computations and simplify contraction expressions. Firstly, we define the source vectors

$$ \varrho^{rI}_{a \alpha}(\pmb x,t) = \sum_l v_{a l}(\pmb x, t) \eta^{rI}_{\alpha l}(t) .$$

The inversion information is contained in the perambulator

$$ \tau^{r I}_{\alpha l}(t) = \sum_{\pmb x', a'} v_{a' l} (\pmb x' ,t)^* \sum_{\beta',t'} \sum_{\pmb y', b'} D^{-1}_{\alpha \beta',a' b'}(\pmb x',t ; \pmb y' , t') \varrho^{rI}_{b' \beta'}(\pmb y',t'). $$
Another important intermediate object is the solve (or sink) vector
$$ \varphi^{r I}_{a \alpha}(\pmb x,t) = \sum_{l=1}^{N_{vec}} v_{al} (\pmb x,t) \tau^{r I}_{\alpha l}(t). $$

The next objects needed in the distillation workflow are the meson fields. They are constructed from source and solve vectors in position space as

$$ M_{\Gamma}^{IJ}(x) = \sum_{a,\alpha,\beta} \left[ V_{a\alpha}^I(x)^{\dagger} \Gamma_{\alpha\beta} W_{a\beta}^J(x) \right], $$

with $V,W \in \lbrace \varphi,\varrho \rbrace$, the aforementioned distillation solves and sources. $I,J$ are compound dilution indices that uniquely identify a time-Laplacian-spin dilution component, say $(I_T,I_L,I_S)$. Also, $ N_D = N_{D_T} N_{D_L} N_{D_S}$, noting that in general $N^{V}_D \neq N^{W}_D$.

## LapEvec

<!-- needs further explanation of chebyshev and lanczos parameters -->

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Calculates the low-lying $ N_{vec} $ `Laplacian eigenvectors` $v$ and `eigenvalues` $\lambda$ by solving the 3D Laplacian eigenvalue equation on each time slice $ t $.

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `gauge`               | `GaugeField`               | $U_{\mu}$              |
| `chebyshevParameters` | `{int,double,double}`      | polyOrder, alpha, beta |
| `lanczosParameters`   | `{int,int,int,int,double,int}` | nVec,nK,nP,maxIt,resid,irlLog |
| `fileName`            | `std::string`              | if non-empty, saves $v$ and $\lambda$ to that location on disk |

### Dependencies

- gauge field

### Products

Laplacian eigenvectors `LapPack`. Note that the eigenvectors are 4D objects, which are reinterpreted in each time slice as 3D Laplacian eigenvectors.

-----------

## Perambulator

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

This module has an enum variable `perambMode` that switches between $3$ different behaviors:

#### pMode::perambOnly (default behavior)

Computes the perambulator $\tau^{r I}_{\alpha l}(t)$ from Laplacian eigenvectors, distillation noise and a solver. The dependency `distilNoise` dictates the structure on the noise and dilution indices ($r,I$). For example, passing a [`InterlacedDistillation<FImpl>`](localhost:3000/#/mnoise?id=interlaceddistillation) noise to `distilNoise` will specify the perambulator to be evaluated stochastically and with the interlaced dilution.

#### pMode::outputSolve

Same as above, but additionally computes the full solves [Mastropas, Richards, 2014, 1403.5575, eq. (20)]

$$ \phi^{rI}_{a\alpha}(\pmb x,t)  = \sum_{b,\beta,\pmb x', t'} D^{-1}_{a\alpha,b\beta}(\pmb x,t;\pmb x', t') \varrho^{rI}_{b \beta} (\pmb x',t') $$

Unlike the perambulators, the full solves are unsmeared at the sink and so not projected into the distillation basis. This makes them lattice-sized objects and therefore much larger. The `outputSolve` mode only hands in the `solve` to the environment and does not write it to disk.

#### pMode::saveSolve

Same as above, but saves full solve to disk instead of handling it to the environment.

#### pMode::inputSolve

In this mode the perambulators are reconstructed from a `fullSolve` (read from `Hadrons` environment) by smearing the sink with a potentially different number of Laplacian eigenvectors, instead of using a solver. The input `nVec` specifies the number Laplacian eigenvectors to smear the sink, which of course needs to be less or equal the original number present at the source.


### Parameters

| Parameter             | Type              | Description                                                                                           |
|-----------------------|-------------------|-------------------------------------------------------------------------------------------------------|
| `lapEigenPack`        | `std::string`     | Laplacian eigenvectors/eigenvalues (`LapPack`)                                                        |
| `solver`              | `std::string`     | solver implementation                                                                                 |
| `distilNoise`         | `std::string`     | `DistillationNoise<FImpl>` implementation (noise policy)                                                                   |
| `timeSources`         | `std::string`     | desired list of time sources to be inverted over; uses all if empty                                           |
| `perambFileName`      | `std::string`     | file name for the perambulators saved to disk; not saved if empty                                      |
| `perambMode`          | `pMode`           | mode switch between `perambOnly: 0`, `inputSolve: 1`, `outputSolve: 2`, `saveSolve: 3` (`perambOnly` if empty)        |
| `fullSolveFileName`   | `std::string`     | file name for full solve saved to disk; not saved if empty (`saveSolve` mode only)  |
| `fullSolve`           | `std::string`     | full solve read from environment (`inputSolve` mode only)                                 |
| `nVec`                | `std::string`     | $N_{vec}$ to be used from `fullSolve`(`inputSolve` mode only)                                           				|

### Dependencies

- Laplacian eigenvectors/eigenvalues (`LapPack`) 

- `DistillationNoise<FImpl>` implementation (e.g., [`InterlacedDistillation<FImpl>`](localhost:3000/#/mdistil?id=interlaceddistillation))

- solver

- full solve (`inputSolve` mode only)

### Products

- perambulator $\tau^{r I}_{\alpha l}(t)$

- full solve (`outputSolve` mode only)

### File specification
#### Perambulator
Folder tree in the format `perambFileName/iDT_{t_idx}.{traj}.h5` is created.

##### Data description
Each file contains inversion information corresponding to a single time source `t_idx`. The data section of the file is a single HDF5 Dataset `/Perambulator` of complex (implementation dependent). The Dataspace has rank $6$ with sizes $( N_t, N_{vec}, N_{D_L}, N_{noise}, N_{D_S} , N_{spin} )$, where $N_{spin}$ is the number of spin degrees of freedom.

##### Metadata description
| Parameter                     | HDF5 Type     | Description            |
|-------------------------------|---------------|------------------------|
|`/GridDimensions` 				| H5T_STD_U64LE | underlying `Grid` object dimensions	|
|`/TensorDimensions` 			| H5T_STD_U64LE | underlying `NamedTensor` dimensions 	|
|`/IndexNames/IndexNames_{idx}` | H5T_STRING    | names of `NamedTensor` indices `{ nT, nVec, nDL, nNoise, nDS }` |
|`/Metadata/Version` 			| H5T_STRING    | attribute identifying code version  |
|`/Metadata/TimeDilutionIndex`  | H5T_STD_I32LE | attribute identifying time source  |
|`/Metadata/NoiseHash_{idx}` 	| H5T_STD_I32LE | attribute containing the hash corresponding to the noise hit `idx`; `0` if exact distillation  |

-----------

## DistilMesonFieldRelative

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Computes the distillation meson field (for a certain momentum projection and gamma matrix)

$$ \tilde M_{\Gamma \mathbf p}^{IJ}(t) = \int d^3\mathbf x \ e^{-i\mathbf p . \mathbf x} M_{\Gamma}^{IJ}(\mathbf x, t),  \quad I=1,\ldots,N^{left}_D , \quad J=1,\ldots,N^{right}_D  $$

from Laplacian eigenvectors, perambulators and noise objects.

A meson field is a sparse matrix on the time dilution indices. This module abstracts the sparseness by not saving most of predictable zeros, which is specified below. The momentum-space meson field is split in 2D *time-dilution blocks*

$$ \left[ \tilde{\mathcal{M}}^{I_T J_T}(t) \right]_{I_{LS}J_{LS}} \equiv \tilde M^{(I_T,I_L,I_S)(J_T,J_L,J_S)}(t), $$

where $I_{LS} = I_S + N_{D_S} I_L$ is a compound Laplacian-spin index (same for $J$). Each 2D block $ \tilde{\mathcal{M}}^{I_T J_T}(t) $ has dimensions along the indices $I_{LS}$ and $J_{LS}$. Their ranges are

$$ I_{LS} = 0,\ldots, N^{left}_{D_L} N^{left}_{D_S}-1, $$
$$ J_{LS} = 0,\ldots, N^{right}_{D_L} N^{right}_{D_S}-1, $$

The parameters $\mathbf p, \Gamma$ are fixed on each meson field.

### Parameters

| Parameter             | Type                          | Description            |
|-----------------------|-------------------------------|------------------------|
| `outPath`             | `std::string`                 | path of output folder tree          |
| `lapEigenPack`        | `std::string`                 | 3D Laplacian eigenvectors |
| `gamma`               | `std::string`                 | list of gamma matrices |
| `momenta`             | `std::vector<std::string>`    | list of 3D momenta |
| `leftTimeSources`     | `std::string`                 | desired list of time-dilution indices chosen to be used on left, e.g. `"0 2 4 6"` (if blank, uses all available)|
| `rightTimeSources`    | `std::string`                 | same as above for right side |
| `leftNoise`           | `std::string`                 | left `DistillationNoise<FImpl>` implementation (always needed) |
| `rightNoise`          | `std::string`                 | same as above for left side |
| `noisePairs`          | `std::vector<std::string>`    | desired list of "left right" noise index pairs available in the correspondent `DistillationNoise<FImpl>` input, e.g. `{"0 1" , "1 0"}`; unused in exact distillation |
| `leftPeramb`          | `std::string`                 | `Perambulator` used to compute the `phi` vector on the left ; if empty, a `rho` vector is assumed on the left |
| `rightPeramb`         | `std::string`                 | same as above for right side |
| `leftVectorStem`         | `std::string`              | full solve stem to be read from disk and used as the `phi` field instead of computing it from the `leftPeramb`; if empty, `phi` is computed from `leftPeramb`|
| `rightVectorStem`         | `std::string`             | same as above for right-hand side |
| `cacheSize`           | `std::string`                 | size of the cache blocks (along spin-Laplacian axes) passed to meson field kernel (`<= blockSize`)|
| `blockSize`           | `std::string`                 | size of IO blocks that are obtained from cache blocks by copy ($\le N_{D_L} N_{D_S}$)|
| `deltaT`              | `std::string`     			| $\Delta t$ to be used (see below), e.g., `"0 1 2 3"`; if blank, uses all possible $\Delta t$ (full meson field) 			|
| `relativeSide`             | `std::string`     		| `left` or `right` (see below); if blank, assumes `left` if there is at least one `rho` field and `right` otherwise 	|

### Dependencies

- Laplacian eigenvectors/eigenvalues (`LapPack`)
- `DistillationNoise<FImpl>` implementation (e.g., [`InterlacedDistillation<FImpl>`](localhost:3000/#/mdistil?id=interlaceddistillation))
- perambulator $\tau^{r I}_{\alpha l}(t)$

### Products

Distillation meson field (saved to disk).

### Relative Side and $\Delta t$ parameters

Due to performance reasons, the blocks $ \tilde{\mathcal{M}}^{I J}(t) $ are computed according to 

$$ \tilde{\mathcal{M}}^{I J}(t) \ \chi_t^{S + \Delta t} ,$$

where $S=I$ if `relativeSide == left` and $J$ otherwise. The characteristic function $ \chi_t^{S} $ is defined as usual to describe the time-dilution scheme ($1$ if $t$ is contained in the dilution partition $S$ and $0$ otherwise). According to the equation above, the vanishing blocks are not computed nor saved to disk. The input parameter `deltaT` specifies the values of $ \Delta t $. From the sparsity of the $\varrho$ vector, when the relative side is a `rho`, $\Delta t$ is automatically $ 0 $.

### File specification
A folder tree in the format `outPath/{mesonFieldType}.{traj}/{gamma}_p{momentum}_n{noisepair}.h5` is created. In case `ExactDistillation<FImpl>` is passed as noise policy, the piece `_n{noisepair}` is not present.

The string `{mesonFieldType}` is one of the following combinations: `phi-phi`, `phi-rho`, `rho-phi`, `rho-rho`. A `phi` vector will be present whenever a `Perambulator` is passed as input on the correspondent side. The `rho` field will be present otherwise.

#### Data description
The data section of the file is organized as groups at the level of the $t$ index and 2D datasets at the level of the $ (I_T,J_T) $ pairs: each dataset named `{t}/{I_T}-{J_T}` contains the correspondent 2D array $ \tilde{\mathcal{M}}^{I_T J_T}(t) $.

- `/DistilMesonField/{t}/{I_T}-{J_T}` : Dataset of complex (implementation dependent) type containing a time-dilution block $ \tilde{\mathcal{M}}^{I_T J_T}(t) $. The Dataspace has rank $2$ with sizes $( N^{left}_{D_L} N^{left}_{D_S}, N^{right}_{D_L} N^{right}_{D_S} )$. These datasets are chunked according to `blockSize` input.


#### Metadata description

| Parameter             | HDF5 Type                          | Description            |
|-----------------------|-------------------------------|------------------------|
|`/Metadata/Nt` 			        			| H5T_STD_U32LE | attribute identifying original time extension  |
|`/Metadata/Nvec` 			    				| H5T_STD_U32LE | attribute identifying total number of 3D-Laplacian eigenvectors used for smearing   |
|`/Metadata/Operator` 							| H5T_STRING | attribute identifying gamma matrix |
|`/Metadata/Momentum` 							| H5T_STRING | attribute identifying 3d momentum  |
|`/Metadata/MesonFieldType` 	    			| H5T_STRING | attribute identifying combination of distillation vectors  |
|`/Metadata/RelativeSide` 	    				| H5T_STRING | indicates which time-dilution index (left or right) was pinned to certain time slices on the computation  |
|`/Metadata/TimeDilutionPairs` 					| H5T_STD_U32LE | list of time-dilution blocks saved in the data section |
|`/Metadata/NoisePair`		    				| H5T_STRING | attribute containing noise pair used |
|`/Metadata/NoiseHashLeft`						| H5T_STRING | attribute containing the hash corresponding to the noise hit used; `0` if exact distillation |
|`/Metadata/NoiseHashRight`						| same as above for right noise |
|`/Metadata/DilutionSchemes/TimeDilutionLeft`	| H5T_STD_U32LE | attribute identifying the complete time-dilution scheme of the left vector as a 2d array  |
|`/Metadata/DilutionSchemes/TimeDilutionRight`	| same as above for right vector    |
|`/Metadata/DilutionSchemes/SpinDilutionLeft`	| analogue to above for spin dilution and left vector   |
|`/Metadata/DilutionSchemes/SpinDilutionRight`	| same as above for right vector    |
|`/Metadata/DilutionSchemes/LapDilutionLeft`	| analogue to above for laplacian dilution and left vector  |
|`/Metadata/DilutionSchemes/LapDilutionRight`	| same as above for right vector    |

-----------

## DistilMesonFieldFixed

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

In principle, this module performs the same as `DistilMesonFieldRelative`, but with a different way of inputing which time-dilution blocks and time slices will be computed and in which order. This version is more suitable for computing whole meson fields or whole diagonal time-dilution blocks.

### Parameters

| Parameter             | Type                          | Description            |
|-----------------------|-------------------------------|------------------------|
| `outPath`             | `std::string`                 | same as in `DistilMesonFieldRelative` |
| `lapEigenPack`        | `std::string`                 | same as above |
| `gamma`               | `std::string`                 | same as above |
| `momenta`             | `std::vector<std::string>`    | same as above |
| `leftTimeSources`     | `std::string`                 | same as above |
| `rightTimeSources`    | `std::string`                 | same as above |
| `leftNoise`           | `std::string`                 | same as above |
| `rightNoise`          | `std::string`                 | same as above |
| `noisePairs`          | `std::vector<std::string>`    | same as above |
| `leftPeramb`          | `std::string`                 | same as above |
| `rightPeramb`         | `std::string`                 | same as above |
| `leftVectorStem`      | `std::string`                 | same as above |
| `rightVectorStem`     | `std::string`                 | same as above |
| `blockSize`           | `std::string`                 | same as above |
| `cacheSize`           | `std::string`                 | same as above |
| `onlyDiagonal`        | `std::string`     			| computes and saves only the diagonal time-dilution blocks (`"true"` or `"false"`, set to `"false"` if empty) |
| `deltaT`              | `std::string`     			| shift to the right-hand dilution index when computing only the diagonal of a `phi-phi` field |

### Dependencies

- Laplacian eigenvectors/eigenvalues (`LapPack`)
- `DistillationNoise<FImpl>` implementation (e.g., [`InterlacedDistillation<FImpl>`](localhost:3000/#/mdistil?id=interlaceddistillation))
- perambulator $\tau^{r I}_{\alpha l}(t)$

### Products

Distillation meson field (saved to disk).

### Diagonal and $\Delta T$ parameters

Following the notation stablished on `DistilMesonFieldRelative`, the `onlyDiagonal` mode computes $ \tilde{\mathcal{M}}^{I_T J_T}(t) $ for $I_T=J_T$ and all $t$. If a `phi-phi` field is being computed, `deltaT` ($\Delta T$) is not empty and the diagonal mode is on, the subdiagonal blocks $I_T=J_T+\Delta T$ are computed instead (it will periodically wrap around the dilution extension $N_{D_T}$ if necessary).


### File specification
The file format follows the same structure as in `DistilMesonFieldRelative`, with the difference that `RelativeSide` is set to `"none"`.
