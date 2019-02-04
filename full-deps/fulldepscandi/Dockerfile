#-#-#FROM dealii/base:gcc-mpi
FROM alexisespinosa/dealii_base:gcc-mpi

#-#-#MAINTAINER timo.heister@gmail.com

MAINTAINER timo.heister@gmail.com

USER root
RUN apt-get update && apt-get -yq install \
    gmsh

#::AEG::Adding a place to install software:
USER root
WORKDIR /software
RUN chmod 777 /software
USER $USER
ENV SOFTDIR /software

#::AEG:: Replacing the installation dir of the tools outside $HOME and into $SOFTDIR
#-#-#ADD candi/ $HOME/candi/
ADD candi/ $SOFTDIR/candi/
#-#-#RUN chown -R $USER:$USER $HOME/candi/
USER $USER

#::::::::::::::::::::::::::::::::::::::::::
#::AEG:: Now the replacement of $HOME with $SOFTDIR will not be indicated with comments,
#        but will be perfoformed as /home is wiped out when using shifter in magnus.pawsey.org.au
#::::::::::::::::::::::::::::::::::::::::::

RUN \
  cd $SOFTDIR/candi && \
  ./candi.sh -j4 \
	--platform=deal.II-toolchain/platforms/supported/ubuntu16.platform \
	--prefix=$SOFTDIR/libs \
	--packages="load:dealii-prepare once:arpack-ng once:opencascade once:parmetis once:superlu_dist once:hdf5 once:p4est once:trilinos once:petsc once:slepc once:assimp once:nanoflann once:sundials once:adolc" &&\
  rm -rf $SOFTDIR/candi $SOFTDIR/libs/tmp

# I don't know how to import env vars from the configuration folder.  This
# probably doesn't work at all, because we don't use bash but sh and so we can
# not use 'source' or any of the bash startup files. Manually export the paths
# here instead:
RUN cat $SOFTDIR/libs/configuration/* | grep export | sort

ENV ADOLC_DIR=$SOFTDIR/libs/adolc-2.6.4-rc1
ENV ARPACK_DIR=$SOFTDIR/libs/arpack-ng-3.6.2
ENV ASSIMP_DIR=$SOFTDIR/libs/assimp-3.3.1
ENV HDF5_DIR=$SOFTDIR/libs/hdf5-1.10.1
ENV METIS_DIR=$SOFTDIR/libs/parmetis-4.0.3
ENV NANOFLANN_DIR=$SOFTDIR/libs/nanoflann-v1.2.3
ENV OPENCASCADE_DIR=$SOFTDIR/libs/oce-OCE-0.18.2
ENV P4EST_DIR=$SOFTDIR/libs/p4est-2.0
ENV PARMETIS_DIR=$SOFTDIR/libs/parmetis-4.0.3
ENV PETSC_DIR=$SOFTDIR/libs/petsc-3.7.6
ENV SLEPC_DIR=$SOFTDIR/libs/slepc-3.7.3
ENV SUNDIALS_DIR=$SOFTDIR/libs/sundials-3.1.0
ENV SUPERLU_DIR=$SOFTDIR/libs/superlu_dist_5.1.2
ENV TRILINOS_DIR=$SOFTDIR/libs/trilinos-release-12-10-1
ENV SCALAPACK_DIR=$SOFTDIR/libs/petsc-3.7.6
