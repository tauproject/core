# @(#) $Tau: core/docker/core/Dockerfile $

## ------------------------------------------------------------------------- ##
##                          T A U    P R O J E C T                           ##
## ------------------------------------------------------------------------- ##
##                                                                           ##
##  Licensed under the Apache License, Version 2.0 (the "License");          ##
##  you may not use this file except in compliance with the License.         ##
##                                                                           ##
##  You may obtain a copy of the License at:                                 ##
##                                                                           ##
##      http://www.apache.org/licenses/LICENSE-2.0                           ##
##                                                                           ##
##  Unless required by applicable law or agreed to in writing, software      ##
##  distributed under the License is distributed on an "AS-IS" BASIS,        ##
##  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. ##
##  See the License for the specific language governing permissions and      ##
##  limitations under the License.                                           ##
##                                                                           ##
## ------------------------------------------------------------------------- ##

ARG TAU_CORE_DEBIAN_RELEASE=stable
FROM debian:${TAU_CORE_DEBIAN_RELEASE}-slim
ARG TAU_CORE_DEBIAN_RELEASE
ARG TAU_ROOT_CA_NAME=tau-root-ca.g1
ARG TAU_ROOT_CA_HASH=764e57f5
ARG TAU_PACKAGING_KEY_NAME=tau-packaging.g1
ARG TAU_PACKAGING_KEY_ID=5DB6BF53215120486D5176929BD99750BDCA4F5F

# Global environment
ENV CONTAINER docker
ENV LC_ALL C
ENV TZ=UTC

# Work within root's home directory by default
WORKDIR /root

# Install the Tau trust anchors
RUN mkdir -p /etc/ssl/certs /usr/share/ca-certificates/tau /etc/apt/trusted.gpg.d
COPY ${TAU_ROOT_CA_NAME}.cert.pem /usr/share/ca-certificates/tau/${TAU_ROOT_CA_NAME}.crt
COPY ${TAU_PACKAGING_KEY_NAME}.gpg /etc/apt/trusted.gpg.d
RUN cd /etc/ssl/certs && \
	ln -sf /usr/share/ca-certificates/tau/${TAU_ROOT_CA_NAME}.crt ${TAU_ROOT_CA_NAME}.pem && \
	ln -sf ${TAU_ROOT_CA_NAME}.pem ${TAU_ROOT_CA_HASH}.0

# Configure APT
ENV DEBIAN_FRONTEND noninteractive

RUN echo 'APT::Install-Recommends "0"; \n\
APT::Install-Suggests "0"; \n\
APT::Get::Assume-Yes "true"; \n\
' > /etc/apt/apt.conf.d/01tau && \
\
	mkdir -p /etc/apt/sources.list.d && echo > /etc/apt/sources.list && \
	echo "deb http://deb.debian.org/debian ${TAU_CORE_DEBIAN_RELEASE} main contrib non-free\n\
deb-src http://deb.debian.org/debian ${TAU_CORE_DEBIAN_RELEASE} main contrib non-free\n\
\n\
deb http://security.debian.org/debian-security ${TAU_CORE_DEBIAN_RELEASE}/updates main contrib non-free\n\
deb-src http://security.debian.org/debian-security ${TAU_CORE_DEBIAN_RELEASE}/updates main contrib non-free\n\
\n\
deb http://deb.debian.org/debian ${TAU_CORE_DEBIAN_RELEASE}-updates main contrib non-free\n\
deb-src http://deb.debian.org/debian ${TAU_CORE_DEBIAN_RELEASE}-updates main contrib non-free" > /etc/apt/sources.list.d/${TAU_CORE_DEBIAN_RELEASE}.list && \
\
	apt-get update -qq && \
	apt-get upgrade && \
	apt-get install apt-transport-https ca-certificates gnupg2 lsb-release curl && \
	\
	if ! grep -e "tau/${TAU_ROOT_CA_NAME}.crt" /etc/ca-certificates.conf >/dev/null ; then \
		printf "Adding Tau trust anchor to /etc/ca-certificates.conf\n" >&2 ; \
		echo "tau/${TAU_ROOT_CA_NAME}.crt" >> /etc/ca-certificates.conf ; \
	fi ; \
	update-ca-certificates -f -v && \
	printf "Packaging key fingerprint:\n" >&2 && \
	apt-key fingerprint ${TAU_PACKAGING_KEY_ID} && \
	printf "Root CA hash symlink contents:\n" >&2 && \
	readlink /etc/ssl/certs/${TAU_ROOT_CA_HASH}.0 && \
	printf "Root CA certificate symlink contents:\n" >&2 && \
	readlink /etc/ssl/certs/${TAU_ROOT_CA_NAME}.pem

# Always run an `apt-get update` before any downstream commands. This means that
# images don't need to run it themselves.

ONBUILD RUN apt-get update -qq

