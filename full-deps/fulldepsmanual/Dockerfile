#-#-#FROM dealii/base:gcc-mpi
FROM dansand/dealii_base:gcc-mpi

#-#-#MAINTAINER luca.heltai@gmail.com
# based on work by Rene Gassmoeller and Timo Heister

# The base system already contains everything that is needed.
ENV HOME /home/$USER
WORKDIR $HOME
USER $USER

#::::::::::::::::::::::::::::::::::::::::::
#::AEG:: installations can not be performed into /home because this directory
#        is overwritten with pawsey's /home directory when using shifter in magnus.pawsey.org.au
#
#        Therefore, installations will be performed into /software ($SOFTDIR)
#::::::::::::::::::::::::::::::::::::::::::

#::AEG:: Creating the /software directory
USER root
WORKDIR /software
RUN chmod 777 /software
USER $USER
ENV SOFTDIR /software

#Build p4est
#ENV P4EST_VERSION 1.1
#::AEG::Consider the use of the latest version listed in:
#       https://github.com/cburstedde/p4est/releases
ENV P4EST_VERSION 2.0

WORKDIR $SOFTDIR
RUN wget http://p4est.github.io/release/p4est-$P4EST_VERSION.tar.gz && \
    wget http://www.dealii.org/developer/external-libs/p4est-setup.sh && \
    chmod +x p4est-setup.sh && \
#::AEG:: Replacing the installation dir of the tools into $SOFTDIR instead of $HOME
#-#-#    ./p4est-setup.sh p4est-$P4EST_VERSION.tar.gz ~/libs/p4est-$P4EST_VERSION && \
    ./p4est-setup.sh p4est-$P4EST_VERSION.tar.gz $SOFTDIR/libs/p4est-$P4EST_VERSION && \
    rm -rf p4est-build p4est-$P4EST_VERSION p4est-setup.sh p4est-$P4EST_VERSION.tar.gz
#::AEG:: Replacing the installation dir of the tools into $SOFTDIR instead of $HOME
#-#-#ENV P4EST_DIR $HOME/libs/p4est-$P4EST_VERSION
ENV P4EST_DIR $SOFTDIR/libs/p4est-$P4EST_VERSION

#::::::::::::::::::::::::::::::::::::::::::
#::AEG:: The replacement of "~" or "/home/dealii" or "$HOME" with "$SOFTDIR" or "/software"
#        has been indicated above with clear explicit comments.
#        For the rest of the Dockerfile, the replacement will still be performed, but no comments will
#        be indicated.
#::::::::::::::::::::::::::::::::::::::::::

#Build HDF5
ENV HDF5_VERSION 1.10.0-patch1
#::AEG::Consider the use of the latest version listed in:
#       https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.10/
#       or
#       https://support.hdfgroup.org/ftp/HDF5/current/src/
#ENV HDF5_VERSION 1.10.4

WORKDIR $SOFTDIR
#::AEG::Changing source path to "releases/hdf5-1.10" instead of "current" in order to keep using old version
#-#-#RUN wget http://www.hdfgroup.org/ftp/HDF5/current/src/hdf5-$HDF5_VERSION.tar.bz2 && \
RUN wget http://www.hdfgroup.org/ftp/HDF5/releases/hdf5-1.10/hdf5-$HDF5_VERSION/src/hdf5-$HDF5_VERSION.tar.bz2 && \
    tar xjf hdf5-$HDF5_VERSION.tar.bz2 && \
    cd hdf5-$HDF5_VERSION &&  \
    ./configure --enable-parallel \
                --enable-shared \
                --prefix=$SOFTDIR/libs/hdf5-$HDF5_VERSION/ && \
    make -j2 && make install && \
    cd $SOFTDIR && \
    rm -rf hdf5-$HDF5_VERSION hdf5-$HDF5_VERSION.tar.bz2
ENV HDF5_DIR $SOFTDIR/libs/hdf5-$HDF5_VERSION

#Build Trilinos
ENV TRILINOS_VERSION 12-8-1
#::AEG::Consider the use of the latest version listed in:
#       https://github.com/trilinos/Trilinos/releases
#ENV TRILINOS_VERSION 12-12-1

WORKDIR $SOFTDIR
RUN wget https://github.com/trilinos/Trilinos/archive/trilinos-release-$TRILINOS_VERSION.tar.gz && \
    tar xfz trilinos-release-$TRILINOS_VERSION.tar.gz && \
    mkdir Trilinos-trilinos-release-$TRILINOS_VERSION/build && \
    cd Trilinos-trilinos-release-$TRILINOS_VERSION/build && \
    cmake \
     -D BUILD_SHARED_LIBS=ON \
     -D CMAKE_BUILD_TYPE=RELEASE \
     -D CMAKE_CXX_FLAGS="-O3" \
     -D CMAKE_C_FLAGS="-O3" \
     -D CMAKE_FORTRAN_FLAGS="-O5" \
     -D CMAKE_INSTALL_PREFIX:PATH=$SOFTDIR/libs/trilinos-$TRILINOS_VERSION \
     -D CMAKE_VERBOSE_MAKEFILE=FALSE \
     -D TPL_ENABLE_Boost=OFF \
     -D TPL_ENABLE_MPI=ON \
     -D TPL_ENABLE_Netcdf:BOOL=OFF \
     -D TrilinosFramework_ENABLE_MPI:BOOL=ON \
     -D Trilinos_ASSERT_MISSING_PACKAGES:BOOL=OFF \
     -D Trilinos_ENABLE_ALL_OPTIONAL_PACKAGES:BOOL=ON \
     -D Trilinos_ENABLE_ALL_PACKAGES:BOOL=OFF \
     -D Trilinos_ENABLE_Amesos:BOOL=ON \
     -D Trilinos_ENABLE_AztecOO:BOOL=ON \
     -D Trilinos_ENABLE_Epetra:BOOL=ON \
     -D Trilinos_ENABLE_EpetraExt:BOOL=ON \
     -D Trilinos_ENABLE_Ifpack:BOOL=ON \
     -D Trilinos_ENABLE_Jpetra:BOOL=ON \
     -D Trilinos_ENABLE_Kokkos:BOOL=ON \
     -D Trilinos_ENABLE_Komplex:BOOL=ON \
     -D Trilinos_ENABLE_ML:BOOL=ON \
     -D Trilinos_ENABLE_MOOCHO:BOOL=ON \
     -D Trilinos_ENABLE_MueLu:BOOL=ON \
     -D Trilinos_ENABLE_OpenMP:BOOL=OFF \
     -D Trilinos_ENABLE_Piro:BOOL=ON \
     -D Trilinos_ENABLE_Rythmos:BOOL=ON \
     -D Trilinos_ENABLE_STK:BOOL=OFF \
     -D Trilinos_ENABLE_Sacado=ON \
     -D Trilinos_ENABLE_TESTS:BOOL=OFF \
     -D Trilinos_ENABLE_Stratimikos=ON \
     -D Trilinos_ENABLE_Teuchos:BOOL=ON \
     -D Trilinos_ENABLE_Thyra:BOOL=ON \
     -D Trilinos_ENABLE_Tpetra:BOOL=ON \
     -D Trilinos_ENABLE_TrilinosCouplings:BOOL=ON \
     -D Trilinos_EXTRA_LINK_FLAGS="-lgfortran" \
     -D Trilinos_VERBOSE_CONFIGURE=FALSE \
     .. && \
   make -j4 && make install && \
   cd $SOFTDIR && \
   rm -rf Trilinos-trilinos-release-* &&\
   rm -rf trilinos-release-*
ENV TRILINOS_DIR $SOFTDIR/libs/trilinos-$TRILINOS_VERSION

#petsc
ENV PETSC_VERSION 3.7.4
#::AEG::Consider the use of the latest version listed in:
#       http://ftp.mcs.anl.gov/pub/petsc/release-snapshots
#ENV PETSC_VERSION 3.10.3

WORKDIR $SOFTDIR
RUN wget http://ftp.mcs.anl.gov/pub/petsc/release-snapshots/petsc-lite-$PETSC_VERSION.tar.gz && \
    tar xf petsc-lite-$PETSC_VERSION.tar.gz && rm -f petsc-lite-$PETSC_VERSION.tar.gz && \
    cd petsc-$PETSC_VERSION && \
    ./configure \
	--download-fblaslapack \
	--download-hypre=1  \
	--download-scalapack \
	--download-mumps \
	--download-metis \
	--download-parmetis \
	--download-superlu \
	--download-superlu_dist \
	--prefix=$SOFTDIR/libs/petsc-$PETSC_VERSION \
	--with-clanguage=C++ \
	--with-debugging=1 \
    	--with-shared-libraries=1 \
	--with-x=0 \
    	COPTFLAGS='-O3' FOPTFLAGS='-O3' && \
    make PETSC_DIR=`pwd` PETSC_ARC=docker all &&\
    make PETSC_DIR=`pwd` PETSC_ARC=docker install && \
    make PETSC_DIR=$SOFTDIR/libs/petsc-$PETSC_VERSION PETSC_ARCH= test && \
    cd $SOFTDIR && rm -rf petsc-$PETSC_VERSION

ENV PETSC_DIR $SOFTDIR/libs/petsc-$PETSC_VERSION
ENV METIS_DIR $PETSC_DIR
ENV SCALAPACK_DIR $PETSC_DIR
ENV PARMETIS_DIR $PETSC_DIR
ENV SUPERLU_DIR $PETSC_DIR
ENV SUPERLU_DIST_DIR $PETSC_DIR
ENV MUMPS_DIR $PETSC_DIR

#slepc
ENV SLEPC_VERSION 3.7.3
#::AEG::Consider the use of the latest version listed in:
#       http://slepc.upv.es/download/
#ENV SLEPC_VERSION 3.10.1

WORKDIR $SOFTDIR
#::AEG::Changing source path to "distrib" instead of "download.php" in order to make the download to work
#-#-#RUN wget http://slepc.upv.es/download/download.php?filename=slepc-$SLEPC_VERSION.tar.gz \
RUN wget http://slepc.upv.es/download/distrib/slepc-$SLEPC_VERSION.tar.gz \
    -O slepc-$SLEPC_VERSION.tar.gz && \
    tar xfz slepc-$SLEPC_VERSION.tar.gz && rm -f slepc-$SLEPC_VERSION.rag.gz && \
    cd slepc-$SLEPC_VERSION && \
    ./configure --prefix=$SOFTDIR/libs/slepc-$SLEPC_VERSION && \
    make SLEPC_DIR=`pwd` && make SLEPC_DIR=`pwd` install && \
    cd $SOFTDIR && rm -rf slepc-$SLEPC_VERSION*

ENV SLEPC_DIR $SOFTDIR/libs/slepc-$SLEPC_VERSION

#oce
ENV OCE_VERSION 0.17.2
#::AEG::Consider the use of the latest version listed in:
#       https://github.com/tpaviot/oce/releases
#ENV OCE_VERSION 0.18.3

WORKDIR $SOFTDIR
RUN \
    wget https://github.com/tpaviot/oce/archive/OCE-$OCE_VERSION.tar.gz &&\
    tar xf OCE-$OCE_VERSION.tar.gz && rm -f OCE-$OCE_VERSION.tar.gz && \
    cd oce-OCE-$OCE_VERSION && \
    mkdir build && cd build && \
    cmake \
    -DOCE_INSTALL_PREFIX=$SOFTDIR/libs/oce-$OCE_VERSION \
    -DOCE_BUILD_SHARED_LIB:BOOL=ON \
    -DOCE_DATAEXCHANGE:BOOL=ON \
    -DOCE_DISABLE_X11:BOOL=ON \
    -DOCE_DRAW:BOOL=OFF \
    -DOCE_MODEL:BOOL=ON \
    -DOCE_MULTITHREAD_LIBRARY:STRING=NONE \
    -DOCE_OCAF:BOOL=ON \
    -DOCE_OSX_USE_COCOA:BOOL=ON \
    -DOCE_USE_TCL_TEST_FRAMEWORK:BOOL=OFF \
    -DOCE_VISUALISATION:BOOL=OFF \
    -DOCE_WITH_FREEIMAGE:BOOL=OFF \
    -DOCE_WITH_GL2PS:BOOL=OFF \
    -DOCE_WITH_OPENCL:BOOL=OFF \
    .. && \
    make -j4 && make install && \
    cd ../.. && rm -rf oce-*

ENV OPENCASCADE_DIR $SOFTDIR/libs/oce-$OCE_VERSION
ENV OCE_DIR $SOFTDIR/libs/oce-$OCE_VERSION

# Arpack
ENV ARPACK_VERSION 3.4.0
#::AEG::Consider the use of the latest version listed in:
#       https://github.com/opencollab/arpack-ng/releases
#ENV ARPACK_VERSION 3.7.0

USER root
#-#-#RUN apt-get -yq install autoconf
#::AEG:: Using the update first in order to find autoconf and adding libtool wich is needed for arpack-ng
RUN apt-get update \
 && apt-get -yq install autoconf \
    libtool
USER $USER

WORKDIR $SOFTDIR
RUN \
    wget https://github.com/opencollab/arpack-ng/archive/$ARPACK_VERSION.tar.gz &&\
    tar xf $ARPACK_VERSION.tar.gz && rm -f $ARPACK_VERSION.tar.gz &&\
    cd arpack-ng-$ARPACK_VERSION && \
    ./bootstrap && ./configure --prefix=$SOFTDIR/libs/arpack-$ARPACK_VERSION --enable-mpi &&\
    make -j4 install &&\
    cd .. && rm -rf arpack-ng-*

ENV ARPACK_DIR $SOFTDIR/libs/arpack-$ARPACK_VERSION

# gsl
USER root
RUN apt-get -yq install gsl-bin libgsl0-dev
USER $USER
