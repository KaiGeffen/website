---
slug: lisbon
title: Reflection on ZK Hack & ZK Summit 
authors: [paul]
tags: [zkhack, zksummit, lisbon]
---

After two weeks in Lisbon for ZK Hack & ZK Summit, there are a few research threads I'd like to highlight. 

## Folding Schemes: Nova, Supernova, and Sangria
[Video Reference](https://www.youtube.com/live/YwSGyNr_yUU?feature=share&t=25164) <br/>
[Blog Reference](https://geometry.xyz/notebook/sangria-a-folding-scheme-for-plonk)

This is perhaps the biggest emerging idea in zk research right now. 
These schemes achieve Incrementally Verifiable Computation (IVC) by *combining instances that haven't been proven*. 
This contrasts with the more familiar approach of proving each instance and then rolling up the results. 

Nova introduced a technique for combining R1CS instances. 
Supernova extended that idea to [TODO]. 
Sangria uses the same technique in order to combine PLONK instances. 
A natural research question arises: can we apply this approach to AIR-FRI STARKs? 
The answer isn't obvious, as this technique depends on a homomorphic encryption scheme such as KZG. 

## The Red Wedding 
[Video Reference](https://youtu.be/YwSGyNr_yUU?t=21322) <br/>

This idea proposes to reduce the verification time in a STARK system by shifting work from the Verifier to the Prover. 
Specifically, the Prover can generate a SNARK proof in order to alleviate the need for the Verifier to evaluate the AIR constraints at the DEEP query point. 

## LogUp: Batched Lookup Arguments
Video Reference coming soon <br/>
[ePrint Reference](https://eprint.iacr.org/2022/1530.pdf)

About 30% of the witness-generation in RISC Zero is spent generating columns for a PLOOKUP-based lookup argument. 
LogUp offers a ~50% reduction in the number of columns necessary in lookup tables. 

## The Mersenne Prime $2^{31}-1$
[Video Reference](https://www.youtube.com/watch?v=giFA3UXbu_s&t=305s) <br/>
[Slides](https://drive.google.com/file/d/1JaoqFUARyUXuaz4PBUG0R64_Yuzc5O-_/view?usp=sharing)

In Denver, Daniel Lubarov discussed the idea of using the finite field of order $2^{31} - 1$ for building Plonky3.
This field is particularly nice for handling finite field arithmetic, since $2^{31} = 1$. 
The major obstacle is that the multiplicative group of this field isn't good for NTTs, since $2^{31} - 2$ doesn't have many 2s in its prime factorization. 

There are a number of options for how to make this work: 
- changing from a FRI-based commitment scheme to one based on Orion or Breakdown
- using a mixed-radix version of FRI that doesn't depend on high 2-adicity
- doing NTTs in a quadratic extension field (note that the multiplicative group of the quadratic extension has order $2^{62} - 2^{32}$)

In playing around with this field, I found that $f(x) = x^2 - x - 1$ is irreducible in this field. Appending a root of $f$ (call it $g$) to $\mathbb{F}_p$ gives a quadratic extension field $\mathbb{F}_p[g]$, where $g$ has multiplicative order $2^{32}$ and powers of $g$ can be expressed using the Fibonacci sequence: 
$g^n = F_{n-1} + g\cdot F_n$. 

Looking forward to seeing what the Polygon Zero team is brewing up on this front. 

## Thanks!
Big thanks to the ZK Hack & ZK Summit teams for bringing together this fantastic collection of people and ideas. 