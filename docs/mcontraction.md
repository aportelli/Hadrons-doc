# MContraction

## Baryon

### Template structure

This module takes a `FImpl` template argument which is expected to be fermion implimentations.
 
### Description

This module computes a baryon two-point function

$$C_2 = \langle O_B(n)_\rho \bar{O}_{B'}(m)_\rho \rangle$$

of baryon interpolators

$$O_B(n)_\rho = \epsilon^{a'b'c'} (P_\pm \Gamma^{A'})_{\rho \gamma'}   q^1_{c',\gamma'} (q^2_{a',\alpha'} \Gamma^{B'}_{\alpha' \beta'} q^3_{b',\beta'}),$$
$$\bar{O}_{B'}(m)_\rho = \epsilon^{abc} (\bar{q}^5_{a,\alpha} \Gamma^B_{\alpha \beta} \bar{q}^6_{b,\beta})  \bar{q}^4_{c,\gamma}  (\Gamma^A P_\pm)_{\gamma \rho},$$

which consist of a diquark and a quark which shares its spin components with the interpolator. $\Gamma^A,\Gamma^B$ are projectors into certain total baryon spins $J$. The two-point function ca be evaluated to

$$\langle O_B(n)_\rho \bar{O}_{B'}(m)_\rho \rangle =- \nonumber \langle  \bar{O}_{B'}(m)_\rho  O_B(n)_\rho \rangle =  \epsilon^{abc} \epsilon^{a'b'c'} (\Gamma^A P_\pm \Gamma^{A'})_{\gamma \gamma'} (\Gamma^B)_{\alpha \beta} (\Gamma^{B'})_{\alpha' \beta'}$$
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
| `quarks` | `std::string` | final state quark content - order must match order of `q1,q2,q3` |
| `shuffle` | `std::string` | defines the inital state quarks from the final state quarks - must be a permutation of `123` |
| `sinkq1` | `std::string` | module used to compute the sink of the final state quark q1|
| `sinkq2` | `std::string` | module used to compute the sink of the final state quark q2|
| `sinkq3` | `std::string` | module used to compute the sink of the final state quark q3|
| `sim_sink` | `bool` | flag for use of a simultatious sink - `false` uses the three sinks above separatly - `true` checks `sinkq1`=`sinkq2`=`sinkq3` and uses this to sink the quarks together |
| `gammas` | `std::string` | Gamma matrices. List of pairs of pairs - see documentation |
| `parity` | `int` | parity - only $+1$ or $-1$ are allowed values (+1 is default) |
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on three propagators being generated and three sink module being specified.

### Products

This module produces a correlator called `baryon` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing.














## Baryon

### Template structure

This module takes a `FImpl` template argument which is expected to be fermion implimentations.
 
### Description

This module computes a baryon two-point function

$$C_2 = \langle O_{B'}(n)_\rho \bar{O}_{B}(m)_\rho \rangle$$

of baryon interpolators

$$O_{B'}(n)_\rho = \epsilon^{a'b'c'} (P_\pm \Gamma^{A'})_{\rho \gamma'}   {q^R_{1}}_{c',\gamma'} ({q^R_{2}}_{a',\alpha'} \Gamma^{B'}_{\alpha' \beta'} {q^R_{3}}_{b',\beta'}),$$
$$\bar{O}_{B}(m)_\rho = \epsilon^{abc} {\bar{q}^R_{1}}_{c,\gamma}  (\Gamma^A P_\pm)_{\gamma \rho} ({\bar{q}^L_{2}}_{a,\alpha} \Gamma^B_{\alpha \beta} {\bar{q}^R_{3}}_{b,\beta}),$$

The quarks of the final state are given as a module input $(q^R_1,q^R_2,q^R_3) = (q_1,q_2,q_3)$.
The inintal state quarks are defined as a permuation of the final state via a shuffle $(\sigma_1,\sigma_2,\sigma_3)$, giving the initial state quarks $(q^L_1,q^L_2,q^L_3) = (q_{\sigma_1},q_{\sigma_2},q_{\sigma_3})$.

The propagators $(\Delta_1, \Delta_2 ,\Delta_3)$ are given in the order corresponding to the **final** state quarks. The initial state propagators are then given by $(\Delta^L_1, \Delta^L_2 ,\Delta^L_3) = (\Delta_{\sigma_1}, \Delta_{\sigma_2} ,\Delta_{\sigma_3})$, where $\Delta^L_i$ is inverted on the source for inital state quark $i$.

Sinks $(S_1,S_2,S_3)$ are functions for sinking the corresponding **final** state quarks.

The ouput of the module is then given by
$$\langle O_{B'}(n)_\rho \bar{O}_{B}(m)_\rho \rangle = \epsilon^{abc}\epsilon^{a'b'c'} P^{\pi}_{\rho \rho'} \Gamma^{A}_{\gamma \rho} \Gamma^{A'}_{\rho' \gamma'} \Gamma^{B}_{\alpha \beta} \Gamma^{B'}_{\alpha' \beta'}$$
$$\times \left[ + \delta^{q^R_1 q^R_2 q^R_3}_{q^L_1 q^L_2 q^L_3} S_{1}(\Delta^L_1)_{\gamma' \gamma}^{c' c} S_{2}(\Delta^L_2)_{\alpha' \alpha}^{a' a} S_{3}(\Delta^L_3)_{\beta' \beta}^{b' b} \right.$$
$$+ \delta^{q^R_2 q^R_3 q^R_1}_{q^L_1 q^L_2 q^L_3} S_{2}(\Delta^L_1)_{\alpha \gamma}^{a' c} S_{3}(\Delta^L_2)_{\beta' \alpha}^{b' a} S_{1}(\Delta^L_3)_{\gamma' \beta}^{c' b}$$
$$+ \delta^{q^R_3 q^R_1 q^R_2}_{q^L_1 q^L_2 q^L_3} S_{3}(\Delta^L_1)_{\beta' \gamma}^{b' c} S_{1}(\Delta^L_2)_{\gamma' \alpha}^{c' a} S_{2}(\Delta^L_3)_{\alpha' \beta}^{a' b}$$
$$- \delta^{q^R_1 q^R_3 q^R_2}_{q^L_1 q^L_2 q^L_3} S_{1}(\Delta^L_1)_{\gamma' \gamma}^{c' c} S_{3}(\Delta^L_2)_{\beta' \alpha}^{b' a} S_{2}(\Delta^L_3)_{\alpha' \beta}^{a' b}$$
$$- \delta^{q^R_3 q^R_2 q^R_1}_{q^L_1 q^L_2 q^L_3} S_{3}(\Delta^L_1)_{\beta' \gamma}^{b' c} S_{2}(\Delta^L_2)_{\alpha' \alpha}^{a' a} S_{1}(\Delta^L_3)_{\gamma' \beta}^{c' b}$$
$$\left. -\delta^{q^R_2 q^R_1 q^R_3}_{q^L_1 q^L_2 q^L_3} S_{2}(\Delta^L_1)_{\alpha' \gamma}^{a' c} S_{1}(\Delta^L_2)_{\gamma' \alpha}^{c' a} S_{3}(\Delta^L_3)_{\beta' \beta}^{b' b} \right]$$
where each terms corresponds to a wick contraction labelled in order 1-6.

When computing baryons with sums over different interpolators like the $\Lambda$-baryon
$$O_{\Lambda}(n)_\rho = \epsilon^{a'b'c'} (P_\pm \Gamma^{A'})_{\rho \gamma'} \left[  2{s}_{c',\gamma'} ({u}_{a',\alpha'} \Gamma^{B'}_{\alpha' \beta'} {d}_{b',\beta'}) + {d}_{c',\gamma'} ({u}_{a',\alpha'} \Gamma^{B'}_{\alpha' \beta'} {s}_{b',\beta'}) - {u}_{c',\gamma'} ({d}_{a',\alpha'} \Gamma^{B'}_{\alpha' \beta'} {s}_{b',\beta'}) \right]$$
or the $\Sigma^0$-baryon
$$O_{\Lambda}(n)_\rho = \epsilon^{a'b'c'} (P_\pm \Gamma^{A'})_{\rho \gamma'} \left[ {d}_{c',\gamma'} ({u}_{a',\alpha'} \Gamma^{B'}_{\alpha' \beta'} {s}_{b',\beta'}) + {u}_{c',\gamma'} ({d}_{a',\alpha'} \Gamma^{B'}_{\alpha' \beta'} {s}_{b',\beta'}) \right]$$
the module must be run multiple times to get all the combinations. This is 9 and 4 times for the $\Lambda$ and $\Sigma^0$ respectively.

Additionally, since the ordering of the quarks will be different in the intial and final state quarks for some of these combinations, the shuffle parameter will change. For example, the inital state `uds` and a final state `sud` requires a shuffle of `231`.

The input parameter `gammas` must be parsed as a list of pairs of pairs of gamma-Matrices, in the usual naming convention in Grid. The gamma structure for the baryons at source and sink can be chosen differently. The charge conjugation matrix $C=\gamma_0 \gamma_2$ must be multiplied manually. A baron two-point function with $(\Gamma^{A'},\Gamma^{B'})$ = $(1,C \gamma_1)$ at the sink and $(\Gamma^{A},\Gamma^{B})$ = $(1,C \gamma_2)$ at the source needs the input string `gammas = "((Identity MinusGammaZGamma5) (Identity GammaT))"`

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
| `q1` | `std::string` | input propagator $\Delta_1$ |
| `q2` | `std::string` | input propagator $\Delta_2$ |
| `q3` | `std::string` | input propagator $\Delta_3$ |
| `quarks` | `std::string` | final state quark content - order must match order of `q1,q2,q3` |
| `shuffle` | `std::string` | defines the inital state quarks from the final state quarks - must be a permutation of `123` |
| `sinkq1` | `std::string` | module used to compute the sink of the final state quark q1|
| `sinkq2` | `std::string` | module used to compute the sink of the final state quark q2|
| `sinkq3` | `std::string` | module used to compute the sink of the final state quark q3|
| `sim_sink` | `bool` | flag for use of a simultatious sink - `false` uses the three above sinks separatly - `true` checks `sinkq1`=`sinkq2`=`sinkq3` and uses this to sink the quarks together |
| `gammas` | `std::string` | List of pairs of pairs Gamma matrices. |
| `parity` | `int` | parity - only $+1$ or $-1$ are allowed values (+1 is default) |
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on three propagators being generated and three sink module being specified.

### Products

This module produces a correlator called `baryon` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing.

-----------

## DiscLoop

### Template structure

This module takes a `FImpl` template argument, which is expected to be a fermion implimentation.
 
### Description

This module computes the disconnected loop

$$C(t)=\sum_{\mathbf{x}}\mathrm{tr}[S(t,\mathbf{x})\gamma_{\mu}] \, .$$

### Parameters
| Parameter   | Type   |   Description                       |
|-------------|----------------|-----------------------------|
| `q1` | `std::string` | input propagator 1 (must be sinked already) |
| `q2` | `std::string` | input propagator 2 |
| `q3` | `std::string` | input propagator 3 |
| `gamma` | `std::string` | list of gamma matrices|
| `tSnk` | `int` | sink time $x_f$|
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on a propagator being generated.

### Products

This module produces a correlator called `disc` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing.

## Gamma3pt

### Template structure

This module takes three `FImpl` template arguments `FImpl1`, `FImpl2`  and `FImpl3`, all of which are expected to be fermion implimentations.

### Description

This module computes the mesonic three-point function

$$C(t)=\langle O^{\gamma_5}_{M_1}(x_i) \bar{O}_{M_2}(x) \bar{O}^{\gamma_5}_{M_3}(x_f) \rangle \, ,$$

where the gamma insertions at source and sink are fixed to $\gamma_5$

This module takes an already sink smeared propagator `q1` with the source at $x_i$ and sinked at $x_f$, as well as two non-sinked propagators `q2` (source at $x_i$), `q3` (source at $x_f$).

The gamma matrices can be a list, seperated by spaces. There is also the option to compute all 16 matrices by specifying the `all` option in the input parameter `gamma`. 

### Parameters
| Parameter   | Type   |   Description                       |
|-------------|----------------|-----------------------------|
| `q_loop` | `std::string` | input loop propagator |
| `gamma` | `Gamma::Algebra` | gamma matrix|
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on three propagators being generated, one of which has to be sinked already (e.g. using `MSink::Smear`).

### Products

This module produces a correlator called `gamma3pt` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing.

## Meson

### Template structure

This module takes two `FImpl` template arguments `FImpl1` and `FImpl2,` both are expected to be 
fermion implimentations.
 
### Description

This module computes a meson two-point function

$$C_2 = \langle O_M(n) \bar{O}_{M'}(m) \rangle$$

This module takes a pair of quark propagators $q_1,q_2$ and a source and sink gamma structure $\Gamma_{\mathrm{src}},\Gamma_{\mathrm{snk}}$.

The gamma matrices have to be specified as pairs of gammas in round brackets, seperated by a space. There is also the option to compute all 256 combinations of $\Gamma_{\mathrm{src}},\Gamma_{\mathrm{snk}}$ by specifying the `all` option in the input parameter `gammas`. 

This module computes the trace

$$ \gamma_5 \Gamma_{\mathrm{snk}} q_1 (\Gamma_{\mathrm{src}} \gamma_5)^\dagger q_2^\dagger $$

The `sink` object can either be from the `MSource` namespace or the `MSink` namespace and the module handles this sinking procedure accordingly.

The input propagator can optionally be already a `SlicedPropagator`, i.e. a propagator $D^{-1}(x_1,x_2)$ from one fixed point $x_1$ to another fixed point $x_2$, e.g. an output from the module `MSink::Smear`. In this case, the `sink` input parameter is not used by the module.

### Parameters
| Parameter   | Type   |   Description                       |
|-------------|----------------|-----------------------------|
| `q1` | `std::string` | input propagator 1|
| `q2` | `std::string` | input propagator 2|
| `gammas` | `std::string` | gamma products to insert and source and sink.|
| `sink` | `std::string` | module used to compute the sink of the module.|
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on a pair of propagators being generated and a sink module being specified.

### Products

This module produces a correlator called `meson` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing.

-----------


## A2AMesonField

### Note

This module is also used for distillation. Instead of `v` and `w` vectors, the vector inputs are  $\varrho$ and $\varphi$ from `MDistil::DistilVectors` or the unsmeared sinks $\phi$ from `MDistil::Perambulator`.

### Template Stucture

One template argument `FImpl`, expected to be a fermion implementation.

### Description

`mom` is the momentum in lattice units, i.e. the full momentum the meson field is projected to is 

$$p = 2\pi/L * mom\, .$$

The gamma matrices have to be specified as pairs of gammas in round brackets, seperated by a space. There is also the option to compute all 256 combinations of $\Gamma_{\mathrm{src}},\Gamma_{\mathrm{snk}}$ by specifying the `all` option in the input parameter `gammas`. 

Detail complex blocking strucutres used to gain performance. discuss with peter and Fionn after a detailed read.

### Parameters

| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
|     `v`     | `std::string`  | Set of eigen vectors low and high modes |
|     `w`     | `std::string`  | Set of eigen vectors low and high modes |
|    `gammas` | `std::string`  | gamma products to insert and source and sink. |
|    `mom`    | `std::vector<std::string>` | three-momentum of the meson field, e.g `"1 0 0"`.        |
|   `output`  | `std::string`  | name of the output file that the meson field will be saved to.         |
|   `cacheBlock`| `int`        | performance tuning parameter (give values for various architectures)   |
|   `block`   |  `int`         | performance tuning parameter (give values for various architectures)   |

### Dependencies

This module only works when grid is compiled using hdf5.

This module depends on the creation of the `v` and `w` eigenvectors being determined.

Alternatively, distillation vectors $\varrho$, $\varphi$, $\phi$ can be used.

### Products

This module produces a `meson field` that is saved to a hdf5 file at a location of your choosing.
