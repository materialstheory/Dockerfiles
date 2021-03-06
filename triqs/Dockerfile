FROM materialstheory/base-bionic-sci-python

# set some compiler flags
ENV CFLAGS="-m64 -O3 -Wl,--no-as-needed"
ENV CXXFLAGS="-m64 -O3 -Wl,--no-as-needed"
ENV LDFLAGS="-ldl -lm"
ENV FFLAGS="-m64 -O3"

# triqs
RUN cd / && mkdir -p triqs && mkdir -p source

ENV CPATH=/triqs/include:${CPATH} \
    PATH=/triqs/bin:${PATH} \
    LIBRARY_PATH=/triqs/lib:${LIBRARY_PATH} \
    LD_LIBRARY_PATH=/triqs/lib:${LD_LIBRARY_PATH} \
    PYTHONPATH=/triqs/lib/python2.7/site-packages:${PYTHONPATH} \
    CMAKE_PREFIX_PATH=/triqs/share/cmake:${CMAKE_PREFIX_PATH}

RUN cd /source && git clone -b 2.2.x https://github.com/TRIQS/triqs triqs.src \
    && mkdir -p triqs.build && cd triqs.build \
    && cmake ../triqs.src -DCMAKE_INSTALL_PREFIX=/triqs -DLAPACK_LIBS=/opt/intel/compilers_and_libraries_2018.3.222/linux/mkl/lib/intel64_lin/libmkl_rt.so \
    && make -j$(nproc) && make install

ENV TRIQS_ROOT=/triqs

# dft_tools
RUN cd /source && git clone -b 2.2.x https://github.com/TRIQS/dft_tools.git dft_tools.src \
    && mkdir -p dft_tools.build && cd dft_tools.build \
    && cmake ../dft_tools.src -DCMAKE_INSTALL_PREFIX=/triqs \
    && make -j$(nproc) && make install

# cthyb
RUN cd /source && git clone -b 2.2.x https://github.com/TRIQS/cthyb.git cthyb.src \
    && mkdir -p cthyb.build && cd cthyb.build \
    && cmake ../cthyb.src -DCMAKE_INSTALL_PREFIX=/triqs \
    && make -j$(nproc) && make install

# maxent
RUN cd /source && git clone https://github.com/TRIQS/maxent.git maxent.src \
    && mkdir -p maxent.build && cd maxent.build \
    && cmake ../maxent.src -DCMAKE_INSTALL_PREFIX=/triqs \
    && make -j$(nproc) && make install

# tprf
RUN cd /source && git clone https://github.com/TRIQS/tprf.git tprf.src \
    && mkdir -p tprf.build && cd tprf.build \
    && cmake ../tprf.src -DCMAKE_INSTALL_PREFIX=/triqs \
    && make -j$(nproc) && make install

# PyEd Exact diagonalization solver 
RUN cd / && git clone https://github.com/HugoStrand/pyed.git
ENV PYTHONPATH=/pyed:$PYTHONPATH 

# download solidmft inside container 
RUN cd / && git clone https://github.com/materialstheory/soliDMFT.git

# remove source
RUN cd / && rm -rf source

# create a useful work dir
RUN cd / && mkdir work && cd work

# make sure openmp does not start
ENV OMP_NUM_THREADS=1
ENV OMPI_MCA_btl_vader_single_copy_mechanism=none 


# expose port for jupyter
EXPOSE 8378

COPY entrypoint.sh /usr/local/bin/entrypoint.sh
COPY jupyter_start.sh /usr/local/bin/jupyter.sh

RUN ["chmod", "+x", "/usr/local/bin/entrypoint.sh"]
RUN ["chmod", "+x", "/usr/local/bin/jupyter.sh"]

# change user and group id to match host machine if options are passed accordingly
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
