FROM materialstheory/base-cuda

ADD vasp.5.4.4.tar.gz /software/vasp
ADD makefile.include /software/vasp/vasp.5.4.4/
RUN cd /software/vasp/vasp.5.4.4/ \
    && make gpu \
    && rm -rf src/ build/ arch/

ENV PATH=/software/vasp/vasp.5.4.4/bin:${PATH}

