# Copyright (c) 2019 Red Hat, Inc.
# This program and the accompanying materials are made
# available under the terms of the Eclipse Public License 2.0
# which is available at https://www.eclipse.org/legal/epl-2.0/
#
# SPDX-License-Identifier: GPLv-3.0
#
# Contributors:
#   Red Hat, Inc. - initial API and implementation
ARG CHE_RUNTIME_VERSION=7.1.0
ARG PYTHON_VERSION=36

FROM eclipse/che-theia-endpoint-runtime:${CHE_RUNTIME_VERSION} as endpoint
FROM registry.access.redhat.com/ubi7/python-${PYTHON_VERSION}:latest
USER root

# ANSIBLE VERSION
ENV ANSIBLE_VERSION 2.9.23

ENV HOME=/home/theia

RUN yum install -y --disableplugin=subscription-manager https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN yum install -y --disableplugin=subscription-manager nodejs sshpass

RUN	pip install --upgrade pip && \
    pip install --no-cache-dir virtualenv && \
    pip install --upgrade setuptools

RUN pip install "ansible==${ANSIBLE_VERSION}" \
	pylint \
	pyinotify \
	apache-libcloud \
	google-cloud \
	azure \
	bot
	boto3 \
	docker \
	ovirt-engine-sdk-python \
	pyvmomi \
	netaddr \
	requests \
	"idna<2.8" \
	ansible-tower-cli \
	"pexpect==4.6.0" \
	python-memcached \
	molecule \
	xmltodict \
	ncclient \
	f5-sdk \
	f5-icontrol-rest \
	passlib \
	pandevice \
	pan-python \
	avisdk \
	ansible-review \
	infoblox-client \
	jmespath \
	yaql \
	"click==6.7" \
	"colorama==0.3.9" \
	"Jinja2==2.10.1" \
	"PyYAML<6,>=5.1" \
	"six==1.11.0" 

# RUN rpm --import https://packages.microsoft.com/keys/microsoft.asc
# RUN echo -e $'[azure-cli] \n\
# name=Azure CLI \n\
# baseurl=https://packages.microsoft.com/yumrepos/azure-cli \n\
# enabled=1 \n\
# gpgcheck=1 \n\
# gpgkey=https://packages.microsoft.com/keys/microsoft.asc' | tee /etc/yum.repos.d/azure-cli.repo

# RUN yum install -y --disableplugin=subscription-manager  azure-cli
RUN curl -sL https://azurecliprod.blob.core.windows.net/rhel7_6_install.sh | bash


COPY --from=endpoint /home/theia /home/theia
COPY --from=endpoint /projects /projects
COPY --from=endpoint /etc/passwd  /etc/passwd
COPY --from=endpoint /etc/group   /etc/group
COPY entrypoint.sh /entrypoint.sh
COPY cmd.sh /cmd.sh

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
RUN echo -e '[local]\nlocalhost ansible_connection=local ansible_python_interpreter=/opt/app-root/bin/python3' > /etc/ansible/hosts


RUN chmod -R 777 ${HOME} /etc/passwd /etc/group /etc/ansible

USER node

ENTRYPOINT ["/entrypoint.sh"]
CMD ["bash", "/cmd.sh"]

