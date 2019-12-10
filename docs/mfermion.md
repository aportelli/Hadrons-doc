# MFermion

-----------

## EMLepton

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Calculates two `PropagatorField` objects: 

1) A free lepton propagator with a sequential insertion of $i \gamma_\mu A_\mu$ with a photon field $A_\mu$ 

$$L(x) = \sum_y S(x,y) i*\gamma_\mu*A_\mu S(y,x_l) \delta_{(t_l-x_0),\Delta T}$$

with a wall source for the lepton at $t_l$ and a source-sink separation $\Delta T$.

2) The propagator without photon vertex

$$L^\mathrm{free}(x) =  S(x,xl) \delta_{(t_l-x_0),\Delta T}.$$

$S$ is the Dirac operator corresponding to the `action` which is specified in the `solver` used for the inversion.

### Parameters

| Parameter   | Type           | Description                                                                            |
|-------------|----------------|-------------------------------------------------------|
| `action`    | `std::string`  | fermion `action` for the free propagator                                               |
| `emField`    | `std::string`  | `EmField` $A_\mu$                        |
| `mass`    | `double`  | input mass for the lepton propagator                       |
| `boundary`    | `std::string`  | boundary conditions for the lepton propagator, e.g. `"1 1 1 -1"`                       |
| `twist`    | `std::string`  | twisted boundary conditions for the lepton propagator, e.g. `"0. 0. 0. 0.5"`                       |
| `deltat`    | `std::vector<unsigned int>`  | list of source-sink separations $\Delta T$         |

### Dependencies

- Fermion action, i.e. object named by `action`.

- photon field, i.e. the `EmField` named by `emField`. 

### Products

The `PropagatorFields` $L(x)$ and $L^\mathrm{free}(x)$.

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


