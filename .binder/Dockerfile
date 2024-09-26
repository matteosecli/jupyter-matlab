FROM ghcr.io/mathworks-ref-arch/matlab-integration-for-jupyter/jupyter-matlab-notebook:r2024a-ubuntu22.04

# Switch to root user
USER root
ENV DEBIAN_FRONTEND="noninteractive" TZ="Etc/UTC"

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

# Switch to notebook user
USER $NB_USER
WORKDIR /home/${NB_USER}

# Install nbgitpuller and octave_kernel
RUN python -m pip install nbgitpuller octave_kernel
