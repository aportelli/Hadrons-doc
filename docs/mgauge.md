# MGauge

-----------

## Electrify

### Template structure

One template argument `GImpl`, expected to be a gauge implementation.

### Description

Multiplies a gauge field $U_\mu(x)$ with a $U(1)$ field

$$U^e_\mu(x) = U_\mu(x)\cdot exp(ieqA_\mu(x))$$

where $A_\mu(x)$ is an `EmField`, $e$ is the elementary charge and $q$ the charge in units of $e$.

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
| `gauge`     | `std::string`  | Name of the input `GaugeField` $U_\mu(x)$      |
| `emField`   | `std::string`  | Name of the input `EMField` $A_\mu(x)$         |
| `e`         | `double`       | Value for the elementary charge $e$            |
| `charge`    | `double`       | charge $q$ in units of $e$                     |

### Dependencies

- Input `GaugeField` $U_\mu(x)$ , i.e. object named by `gauge`.

- Input `EmField` $A_\mu(x)$, i.e. object named by `emField`

### Products

The `GaugeField` $U^e_\mu(x)$

-----------

## FundtoHirep

### Template structure

One template argument `GImpl`, expected to be a gauge implementation.

### Description


### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
|      |  |      |


### Dependencies


### Products

-----------

## GaugeFix

### Template structure

One template argument `GImpl`, expected to be a gauge implementation.

### Description

Fixes the gauge (`coulomb` or `landau`) for an input gauge field.

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
| `gauge`     | `std::string`  | Name of the input `GaugeField` $U_\mu(x)$      |
| `alpha`   | `Real`  |      |
| `maxiter`         | `int`       |           |
| `Omega_tol`    | `Real`       |                |
| `Phi_tol`    | `Real`       |                |
| `gaugeFix`    | `Fix`       |   `coulomb` or `landau`             |
| `Fourier`    | `bool`       |                |


### Dependencies

- Input `GaugeField` $U_\mu(x)$ , i.e. object named by `gauge`.

### Products

Gauge-fixed `GaugeField` $U^{\'}_\mu(x)$

-----------

## Random

### Template structure

One template argument `GImpl`, expected to be a gauge implementation.

### Description

Creates a random gauge field. 

### Parameters

No parameters. Random seed is set by the module name and runId tag.

### Dependencies

None.

### Products

Random `GaugeField` $U_\mu(x)$

-----------

## StochEm

### Template structure

NOTE: No template argument yet, not strictly needed but desired.

### Description

Creates a stochastic electromagnetic gauge field from a gauge (`feynman`,`coulomb` or `landau`) and zero-mode scheme (`qedL` or `qedTL`).

Find out how weight works???

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
|  `gauge`    | `PhotonR::Gauge` |  `feynman`,`coulomb` or `landau`    |
|  `zmScheme`    | `PhotonR::zmScheme` |  `qedL` or `qedTL`    |
|  `improvement`    | `std::string` | improvement coefficients, parsed as a list of `Real`    |

### Dependencies

None.

### Products

Stochastic `EmField` $A_\mu(x)$


-----------

## StoutSmearing

### Template structure

One template argument `GImpl`, expected to be a gauge implementation.

### Description

Applies Stout-Smearing (arxiv:0311018) to a gauge field using $n_\rho$ `steps` with smearing weight $\rho$ (constant in all lattice diections, $\rho_{\mu\nu} = \rho$).

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
| `gauge`     | `std::string`  | Name of the input `GaugeField` $U_\mu(x)$      |
| `steps`         | `int`       | Number of smearing iterations $n_\rho$        |
| `rho`    | `double`       | smearing weight $\rho$                    |


### Dependencies

- Input `GaugeField` $U_\mu(x)$ , i.e. object named by `gauge`.

### Products

Stout-smeared `GaugeField` $U_\mu(x)$


-----------

## Unit

### Template structure

One template argument `GImpl`, expected to be a gauge implementation.

### Description

Creates a unit gauge field. 

### Parameters

None.

### Dependencies

None.

### Products

Unit `GaugeField` $U_\mu(x)$


-----------

## UnitEm

### Template structure

NOTE: No template argument yet, not strictly needed but desired.

### Description

Creates a unit electromagnetic gauge field. 

### Parameters

None.

### Dependencies

None.

### Products

Unit `EmField` $A_\mu(x)$



