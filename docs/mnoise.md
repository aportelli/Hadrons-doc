# MDistil

-----------

## ExactDistillation

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Generates a `DistillationNoise<FImpl>` playing the role of noise policy in distillation modules (perambulator and meson field), using the fact that exact distillation is a limit case of diluted distillation.

The noise policy values are all set to `1`, since there is no stochasticity.

### Parameters

| Parameter          | Type                         | Description                            |
|--------------------|------------------------------|----------------------------------------|
| `lapEigenPack`     | `lapEigenPack`               | Laplacian eigenvectors/eigenvalues (`LapPack`) |

### Dependencies

- Laplacian eigenvectors and eigenvalues (`LapPack`, see `MDistil::LapEvec`)

### Products

`DistillationNoise<FImpl>` $\eta^{r=1,I}_{l\alpha}(t)$, with $I$ being a full dilution index. This object is used by `MDistil::Perambulator` and `MDistil::DistilMesonField` to specify the exact distillation scheme.

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

- Laplacian eigenvectors and eigenvalues (`LapPack`, see `MDistil::LapEvec`)

### Products

`DistillationNoise<FImpl>` $\eta^{rI}_{l\alpha}(t)$. This object is used by `MDistil::Perambulator` and `MDistil::DistilMesonField` to specify the distillation scheme.


