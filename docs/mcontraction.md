# MContraction

## Baryon

### Template structure

This module takes three `FImpl` template arguments `FImpl1`, `FImpl2`  and `FImpl3`, all of which are expected to be fermion implimentations.
 
### Description

This module computes a baryon two-point function

$$C_2 = \langle O_B(n)_\rho \bar{O}_B(m)_\rho \rangle$$

of baryon interpolators

$$O_B(n)_\rho = \epsilon^{a'b'c'} (P_\pm \Gamma^{A'})_{\rho \gamma'}   q^1_{c',\gamma'} (q^2_{a',\alpha'} \Gamma^{B'}_{\alpha' \beta'} q^3_{b',\beta'}),$$
$$\bar{O}_B(m)_\rho = \epsilon^{abc} (\bar{q}^5_{a,\alpha} \Gamma^B_{\alpha \beta} \bar{q}^6_{b,\beta})  \bar{q}^4_{c,\gamma}  (\Gamma^A P_\pm)_{\gamma \rho},$$

which consist of a diquark and a quark which shares its spin components with the interpolator. $\Gamma^A,\Gamma^B$ are projectors into certain total baryon spins $J$. The two-point function ca be evaluated to

$$\langle O_B(n)_\rho \bar{O}_B(m)_\rho \rangle =- \nonumber \langle  \bar{O}_B(m)_\rho  O_B(n)_\rho \rangle =  \epsilon^{abc} \epsilon^{a'b'c'} (\Gamma^A P_\pm \Gamma^{A'})_{\gamma \gamma'} (\Gamma^B)_{\alpha \beta} (\Gamma^{B'})_{\alpha' \beta'}$$
$$\times \Big( D^{q^1}(n|m)_{\gamma',c';\gamma,c} D^{q^2}(n|m)_{\alpha',a';\alpha,a} D^{q^3}(n|m)_{\beta',b';\beta,b} \ \delta_{q^4 q^5 q^6}^{q^1 q^2 q^3}$$
$$+ D^{q^1}(n|m)_{\gamma',c';\alpha,a} D^{q^2}(n|m)_{\alpha',a';\beta,b} D^{q^3}(n|m)_{\beta',b';\gamma,c} \ \delta_{q^4 q^5 q^6}^{q^2 q^3 q^1}$$
$$+ D^{q^1}(n|m)_{\gamma',c';\beta,b} D^{q^2}(n|m)_{\alpha',a';\gamma,c} D^{q^3}(n|m)_{\beta',b';\alpha,a} \ \delta_{q^4 q^5 q^6}^{q^3 q^1 q^2}$$
$$- D^{q^1}(n|m)_{\gamma',c';\gamma,c} D^{q^2}(n|m)_{\alpha',a';\beta,b} D^{q^3}(n|m)_{\beta',b';\alpha,a} \ \delta_{q^4 q^5 q^6}^{q^1 q^3 q^2}$$
$$- D^{q^1}(n|m)_{\gamma',c';\beta,b} D^{q^2}(n|m)_{\alpha',a';\alpha,a} D^{q^3}(n|m)_{\beta',b';\gamma,c} \ \delta_{q^4 q^5 q^6}^{q^3 q^2 q^1}$$
$$- D^{q^1}(n|m)_{\gamma',c';\alpha,a} D^{q^2}(n|m)_{\alpha',a';\gamma,c} D^{q^3}(n|m)_{\beta',b';\beta,b} \ \delta_{q^4 q^5 q^6}^{q^2 q^1 q^3} \Big).$$

In here, the kronecker deltas
$$\delta^{q^1 q^2 q^3}_{q^4 q^5 q^6} \equiv \delta^{q^1}_{q^4} \delta^{q^2}_{q^5} \delta^{q^3}_{q^6}$$
arise through the different possible Wick-contractions.

$$J=\frac 12: (\Gamma^A,\Gamma^B) \in \{ (1,C \gamma_5),(\gamma_5, C),(1,\gamma_0 C \gamma_5) \}$$

$$J=\frac 32: (\Gamma^A,\Gamma^B) \in \{ (1,C \gamma_i)\}$$

`quarks` is a vector of quark structures, `prefactors` is a vector of their multiplicities in the baryon interpolator. Two examples for the input structure: 

Nucleon interpolator:

$$O_{N_\pm}=\epsilon^{abc} P_\pm \Gamma^A u_a (u_b^T \Gamma^B d_c),$$

Here, `quarks = "uud"`, `prefactors = "1.0"`.

Lambda interpolator:

$$O_{\Lambda_\pm}=\epsilon^{abc} P_\pm \Gamma^A (2s_a (u_b^T \Gamma^B d_c) + d_a (u_b^T \Gamma^B s_c) - u_a (d_b^T \Gamma^B s_c)),$$

Here, `quarks = "sud dus uds"`, `prefactors = "2.0 1.0 -1.0"`.  The order of the propagators must match the order of the first quark-structure string. In this example, the order is `q1 = s-type`, `q2,q3 = l-type`. 

The input parameter `gammas` must be parsed as a list of pairs of pairs of gamma-Matrices, in the usual naming convention in Grid. The gamma structure for the baryons at source and sink can be chosen differently. The charge conjugation matrix $C=\gamma_0 \gamma_2$ must be multiplied manually. A $\Lambda$-baron two-point function with $(\Gamma^{A'},\Gamma^{B'})$ = $(1,C \gamma_1)$ at the sink and $(\Gamma^{A},\Gamma^{B})$ = $(1,C \gamma_2)$ at the source needs the input string `gammas = "((Identity MinusGammaZGamma5) (Identity GammaT))"`

There are some shorthands for the most common gamma structures:

`"j12" = "(Identity SigmaXZ)"` for $(\Gamma^{A},\Gamma^{B})$ = $(1,C \gamma_5)$

`"j32X" = "(Identity MinusGammaZGamma5)"` for $(\Gamma^{A},\Gamma^{B})$ = $(1,C \gamma_1)$

`"j32Y" = "(Identity GammaT)"` for $(\Gamma^{A},\Gamma^{B})$ = $(1,C \gamma_2)$

`"j32Z" = "(Identity GammaXGamma5)"` for $(\Gamma^{A},\Gamma^{B})$ = $(1,C \gamma_3)$

`"j12_alt1" = "(Gamma5 MinusSigmaYT)"` for $(\Gamma^{A},\Gamma^{B})$ = $(C, \gamma_5)$

`"j12_alt2" = "(Identity GammaYGamma5)"` for $(\Gamma^{A},\Gamma^{B})$ = $(1, \gamma_0 C\gamma_5)$

The input for a simple $J=\frac 12$ baryon could therefore be `gammas = "(j12 j12)"`, and for several combinations of $J=\frac 32$ baryon the input `gammas = "(j32X j32X) (j32Y j32Y) (j32Z j32Z) (j32X j32Y) (j32X j32Z) (j32Y j32X) (j32Y j32Z) (j32Z j32X) (j32Z j32Y)"` can be used.

### Parameters

| Parameter   | Type           | Description                                                            |
|--------------------|----------------------------|----------------------------------------|
| `q1` | `std::string` | input propagator 1 |
| `q2` | `std::string` | input propagator 2 |
| `q3` | `std::string` | input propagator 3 |
| `gammas` | `std::string` | Gamma matrices. List of pairs of pairs - see documentation |
| `quarks` | `std::string` | quark content - order of first entry must match order of `q1,q2,q3` |
| `prefactors` | `std::string` | multiplicities of quark contents |
| `parity` | `int` | parity - only $+1$ or $-1$ are allowed values (+1 is default) |
| `sink` | `std::string` | module used to compute the sink of the module|
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on three propagators being generated and a sink module being specified.

### Products

This module produces a correlator called `baryon` that is saved to a hdf5 file at a location of your choosing.

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
