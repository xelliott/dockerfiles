FROM silkeh/clang:14

ARG CMAKE_VERSION=3.23.2
ARG CUDA_VERSION=11-5
ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Install minimal dependencies
RUN \
    sed -i -e's/ main/ main contrib non-free/g' /etc/apt/sources.list  \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
       ninja-build \
       sudo \
       less \
       git \
       autoconf \
       xsltproc \
       vim \
       build-essential \
       libfftw3-dev \
       libhdf5-mpi-dev \
    && rm -rf /var/lib/apt/lists/*
RUN \
    wget https://developer.download.nvidia.com/compute/cuda/repos/debian11/x86_64/cuda-keyring_1.0-1_all.deb \
    dpkg -i cuda-keyring_1.0-1_all.deb \
    add-apt-repository contrib \
    apt-get update \
    apt-get install -y cuda-${CUDA_VERSION} \
    && rm -rf /var/lib/apt/lists/*

# Install CMake
RUN wget https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-Linux-x86_64.sh \
      -q -O /tmp/cmake-install.sh \
      && chmod u+x /tmp/cmake-install.sh \
      && mkdir /usr/bin/cmake-${CMAKE_VERSION} \
      && /tmp/cmake-install.sh --skip-license --prefix=/usr/bin/cmake-${CMAKE_VERSION} \
      && rm /tmp/cmake-install.sh

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

ENV PATH="/usr/bin/cmake-${CMAKE_VERSION}/bin:${PATH}"

USER $USERNAME

CMD /bin/bash -l
