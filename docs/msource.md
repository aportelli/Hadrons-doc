# MSource

-----------

## Point

### Template structure

### Description

### Parameters

### Dependencies

### Products


-----------

## SeqConserved

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Sequential source with an insertion of a conserved current $J_\mu$ using a `PropagatorField` $q(x)$ for $t_A\leq x_3 \leq t_B$:

$$source(x) = \sum\limits_{\mu=\mu_{min}}^{\mu_{max}} \theta(x_3 - t_A)\theta(t_B - x_3)\exp(i x\cdot p)\,J_\mu q(x)$$

Optionally an `EmField` $A_\mu$ can be inserted in addition:

$$source(x) = \sum\limits_{\mu=\mu_{min}}^{\mu_{max}} \theta(x_3 - t_A)\theta(t_B - x_3)\exp(i x\cdot p)\,A_\mu \,J_\mu q(x)$$

The following currents can be computed:

- Wilson action
	- Conserved Vector Current
	$$\mathcal{V}_\mu(x) = \overline{q}(x) J_\mu q(x) = \frac{1}{2}\left[\overline{q}(x+\mu)(1+\gamma_\mu)U_\mu^\dagger(x)q(x)-\overline{q}(x)(1-\gamma_\mu)U_\mu(x)q(x+\mu)\right]$$

- Domain Wall action
	- Conserved Vector Current
	$$\mathcal{V}_\mu(x) = \overline{q}(x) J_\mu q(x) = \sum_{s=0}^{L_s-1} \frac{1}{2}\left[\overline{Q}(x+\mu,s)(1+\gamma_\mu)U_\mu^\dagger(x)Q(x,s)-\overline{Q}(x,s)(1-\gamma_\mu)U_\mu(x)Q(x+\mu,s)\right]$$
	- Conserved Axialvector Current
	$$\mathcal{A}_\mu(x) = \overline{q}(x) J_\mu q(x) = \sum_{s=0}^{L_s-1} \textrm{sign}\left(s-\frac{L_s-1}{2}\right) \frac{1}{2}\left[\overline{Q}(x+\mu,s)(1+\gamma_\mu)U_\mu^\dagger(x)Q(x,s)-\overline{Q}(x,s)(1-\gamma_\mu)U_\mu(x)Q(x+\mu,s)\right]$$
	- Tadpole Operator 
	$$\mathcal{T}_\mu(x) = \overline{q}(x) J_\mu q(x) = \sum_{s=0}^{L_s-1} \frac{1}{2}\left[\overline{Q}(x+\mu,s)(1+\gamma_\mu)U_\mu^\dagger(x)Q(x,s)+\overline{Q}(x,s)(1-\gamma_\mu)U_\mu(x)Q(x+\mu,s)\right]$$

If an `EmField` $A_\mu$ is inserted in addition, replace $U_\mu(x)$ by $A_\mu(x)U_\mu(x)$ in the above formulae.

### Parameters

| Parameter   | Type           | Description                                                                                                     |
|-------------|----------------|-----------------------------------------------------------------------------------------------------------------|
| `q`         | `std::string`  | Name of the input `PropagatorField` $q(x)$                                                                      |
| `action`    | `std::string`  | Fermion action used for $q(x)$                                                                                  |
| `tA`        | `unsigned int` | begin timeslice                                                                                                 |
| `tB`        | `unsigned int` | end timeslice                                                                                                   |
| `curr_type` | `Current`      | Current type that is used, possible values are `Vector`, `Axial`, `Tadpole`                                     |
| `mu_min`    | `unsigned int` | begin index (source will be summed over mu_min <= $\mu$ <= mu_max)                                              | 
| `mu_max`    | `unsigned int` | end index (source will be summed over mu_min <= $\mu$ <= mu_max)                                                |
| `mom`       | `std::string`  | momentum insertion, space-separated float sequence (e.g ".1 .2 1. 0."), with $p_\mu=$mom$_\mu$$\cdot 2\pi/L_\mu$|
| `photon`    | `std::string`  | Optional parameter: Name of the input `EMField` $A(x)$, if required                                             |


### Dependencies

- Input `PropagatorField` $q(x)$, i.e. object named by `q`.

- Fermion action used for $q(x)$, i.e. object named by `action`.

- Optionally: `EmField` $A_\mu(x)$, i.e. object named by `photon`.

### Products

The `PropagatorField` $source(x)$.

-----------

## SeqGamma

### Template structure

### Description

### Parameters

### Dependencies

### Products

-----------

## Wall

### Template structure

### Description

### Parameters

### Dependencies

### Products

-----------

## Z2

### Template structure

### Description

### Parameters

### Dependencies

### Products
