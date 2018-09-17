FROM base-bionic-sci-python

# set some compiler flags
ENV CFLAGS="-m64 -O3 -Wl,--no-as-needed"
ENV CXXFLAGS="-m64 -O3 -Wl,--no-as-needed"
ENV LDFLAGS="-ldl -lm"
ENV FFLAGS="-m64 -O3"

# triqs
RUN cd / && mkdir -p triqs && mkdir -p source

RUN cd /source && git clone https://github.com/TRIQS/cpp2py cpp2py.src \
    && mkdir -p cpp2py.build && cd cpp2py.build \
    && cmake ../cpp2py.src -DCMAKE_INSTALL_PREFIX=/triqs  -DPYTHON_LIBRARY=/usr/lib/python2.7/config-x86_64-linux-gnu/libpython2.7.so \
    && make && make install

ENV CPATH=/triqs/include:${CPATH} \
    PATH=/triqs/bin:${PATH} \
    LIBRARY_PATH=/triqs/lib:${LIBRARY_PATH} \
    LD_LIBRARY_PATH=/triqs/lib:${LD_LIBRARY_PATH} \
    PYTHONPATH=/triqs/lib/python2.7/site-packages:${PYTHONPATH} \
    CMAKE_PREFIX_PATH=/triqs/share/cmake:${CMAKE_PREFIX_PATH}

RUN cd /source && git clone https://github.com/TRIQS/triqs triqs.src \
    && mkdir -p triqs.build && cd triqs.build \
    && cmake ../triqs.src -DCMAKE_INSTALL_PREFIX=/triqs -DLAPACK_LIBS=/opt/intel/compilers_and_libraries_2018.3.222/linux/mkl/lib/intel64_lin/libmkl_rt.so \
    && make -j$(nproc) && make install

ENV TRIQS_ROOT=/triqs

# dft_tools
RUN cd /source && git clone https://github.com/TRIQS/dft_tools.git dft_tools.src \
    && mkdir -p dft_tools.build && cd dft_tools.build \
    && cmake ../dft_tools.src -DCMAKE_INSTALL_PREFIX=/triqs \
    && make && make install

# cthyb
RUN cd /source && git clone https://github.com/TRIQS/cthyb.git cthyb.src \
    && mkdir -p cthyb.build && cd cthyb.build \
    && cmake ../cthyb.src -DCMAKE_INSTALL_PREFIX=/triqs \
    && make -j$(nproc) && make install

# remove source
RUN cd / && rm -rf source

# create a useful work dir
RUN cd / && mkdir work && cd work

# make sure openmp does not start
ENV OMP_NUM_THREADS=1
