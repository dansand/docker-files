#-#-#FROM dealii/full-deps:fulldepsmanual
FROM dansand/dealii_full-deps:fulldepsmanual

#-#-#MAINTAINER luca.heltai@gmail.com

# get deal.II repo
ARG VER=master
ARG BUILD_TYPE=Release
ARG SOFTDIR=/software

WORKDIR $SOFTDIR
RUN git clone https://github.com/dealii/dealii.git dealii-$VER-src && \
    cd dealii-$VER-src && \
    git checkout $VER && \
    mkdir build && cd build && \
    cmake -DDEAL_II_WITH_MPI=ON \
          -DDEAL_II_COMPONENT_EXAMPLES=OFF \
#::AEG:: Replacing the installation dir of the tools into $SOFTDIR instead of $HOME
#-#-#          -DCMAKE_INSTALL_PREFIX=~/dealii-$VER \
          -DCMAKE_INSTALL_PREFIX=$SOFTDIR/dealii-$VER \
          -DCMAKE_BUILD_TYPE=$BUILD_TYPE \
	  -GNinja \
          ../ && \
    ninja install && \
    ninja test && \
#::AEG:: Replacing the installation dir of the tools into $SOFTDIR instead of $HOME
#-#-#    cp summary.log ~/dealii-$VER/summary.log && \
    cp summary.log $SOFTDIR/dealii-$VER/summary.log && \
    cd .. && rm -rf build .git

#::AEG:: Replacing the installation dir of the tools into $SOFTDIR instead of $HOME
#-#-#ENV DEAL_II_DIR ~/dealii-$VER
ENV DEAL_II_DIR $SOFTDIR/dealii-$VER

WORKDIR $HOME
