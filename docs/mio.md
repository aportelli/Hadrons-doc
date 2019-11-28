# MIO

-----------

Documentation for the IO modules in Hadrons

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




