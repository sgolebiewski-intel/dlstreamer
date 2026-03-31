# ==============================================================================
# Copyright (C) 2022 Intel Corporation
#
# SPDX-License-Identifier: MIT
# ==============================================================================

ARG DOCKER_PRIVATE_REGISTRY

FROM ${DOCKER_PRIVATE_REGISTRY}ubuntu:22.04

ARG DOCS_DIR=${HOME}

ENV DEBIAN_FRONTEND noninteractive

RUN apt update && \
    apt install -y -q --no-install-recommends \
    python3 python3-pip wget unzip xz-utils git enchant-2 \
    && apt-get clean

# Install Sphinx and required extensions
RUN pip3 install --upgrade pip \
    && pip3 install sphinx==6.2.0 sphinx-book-theme sphinxcontrib-mermaid sphinxcontrib-spelling sphinx-copybutton sphinx-tabs myst-parser jsonschema

# Copy source
COPY . ${DOCS_DIR}
WORKDIR ${DOCS_DIR}

# Download open_model_zoo and generate supported_models.rst
RUN cd ${HOME} && wget https://github.com/openvinotoolkit/open_model_zoo/archive/refs/heads/master.zip \
    && unzip master.zip && rm -f master.zip
RUN pip3 install PyYAML \
    && python3 ${DOCS_DIR}/scripts/models_table_from_yaml.py \
      --model_index=${DOCS_DIR}/scripts/all_models.yaml \
      --verified_models=${DOCS_DIR}/scripts/supported_models.json \
      --output=${DOCS_DIR}/user-guide/supported_models.rst

#ENTRYPOINT ["/usr/local/bin/sphinx-build"]
#CMD ["-b", "html", "./user-guide", "./build"]
