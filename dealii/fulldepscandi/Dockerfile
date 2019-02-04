#-#-#FROM dealii/full-deps:fulldepscandi
FROM alexisespinosa/dealii_full-deps:fulldepscandi

#-#-#MAINTAINER luca.heltai@gmail.com

#::AEG::Adding a place to install software:
USER root
WORKDIR /software
RUN chmod 777 /software
USER $USER

# get deal.II repo
ARG VER=master
ARG BUILD_TYPE=DebugRelease
ARG SOFTDIR=/software

RUN git clone https://github.com/dealii/dealii.git dealii-$VER-src && \
    cd dealii-$VER-src && \
    git checkout $VER && \
    mkdir build && cd build && \
    cmake -DDEAL_II_WITH_MPI=ON \
          -DDEAL_II_COMPONENT_EXAMPLES=OFF \
#-#-#          -DCMAKE_INSTALL_PREFIX=~/dealii-$VER \
#::AEG::installing in the software dir
          -DCMAKE_INSTALL_PREFIX=$SOFTDIR/dealii-$VER \
          -DCMAKE_BUILD_TYPE=$BUILD_TYPE \
	  -GNinja \
          ../ && \
    ninja install && \
    ninja test && \
#-#-#    cp summary.log ~/dealii-$VER/summary.log && \
#::AEG::same thing
    cp summary.log $SOFTDIR/dealii-$VER/summary.log && \
    cd .. && rm -rf build .git 

#-#-#ENV DEAL_II_DIR ~/dealii-$VER
#::AEG:: same thing
ENV DEAL_II_DIR $SOFTDIR/dealii-$VER
WORKDIR $HOME
