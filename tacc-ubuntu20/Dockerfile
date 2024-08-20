# Use Ubuntu 20.04 LTS
ARG ARCH
FROM ${ARCH}ubuntu:focal

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
        less \
        lbzip2 \
        wget \
    && docker-clean

########################################
# Add mount points
########################################
RUN mkdir /scratch /work /home1 /gpfs /corral-repl /corral-tacc /data /corral-secure
