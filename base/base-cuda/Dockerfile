FROM nvidia/cuda:9.0-devel-ubuntu16.04

RUN apt-get update && apt-get install -y \
        apt-transport-https \
        build-essential=12.1ubuntu2 \
        ca-certificates \
        curl \
        gfortran=4:5.3.1-1ubuntu1 \
        openssl \
        rsync \
        wget \
        --no-install-recommends     \
    && rm -rf /var/lib/apt/lists/*

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
# Make sure to not overwrite the existing vars used by CUDA
ENV INTELPATH           /opt/intel/compilers_and_libraries_2018.3.222/linux
ENV MKLROOT             ${INTELPATH}/mkl
ENV INTEL_LICENSE_FILE  ${INTELPATH}/licenses:/opt/intel/licenses:/root/intel/licenses
ENV CPATH               ${INTELPATH}/mkl/include
ENV NLSPATH             ${INTELPATH}/compiler/lib/intel64/locale/%l_%t/%N:${INTELPATH}/mkl/lib/intel64_lin/locale/%l_%t/%N
ENV PKG_CONFIG_PATH     ${INTELPATH}/mkl/bin/pkgconfig

ENV PATH                ${INTELPATH}/bin/intel64:${PATH}
ENV LIBRARY_PATH        ${INTELPATH}/tbb/lib/intel64_lin/gcc4.7:${INTELPATH}/compiler/lib/intel64_lin:${INTELPATH}/mkl/lib/intel64_lin:${LIBRARY_PATH}
ENV LD_LIBRARY_PATH     ${INTELPATH}/compiler/lib/intel64:${INTELPATH}/compiler/lib/intel64_lin:${INTELPATH}/tbb/lib/intel64_lin/gcc4.7:${INTELPATH}/compiler/lib/intel64_lin:${INTELPATH}/mkl/lib/intel64_lin:${LD_LIBRARY_PATH}

