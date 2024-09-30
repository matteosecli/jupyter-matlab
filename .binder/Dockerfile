FROM ghcr.io/mathworks-ref-arch/matlab-integration-for-jupyter/jupyter-matlab-notebook:r2024a-ubuntu22.04

# Switch to root user
USER root
ENV DEBIAN_FRONTEND="noninteractive" TZ="UTC"

# Install Octave
RUN export DEBIAN_FRONTEND=noninteractive && apt-get update \
    && apt-get install -y software-properties-common \
    && add-apt-repository ppa:ubuntuhandbook1/octave -y \
    && apt-get update \
    && apt-get install -y \
    octave octave-dev octave-doc \
    libgl1-mesa-dev \
    texlive texlive-latex-extra \
    dvipng dvisvgm

# Set entrypoint
RUN echo '#!/usr/bin/env bash' > /start && \
    echo 'xvfb-run "$@"' >> /start && \
    chmod +x /start
ENTRYPOINT ["/start"]

# Switch to notebook user
USER $NB_USER
WORKDIR /home/${NB_USER}

# Install nbgitpuller and octave_kernel
RUN python -m pip install nbgitpuller octave_kernel

# Install Octave extensions
RUN octave-cli --eval 'pkg install -forge tablicious'

# Switch to main octave executable ('octave-cli' doesn't have 'qt')
ENV OCTAVE_EXECUTABLE=octave

# Install Julia
RUN wget -q -O - https://install.julialang.org | sh -s -- -y

# Installa Julia kernel
RUN source /home/${NB_USER}/.profile && julia -e 'using Pkg; Pkg.add(["IJulia","LinearAlgebra","SparseArrays","Arpack","DifferentialEquations","Plots","KrylovKit","BenchmarkTools"]); using IJulia'