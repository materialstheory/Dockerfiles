FROM ubuntu:18.04

# get essential packages
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends software-properties-common apt-transport-https curl build-essential ca-certificates curl openssl rsync wget cmake g++ gfortran git libgfortran3 libgmp-dev python-dev clang libclang-dev sudo openssh-client gpg-agent python-pip python-setuptools python-configparser python-jinja2 python-mako python-tornado python-zmq python-notebook libfftw3-dev binutils libnfft3-dev python-backports-shutil-get-terminal-size zlib1g-dev less \
    && \
    apt-get autoremove --purge -y && \
    apt-get autoclean -y && \
    rm -rf /var/cache/apt/* /var/lib/apt/lists/*

ENV CMAKE_PREFIX_PATH=/usr/share/cmake \
CPATH=/usr/include:$CPATH

# MKL 2018.3-055
RUN curl https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB \
    | apt-key add - \
    && echo "deb https://apt.repos.intel.com/mkl all main" > /etc/apt/sources.list.d/intel-mkl.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends intel-mkl-2018.3-051 \
    && rm -rf /var/lib/apt/lists/*

# MPICH 3.1.4
RUN cd && wget -q http://www.mpich.org/static/downloads/3.1.4/mpich-3.1.4.tar.gz \
    && tar xf mpich-3.1.4.tar.gz \
    && cd mpich-3.1.4 \
    && ./configure --enable-fast=all,O3 --prefix=/usr \
    && make -j$(nproc) \
    && make install \
    && ldconfig \
    && cd .. \
    && rm -rf mpich-3.1.4 \
    && rm mpich-3.1.4.tar.gz

# Set paths for intel libraries, so that we can use them during docker build
ENV INTELPATH           /opt/intel/compilers_and_libraries_2018.3.222/linux
ENV MKLROOT             ${INTELPATH}/mkl
ENV INTEL_LICENSE_FILE  ${INTELPATH}/licenses:/opt/intel/licenses:/root/intel/licenses
ENV LIBRARY_PATH        ${INTELPATH}/tbb/lib/intel64_lin/gcc4.7:${INTELPATH}/compiler/lib/intel64_lin:${INTELPATH}/mkl/lib/intel64_lin:${LIBRARY_PATH}
ENV LD_LIBRARY_PATH     ${INTELPATH}/compiler/lib/intel64:${INTELPATH}/compiler/lib/intel64_lin:${INTELPATH}/tbb/lib/intel64_lin/gcc4.7:${INTELPATH}/compiler/lib/intel64_lin:${INTELPATH}/mkl/lib/intel64_lin:${LD_LIBRARY_PATH}
ENV CPATH               ${INTELPATH}/mkl/include:${CPATH}
ENV NLSPATH             ${INTELPATH}/compiler/lib/intel64/locale/%l_%t/%N:${INTELPATH}/mkl/lib/intel64_lin/locale/%l_%t/%N
ENV PATH                ${INTELPATH}/bin/intel64:${PATH}
ENV PKG_CONFIG_PATH     ${INTELPATH}/mkl/bin/pkgconfig

# BOOST
RUN cd && wget -q https://dl.bintray.com/boostorg/release/1.68.0/source/boost_1_68_0.tar.gz \
    && tar -xf boost_1_68_0.tar.gz \
    && cd boost_1_68_0 \
    && ./bootstrap.sh --prefix=/usr ⁠\
    && ./b2 \
    && ./b2 install \
    && ldconfig \
    && cd .. \
    && rm -rf boost_1_68_0 \
    && rm boost_1_68_0.tar.gz

# needed python packages that dont need manual installation
RUN pip install cython matplotlib mpi4py

# Numpy
RUN cd && git clone --branch v1.15.1 https://github.com/numpy/numpy.git numpy \
    && cd numpy \
    && python setup.py build \
    && pip install . \
    && cd .. \
    && rm -rf numpy

# Scipy
RUN cd && git clone --branch v1.1.0  https://github.com/scipy/scipy.git scipy \
    && cd scipy \
    && python setup.py build \
    && pip install . \
    && cd .. \
    && rm -rf scipy

# hdf
RUN cd && wget -q https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.10/hdf5-1.10.2/src/hdf5-1.10.2.tar.gz \
    && tar xf hdf5-1.10.2.tar.gz \
    && cd hdf5-1.10.2 \
    && ./configure --prefix=/usr --enable-fortran --enable-cxx \
    && make \
    && make install \
    && cd .. \
    && rm -rf hdf5-1.10.2 \
    && rm  hdf5-1.10.2.tar.gz

# h5py
RUN cd && wget -q https://files.pythonhosted.org/packages/74/5d/6f11a5fffc3d8884bb8d6c06abbee0b3d7c8c81bde9819979208ba823a47/h5py-2.8.0.tar.gz \
    && tar xf h5py-2.8.0.tar.gz \
    && cd h5py-2.8.0 \
    && python setup.py install \
    && cd .. \
    && rm -rf h5py-2.8.0 \
    && rm h5py-2.8.0.tar.gz
