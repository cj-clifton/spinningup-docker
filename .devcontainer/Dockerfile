FROM nvidia/cudagl:10.2-devel-ubuntu16.04

ARG USERNAME=engineering
ARG INSTALL_ZSH="false"
ARG UPGRADE_PACKAGES="false"
ARG INSTALL_OH_MYS="false"
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG GIT_CLONE="git clone --depth 10"

ENV LANG C.UTF-8

COPY library-scripts/*.sh /tmp/library-scripts/

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "${INSTALL_OH_MYS}" \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts

RUN apt-get update && \
    apt-get install \
        libopenmpi-dev git --yes \
        freeglut3-dev \
        mesa-utils \
        libgl1-mesa-glx \
        git 

USER engineering

ARG CONDA_INSTALL_PATH=/home/${USERNAME}/miniconda3
ENV PATH=${CONDA_INSTALL_PATH}/bin:${PATH}

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p ${CONDA_INSTALL_PATH} && \
    rm ~/miniconda.sh && \
    ${CONDA_INSTALL_PATH}/bin/conda clean -tipsy && \
    echo ". ${CONDA_INSTALL_PATH}/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc && \
    find ${CONDA_INSTALL_PATH}/ -follow -type f -name '*.a' -delete && \
    find ${CONDA_INSTALL_PATH}/ -follow -type f -name '*.js.map' -delete && \
    ${CONDA_INSTALL_PATH}/bin/conda clean -afy

RUN ${CONDA_INSTALL_PATH}/bin/conda create -n spinup python=3.7.3

WORKDIR /home/engineering
RUN git clone https://github.com/openai/spinningup.git
WORKDIR /home/engineering/spinningup
RUN /bin/bash -c "source activate spinup && pip install -e ."
