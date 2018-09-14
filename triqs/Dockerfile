FROM materialstheory/base-bionic-sci-python

# triqs
RUN cd / && mkdir triqs && mkdir source && cd source

RUN git clone https://github.com/TRIQS/cpp2py cpp2py.src \
    && mkdir -p cpp2py.build && cd cpp2py.build \
    && cmake ../cpp2py.src -DCMAKE_INSTALL_PREFIX=/triqs  -DPYTHON_LIBRARY=/usr/lib/python2.7/config-x86_64-linux-gnu/libpython2.7.so \
    && make && make install \
    && cd ../

ENV CPATH=/triqs/include:${CPATH} \
    PATH=/triqs/bin:${PATH} \
    LIBRARY_PATH=/triqs/lib:${LIBRARY_PATH} \
    LD_LIBRARY_PATH=/triqs/lib:${LD_LIBRARY_PATH} \
    PYTHONPATH=/triqs/lib/python2.7/site-packages:${PYTHONPATH} \
    CMAKE_PREFIX_PATH=/triqs/share/cmake:${CMAKE_PREFIX_PATH}

RUN git clone https://github.com/TRIQS/triqs triqs.src \
    && mkdir -p triqs.build && cd triqs.build \
    && cmake ../triqs.src -DCMAKE_INSTALL_PREFIX=/triqs \
    && make && make install \
    && cd ../

ENV TRIQS_ROOT=/triqs

# dft_tools
RUN git clone https://github.com/TRIQS/dft_tools.git dft_tools.src \
    && mkdir -p dft_tools.build && cd dft_tools.build \
    && cmake ../dft_tools.src -DCMAKE_INSTALL_PREFIX=/triqs \
    && make && make install \
    && cd ..

# cthyb
RUN git clone https://github.com/TRIQS/cthyb.git cthyb.src \
    && mkdir -p cthyb.build && cd cthyb.build \
    && cmake ../cthyb.src -DCMAKE_INSTALL_PREFIX=/triqs \
    && make && make install \
    && cd ..

# remove source
RUN cd / && rm -rf source

# create a useful work dir
RUN cd / && mkdir work && cd work
