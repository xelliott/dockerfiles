FROM gcc:6.1

ENV SPACK_VERSION=0.15.4 \
    SPACK_ROOT=/opt/spack \
    FORCE_UNSAFE_CONFIGURE=1

# Install minimal dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
       autoconf \
       xsltproc \
       cmake \
       environment-modules \
       vim \
    && rm -rf /var/lib/apt/lists/*

# install spack
RUN mkdir -p $SPACK_ROOT && cd $SPACK_ROOT \
    && curl -s -L https://github.com/spack/spack/releases/download/v$SPACK_VERSION/spack-$SPACK_VERSION.tar.gz \
    | tar xz --strip 1
COPY packages.yaml $SPACK_ROOT/etc/spack/

# install libraries
RUN . $SPACK_ROOT/share/spack/setup-env.sh \
    && spack install cmake@3.13.0 \
    && spack install openmpi \
    && spack install cuda@9.1.85 \
    && spack install fftw \
    && spack install hdf5 \
    && spack clean -a

# load spack environment on login
RUN . $SPACK_ROOT/share/spack/setup-env.sh \
    && echo "source $SPACK_ROOT/share/spack/setup-env.sh" > /etc/profile.d/spack.sh \
    && spack module tcl loads cmake >> /etc/profile.d/spack.sh \
    && spack module tcl loads openmpi >> /etc/profile.d/spack.sh \
    && spack module tcl loads cuda >> /etc/profile.d/spack.sh \
    && spack module tcl loads hdf5 >> /etc/profile.d/spack.sh \
    && spack module tcl loads fftw >> /etc/profile.d/spack.sh

CMD /bin/bash -l
