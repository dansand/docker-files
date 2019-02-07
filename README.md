# Docker files to create Docker images with deal.II  

[![Build Status](https://travis-ci.org/dealii/docker-files.svg?branch=master)](https://travis-ci.org/dealii/docker-files)


to build Dealii image, with mpich (from Alexis Espinosa)

at docker-files/base/gcc-mpi:

```
docker build -t dansand/dealii_base:gcc-mpi .
```

at docker-files/full-deps/fulldepsmanual

```
docker build -t dansand/dealii_full-deps:fulldepsmanual .
```
(This one is the one you want to modify/adapt in order to use the right versions of the tools needed. At least you should modify the part for the P4EST to use the latest version (2.0).

-----

at docker-files/dealii/fulldepsmanual
```
docker build -t dansand/dealii:v9.1.0-pre-gcc-mpichpawsey-fulldepsmanual .
```





Started from @tjhei, edited by @luca-heltai

For each directory in this repository, one (or more) images are built either on dockerhub, or on travis.

In particular

- ubuntu16
- base

contain the startup OS and a minimal set of packages to compile serial builds. Both images are built
automatically on dockerhub:

- https://hub.docker.com/r/dealii/ubuntu16/
- https://hub.docker.com/r/dealii/base/

The directory full-deps contain docker images that download and install all external dependencies
for deal.II, using two possible different methods:

- fulldepscandi  (gcc based)
- fulldepsmanual (clang based)

The building of these images is very expensive, and it is done manually, when important updates
are available on the external libraries.

The directory dealii, finally, contains the Dockerfiles to build the actual deal.II library images,
in particular we build several different types of deal.II images, each on its own tag. Tags are named
based on the type of base system that is used to bootstrap the build:

- bare
- fulldepscandi
- fulldepsmanual

And for each of these, build types *Debug*, *Release*, or *DebugRelease* versions of the library are
built, with the final images stored in

- dealii/dealii:ver-compiler-serialormpi-depstype-buildtype

You can pull any of the above image from dockerhub using (for example)

    docker pull dealii/dealii:v8.4.2-clang-mpi-fulldepsmanual-debugrelease

after which you could run an interactive shell in it:

    docker run -i -t dealii/dealii:v8.4.2-clang-mpi-fulldepsmanual-debugrelease

This will drop you in an isolated environment where you can experiment with deal.II.

A list of all available images is here: https://hub.docker.com/r/dealii/dealii/tags/

Notice that if a debug build is present in the image, then you'll also have access to
deal.II source files.

All images are built by the user `dealii`.
