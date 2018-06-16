# Quantum Espresso

There are Dockerfiles for the following versions:
- 6.2.0

The image is available on dockerhub:
```bash
docker pull materialstheory/qe-6.2.0
```

## Usage

The executables are in the `$PATH`.
One way of running `pw.x` on a file `scf.in` in the current folder is:
```bash
docker run -it --rm -v $(pwd):/data -u $(id -u) materialstheory/qe-6.2.0 \
       sh -c "cd /data && mpirun -np 2  pw.x < scf.in > scf.out"
```
Make sure that in the input file, `pseudo_dir` is set correctly so that the pseudopotentials are accessible below the current directory.