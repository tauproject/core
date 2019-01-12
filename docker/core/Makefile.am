# @(#) $Tau: core/docker/core/Makefile.am $

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

dist_noinst_DATA = Dockerfile

all-local: container

# Work around <https://github.com/moby/moby/issues/34199> by first
# pulling with trust enabled, then disabling for the build itself.

container:
	$(DOCKER) pull --disable-content-trust=${disable_content_trust} docker.io/library/debian:$(TAU_CORE_DEBIAN_RELEASE)-slim
	$(DOCKER) build --disable-content-trust=true \
		--build-arg TAU_CORE_DEBIAN_RELEASE="$(TAU_CORE_DEBIAN_RELEASE)" \
		-t "$(PROJECT)/$(PACKAGE_NAME)" \
		-f "$(srcdir)/Dockerfile" \
		"$(srcdir)/.."
	$(DOCKER) tag $(PROJECT)/$(PACKAGE_NAME) $(PROJECT)/$(PACKAGE_NAME):$(TAU_RELEASE)-$(TAU_CORE_DEBIAN_RELEASE)
	$(DOCKER) tag $(PROJECT)/$(PACKAGE_NAME) $(PROJECT)/$(PACKAGE_NAME):$(TAU_RELEASE)

shell: container
	$(DOCKER) run -it $(PROJECT)/$(PACKAGE_NAME)

push:
	$(DOCKER) push --disable-content-trust=${disable_content_trust} $(PROJECT)/$(PACKAGE_NAME):$(TAU_RELEASE)-$(TAU_CORE_DEBIAN_RELEASE)
	$(DOCKER) push --disable-content-trust=${disable_content_trust} $(PROJECT)/$(PACKAGE_NAME):$(TAU_RELEASE)
	$(DOCKER) push --disable-content-trust=${disable_content_trust} $(PROJECT)/$(PACKAGE_NAME):latest

