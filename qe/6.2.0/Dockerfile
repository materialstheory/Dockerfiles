FROM materialstheory/base-gnu

RUN wget --quiet https://github.com/QEF/q-e/archive/qe-6.2.0.tar.gz \
   && tar -xf qe-6.2.0.tar.gz \
   && cd /q-e-qe-6.2.0 \
   && ./configure \
   && make all \
   && make install \
   && cd / \
   && rm -rf q-e-qe-6.2.0/
