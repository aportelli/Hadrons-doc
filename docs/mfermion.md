# MFermion

-----------

## GaugeProp

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Calculates the `PropagatorField` $S$ using a given `solver` to solve the Dirac equation

$$D\cdot S = \eta$$

with a `PropagatorField` $\eta$ as a source. $D$ is the Dirac operator corresponding to the `action` which is specified in the `solver` used for the inversion.

### Parameters

| Parameter   | Type           | Description                                                                            |
|-------------|----------------|----------------------------------------------------------------------------------------|
| `source`    | `std::string`  | `source` $\eta$                                                                        |
| `solver`    | `std::string`  | `solver` used to solve the Dirac equation                                              |

### Dependencies

- Source, i.e. the `PropagatorField` named by `source`. 

- Solver, i.e. the object named by `solver`.

### Products

The `PropagatorField` $S(x)$.

If the `action` is 5D (e.g. Domain Wall Fermions), the 5D `PropagagtorField` as well as the projected 4D `PropagagtorField` are products of the module.

-----------

## FreeProp

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Calculates the free propagator `PropagatorField` $S$ for a given fermion `action` using a `PropagatorField` $\eta$ as a source, such that

$$D\cdot S = \eta$$

where $D$ is the free Dirac operator corresponding to `action`. Free propagators are calculated using a Fast Fourier Transform (FFT) and Feynman rules in momentum space. Twisted boundary conditions can be inserted for the free propagator. The momentum for a given twist angle $\theta_\mu$ is given by $p^\theta_\mu=\theta_\mu$$\cdot  2\pi/L_\mu$ .

### Parameters

| Parameter   | Type           | Description                                                                            |
|-------------|----------------|----------------------------------------------------------------------------------------|
| `source`    | `std::string`  | `source` $\eta$ for the free propagator                                                |
| `action`    | `std::string`  | fermion `action` for the free propagator                                               |
| `mass`      | `double`       | the `mass` of the free fermion                                                         |
| `twist`     | `std::string`  | twisted boundary conditions, space-separated float sequence (e.g `"0.0 0.0 0.0 0.5"`)  |





### Dependencies

- Source, i.e. the `PropagatorField` named by `source`.

- Fermion action, i.e. object named by `action`.


### Products

The `PropagatorField` $S(x)$.

If the `action` is 5D (e.g. Domain Wall Fermions), the 5D `PropagagtorField` as well as the projected 4D `PropagagtorField` are products of the module.


