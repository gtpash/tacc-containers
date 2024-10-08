FROM gtpash/tacc-ubuntu22:latest

########################################
# Add OS updates and IB stack
########################################
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    gfortran \
    bison \
    ca-certificates \
    libfabric-dev \
    libibmad-dev \
    libibumad-dev \
    libibverbs-dev \
    libnuma-dev \
    librdmacm-dev \
    libxml2-dev \
    && docker-clean

########################################
# Install mpi
########################################
# Install mvapich2-2.3
ARG MAJV=2
ARG MINV=3
ARG BV=.7
ARG DIR=mvapich${MAJV}-${MAJV}.${MINV}${BV}

RUN curl http://mvapich.cse.ohio-state.edu/download/mvapich/mv${MAJV}/${DIR}.tar.gz | tar -xzf - \
    && cd ${DIR} \
    && ./configure \
       --with-device=ch3 \
       --with-ch3-rank-bits=32 \
       --enable-fortran=yes \
       --enable-cxx=yes \
       --enable-romio \
       --enable-fast=O3 \
    && make -j $(nproc --all 2>/dev/null || echo 2) \
    && make install \
    && cd ../ && rm -rf ${DIR} \
    && rm -rf /usr/local/share/doc/mvapich2 \
    && docker-clean

# Add hello world
COPY extras/hello.c /tmp/hello.c
RUN mpicc /tmp/hello.c -o /usr/local/bin/hellow \
    && rm /tmp/hello.c

# Build benchmark programs
COPY extras/install_benchmarks.sh /tmp/install_benchmarks.sh
RUN bash /tmp/install_benchmarks.sh

# Test installation
RUN MV2_SMP_USE_CMA=0 mpirun -n 2 hellow
