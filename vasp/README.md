# Dockerfiles for VASP

There are Dockerfiles for the following versions:
- 5.4.4 with wannier90 ver1.2 interface
- 5.4.4 with GPU support

## How to build and use

To build the images, you need a valid license for VASP.

1. Copy a tar file with the source code into the directory with the Dockerfile. The build process expects to unpack the source code into a directory called `vasp.5.4.4/`.

2. Then build the image using docker:
` $ docker build -t vasp-5.4.4 .`

3. To run the code, you have to get the files into a location in the container that is writable by the vasp process.
   - bind mounts with user permissions: `$ docker run -v $(pwd):/data -u $(id -u) vasp-5.4.4`
   - On some systems when using shifter, the current (scratch) directory is automatically mounted and writable.

The executables are in the `$PATH`, so they can be executed by `$ docker run vasp-5.4.4 vasp_std`.
