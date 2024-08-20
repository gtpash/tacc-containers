# Use Ubuntu 22.04 LTS
ARG ARCH
FROM ${ARCH}ubuntu:jammy

########################################
# Configure ENV 
########################################
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

########################################
# Add docker-clean & entrypoint script
########################################
COPY extras/docker-clean /usr/bin/docker-clean
RUN chmod a+rx /usr/bin/docker-clean && docker-clean

COPY extras/entry.sh /entry.sh
RUN chmod a+rx /entry.sh
ENTRYPOINT [ "/entry.sh" ]

########################################
# Add OS updates
########################################
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        bison \
        build-essential \
        ca-certificates \
        curl \
        gfortran \
        gfortran-9 \
        less \
        lbzip2 \
        wget \
    && docker-clean

 # set correct version of gfortran
 RUN update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-11 11 \
     && update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-9 9 \
     && update-alternatives --set gfortran /usr/bin/gfortran-9

########################################
# Add mount points
########################################
RUN mkdir /scratch /work /home1 /gpfs /corral-repl /corral-tacc /data /corral-secure
