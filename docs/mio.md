# MIO

-----------

## LoadA2AMatrixDiskVector

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Loads an `EigenDiskVector<ComplexD>` from disk.

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `file`         | `std::string`             | filestem of the disk vector          |
| `dataset`     | `std::string`             | dataset within the file  |
| `diskVectorDir`     | `std::string`             | directory of the disk vector        |
| `cacheSize`     | `int`             | cache size  - used for optimization            |

### Dependencies

- None

### Products

- A2A Matrix disk vector `EigenDiskVector<ComplexD>`

-----------

## LoadA2AVectors

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Loads an all-to-all vector (`std::vector<FermionField>`) from disk.

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `filestem`         | `std::string`             | filestem of the A2A vector file on disk            |
| `multiFile`     | `bool`             | specify whether hdf5 saved as multiFile        |
| `size`     | `int`             | size of the vector       |

### Dependencies

- None

### Products

- A2AVector $v$ or $w$

-----------

## LoadBinary

### Template structure

One template argument `Impl`.

### Description

Loads a binary Nersc file from disk. 

Byte order formats must be one of `"IEEE32BIG"`,`"IEEE32"`,`"IEEE64BIG"`,`"IEEE64"`.

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `file`         | `std::string`             | filestem of the configuration on disk            |
| `format`     | `std::string`             | byte order format of the file       |

### Dependencies

- None

### Products

- Nersc `Field`

-----------

## LoadCoarseEigenPack

### Template structure

One template argument `GImpl`, expected to be a gauge implementation.

### Description

Loads two local coherence eigenPack (eigenvectors + eigenvalues) `BasePack` from disk. One is defined on a fine and one on a coarse Grid with `blockSize`.

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `filestem`         | `std::string`             | filestem of the configuration on disk            |
| `multiFile`     | `bool`             | specify whether file saved as multiFile        |
| `sizeFine`     | `unsigned int`             | size of the fine-grid eigenPack     |
| `sizeCoarse`     | `unsigned int`             | size of the coarse-grid eigenPack     |
| `Ls`     | `unsigned int`             | length of DW 5-dimension $L_s$  (must be $1$ for 4D eigenPack)     |
| `blockSize`     | `std::vector<int>`             | block size for eigenPack    |

### Dependencies

- None

### Products

- eigenPacks $(\lambda,v)^\mathrm{fine}$,$(\lambda,v)^\mathrm{coarse}$

-----------

## LoadDistilNoise

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Loads a distillation noise vector (created e.g. by `MDistil::Noises`) from disk.

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `NoiseFileName`         | `std::string`             | filestem of the noise file on disk            |
| `DistilParameters`     | `std::string`             | Distillation paramaters of the perambulator            |

### Dependencies

- Distillation parameters `MDistil::DistilParams`

### Products

- distillation noise $\rho$

-----------

## LoadEigenPack

### Template structure

One template argument `GImpl`, expected to be a gauge implementation.

### Description

Loads an eigenPack (eigenvectors + eigenvalues) `BasePack` from disk. A gauge transformation `gaugeXform` can be applied optionally. 

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `file`         | `std::string`             | filestem of the configuration on disk            |
| `multiFile`     | `bool`             | specify whether file saved as multiFile        |
| `size`     | `unsigned int`             | size of the eigenPack       |
| `Ls`     | `unsigned int`             | length of DW 5-dimension $L_s$  (must be $1$ for 4D eigenPack)     |
| `gaugeXform`     | `std::string`             | guage transformation to be applied to eigenvector       |

### Dependencies

- None

### Products

- eigenPack $(\lambda,v)$

-----------

## LoadCosmoHol

### Template structure

One template argument `SImpl`, expected to be a scalar implementation.

### Description

Loads a scalar $SU(N)$ configuration `Field` from disk.

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `file`         | `std::string`             | filestem of the configuration on disk            |

### Dependencies

- None

### Products

- CosmoHol configuration

-----------

## LoadNersc

### Template structure

One template argument `GImpl`, expected to be a gauge implementation.

### Description

Loads a Nersc configuration `GaugeField` from disk. 

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `file`         | `std::string`             | filestem of the configuration on disk            |

### Dependencies

- None

### Products

- Nersc configuration $U_\mu(x)$

-----------

## LoadPerambulator

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Loads a (stochastic) perambulator (created e.g. by `MDistil::Perambulator`) from disk.

### Parameters

| Parameter             | Type                       | Description            |
|-----------------------|----------------------------|------------------------|
| `PerambFileName`         | `std::string`             | filestem of the perambulator file on disk            |
| `DistilParameters`     | `std::string`             | Distillation paramaters of the perambulator            |

### Dependencies

- Distillation parameters `MDistil::DistilParams`

### Products

- perambulator $\tau$

-----------






