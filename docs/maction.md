# MAction

-----------

## DWF

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Creates a domain-wall action based on the gauge field `gauge` with quark mass $m_q$, lattice extent $L_s$ in domain-wall 5-dimension and domain-wall mass parameter $M_5$.

Boundary conditions can be i.e. `1 1 1 -1` for periodic in space, anti-periodic in time.

Twist can be i.e. `0. 0. 0. 0.` for no twist

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
| `gauge`     | `std::string`  | Name of the input `GaugeField` $U_\mu(x)$      |
| `Ls`     | `int`  | domain-wall dimension extent $L_s$      |
| `mass`     | `double`  | quark mass $m_q$      |
| `M5`     | `double`  | domain-wall mass $M_5$      |
| `boundary`     | `std::string`  | boundary conditions, list of four integers $\pm 1$     |
| `twist`     | `std::string`  | twist on boundary, list of four Reals   |


### Dependencies

- Input `GaugeField` $U_\mu(x)$ , i.e. object named by `gauge`.


### Products

`DomainWallFermion` 

-----------

## MobiusDWF

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Creates a M\"obius domain-wall action based on the gauge field `gauge` with quark mass $m_q$, lattice extent $L_s$ in domain-wall 5-dimension and domain-wall mass parameter $M_5$, and M\"obius parameters $b$ and $c$. [arxiv:1206.5214]

$$D^\textrm{M\"obius}(M_5) \frac{(b+c) D^\mathrm{Wilson}(M_5)}{2+(b-c) D^\mathrm{Wilson}(M_5)} $$

Boundary conditions can be i.e. `1 1 1 -1` for periodic in space, anti-periodic in time.

Twist can be i.e. `0. 0. 0. 0.` for no twist

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
| `gauge`     | `std::string`  | Name of the input `GaugeField` $U_\mu(x)$      |
| `Ls`     | `int`  | domain-wall dimension extent $L_s$      |
| `mass`     | `double`  | quark mass $m_q$      |
| `M5`     | `double`  | domain-wall mass $M_5$      |
| `b`     | `double`  | M\"obius parameter $b$     |
| `c`     | `double`  | M\"obius parameter $c$      |
| `boundary`     | `std::string`  | boundary conditions, list of four integers $\pm 1$     |
| `twist`     | `std::string`  | twist on boundary, list of four Reals   |


### Dependencies

- Input `GaugeField` $U_\mu(x)$ , i.e. object named by `gauge`.


### Products

`MobiusFermion` 

-----------

## ScaledDWF

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
| `gauge`     | `std::string`  | Name of the input `GaugeField` $U_\mu(x)$      |


### Dependencies

- Input `GaugeField` $U_\mu(x)$ , i.e. object named by `gauge`.


### Products

`DomainWallFermion` 

-----------

## Wilson

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Creates a Wilson action based on the gauge field `gauge` with quark mass $m_q$.

Boundary conditions can be i.e. `1 1 1 -1` for periodic in space, anti-periodic in time.

Twist can be i.e. `0. 0. 0. 0.` for no twist

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
| `gauge`     | `std::string`  | Name of the input `GaugeField` $U_\mu(x)$      |
| `mass`     | `double`  | quark mass $m_q$      |
| `boundary`     | `std::string`  | boundary conditions, list of four integers $\pm 1$     |
| `string`     | `std::string`  | ?     |
| `twist`     | `std::string`  | twist on boundary, list of four Reals   |


### Dependencies

- Input `GaugeField` $U_\mu(x)$ , i.e. object named by `gauge`.


### Products

`WilsonFermion` 

-----------

## WilsonClover

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Creates a Wilson-Clover action based on the gauge field `gauge` with quark mass $m_q$ with Clover-term coefficients $c_\mathrm{SW}^r$ (spatial), $c_\mathrm{SW}^t$ (temporal). If an anisotropic lattice is used, `clover_anisotropy` can set the coefficients for that.

`WilsonAnisotropyCoefficients` comprise:
`bool`, isAnisotropic
`int`, t_direction
`double`, xi_0
`double`, nu

Boundary conditions can be i.e. `1 1 1 -1` for periodic in space, anti-periodic in time.

Twist can be i.e. `0. 0. 0. 0.` for no twist

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
| `gauge`     | `std::string`  | Name of the input `GaugeField` $U_\mu(x)$      |
| `mass`     | `double`  | quark mass $m_q$      |
| `csw_r`     | `double`  | Clover-term coefficient $c_\mathrm{SW}^r$      |
| `csw_t`     | `double`  | Clover-term coefficient $c_\mathrm{SW}^t$      |
| `clover_anisotropy`     | `WilsonAnisotropyCoefficients`  | quark mass $m_q$      |
| `boundary`     | `std::string`  | boundary conditions, list of four integers $\pm 1$     |
| `twist`     | `std::string`  | twist on boundary, list of four Reals   |


### Dependencies

- Input `GaugeField` $U_\mu(x)$ , i.e. object named by `gauge`.


### Products

`WilsonCloverFermion` 

-----------

## ZMobiusDWF

### Template structure

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Creates a Zolotarev-M\"obius domain-wall action based on the gauge field `gauge` with quark mass $m_q$, lattice extent $L_s$ in domain-wall 5-dimension and domain-wall mass parameter $M_5$, and M\"obius parameters $b$ and $c$. [arxiv:0304002]

`omega` are the weights used for the action.

Boundary conditions can be i.e. `1 1 1 -1` for periodic in space, anti-periodic in time.

Twist can be i.e. `0. 0. 0. 0.` for no twist

### Parameters

| Parameter   | Type           | Description                                    |
|-------------|----------------|------------------------------------------------|
| `gauge`     | `std::string`  | Name of the input `GaugeField` $U_\mu(x)$      |
| `Ls`     | `int`  | domain-wall dimension extent $L_s$      |
| `mass`     | `double`  | quark mass $m_q$      |
| `M5`     | `double`  | domain-wall mass $M_5$      |
| `b`     | `double`  | M\"obius parameter $b$     |
| `c`     | `double`  | M\"obius parameter $c$      |
| `omega`     | `std::vector<std::complex<double>>`  | M\"obius parameter $c$      |
| `boundary`     | `std::string`  | boundary conditions, list of four integers $\pm 1$     |
| `twist`     | `std::string`  | twist on boundary, list of four Reals   |


### Dependencies

- Input `GaugeField` $U_\mu(x)$ , i.e. object named by `gauge`.

### Products

`ZMobiusFermion` 

-----------
