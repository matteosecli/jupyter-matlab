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

# Create xvfb-run-wrapped octave
RUN echo '#!/usr/bin/env bash' > /usr/bin/octave-x && \
    echo 'xvfb-run -a /usr/bin/octave "$@"' >> /usr/bin/octave-x && \
    chmod +x /usr/bin/octave-x

# Switch to notebook user
USER $NB_USER
WORKDIR /home/${NB_USER}

# Install nbgitpuller and octave_kernel
RUN python -m pip install nbgitpuller octave_kernel

# Install Octave extensions
RUN octave-cli --eval 'pkg install -forge tablicious'

# Switch to main octave executable ('octave-cli' doesn't have 'qt')
ENV OCTAVE_EXECUTABLE=octave-x

# Install Julia
RUN wget -q -O - https://install.julialang.org | sh -s -- -y

# Installa Julia kernel
RUN source /home/${NB_USER}/.profile && julia -e 'using Pkg; Pkg.add(["IJulia","LinearAlgebra","SparseArrays","Arpack","DifferentialEquations","Plots","KrylovKit","BenchmarkTools"]); using IJulia'
