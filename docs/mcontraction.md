# MContraction

## Baryon

### Template structure

This module takes three `FImpl` template arguments `FImpl1`, `FImpl2`  and `FImpl3`, all of which are expected to be fermion implimentations.
 
### Description

This module computes a baryon two-point function

$$C_2 = \langle O_B(n)_\rho \bar{O}_B(m)_\rho \rangle$$

of baryon interpolators

$$O_B(n)_\rho = \epsilon^{a'b'c'} (P_\pm \Gamma^A)_{\rho \gamma}   q^1_{c',\gamma'} (q^2_{a',\alpha'} \Gamma^B_{\alpha' \beta'} q^3_{b',\beta'}),$$
$$\bar{O}_B(m)_\rho = \epsilon^{abc} (\bar{q}^5_{a,\alpha} \Gamma^B_{\alpha \beta} \bar{q}^6_{b,\beta})  \bar{q}^4_{c,\gamma}  (\Gamma^A P_\pm)_{\gamma \rho},$$

which consist of a diquark and a quark which shares its spin components with the interpolator. $\Gamma^A,\Gamma^B$ are projectors into certain total baryon spins $J$. The two-point function ca be evaluated to

$$\langle O_B(n)_\rho \bar{O}_B(m)_\rho \rangle =- \nonumber \langle  \bar{O}_B(m)_\rho  O_B(n)_\rho \rangle =  \epsilon^{abc} \epsilon^{a'b'c'} (\Gamma^A P_\pm \Gamma^A)_{\gamma \gamma'} (\Gamma^B)_{\alpha \beta} (\Gamma^B)_{\alpha' \beta'}$$
$$\times \Big( D^{q^1}(n|m)_{\gamma',c';\gamma,c} D^{q^2}(n|m)_{\alpha',a';\alpha,a} D^{q^3}(n|m)_{\beta',b';\beta,b} \ \delta_{q^4 q^5 q^6}^{q^1 q^2 q^3}$$

This module takes three propagators and a source and sink gamma structure.

Explain how lists of gamma structures are dealt with and the `all` option.

Define the contraction macro,

$$#define mesonConnected(q1, q2, gSnk, gSrc) (g5*(gSnk))*(q1)*(adj(gSrc)*g5)*adj(q2)$$


If has sliced Propagator:

get sliced propagators and gamma matrixes and conntract them. Trace and remove Tensor (TensorRemove).

else:

	if sink parameter is a `MSource` object:

		get the sink object trace over the graph and perform a slice sum.

	else if the sink parameter is a `MSink` object:

		trace over the graph and input this to the sink function.

	Remove the Tensor structures (TensorRemove) and pass to output.

### Parameters
| Parameter   | Type           | Description                                                            |
| `q1` | `std::string` | input propagator 1|
| `q2` | `std::string` | input propagator 2|
| `gammas` | `std::string` | gamma products to insert and source and sink, pairs of gammas seperated by a space in round brackets.
		      Special value: `all` - perform all possible contractions.|
| `sink` | `std::string` | module used to compute the sink of the module.|
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on a pair of propagators being generated and a sink module being specified.

### Products

This module produces a correlator called `meson` that is saved to a hdf5 file at a location of your
choosing.

-----------

## DiscLoop

$$C(t)=\sum_{\mathbf{x}}\mathrm{tr}[S(t,\mathbf{x})\gamma_{\mu}]$$

## Gamma3pt

-----------

## Meson

### Template structure

This module takes two `FImpl` template arguments `FImpl1` and `FImpl2,` both are expected to be 
fermion implimentations.
 
### Description

This module takes a pair of propagators and a source and sink gamma structure.

Explain how lists of gamma structures are dealt with and the `all` option.

Define the contraction macro,

$$#define mesonConnected(q1, q2, gSnk, gSrc) (g5*(gSnk))*(q1)*(adj(gSrc)*g5)*adj(q2)$$


If has sliced Propagator:

get sliced propagators and gamma matrixes and conntract them. Trace and remove Tensor (TensorRemove).

else:

	if sink parameter is a `MSource` object:

		get the sink object trace over the graph and perform a slice sum.

	else if the sink parameter is a `MSink` object:

		trace over the graph and input this to the sink function.

	Remove the Tensor structures (TensorRemove) and pass to output.

### Parameters
| Parameter   | Type           | Description                                                            |
| `q1` | `std::string` | input propagator 1|
| `q2` | `std::string` | input propagator 2|
| `gammas` | `std::string` | gamma products to insert and source and sink, pairs of gammas seperated by a space in round brackets.
		      Special value: `all` - perform all possible contractions.|
| `sink` | `std::string` | module used to compute the sink of the module.|
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on a pair of propagators being generated and a sink module being specified.

### Products

This module produces a correlator called `meson` that is saved to a hdf5 file at a location of your
choosing.

-----------


## A2AMesonField

### Template Stucture

One template argument `FImpl`, expected to be a fermion implementation.

### Description

momentum is $$p = 2\pi/L * mom$$.

give detail of meson field structure.

Detail complex blocking strucutres used to gain performance. discuss with peter and Fionn after a detailed read.


### Parameters

| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
|     `v`     | `std::string`  | Set of eigen vectors low and high modes                                |
|     `w`     | `std::string`  | Set of eigen vectors low and high modes                                |
|    `gammas` | `std::string`  | gamma products to insert and source and sink, pairs of gammas seperated by a space in round brackets.
		      Special value: `all` - perform all possible contractions. |
|    `mom`    | `std::vector<std::string>` | what momentum this meson field has e.g `"0 1 0 0"`.        |
|   `output`  | `std::string`  | name of the output file that the meson field will be saved to.         |
|   `cacheBlock`| `int`        | performance tuning parameter (give values for various architectures)   |
|   `block`   |  `int`         | performance tuning parameter (give values for various architectures)   |

### Dependencies

Theis module depends on the creation of the `v` and `w` eigenvectors being determind.

### Products

This module produces a `meson field` that is saved to a hdf5 file at a location of your choosing.
