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

## InterlacedDistillation

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Generates a ($Z_2$) `DistillationNoise<FImpl>` embedded in an interlaced dilution scheme over time-Laplacian-spin indices. The dilution projectors can be explicitly written as $ P^I = P^{I_T} P^{I_L} P^{I_S}$ with 

$$ P_{tt'}^{I_T}            = \delta_{tt'} \delta_{I_T,t \text{ mod } TI}, \qquad I_T=0,\ldots,TI-1, $$
$$ P_{lk}^{I_L}             = \delta_{lk} \delta_{I_L,l \text{ mod } LI}, \qquad I_L=0,\ldots,LI-1, $$
$$ P_{\alpha\beta}^{I_S}    = \delta_{\alpha\beta} \delta_{I_S,\alpha \text{ mod } SI}, \qquad I_S=0,\ldots,SI-1, $$

and the diluted noise is

$$  \eta^{rI}_{l\alpha}(t) = \sum_{t'}^{I_T} \sum_{k}^{I_L} \sum_{\beta}^{I_S} P_{tt'}^{I_T} P_{lk}^{I_L} P_{\alpha\beta}^{I_S} \eta^{r}_{k\beta}(t'). $$

The values for each noise hit $r$ are seeded from a Unique Identifier (string). If `fileName` is unspecified, the noises are not saved to disk.

### Parameters

| Parameter          | Type                         | Description                            |
|--------------------|------------------------------|----------------------------------------|
| `ti`               | `unsigned int`               | number of time dilution partitions |
| `li`               | `unsigned int`               | number of Laplacian dilution partitions |
| `si`               | `unsigned int`               | number of spin dilution partitions |
| `nNoise`           | `unsigned int`               | number of noise hits |
| `lapEigenPack`     | `lapEigenPack`               | Laplacian eigenvectors/eigenvalues (`LapPack`) |
| `fileName`         | `std::string`                | file name for the noise saved to disk; not saved if empty |

### Dependencies

- Laplacian eigenvectors and eigenvalues (`LapPack`)

### Products

`DistillationNoise<FImpl>` $\eta^{rI}_{l\alpha}(t)$. This object is used by `Perambulator` and `DistilMesonField` to specify the distillation scheme.

-----------

## ExactDistillation

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Generates a `DistillationNoise<FImpl>` playing the role of noise policy in other modules, using the fact that exact distillation is a limit case of diluted distillation.

The noise policy values are all set to `1`, since there is no stochasticity.

### Parameters

| Parameter          | Type                         | Description                            |
|--------------------|------------------------------|----------------------------------------|
| `nVec`             | `unsigned int`               | number of Laplacian eigenvectors |
| `lapEigenPack`     | `lapEigenPack`               | Laplacian eigenvectors/eigenvalues (`LapPack`) |

### Dependencies

- Laplacian eigenvectors and eigenvalues (`LapPack`)

### Products

`DistillationNoise<FImpl>` $\eta^{r=1,I}_{l\alpha}(t)$, with $I$ being a full dilution index. This object is used by `Perambulator` and `DistilMesonField` to specify the exact distillation scheme.

-----------

## Perambulator

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

This module has an enum variable `perambMode` that switches between $3$ different behaviors:

#### pMode::perambOnly (default behavior)

Computes the perambulator $\tau^{r I}_{\alpha l}(t)$ from Laplacian eigenvectors, distillation noise and a solver. The dependency `distilNoise` dictates the structure on the noise and dilution indices ($r,I$). For example, passing a [`InterlacedDistillation`](localhost:3000/#/mdistil?id=interlaceddistillation) noise to `distilNoise` will specify the perambulator to be evaluated stochastically and with the interlaced dilution.

#### pMode::outputSolve

Same as above, but additionally computes the full solves [Mastropas, Richards, 2014, 1403.5575, eq. (20)]

$$ \phi^{rI}_{a\alpha}(\pmb x,t)  = \sum_{b,\beta,\pmb x', t'} D^{-1}_{a\alpha,b\beta}(\pmb x,t;\pmb x', t') \varrho^{rI}_{b \beta} (\pmb x',t') $$

Unlike the perambulators, the full solves are unsmeared at the sink and so not projected into the distillation basis. This makes them lattice-sized objects and therefore much larger.

#### pMode::inputSolve

In this mode the perambulators are reconstructed from a `fullSolve` by smearing the sink with a potentially different number of Laplacian eigenvectors, instead of using a solver. The input `nVec` specifies the number Laplacian eigenvectors to smear the sink, which of course needs to be less or equal the original number present at the source.


### Parameters

| Parameter             | Type              | Description                                                                                           |
|-----------------------|-------------------|-------------------------------------------------------------------------------------------------------|
| `lapEigenPack`        | `std::string`     | Laplacian eigenvectors/eigenvalues (`LapPack`)                                                        |
| `solver`              | `std::string`     | solver implementation                                                                                 |
| `distilNoise`         | `std::string`     | `DistillationNoise<FImpl>` implementation (noise policy)                                                                   |
| `timeSources`         | `std::string`     | desired list of time sources to be inverted over; uses all if empty                                           |
| `perambFileName`      | `std::string`     | file name for the perambulators saved to disk; not saved if empty                                      |
| `perambMode`          | `pMode`           | mode switch between `perambOnly: 0`, `inputSolve: 1`, `outputSolve: 2` (`perambOnly` if empty)        |
| `fullSolveFileName`   | `std::string`     | file name for full solve saved to disk; not saved if empty (`outputSolve` mode only)  |
| `multiFileFullSolve`  | `std::string`     | if true saves full solve as multiple files  (`outputSolve` mode only)                                 |
| `fullSolve`           | `std::string`     | full solve loaded from disk (`inputSolve` mode only)                                 |
| `nVec`                | `std::string`     | $N_{vec}$ to be used from `fullSolve`(`inputSolve` mode only)                                         |

### Dependencies

- Laplacian eigenvectors/eigenvalues (`LapPack`) 

- `DistillationNoise<FImpl>` implementation (e.g., [`InterlacedDistillation`](localhost:3000/#/mdistil?id=interlaceddistillation))

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
|`/GridDimensions`              | H5T_STD_U64LE | attribute identifying dimensions of the underlying Grid object  |
|`/TensorDimensions` 	        | H5T_STD_U64LE | attribute identifying original `NamedTensor` dimensions  |
|`/Metadata/Version` 			| H5T_STRING    | attribute identifying code version  |
|`/Metadata/TimeDilutionIndex`  | H5T_STD_I32LE | attribute identifying time source  |
|`/Metadata/NoiseHash_{idx}` 	| H5T_STD_I32LE | attribute containing the hash corresponding to the noise hit `idx`; `0` if exact distillation  |

-----------

## DistilMesonField

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Computes distillation meson field (for a certain momentum projection and gamma matrix)

$$ \tilde M_{\Gamma \mathbf p}^{IJ}(t) = \int d^3\mathbf x \ e^{-i\mathbf p . \mathbf x} M_{\Gamma}^{IJ}(\mathbf x, t),  \quad I=1,\ldots,N^{left}_D , \quad J=1,\ldots,N^{right}_D  $$

from Laplacian eigenvectors, perambulators and noise objects.

A meson field is a sparse matrix on the time dilution indices. This module abstracts the sparseness by not saving most of predictable zeros, which is specified next. The momentum-space meson field is split in 3D *time-dilution blocks*

$$ B^{I_T J_T} \equiv \tilde M^{(I_T,I_L,I_S)(J_T,J_L,J_S)}(t) = \left[ \tilde M^{t I_{LS}J_{LS}} \right]^{I_T J_T}, $$

where $I_{LS} = I_S + N_{D_S} I_L$ is a compound Laplacian-spin index (same for $J$). The 3D block has dimensions along the indices $t, I_{LS}$ and $J_{LS}$. Their ranges are

$$ I_{LS} = 0,\ldots, N^{left}_{D_L} N^{left}_{D_S}-1, $$
$$ J_{LS} = 0,\ldots, N^{right}_{D_L} N^{right}_{D_S}-1, $$
$$ t = 0,\ldots, N_t^{sparse}-1, $$

where $N_t^{sparse} \le N_t$ is a minimal time extension accounting for non-zero time slices (see data description). The parameters $\mathbf p, \Gamma$ are fixed.

### Parameters

| Parameter             | Type                          | Description            |
|-----------------------|-------------------------------|------------------------|
| `outPath`             | `std::string`                 | path of output folder tree          |
| `mesonFieldType`      | `std::string`                 | `"phi-phi"`,`"phi-rho"`, `"rho-phi"` or `"rho-rho"` |
| `lapEvec`             | `std::string`                 | 3D Laplacian eigenvectors |
| `gamma`               | `std::string`                 | list of gamma matrices |
| `momenta`             | `std::vector<std::string>`    | list of 3D momenta |
| `leftTimeSources`     | `std::string`                 | desired list of time-dilution indices chosen to be inverted over on left, e.g. `"0 2 4 6"` (if blank, uses all available)|
| `rightTimeSources`    | `std::string`                 | same as above for right side|
| `leftNoise`           | `std::string`                 | left `DistillationNoise<FImpl>` implementation (noise policy) |
| `rightNoise`          | `std::string`                 | right `DistillationNoise<FImpl>` implementation (noise policy) |
| `noisePairs`          | `std::vector<std::string>`    | desired list of "left right" noise index pairs available on the noise policies, e.g. `{"0 1" , "1 0"}`; can be left blank if using exact distillation |
| `leftPeramb`          | `std::string`                 | left perambulator (only when type is `"phi-..."`) |
| `rightPeramb`         | `std::string`                 | right perambulator (only when type is `...-phi"`) |
| `blockSize`           | `std::string`                 | high-level IO parameter |
| `cacheSize`           | `std::string`                 | low-level IO parameter |



### Dependencies

- Laplacian eigenvectors/eigenvalues (`LapPack`)
- `DistillationNoise<FImpl>` implementation (e.g., [`InterlacedDistillation`](localhost:3000/#/mdistil?id=interlaceddistillation))
- perambulator $\tau^{r I}_{\alpha l}(t)$

### Products

Distillation meson field (saved to disk).

### File specification
Folder tree in the format `outPath/{mesonFieldType}/{gamma}_p{momentum}_n{noisepair}.{traj}.h5` is created.

#### Data description
The data section of the file is organized in $(I_T,J_T)$ pairs: each dataset named `{I_T}-{J_T}` contains the correspondent 3D array $B^{I_T J_T}$. The pairs `{I_T}-{J_T}` are listed in `/DistilMesonField/Metadata/TimeDilutionPairs`.

- `/DistilMesonField/{I_T}-{J_T}` : Dataset of complex (implementation dependent) type containing a time-dilution block $B^{I_T J_T}$. The Dataspace has rank $3$ with sizes $( N_t^{sparse}, N^{left}_{D_L} N^{left}_{D_S}, N^{right}_{D_L} N^{right}_{D_S} )$.

$N_t^{sparse}$ is potentially different for each dataset and the content of the time dimension is described by `TimeSlices` (next item).

- `/DistilMesonField/{I_T}-{J_T}/TimeSlices` : H5T_STD_U32LE attribute metadata listing the (sink) time slices contained in the dataset. 

For example, on a lattice with $N_t=64$ we could have `TimeSlices={0,4}` for a certain time-dilution block, which means all the other $62$ time slices are trivially zero. The non-zero ones appear in the same order as in `TimeSlices`.


#### Metadata description

| Parameter             | HDF5 Type                          | Description            |
|-----------------------|-------------------------------|------------------------|
|`/Metadata/Nt` 			        | H5T_STD_U32LE | attribute identifying original time extension  |
|`/Metadata/Nvec` 			    | H5T_STD_U32LE | attribute identifying total number of 3D-Laplacian eigenvectors used for smearing   |
|`/Metadata/Operator` 			| H5T_STRING | attribute identifying gamma matrix |
|`/Metadata/Momentum` 			| H5T_STRING | attribute identifying 3d momentum  |
|`/Metadata/MesonFieldType` 	    | H5T_STRING | attribute identifying combination of distillation vectors  |
|`/Metadata/TimeDilutionPairs` 	| H5T_STD_U32LE | list of time-dilution blocks saved in the data section |
|`/Metadata/NoisePair`		    | H5T_STRING | attribute containing noise pair used |
|`/Metadata/NoiseHashLeft/NoiseHashLeft_{idx}`		| H5T_STRING | attribute containing the hash corresponding to the noise hit `idx`; `0` if exact distillation |
|`/Metadata/NoiseHashRight/NoiseHashRight_{idx}`		| same as above for right noise |
|`/Metadata/DilutionSchemes/TimeDilutionLeft`	    | H5T_STD_U32LE | attribute identifying the complete time-dilution scheme of the left vector as a 2d array  |
|`/Metadata/DilutionSchemes/TimeDilutionRight`	| same as above for right vector    |
|`/Metadata/DilutionSchemes/SpinDilutionLeft`	    | analogue to above for spin dilution and left vector   |
|`/Metadata/DilutionSchemes/SpinDilutionRight`	| same as above for right vector    |
|`/Metadata/DilutionSchemes/LapDilutionLeft`	    | analogue to above for laplacian dilution and left vector  |
|`/Metadata/DilutionSchemes/LapDilutionRight`	    | same as above for right vector    |

-----------


