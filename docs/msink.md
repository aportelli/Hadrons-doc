# MSink

-----------

## Point

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Sets up a point sink function $e^{i \mathbf{p} \cdot \mathbf{x}}$ with momentum `mom`.

### Parameters

| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
|     `mom`    | `std::string` | 3-momentum $\mathbf{p}$ of the phase.    |


### Dependencies

- None

### Products

- Sink function `SinkFn` $e^{i \mathbf{p} \cdot \mathbf{x}}$.

-----------

## Smear

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Sink smears a propagator using an input sink function `SinkFn`.

### Parameters
| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
| `q`  | `std::string`  | Input `PropagatorField`  |
| `sink`  | `std::string`  | sink function `SinkFn`  |

### Dependencies

- sink function `SinkFn`
- Input `PropagatorField` $D(x)$, i.e. object named by `q`.

### Products

- The sinked `SlicedPropagator` $D$.

-----------

