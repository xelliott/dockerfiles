FROM gcc:6.1

ENV OMPI_DIR=/opt/ompi
ENV OMPI_VERSION=4.0.2
ENV PATH="${OMPI_DIR}/bin:${PATH}"

RUN echo "Installing Open MPI"; \
    export OMPI_URL="https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-$OMPI_VERSION.tar.bz2"; \
    mkdir -p /tmp/ompi; \
    mkdir -p /opt; \
    \
# Download
    cd /tmp/ompi && wget -O openmpi-$OMPI_VERSION.tar.bz2 $OMPI_URL && tar -xjf openmpi-$OMPI_VERSION.tar.bz2; \
# Compile and install
    cd /tmp/ompi/openmpi-$OMPI_VERSION && ./configure --prefix=$OMPI_DIR && make install; \
    # Set env variables so we can compile our application
    export PATH=$OMPI_DIR/bin:$PATH; \
    export LD_LIBRARY_PATH=$OMPI_DIR/lib:$LD_LIBRARY_PATH; \
    export MANPATH=$OMPI_DIR/share/man:$MANPATH; \
