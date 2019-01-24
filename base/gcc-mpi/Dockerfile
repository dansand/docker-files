FROM dealii/base:gcc-serial

#Test by Alexis.Espinosa@pawsey.org.au
#-#-#MAINTAINER luca.heltai@gmail.com
# based on work by Rene Gassmoeller and Timo Heister

USER root

RUN apt-get update && apt-get -yq install \
#    libmpich-dev \
#    mpich \
    python

#-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.
#-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.
# Installing MPICH to work on magnus (copied from pawsey/mpi-base Dockerfile

# Add aarnet mirror to speed up package update
RUN perl -p -i.orig -e \
      's/archive.ubuntu.com/mirror.aarnet.edu.au\/pub\/ubuntu\/archive/' /etc/apt/sources.list \
      && sed -i '0,/# deb-src/{s/# deb-src/deb-src/}' /etc/apt/sources.list

# Install package dependencies
RUN apt-get update \
      && apt-get install -y \
         build-essential \
         gdb \
         gfortran \
         python-minimal \
         python-dev \
         wget \
      && apt-get clean all \
      && rm -r /var/lib/apt/lists/*


### Build MPICH ###

ARG MPICH_VERSION="3.1.4"
ARG MPICH_CONFIGURE_OPTIONS="--enable-fast=all,O3 --prefix=/usr"
ARG MPICH_MAKE_OPTIONS="-j4"

WORKDIR /tmp/mpich-build

RUN wget http://www.mpich.org/static/downloads/${MPICH_VERSION}/mpich-${MPICH_VERSION}.tar.gz \
      && tar xvzf mpich-${MPICH_VERSION}.tar.gz \
      && cd mpich-${MPICH_VERSION}  \
      && ./configure ${MPICH_CONFIGURE_OPTIONS} \
      && make ${MPICH_MAKE_OPTIONS} \
      && make install \
      && ldconfig

#...........
# Test MPICH
#AEG:DidNotWork:WORKDIR /tmp/mpich-test
#AEG:DidNotWork:COPY mpich-test .
#AEG:DidNotWork:RUN sh test.sh
RUN mpiexec -n 8 /tmp/mpich-build/mpich-3.1.4/examples/cpi 2>&1 | tee log.cpi


#End of pawsey insertion
#-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.
#-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.


# Select the default compiler
ENV CC mpicc
ENV CXX mpicxx
ENV FC mpif90
ENV FF mpif77

# The base system already contains everything that is needed.
ENV HOME /home/$USER
WORKDIR $HOME
USER $USER


