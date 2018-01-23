# MScalarSUN

-----------

## Div

### Template structure

One template argument `SImpl`, expected to be a scalar field implementation.

*Pre-build modules:*

| Name     | `SImpl`                | Description                           |
|----------|------------------------|---------------------------------------|
| `DivSU2` | `ScalarNxNAdjImplR<2>` | $\mathrm{SU}(2)$ adjoint scalar field |
| `DivSU3` | `ScalarNxNAdjImplR<3>` | $\mathrm{SU}(3)$ adjoint scalar field |
| `DivSU4` | `ScalarNxNAdjImplR<4>` | $\mathrm{SU}(4)$ adjoint scalar field |
| `DivSU5` | `ScalarNxNAdjImplR<5>` | $\mathrm{SU}(5)$ adjoint scalar field |
| `DivSU6` | `ScalarNxNAdjImplR<6>` | $\mathrm{SU}(6)$ adjoint scalar field |

### Description

Compute the divergence of an input complex vector field $j_{\mu}(x)$. Three
types of divergences can be computed:

- Forward divergence

$$div(x) = \sum_{\mu}[j_{\mu}(x+\hat{\mu})-j_{\mu}(x)]$$

- Backward divergence

$$div(x) = \sum_{\mu}[j_{\mu}(x)-j_{\mu}(x-\hat{\mu})]$$

- Central divergence

$$div(x) = \frac12\sum_{\mu}[j_{\mu}(x+\hat{\mu})-j_{\mu}(x-\hat{\mu})]$$

### Parameters

| Parameter | Type                       | Description                                                                                            |
|-----------|----------------------------|--------------------------------------------------------------------------------------------------------|
| `op`      | `std::vector<std::string>` | Components of the current $j_{\mu}$, the number of elements must be equal to the number of dimensions. |
| `type`    | `DivPar::DiffType` (enum)  | Finite difference type, accepted values are `forward`, `backward`, and `central`.                      |
| `output`  | `std::string`              | Output stem.                                                                                           |

### Dependencies

Components of $j_{\mu}(x)$, i.e. objects named by the elements of `op`.

### Products

The field $div(x)$.

-----------

## TrKinetic

### Template structure

### Description

### Parameters

### Dependencies

### Products

-----------

## TrMag

### Template structure

### Description

### Parameters

### Dependencies

### Products

-----------

## TrPhi

### Template structure

### Description

### Parameters

### Dependencies

### Products

-----------

## TwoPoint

### Template structure

### Description

### Parameters

### Dependencies

### Products