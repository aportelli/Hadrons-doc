# MSource

-----------

## MomentumPhase

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Multiplies a given source and multiplies it by a phase $e^{ipx}$ where $p$ is a 3-momentum.

$$source_{out}(x) = source_{in}(x) e^{ipx}$$

(This is useful e.g. for a Z2-wall source, which can then be used to estimate a 2-point correlation function with momentum $p \neq 0$.)

### Parameters

| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
|     `src`    | `std::string` | The source to be multiplied by a phase                                     |
|     `mom`    | `std::string` | 3-momentum $p$ of the phase.                                       |


### Dependencies

This module has no dependencies.

### Products

The `PropagatorField` $source_{out}(x)$.

-----------

## Point

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Point source is a delta function at a given position $x_0$.

$$source(x) = \delta_{x,x_0}$$

$source(x)$ is a unit matrix in Colour and Dirac space.

### Code description


### Parameters
| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
| `position`  | `std::string`  | A space separated integer sequence (e.g. `"0 1 1 0"`)

### Dependencies

This module has no dependancies.

### Products

The `PropagatorField` $source(x)$.

-----------

## SeqConserved

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Sequential source with an insertion of a conserved current $J_\mu$ using a `PropagatorField` $S(x)$ for $t_A\leq x_3 \leq t_B$:

$$source(x) = \sum\limits_{\mu=\mu_{min}}^{\mu_{max}} \theta(x_3 - t_A)\theta(t_B - x_3)\exp(i x\cdot p)J_\mu S(x)$$

Optionally an `EmField` $A_\mu$ can be inserted in addition:

$$source(x) = \sum\limits_{\mu=\mu_{min}}^{\mu_{max}} \theta(x_3 - t_A)\theta(t_B - x_3)\exp(i x\cdot p)A_\mu J_\mu S(x)$$

To obain the $source$ at $x$ in both terms of the point-split current (see below for examples of point-split currents implemented), one of the terms is shifted, e.g. 

$$\frac{1}{2}\overline{q}(x+\mu)(1+\gamma_\mu)U_\mu^\dagger(x)q(x)\exp(i x\cdot p)\rightarrow\frac{1}{2}\overline{q}(x)(1+\gamma_\mu)U_\mu^\dagger(x-\mu)q(x-\mu)\exp(i (x-\mu)\cdot p)$$

Examples of currents that can be inserted are:

- Wilson action
	- Conserved Vector Current
	$$\mathcal{V}_\mu(x) = \overline{q}(x) J_\mu q(x) = \frac{1}{2}\left[\overline{q}(x+\mu)(1+\gamma_\mu)U_\mu^\dagger(x)q(x)-\overline{q}(x)(1-\gamma_\mu)U_\mu(x)q(x+\mu)\right]$$

- Domain Wall action (In the following $Q(x,s)$ are 5D Quark fields)
	- Conserved Vector Current
	$$\mathcal{V}_\mu(x) = \overline{q}(x) J_\mu q(x) = \sum_{s=0}^{L_s-1} \frac{1}{2}\left[\overline{Q}(x+\mu,s)(1+\gamma_\mu)U_\mu^\dagger(x)Q(x,s)-\overline{Q}(x,s)(1-\gamma_\mu)U_\mu(x)Q(x+\mu,s)\right]$$
	- Conserved Axialvector Current
	$$\mathcal{A}_\mu(x) = \overline{q}(x) J_\mu q(x) = \sum_{s=0}^{L_s-1} \textrm{sign}\left(s-\frac{L_s-1}{2}\right) \frac{1}{2}\left[\overline{Q}(x+\mu,s)(1+\gamma_\mu)U_\mu^\dagger(x)Q(x,s)-\overline{Q}(x,s)(1-\gamma_\mu)U_\mu(x)Q(x+\mu,s)\right]$$
	- Tadpole Operator 
	$$\mathcal{T}_\mu(x) = \overline{q}(x) J_\mu q(x) = \sum_{s=0}^{L_s-1} \frac{1}{2}\left[\overline{Q}(x+\mu,s)(1+\gamma_\mu)U_\mu^\dagger(x)Q(x,s)+\overline{Q}(x,s)(1-\gamma_\mu)U_\mu(x)Q(x+\mu,s)\right]$$
  

If an `EmField` $A_\mu$ is inserted in addition, replace $U_\mu(x)$ by $A_\mu(x)U_\mu(x)$ in the above formulae.

### Parameters

| Parameter   | Type           | Description                                                                                                           |
|-------------|----------------|-----------------------------------------------------------------------------------------------------------------------|
| `q`         | `std::string`  | Name of the input `PropagatorField` $S(x)$                                                                            |
| `action`    | `std::string`  | Fermion action used for $S(x)$                                                                                        |
| `tA`        | `unsigned int` | begin timeslice                                                                                                       |
| `tB`        | `unsigned int` | end timeslice                                                                                                         |
| `curr_type` | `Current`      | Current type that is used, possible values are `Vector`, `Axial`, `Tadpole`                                           |
| `mu_min`    | `unsigned int` | begin index (source will be summed over `mu_min` $\leq\mu\leq$ `mu_max`)                                              | 
| `mu_max`    | `unsigned int` | end index (source will be summed over `mu_min` $\leq\mu\leq$ `mu_max`)                                                |
| `mom`       | `std::string`  | momentum insertion, space-separated float sequence (e.g `".1 .2 1. 0."`), with $p_\mu=$mom$_\mu$$\cdot 2\pi/L_\mu$    |
| `photon`    | `std::string`  | Optional parameter: Name of the input `EMField` $A(x)$, if required                                                   |


### Dependencies

- Input `PropagatorField` $S(x)$, i.e. object named by `q`.

- Fermion action used for $S(x)$, i.e. object named by `action`.

- Optionally: `EmField` $A_\mu(x)$, i.e. object named by `photon`.

### Products

The `PropagatorField` $source(x)$.

-----------

## SeqGamma

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Sequential source with an insertion of a gamma matrix $\Gamma$ using a `PropagatorField` $S(x)$ for $t_A\leq x_3 \leq t_B$:

$$source(x) = \theta(x_3 - t_A)\theta(t_B - x_3)\exp(i x\cdot p) \Gamma S(x)$$

### Parameters

| Parameter   | Type            |Description                                                                         |
|-------------|-----------------|------------------------------------------------------------------------------------|
| `q`         | `std::string`   | Name of the input `PropagatorField` $S(x)$                                         |
| `tA`        | `unsigned int`  | begin timeslice                                                                    |
| `tB`        | `unsigned int`  | end timeslice                                                                      |
| `gamma`     | `Gamma::Algebra`| one of the $16$ gamma matrices                                                     |
| `mom`       | `std::string`   | momentum insertion, space-separated float sequence (e.g `".1 .2 1. 0."`), with $p_\mu=$mom$_\mu$$\cdot 2\pi/L_\mu$ |

### Dependencies

Input `PropagatorField` $S(x)$, i.e. object named by `q`.

### Products

The `PropagatorField` $source(x)$.

-----------

## Wall

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Generates a source,

$$source(x) = \delta(x_3 - t_W) \cdot exp(i x\cdot p)$$,

i.e. a Wall source at $t_W$.

Where p is $p_\mu = \frac{2\pi}{L_\mu} \cdot mom_\mu$ were mom is the parameter you provide.

$source(x)$ is a unit matrix in Colour and Dirac space.

### Parameters

| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
|    `tW`     | `unsigned int` | The source timeslice.
|    `mom`    | `std::string`  | Momentum insertion, spaces separated float sequence (e.g `".1 .2 1. 0."`).


### Dependencies

This module has no dependencies

### Products

The `PropagatorField` $Source(x)$.

-----------

## Z2

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Generates a source,

$$source(x) = \eta(x) \theta(x_3 - t_A) \theta(t_B - x_3)$$

where the $\eta(x)$ are independent uniform random numbers in the set $$\frac{1}{\sqrt 2}(\pm 1 \pm i)$$ for each $x$. 

If `tA` and `tB` are equal then a Z_2 wall source is generated at $x_3 = t_A = t_B$. However if `tA` and `tB` are unequal a Z_2 band in generated for $t_A \leq x_3 \leq t_B$.

$source(x)$ is a unit matrix in Colour and Dirac space.

### Parameters

| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
|     `tA`    | `unsigned int` | The begin timeslice of the source.                                     |
|     `tB`    | `unsigned int` | The end timeslice of the source.                                       |


### Dependencies

This module has no dependencies.

### Products

The `PropagatorField` $Source(x)$.
