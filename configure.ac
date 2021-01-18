# @(#) $Tau: core/configure.ac $

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

AC_INIT([core],[shimmer],[https://github.com/tauproject/core/issues],[tau-core],[https://github.com/tauproject/core])

AC_CONFIG_AUX_DIR([buildtools/scripts])
AC_CONFIG_MACRO_DIR([buildtools/m4])
AM_INIT_AUTOMAKE([foreign subdir-objects])
AM_MAINTAINER_MODE

AC_ARG_VAR([TAU_RELEASE], [Build as an official release of the specified track (e.g., TAU_RELEASE=shimmer)])
AS_VAR_SET([default_release], [shimmer])
AS_VAR_SET([is_release], [no])
AC_ARG_ENABLE([release],
  [  --enable-release@<:@=RELEASE@:>@
                          tag built images as official releases],
  [
    AS_IF([test $enableval = yes],
      [AS_IF([test -z "$TAU_RELEASE"], [AS_VAR_SET([is_release], [yes])])],
      [
        AS_IF([test $enableval = no],
          [AS_VAR_SET([is_release], [no])],
          [
            AS_VAR_SET([TAU_RELEASE], [$enableval])
            AS_VAR_SET([is_release], [yes])
          ]
        )
      ]
    )
  ]
)

AS_IF([test -z "$TAU_RELEASE"], [AS_VAR_SET([TAU_RELEASE], [$default_release])])
AS_IF([test $is_release = yes],
  [AS_VAR_SET([PROJECT], [tauproject])],
  [AS_VAR_SET([PROJECT], [taubuild])]
)
AC_SUBST([PROJECT])
AC_SUBST([TAU_RELEASE])

AC_MSG_CHECKING([which Debian release to build images using])
AC_ARG_VAR([TAU_CORE_DEBIAN_RELEASE], [Specify the name of the Debian release that should be used to generate images])
AC_ARG_WITH([debian-release],
  [  --with-debian-release=RELEASE
                          build against the specified Debian release],
  [AS_VAR_SET([TAU_CORE_DEBIAN_RELEASE],[$withval])]
)
AS_IF([test "$TAU_CORE_DEBIAN_RELEASE" = "yes" || test "$TAU_CORE_DEBIAN_RELEASE" = "no"],
  [AC_MSG_ERROR([you must specify a Debian release (e.g., --with-debian-release=sid)])]
)
AS_IF([test -z "$TAU_CORE_DEBIAN_RELEASE"],[AS_VAR_SET([TAU_CORE_DEBIAN_RELEASE], [stable])])
AC_MSG_RESULT([$TAU_CORE_DEBIAN_RELEASE])

AS_VAR_SET([disable_content_trust], [false])
AC_SUBST([disable_content_trust])

AC_CHECK_PROG([DOCKER], [docker], [docker])
if test -z "$DOCKER" ; then
	AC_MSG_ERROR([Docker is required to build this project])
fi

AC_CONFIG_FILES([
Makefile
docker/Makefile
docker/core/Makefile
])

AC_OUTPUT

