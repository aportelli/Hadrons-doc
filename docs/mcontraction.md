# MContraction

## Baryon

## DiscLoop

$$C(t)=\sum_{\mathbf{x}}\mathrm{tr}[S(t,\mathbf{x})\gamma_{\mu}]$$

## Gamma3pt

-----------

## Meson

### Template structure

This module takes two FImpltemplate arguments FImpl1 and FImpl2, both are expected to be 
fermion implimentations.
 
### Description

This module takes a pair of propagators and a source and sink gamma structure. 

### Parameters

q1 | std::string | input propagator 1
q2 | std::string | input propagator 2
gammas | std::string | gamma products to insert and source and sink, pairs of gammas seperated by a space in round brackets.
		      Special value: `all` - perform all possible contractions.
sink | std::string | module used to compute the sink of the module.
output | std::string | Specify the output location of the correlatorthat is generated.

### Dependencies

This module depends on a pair of propagators being generated and a sink module being specified.

### Products

This module produces a correlator called `meson` that is saved to a hdf5 file at a location of your
choosing.

-----------


## Meson
