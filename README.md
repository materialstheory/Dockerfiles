# Dockerfiles for DFT codes

This repository contains the dockerfiles we use to build DFT codes for use on supercomputers.

## Applications
Dockerfiles for the following codes are available:
- VASP: 5.4.4 and 5.4.4 with GPU support (Note that you need a valid license to build the image.)
- Quantum ESPRESSO: 6.2.0

## `base` images
The `base` images provide a reproducible environment to compile applications with specific versions of compilers, libraries and MPI.
Currently, we have:
- `base-gnu`: Ubuntu 16.04 with gcc/gfortran 5.4.1, MKL 2018.3, MPICH 3.1.4
- `base-cuda`: Same as above, but with nvidia/cuda:9.0-ubuntu16.04 as base image

These images can be pulled from Dockerhub at https://hub.docker.com/u/materialstheory/.

