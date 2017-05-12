# FourQ on MSP430

This is a state-of-the-art, 128-bit secure ECC library based on the elliptic curve FourQ [1] for 16-bit MSP430 microcontrollers.

The library was built upon **FourQlib** (https://github.com/Microsoft/FourQlib). 

The current version contains MSP430 assembly code that is only compatible with the IAR C compiler.

The library was written by Zhe Liu (zhelu.liu@uwaterloo.ca), Geovandro Pereira (geovandro.pereira@uwaterloo.ca) and 
Hwajeong Seo (hwajeong84@gmail.com).
 
## Contents

* [`iar-ide`](iar-ide/): project files for compilation with the IAR Workbench.
* [`License.txt`](License.txt): MIT License file.
* [`README.md`](README.md): this readme file.

The source folder `src` contains:
* Main .c and .h files: library and header files. Public API for ECC scalar multiplication, key exchange and signatures 
is in [`src/FourQ_api.h`](src/FourQ_api.h).        
* [`src/MSP430/`](src/MSP430/): folder with library files implementing low-level arithmetic for MSP430.
* [`src/blake2b/`](src/blake2b/): folder with implementation of hash function BLAKE2b.
* [`src/random/`](src/random/): folder with pseudo-random generation function (ONLY FOR TESTING).
* [`src/tests/`](src/tests/): test files for AVR.

### IMPORTANT SECURITY NOTE

Random values are generated with `rand()`. This is NOT a cryptographically secure function.
Users should replace this function with a cryptographically-secure PRNG (see [`random.c`](src/random/random.c)) .

### Complementary cryptographic functions

The library includes an implementation of BLAKE2b which is used by default by SchnorrQ signatures (see [`blake2b/`](src/blake2b/)).

Users can provide their own hash implementations by replacing the functions in [`blake2b/`](src/blake2b/), and applying the corresponding changes to the settings in [`FourQ.h`](src/FourQ.h). 
Refer to [2] for the security requirements for the cryptographic hash function.

## Main features
   
* Support for co-factor Elliptic Curve Diffie-Hellman (ECDH) key exchange [3].
* Support for the SchnorrQ digital signature scheme [2]. 
* Support for 3 core elliptic curve operations: variable-base, fixed-base and double-scalar multiplications.
* Includes an optimized implementation for 16-bit MSP430 microcontrollers with support for the IAR C compiler [5].
* Includes testing and benchmarking code for field arithmetic, elliptic curve and cryptographic functions. 
* All functions evaluating secret data have regular, constant-time execution, protecting against timing and cache attacks.
* Includes an option to disable the use of the fast endomorphisms.

## Instructions

Download the IAR Workbench for MSP430 (https://www.iar.com/iar-embedded-workbench/).

Open the project file [`microFourQ-MSP.eww`](iar-ide/microFourQ-MSP.eww) and click on `Project > Rebuild All`.

Project settings can be accessed and modified by going to `Project > Options...`. 

## License
   
This library is licensed under the MIT License; see [`License.txt`](License.txt) for details.

It is based on the Microsoft library **FourQlib** (https://github.com/Microsoft/FourQlib), which is also licensed under MIT.

The BLAKE2b implementation, written by Thomas Pornin, is under an MIT-like open source license (see [`blake.c`](src/blake2b/blake.c)).
 
# References

[1]   Craig Costello and Patrick Longa, "FourQ: four-dimensional decompositions on a Q-curve over the Mersenne prime". Advances in Cryptology - ASIACRYPT 2015, 2015. 
The extended version is available [`here`](http://eprint.iacr.org/2015/565).

[2]   Craig Costello and Patrick Longa. "SchnorrQ: Schnorr signatures on FourQ". MSR Technical Report, 2016. 
Available [`here`](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/07/SchnorrQ.pdf).

[3]   Watson Ladd, Patrick Longa and Richard Barnes, "Curve4Q". Internet-Draft, draft-ladd-cfrg-4q-01, 2017.
Available [`here`](https://www.ietf.org/id/draft-ladd-cfrg-4q-01.txt).

[4]   Patrick Longa, "FourQNEON: faster elliptic curve scalar multiplications on ARM processors". Selected Areas in Cryptography (SAC 2016), 2016.
Preprint available [`here`](http://eprint.iacr.org/2016/645).

[5]   Zhe Liu, Patrick Longa, Geovandro Pereira, Oscar Reparaz and Hwajeong Seo, "FourQ on embedded devices with strong countermeasures against side-channel attacks".
Preprint to be available soon.
