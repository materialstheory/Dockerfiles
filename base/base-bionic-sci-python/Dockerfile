FROM materialstheory/base-bionic-gnu

# need cython
RUN pip install cython

# Numpy
RUN cd && git clone --branch v1.15.1 https://github.com/numpy/numpy.git numpy
ADD site.cfg /root/numpy/site.cfg
RUN cd /root/numpy/ \
    && python setup.py install \
    && cd .. \
    && rm -rf numpy

# Scipy
RUN cd && git clone --branch v1.1.0  https://github.com/scipy/scipy.git scipy
ADD site.cfg /root/scipy/site.cfg
RUN cd /root/scipy/ \
    && python setup.py install \
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

# needed python packages that dont need manual installation
RUN pip install --no-cache-dir matplotlib mpi4py decorator jupyter ipywidgets jupyterlab 
