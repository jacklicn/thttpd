#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

# Copyright � 1995,1998 by Jef Poskanzer <jef@mail.acme.com>.
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in the
#    documentation and/or other materials provided with the distribution.
# 
# THIS SOFTWARE IS PROVIDED BY THE AUTHOR AND CONTRIBUTORS ``AS IS'' AND
# ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED.  IN NO EVENT SHALL THE AUTHOR OR CONTRIBUTORS BE LIABLE
# FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
# DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
# OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
# HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
# OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
# SUCH DAMAGE.

AC_PREREQ([2.63])
AC_INIT([thttpd], [2.26.0], [thttpd])

AC_CONFIG_SRCDIR([thttpd.c])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_AUX_DIR(build-aux)
AC_CONFIG_HEADERS([autoconf.h])

AM_INIT_AUTOMAKE(AC_PACKAGE_NAME, AC_PACKAGE_VERSION)

AC_CANONICAL_HOST

# Checks for programs.
AC_PROG_CC
AC_PROG_CC_STDC
AC_PROG_CXX
AC_PROG_CPP
AC_PROG_MAKE_SET
AC_PROG_INSTALL
AC_PROG_LIBTOOL

# Checks for libraries.
AC_CHECK_LIB(inet6, getaddrinfo)
AC_CHECK_LIB(crypt, crypt)

# Checks for header files.
AC_CHECK_HEADERS([fcntl.h grp.h memory.h dirent.h sys/ndir.h sys/dir.h ndir.h paths.h poll.h sys/poll.h sys/devpoll.h sys/event.h osreldate.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_SIZE_T
AC_TYPE_INT64_T
AC_SYS_LARGEFILE
AC_SYS_LONG_FILE_NAMES

AC_CHECK_MEMBERS([struct tm.tm_gmtoff],AC_DEFINE(HAVE_TM_GMTOFF,1,[struct tm has tm_gmtoff member]),,[
#if TM_IN_SYS_TIME
#if HAVE_SYS_TIME_H
#include <sys/time.h>
#endif
#elif HAVE_TIME_H
#include <time.h>
#endif
])

AC_CHECK_TYPE(socklen_t,AC_DEFINE(HAVE_SOCKLEN_T,1,[socklen_t is defined by the system headers]),,[
#include <sys/types.h>
#include <sys/socket.h>
#if STDC_HEADERS
#include <stdlib.h>
#include <stddef.h>
#endif])

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_REALLOC

AC_CHECK_FUNCS([waitpid vsnprintf daemon gethostbyname socket poll select gettimeofday setsid setlogin getaddrinfo getnameinfo gai_strerror sigset atoll])

# if kqueue works
AC_CACHE_CHECK([if kqueue works], [ac_cv_kqueue_works],
  AC_TRY_RUN([
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/event.h>
#include <sys/time.h>
int
main(int argc, char **argv)
{
    int fd;

    fd = kqueue();
    if (fd < 0) {
        perror("kqueue:");
        exit(1);
    }
    exit(0);
}
  ], [ac_cv_kqueue_works=yes], [ac_cv_kqueue_works=no]))
AS_IF([test "x$ac_cv_kqueue_works" = "xyes"],
  [AC_DEFINE([HAVE_KQUEUE], [1], [Define to 1 if kqueue is supported])], [])

# if epoll works
AC_CACHE_CHECK([if epoll works], [ac_cv_epoll_works],
  AC_TRY_RUN([
#include <stdio.h>
#include <stdlib.h>
#include <sys/epoll.h>
int
main(int argc, char **argv)
{
    int fd;

    fd = epoll_create(256);
    if (fd < 0) {
        perror("epoll_create:");
        exit(1);
    }
    exit(0);
}
  ], [ac_cv_epoll_works=yes], [ac_cv_epoll_works=no]))
AS_IF([test "x$ac_cv_epoll_works" = "xyes"],
  [AC_DEFINE([HAVE_EPOLL], [1], [Define to 1 if epoll is supported])], [])

# Define Makefiles
AC_CONFIG_FILES([
 Makefile
 cgi-src/Makefile
 extras/Makefile
])

# Generate the "configure" script
AC_OUTPUT
