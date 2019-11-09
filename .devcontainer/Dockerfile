#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

FROM debian:9

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Or your actual UID, GID on Linux if not the default 1000
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Configure apt and install packages
RUN apt-get update \
    && apt-get -y install --no-install-recommends apt-utils dialog 2>&1 \
    # 
    # Verify git, process tools, lsb-release (useful for CLI installs) installed
    && apt-get -y install git procps lsb-release \
    #
    # Install C++ tools
    && apt-get -y install build-essential cmake cppcheck valgrind \
    #
    # Create a non-root user to use if preferred - see https://aka.ms/vscode-remote/containers/non-root-user.
    && groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    # [Optional] Add sudo support for non-root user
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    #
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=

RUN cd ~ \
    && git clone -b 4.0.1 https://github.com/opencv/opencv.git --depth 1

RUN cd ~/opencv \
    && mkdir build \
    && cd build \
    && cmake \
      -D BUILD_opencv_highgui=OFF \
      -D BUILD_DOCS=OFF \
      -D BUILD_EXAMPLES=OFF \
      -D BUILD_TESTS=OFF \
      -D BUILD_PERF_TESTS=OFF \
      -D CMAKE_BUILD_TYPE=Release \
      -D BUILD_opencv_calib3d=OFF \
      -D BUILD_opencv_features2d=OFF \
      -D BUILD_opencv_objdetect=OFF \
      -D BUILD_opencv_photo=OFF \
      -D BUILD_stitching=OFF \
      -D BUILD_gapi=OFF \
      -D CMAKE_INSTALL_PREFIX=/usr/local .. \
    && make \
    && make install
