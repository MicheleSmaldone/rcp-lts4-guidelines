# Image for BackdoorLLM (https://github.com/bboylyg/BackdoorLLM) LoRA training and evaluation.
# The devel variant ships nvcc, which deepspeed needs to build its ops at runtime.
# Set HF_HOME to a directory on scratch when launching jobs, so models are downloaded only once.

# Base image
FROM pytorch/pytorch:2.4.1-cuda12.1-cudnn9-devel

# Install required packages
RUN apt-get update && apt-get install -y --no-install-recommends git && rm -rf /var/lib/apt/lists/*

# Install python dependencies before creating the user, so this layer is cached across users
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt

# Create your user in the container
ARG LDAP_USERNAME
ARG LDAP_UID
ARG LDAP_GROUPNAME
ARG LDAP_GID

RUN groupadd ${LDAP_GROUPNAME} --gid ${LDAP_GID}
RUN useradd -m -s /bin/bash -g ${LDAP_GROUPNAME} -u ${LDAP_UID} ${LDAP_USERNAME}

ENV HOME=/home/${LDAP_USERNAME}
RUN mkdir -p ${HOME}
# Set your user as owner of the new copied files
RUN chown -R ${LDAP_USERNAME}:${LDAP_GROUPNAME} ${HOME}

# Set the working directory in your user's home
WORKDIR ${HOME}
USER ${LDAP_USERNAME}
