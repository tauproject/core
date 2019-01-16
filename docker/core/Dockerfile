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

# Global environment
ENV CONTAINER docker
ENV LC_ALL C
ENV TZ=UTC

# Work within root's home directory by default
WORKDIR /root

# Configure APT
ENV DEBIAN_FRONTEND noninteractive

RUN echo 'APT::Install-Recommends "0"; \n\
APT::Install-Suggests "0"; \n\
APT::Get::Assume-Yes "true"; \n\
APT::Get::Force-Yes "true";' > /etc/apt/apt.conf.d/01tau

RUN echo "deb http://deb.debian.org/debian ${TAU_CORE_DEBIAN_RELEASE} main contrib non-free\n\
deb-src http://deb.debian.org/debian ${TAU_CORE_DEBIAN_RELEASE} main contrib non-free\n\
\n\
deb http://security.debian.org/debian-security ${TAU_CORE_DEBIAN_RELEASE}/updates main contrib non-free\n\
deb-src http://security.debian.org/debian-security ${TAU_CORE_DEBIAN_RELEASE}/updates main contrib non-free\n\
\n\
deb http://deb.debian.org/debian ${TAU_CORE_DEBIAN_RELEASE}-updates main contrib non-free\n\
deb-src http://deb.debian.org/debian ${TAU_CORE_DEBIAN_RELEASE}-updates main contrib non-free" > /etc/apt/sources.list

# Install additional packages required by almost any downstream image.
RUN apt-get update && \
	apt-get upgrade && \
	apt-get install apt-transport-https ca-certificates

# Install the Tau trust anchor
RUN mkdir -p /usr/local/share/ca-certificates
COPY tau-root-ca.g1.crt /usr/local/share/ca-certificates
RUN update-ca-certificates

# Always run an `apt-get update` before any downstream commands. This means that
# images don't need to run it themselves.

ONBUILD RUN apt-get update -qq

