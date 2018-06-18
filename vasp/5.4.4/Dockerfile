FROM materialstheory/base-gnu

# wannier90 1.2
ADD makefile.wannier90 /software/wannier90/
RUN cd /software/wannier90/ \
    && wget -q http://www.wannier.org/code/wannier90-1.2.tar.gz \
    && tar xf wannier90-1.2.tar.gz \
    && cp makefile.wannier90 wannier90-1.2/make.sys \
    && cd wannier90-1.2 \
    && make all \
    && cp libwannier.a wannier90.x ../ \
    && cd .. \
    && rm -rf wannier90-1.2 \
    && rm wannier90-1.2.tar.gz

ADD vasp.5.4.4.tar.gz /software/vasp/
ADD makefile.include /software/vasp/vasp.5.4.4/
RUN  cd /software/vasp/vasp.5.4.4/ \
     && make all \
     && rm -rf src/ build/ arch/

ENV PATH=/software/vasp/vasp.5.4.4/bin:/software/wannier90/:${PATH}
