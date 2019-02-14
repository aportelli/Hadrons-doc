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


