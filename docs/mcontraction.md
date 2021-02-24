# MContraction

## Baryon

### Template structure

This module takes a `FImpl` template argument, expected to be a fermion implimentation.
 
### Description

This module computes a baryon two-point function

$$C_2 = \langle O_{B'}(n)_{\rho'} \bar{O}_{B}(m)_\rho \rangle$$

of baryon interpolators

$$O_{B'}(n)_{\rho'} = \epsilon^{a'b'c'} (P_\pm \Gamma^{A'})_{\rho' \gamma'}   {q^R_{1}}^{c'}_{\gamma'} ({q^R_{2}}^{a'}_{\alpha'} \Gamma^{B'}_{\alpha' \beta'} {q^R_{3}}^{b'}_{\beta'}),$$
$$\bar{O}_{B}(m)_\rho = \epsilon^{abc} {\bar{q}^R_{1}}^{c}_{\gamma}  (\Gamma^A P_\pm)_{\gamma \rho} ({\bar{q}^L_{2}}^{a}_{\alpha} \Gamma^B_{\alpha \beta} {\bar{q}^R_{3}}^{b}_{\beta}),$$


The quarks of the final state are given as a module input $(q^R_1,q^R_2,q^R_3) = (q_1,q_2,q_3)$.
The inintal state quarks are defined as a permuation of the final state via a shuffle $(\sigma_1,\sigma_2,\sigma_3)$, giving the initial state quarks $(q^L_1,q^L_2,q^L_3) = (q_{\sigma_1},q_{\sigma_2},q_{\sigma_3})$.

The propagators $(\Delta_1, \Delta_2 ,\Delta_3)$ are given in the order corresponding to the **final** state quarks. The initial state propagators are then given by $(\Delta^L_1, \Delta^L_2 ,\Delta^L_3) = (\Delta_{\sigma_1}, \Delta_{\sigma_2} ,\Delta_{\sigma_3})$, where $\Delta^L_i$ is inverted on the source for initial state quark $i$.

Sinks $(S_1,S_2,S_3)$ are functions for sinking the corresponding **final** state quarks.

The ouput of the module is then given by
$$\langle O_{B'}(n)_{\rho'} \bar{O}_{B}(m)_{\rho} \rangle = \epsilon^{abc}\epsilon^{a'b'c'} \Gamma^{A}_{\gamma \rho} \Gamma^{A'}_{\rho' \gamma'} \Gamma^{B}_{\alpha \beta} \Gamma^{B'}_{\alpha' \beta'}$$
$$\times \left[ + \delta^{q^R_1 q^R_2 q^R_3}_{q^L_1 q^L_2 q^L_3} S_{1}(\Delta^L_1)_{\gamma' \gamma}^{c' c} S_{2}(\Delta^L_2)_{\alpha' \alpha}^{a' a} S_{3}(\Delta^L_3)_{\beta' \beta}^{b' b} \right.$$
$$+ \delta^{q^R_2 q^R_3 q^R_1}_{q^L_1 q^L_2 q^L_3} S_{2}(\Delta^L_1)_{\alpha \gamma}^{a' c} S_{3}(\Delta^L_2)_{\beta' \alpha}^{b' a} S_{1}(\Delta^L_3)_{\gamma' \beta}^{c' b}$$
$$+ \delta^{q^R_3 q^R_1 q^R_2}_{q^L_1 q^L_2 q^L_3} S_{3}(\Delta^L_1)_{\beta' \gamma}^{b' c} S_{1}(\Delta^L_2)_{\gamma' \alpha}^{c' a} S_{2}(\Delta^L_3)_{\alpha' \beta}^{a' b}$$
$$- \delta^{q^R_1 q^R_3 q^R_2}_{q^L_1 q^L_2 q^L_3} S_{1}(\Delta^L_1)_{\gamma' \gamma}^{c' c} S_{3}(\Delta^L_2)_{\beta' \alpha}^{b' a} S_{2}(\Delta^L_3)_{\alpha' \beta}^{a' b}$$
$$- \delta^{q^R_3 q^R_2 q^R_1}_{q^L_1 q^L_2 q^L_3} S_{3}(\Delta^L_1)_{\beta' \gamma}^{b' c} S_{2}(\Delta^L_2)_{\alpha' \alpha}^{a' a} S_{1}(\Delta^L_3)_{\gamma' \beta}^{c' b}$$
$$\left. -\delta^{q^R_2 q^R_1 q^R_3}_{q^L_1 q^L_2 q^L_3} S_{2}(\Delta^L_1)_{\alpha' \gamma}^{a' c} S_{1}(\Delta^L_2)_{\gamma' \alpha}^{c' a} S_{3}(\Delta^L_3)_{\beta' \beta}^{b' b} \right]$$
where each terms corresponds to a wick contraction labelled in order 1-6.

Optionally, the module can trace the correlation function, which corresponds to multiplying it by $(P_\pm)_{\rho \rho'}$ and then summing over $\rho, \rho'$. Because the trace is done at a relatively low level, the code runs faster in this case.

When computing baryons with sums over different interpolators like the $\Lambda$-baryon
$$O_{\Lambda}(n)_\rho = \epsilon^{a'b'c'} (P_\pm \Gamma^{A'})_{\rho \gamma'} \left[  2{s}^{c'}_{\gamma'} ({u}^{a'}_{\alpha'} \Gamma^{B'}_{\alpha' \beta'} {d}^{b'}_{\beta'}) + {d}^{c'}_{\gamma'} ({u}^{a'}_{\alpha'} \Gamma^{B'}_{\alpha' \beta'} {s}^{b'}_{\beta'}) - {u}^{c'}_{\gamma'} ({d}^{a'}_{\alpha'} \Gamma^{B'}_{\alpha' \beta'} {s}^{b'}_{\beta'}) \right]$$
or the $\Sigma^0$-baryon
$$O_{\Lambda}(n)_\rho = \epsilon^{a'b'c'} (P_\pm \Gamma^{A'})_{\rho \gamma'} \left[ {d}^{c'}_{\gamma'} ({u}^{a'}_{\alpha'} \Gamma^{B'}_{\alpha' \beta'} {s}^{b'}_{\beta'}) + {u}^{c'}_{\gamma'} ({d}^{a'}_{\alpha'} \Gamma^{B'}_{\alpha' \beta'} {s}^{b'}_{\beta'}) \right]$$
the module must be run multiple times with different final states and shuffles to get all the combinations. This is 9 and 4 times for the $\Lambda$ and $\Sigma^0$ respectively. For example, the combination with initial state `uds` and a final state `sud` requires a shuffle of `231`.

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
| `quarks` | `std::string` | final state quark content - order must match order of q1,q2,q3. Each quark must be only one character e.g. `abc` |
| `shuffle` | `std::string` | defines the initial state quarks from the final state quarks - must be a permutation of `123` |
| `sinkq1` | `std::string` | module used to compute the sink of the final state quark q1|
| `sinkq2` | `std::string` | module used to compute the sink of the final state quark q2|
| `sinkq3` | `std::string` | module used to compute the sink of the final state quark q3|
| `sim_sink` | `bool` | flag for use of a simultaneous sink - `false` uses the three above sinks separatly - `true` checks `sinkq1`=`sinkq2`=`sinkq3` and uses this to sink the quarks together |
| `gammas` | `std::string` | List of pairs of pairs Gamma matrices |
| `trace` | `bool` | specifies whether a traced propagator is produced or the whole spin matrix |
| `parity` | `int` | parity - only $+1$ or $-1$ are allowed values (+1 is default, unused if `trace` = `false`) |
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on three propagators being generated and sink modules being specified.

### Products

This module produces a correlator called `baryon` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing. 

If `trace = true`, this correlator has a `Complex` value for each timeslice, if `trace = false` the correlator is a $4 \times 4$ `SpinMatrix` on each timeslice.

-----------

## BaryonGamma3pt

### Template structure

This module takes a `FImpl` template argument which is expected to be a fermion implimentation.

### Description

This module computes the baryonic three-point function

$$C_3 = \langle O_{B'}(x_f)_{\rho'} J(x_J) \bar{O}_{B}(x_i)_\rho \rangle$$
where
$$O_{B'}(x_f)_\rho = \epsilon^{a'b'c'} (P_\pm \Gamma^{A'})_{\rho \gamma'}   {q^R_{1}}^{c'}_{\gamma'} ({q^R_{2}}^{a'}_{\alpha'} \Gamma^{B'}_{\alpha' \beta'} {q^R_{3}}^{b'}_{\beta'}),$$
$$\bar{O}_{B}(x_i)_\rho = \epsilon^{abc} {\bar{q}^R_{1}}^{c}_{\gamma}  (\Gamma^A P_\pm)_{\gamma \rho} ({\bar{q}^L_{2}}^{a}_{\alpha} \Gamma^B_{\alpha \beta} {\bar{q}^R_{3}}^{b}_{\beta}),$$
$$J(x_J) = {\bar{q}^R_J}^{i}_{\sigma} \gamma {q^L_J}^{i}_{\tau}$$

The quarks for the initial and final state and the current are specified as $(q^L_1,q^L_2,q^L_3)$, $(q^R_1,q^R_2,q^R_3)$ and $(q^L_J,q^R_J)$ respectively. This represents a $q^L_J \to q^R_J$ transition.

The 3 initial propagators $(\Delta^L_1,\Delta^L_2,\Delta^L_3)$ are required and correspond to the initial quarks q1, q2 and q3. 3 final state propagators $(\Delta^R_1,\Delta^R_2,\Delta^R_3)$ can be specified. In gerneral not all of these final propagators will be required depending on the quark flavours. Any propagators not required can be left blank, however, if they are specified a message will be logged naming the propagators that are not used. 

3 sinks $(S_1, S_2, S_3)$ corresponding to the final state quarks must be secified.


The resulting contractions of the module are given by
$$\langle O_{B'}(x_f)_{\rho'} J(x_J) \bar{O}_{B}(x_i)_\rho \rangle = \epsilon^{abc} \epsilon^{a'b'c'} \Gamma^{A_R}_{\rho' \gamma'} \Gamma^{A_L}_{\gamma \rho} \Gamma^{B_R}_{\alpha' \beta'} \Gamma^{B_L}_{\alpha \beta} \gamma_{\sigma \tau} (C_1 + C_2 + C_3 )^{abc,a'b'c'}_{\alpha \beta, \alpha' \beta', \sigma \tau, \gamma \gamma'}$$

where

$$C^{\ \ \cdots}_{1\ \cdots} = \delta^{q^L_J}_{q^L_1} (\Delta^L_1)_{\tau \gamma}^{i c} \left[ - \delta_{q^R_1 q^R_2 q^R_3}^{q^R_J q^L_2 q^L_3} \ \  (\gamma_5 (\Delta^R_1)^{\dagger} \gamma_5)_{\gamma' \sigma}^{c' i} S_2(\Delta^L_2)_{\alpha' \alpha}^{a' a} S_3(\Delta^L_3)_{\beta' \beta}^{b' b} \right. $$
$$ \hspace{8em} - \delta_{q^R_2 q^R_3 q^R_1}^{q^R_J q^L_2 q^L_3} \ \  (\gamma_5 (\Delta^R_2)^{\dagger} \gamma_5)_{\alpha' \sigma}^{a' i} S_3(\Delta^L_2)_{\beta' \alpha}^{b' a} S_1(\Delta^L_3)_{\gamma' \beta}^{c' b} $$
$$\hspace{8em} - \delta_{q^R_3 q^R_1 q^R_2}^{q^R_J q^L_2 q^L_3} \ \  (\gamma_5 (\Delta^R_3)^{\dagger} \gamma_5)_{\beta' \sigma}^{b' i} S_1(\Delta^L_2)_{\gamma' \alpha}^{c' a} S_2(\Delta^L_3)_{\alpha' \beta}^{a' b} $$
$$\hspace{8em} + \delta_{q^R_1 q^R_3 q^R_2}^{q^R_J q^L_2 q^L_3} \ \  (\gamma_5 (\Delta^R_1)^{\dagger} \gamma_5)_{\gamma' \sigma}^{c' i} S_3(\Delta^L_2)_{\beta' \alpha}^{b' a} S_2(\Delta^L_3)_{\alpha' \beta}^{a' b} $$
$$\hspace{8em} + \delta_{q^R_3 q^R_2 q^R_1}^{q^R_J q^L_2 q^L_3} \ \  (\gamma_5 (\Delta^R_3)^{\dagger} \gamma_5)_{\beta' \sigma}^{b' i} S_2(\Delta^L_2)_{\alpha' \alpha}^{a' a} S_1(\Delta^L_3)_{\gamma' \beta}^{c' b} $$
$$\hspace{8em} \left. + \delta_{q^R_2 q^R_1 q^R_3}^{q^R_J q^L_2 q^L_3} \ \  (\gamma_5 (\Delta^R_2)^{\dagger} \gamma_5)_{\alpha' \sigma}^{a' i} S_1(\Delta^L_2)_{\gamma' \alpha}^{c' a} S_3(\Delta^L_3)_{\beta' \beta}^{b' b} \right] $$

$$C^{\ \ \cdots}_{2\ \cdots}  =  \delta^{q^L_J}_{q^L_2} (\Delta^L_2)_{\tau \alpha}^{i a} \left[ - \delta_{q^R_1 q^R_2 q^R_3}^{q^L_1 q^R_J q^L_3} \ \ S_1(\Delta^L_1)_{\gamma' \gamma}^{c' c} (\gamma_5 (\Delta^R_2)^{\dagger} \gamma_5)_{\alpha' \sigma}^{a' i} S_3(\Delta^L_3)_{\beta' \beta}^{b' b} \right. $$
$$\hspace{8em} - \delta_{q^R_2 q^R_3 q^R_1}^{q^L_1 q^R_J q^L_3} \ \ S_2(\Delta^L_1)_{\alpha' \gamma}^{a' c} (\gamma_5 (\Delta^R_3)^{\dagger} \gamma_5)_{\beta' \sigma}^{b' i} S_1(\Delta^L_3)_{\gamma' \beta}^{c' b}$$
$$\hspace{8em} - \delta_{q^R_3 q^R_1 q^R_2}^{q^L_1 q^R_J q^L_3} \ \ S_3(\Delta^L_1)_{\beta' \gamma}^{b' c} (\gamma_5 (\Delta^R_1)^{\dagger} \gamma_5)_{\gamma' \sigma}^{c' i} S_2(\Delta^L_3)_{\alpha' \beta}^{a' b} $$
$$\hspace{8em} + \delta_{q^R_1 q^R_3 q^R_2}^{q^L_1 q^R_J q^L_3} \ \ S_1(\Delta^L_1)_{\gamma' \gamma}^{c' c} (\gamma_5 (\Delta^R_3)^{\dagger} \gamma_5)_{\beta' \sigma}^{b' i} S_2(\Delta^L_3)_{\alpha' \beta}^{a' b}$$
$$\hspace{8em} + \delta_{q^R_3 q^R_2 q^R_1}^{q^L_1 q^R_J q^L_3} \ \ S_3(\Delta^L_1)_{\beta' \gamma}^{b' c} (\gamma_5 (\Delta^R_2)^{\dagger} \gamma_5)_{\alpha' \sigma}^{a' i} S_1(\Delta^L_3)_{\gamma' \beta}^{c' b} $$
$$\hspace{8em} \left. + \delta_{q^R_2 q^R_1 q^R_3}^{q^L_1 q^R_J q^L_3} \ \ S_2(\Delta^L_1)_{\alpha' \gamma}^{a' c} (\gamma_5 (\Delta^R_1)^{\dagger} \gamma_5)_{\gamma' \sigma}^{c' i} S_3(\Delta^L_3)_{\beta' \beta}^{b' b} \right] $$


$$C^{\ \ \cdots}_{3\ \cdots} = \delta^{q^L_J}_{q^L_3} (\Delta^L_3)_{\tau \beta}^{i b} \left[ - \delta_{q^R_1 q^R_2 q^R_3}^{q^L_1 q^L_2 q^R_J} \ \ S_1(\Delta^L_1)_{\gamma' \gamma}^{c' c} S_2(\Delta^L_2)_{\alpha' \alpha}^{a' a} (\gamma_5 (\Delta^R_3)^{\dagger} \gamma_5)_{\beta' \sigma}^{b' i} \right. $$
$$\hspace{8em} - \delta_{q^R_2 q^R_3 q^R_1}^{q^L_1 q^L_2 q^R_J} \ \ S_2(\Delta^L_1)_{\alpha' \gamma}^{a' c} S_3(\Delta^L_2)_{\beta' \alpha}^{b' a} (\gamma_5 (\Delta^R_1)^{\dagger} \gamma_5)_{\gamma' \sigma}^{c' i} $$
$$\hspace{8em} - \delta_{q^R_3 q^R_1 q^R_2}^{q^L_1 q^L_2 q^R_J} \ \ S_3(\Delta^L_1)_{\beta' \gamma}^{b' c} S_1(\Delta^L_2)_{\gamma' \alpha}^{c' a} (\gamma_5 (\Delta^R_2)^{\dagger} \gamma_5)_{\alpha' \sigma}^{a' i} $$
$$\hspace{8em} + \delta_{q^R_1 q^R_3 q^R_2}^{q^L_1 q^L_2 q^R_J} \ \ S_1(\Delta^L_1)_{\gamma' \gamma}^{c' c} S_3(\Delta^L_2)_{\beta' \alpha}^{b' a} (\gamma_5 (\Delta^R_2)^{\dagger} \gamma_5)_{\alpha' \sigma}^{a' i} $$
$$\hspace{8em} + \delta_{q^R_3 q^R_2 q^R_1}^{q^L_1 q^L_2 q^R_J} \ \ S_3(\Delta^L_1)_{\beta' \gamma}^{b' c} S_2(\Delta^L_2)_{\alpha' \alpha}^{a' a} (\gamma_5 (\Delta^R_1)^{\dagger} \gamma_5)_{\gamma' \sigma}^{c' i} $$
$$\hspace{8em} \left. + \delta_{q^R_2 q^R_1 q^R_3}^{q^L_1 q^L_2 q^R_J} \ \ S_2(\Delta^L_1)_{\alpha' \gamma}^{a' c} S_1(\Delta^L_2)_{\gamma' \alpha}^{c' a} (\gamma_5 (\Delta^R_3)^{\dagger} \gamma_5)_{\beta' \sigma}^{b' i} \right] $$
where the three groups of 6 contractions are named the wick groups 1-3. For currents where $q^L_J = q^R_J$, there is an additional disconnected part that is not included in this module.



The internal baryon diquark gamma structures `gammaLR` take the same format of the baryon 2pt function above (i.e. a list of pairs of pairs of gamma matrices).

The current gamma structure `gammaJ` is a list of gamma matrices in the usual Grid notation. Additionally the keyword `all` can be used to contract using all 16 gamma structures. 

The momentum parameter adds a 3-momentum phase to the current operator before summing over all space.

### Parameters
| Parameter   | Type   |   Description                       |
|-------------|----------------|-----------------------------|
| `quarksL` | `std::string` | initial state quark content (e.g. `abc`) |
| `quarksR` | `std::string` | final state quark content (e.g. `abd`) |
| `quarksJ` | `std::string` | current quark content (e.g. `cd` for a $c \to d$ transition) |
| `qL1` | `std::string` | initial state propagator $\Delta^L_1$ |
| `qL2` | `std::string` | initial state propagator $\Delta^L_2$ |
| `qL3` | `std::string` | initial state propagator $\Delta^L_3$ |
| `qR1` | `std::string` | final state propagator $\Delta^R_1$ |
| `qR2` | `std::string` | final state propagator $\Delta^R_2$ |
| `qR3` | `std::string` | final state propagator $\Delta^R_3$ |
| `sink1` | `std::string` | module used to compute the sink of the final state quark qR1|
| `sink2` | `std::string` | module used to compute the sink of the final state quark qR2|
| `sink3` | `std::string` | module used to compute the sink of the final state quark qR3|
| `gammaLR` | `std::string` | list of pairs of pairs Gamma matrices |
| `gammaJ` | `std::string` | list of Gamma matrices |
| `mom` | `std::string` | 3-momentum transfer of the current insertion |
| `tf` | `unsigned int` | time of the final state |
| `output` | `std::string` | Specify the output location of the correlator that is generated.|

### Dependencies

This module depends on three initial state propagators being generated, and up to three final state propagators as well as three sink functions.

### Products

This module produces a $4 \times 4$ `SpinMatrix` correlator called `baryonGamma3pt` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing. 

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

## SigmaToNucleonEye


### Template Stucture

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Computes the Sigma-to-nucleon 3pt-diagrams, eye topologies.

```
  Schematics:      qqLoop                |                  
                  /->-¬                  |                             
                 /     \                 |          qsTi      G     qdTf
                 \     /                 |        /---->------*------>----¬         
           qsTi   \   /    qdTf          |       /          /-*-¬          \
        /----->-----* *----->----¬       |      /          /  G  \          \
       *            G G           *      |     *           \     /  qqLoop  * 
       |\                        /|      |     |\           \-<-/          /|   
       | \                      / |      |     | \                        / |      
       |  \---------->---------/  |      |     |  \----------->----------/  |      
        \          quSpec        /       |      \          quSpec          /        
         \                      /        |       \                        /
          \---------->---------/         |        \----------->----------/
                   quSpec                |                 quSpec
 
  analogously to the rare-kaon naming, the left diagram is named 'one-trace' and
  the diagram on the right 'two-trace'
```

### Parameters

| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
|     `qqLoop`     | `std::string`  | $u/c$-quark loop propagator |
|     `quSpec`     | `std::string`  | already sinked $u$-quark propagator (source at $t_i$, sink at $t_f$) |
|     `qdTf`     | `std::string`  | $d$-quark propagator (source at $t_f$) |
|     `qsTi`     | `std::string`  | $s$-quark propagator (source at $t_i$) |
|    `tf` | `unsigned int`  | time $t_f$ |
|    `sink`    | `std::string` | module used to compute the sink of the final state |
|   `output`  | `std::string`  | Specify the output location of the correlator that is generated.  |

### Dependencies

This module depends on four specific propagators being generated and a sink module being specified. One of the propagators has to be sinked already (e.g. using `MSink::Smear`).

### Products

This module produces a $4 \times 4$ `SpinMatrix` correlator called `sigmaToNucleonEye` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing. The two topologies are saved using the axis label `trace = 0` or `trace = 1`.

-----------

## SigmaToNucleonNonEye


### Template Stucture

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Computes the Sigma-to-nucleon 3pt-diagrams, non-eye topologies.

```
  Schematics:     
             qsTi          quTf           |            qsTi            qdTf
           /-->--¬       /-->--¬          |          /-->--¬         /-->--¬       
          /       \     /       \         |         /       \       /       \      
         /         \   /         \        |        /         \     /         \     
        /           \ /           \       |       /           \   /           \    
       *             * G           *      |       *           G * * G          * 
      |\             * G           |      |      |\           /   \           /|
      | \           / \           /|      |      | \         /     \         / |   
      |  \         /   \         / |      |      |  \       /       \       /  |
      |   \       /     \       /  |      |      |   \-->--/         \-->--/   |   
       \   \-->--/       \-->--/  /       |       \   quTi            quTf    /
        \    quTi          qdTf  /        |        \                         /
         \                      /         |         \                       /
          \--------->----------/          |          \--------->-----------/
                  quSpec                  |                  quSpec

  analogously to the rare-kaon naming, the left diagram is named 'one-trace' and
  the diagram on the right 'two-trace'
```

### Parameters

| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
|     `quTi`     | `std::string`  | $u$-quark propagator (source at $t_i$) |
|     `quTf`     | `std::string`  | $u$-quark propagator (source at $t_f$) |
|     `quSpec`     | `std::string`  | already sinked $u$-quark propagator (source at $t_i$, sink at $t_f$) |
|     `qdTf`     | `std::string`  | $d$-quark propagator (source at $t_f$) |
|     `qsTi`     | `std::string`  | $s$-quark propagator (source at $t_i$) |
|    `tf` | `unsigned int`  | time $t_f$ |
|    `sink`    | `std::string` | module used to compute the sink of the final state |
|   `output`  | `std::string`  | Specify the output location of the correlator that is generated.  |

### Dependencies

This module depends on five specific propagators being generated and a sink module being specified. One of the propagators has to be sinked already (e.g. using `MSink::Smear`).

### Products

This module produces a $4 \times 4$ `SpinMatrix` correlator called `sigmaToNucleonNonEye` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing. The two topologies are saved using the axis label `trace = 0` or `trace = 1`.

-----------

## WeakEye3pt


### Template Stucture

One template argument `FImpl`, expected to be a fermion implementation.

### Description

Weak Hamiltonian meson 3-pt diagrams, eye topologies.

```
  Schematics:       loop                 |                  
                   /-<-¬                 |                             
                  /     \                |            qbl     G     qbr
                  \     /                |        /----<------*------<----¬         
             qbl   \   /    qbr          |       /          /-*-¬          \
        /-----<-----* *-----<----¬       |      /          /  G  \          \
   gIn *            G G           * gOut | gIn *           \     /  loop    * gOut
        \                        /       |      \           \->-/          /   
         \                      /        |       \                        /       
          \---------->---------/         |        \----------->----------/        
                    qs                   |                   qs                  
                                         |
                 one trace               |                two traces
```

One trace diagram:

$$\mathrm{Tr}(\bar{q}_r \Gamma_{\mathrm{out}} q_\mathrm{spec} \Gamma^\dagger_{\mathrm{in}} \gamma_5 \bar{q}^\dagger_l \gamma_5 \Gamma_{\mathrm{op}} q_\mathrm{loop} \Gamma_{\mathrm{op}}$$

### Parameters

| Parameter   | Type           | Description                                                            |
|-------------|----------------|------------------------------------------------------------------------|
|     `qqLoop`     | `std::string`  | $u/c$-quark loop propagator |
|     `quSpec`     | `std::string`  | already sinked $u$-quark propagator (source at $t_i$, sink at $t_f$) |
|     `qdTf`     | `std::string`  | $d$-quark propagator (source at $t_f$) |
|     `qsTi`     | `std::string`  | $s$-quark propagator (source at $t_i$) |
|    `tf` | `unsigned int`  | time $t_f$ |
|    `sink`    | `std::string` | module used to compute the sink of the final state |
|   `output`  | `std::string`  | Specify the output location of the correlator that is generated.  |

### Dependencies

This module depends on four specific propagators being generated and a sink module being specified. One of the propagators has to be sinked already (e.g. using `MSink::Smear`).

### Products

This module produces a $4 \times 4$ `SpinMatrix` correlator called `sigmaToNucleonEye` that is saved to a hdf5 file (or xml if grid is compiled without hdf5) at a location of your choosing. The two topologies are saved using the axis label `trace = 0` or `trace = 1`.


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
