# MUtilities

## MomentumProjectorSphere

### Template structure

This module takes two template arguments, `Field` and `ComplexField`, that correspond to the two template arguments of the same name in the Grid `MomentumProject` class. The latter provides the main module functionality.

### Description

This module generates a list with all the Fourier modes in a sphere of radius
$$r \leq \text{maxFourier}$$
following the lexicographic order. For each mode, e.g. {2,-1,1}, the module creates a complex field $e^{i\vec{p} \cdot \vec{x}}$ defined for the entire lattice. Then, one can pass this module name to another module that needs to project data to the specified momenta via lines similar to these
```
auto &momProjector = envGet(MPType, par().momProjector);
momProjector.Project(positionSpace, fourierSpace);
```
where MPType is the specific class type of the template `MomentumProjectorSphere`, `positionSpace` is the input `Field` and `fourierSpace` is the output. For more information, read `Grid/algorithms/blas/MomentumProject.h`.

### Parameters

| Parameter    | Type           | Description                     |
|--------------|----------------|---------------------------------|
| `maxFourier` | `unsigned int` | Maximum radius in Fourier space |

### Dependencies

None.

### Products

This module produces three outputs:
  - A list of all Fourier modes to be computed, named as `getName() + "_momList"`. E.g., {{0,0,1}, {0,0,0}}.
  - A list of complex fields, each corresponding with a particular phase $e^{i\vec{p} \cdot \vec{x}}$, named as `getName() + "_phases"`.
  - An instance of the Grid class `MomentumProject`, named as `getName()`, which can be called upon to project a field of type `Field` to all the Fourier modes specified in the list.

Both the phases and the list of Fourier modes follow the same lexicographic order.