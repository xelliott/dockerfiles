FROM silkeh/clang:14

ARG CMAKE_VERSION=3.23.2

# Install minimal dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
       autoconf \
       xsltproc \
       vim \
       libfftw3-dev \
       libhdf5-mpi-dev \
       nvidia-cuda-toolkit-gcc \
    && rm -rf /var/lib/apt/lists/*

# Install CMake
RUN wget https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-Linux-x86_64.sh \
      -q -O /tmp/cmake-install.sh \
      && chmod u+x /tmp/cmake-install.sh \
      && mkdir /usr/bin/cmake-${CMAKE_VERSION} \
      && /tmp/cmake-install.sh --skip-license --prefix=/usr/bin/cmake-${CMAKE_VERSION} \
      && rm /tmp/cmake-install.sh

ENV PATH="/usr/bin/cmake-${CMAKE_VERSION}/bin:${PATH}"

CMD /bin/bash -l
