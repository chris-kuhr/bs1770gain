##
 # configure.ac.template
 # Copyright (C) 2015 Peter Belkner <pbelkner@users.sf.net>
 #
 # This program is free software; you can redistribute it and/or
 # modify it under the terms of the GNU General Public
 # License as published by the Free Software Foundation; either
 # version 2.0 of the License, or (at your option) any later version.
 #
 # This program is distributed in the hope that it will be useful,
 # but WITHOUT ANY WARRANTY; without even the implied warranty of
 # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
 # General Public License for more details.
 #
 # You should have received a copy of the GNU General Public
 # License along with this library; if not, write to the Free Software
 # Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston,
 # MA  02110-1301  USA
 ##
# http://www.freesoftwaremagazine.com/books/autotools_a_guide_to_autoconf_automake_libtool
# https://autotools.io/autoconf/arguments.html
# http://www.gnu.org/software/libidn/manual/html_node/Autoconf-tests.html
# http://www.clearchain.com/blog/posts/autotools
#
# AC_ARG_WITH (option-name, help-string, action-if-present, action-if-not-present)
# AC_CHECK_HEADER (header-file, [action-if-found], [action-if-not-found], [includes = `default-includes'])
# AC_CHECK_LIB (library, function, [action-if-found], [action-if-not-found], [other-libraries])

AC_INIT([bs1770gain], [0.5.1], [pbelkner@users.sf.net], [], [http://bs1770gain.sourceforge.net/])
AM_INIT_AUTOMAKE([-Wall -Werror foreign])
AM_PROG_AR()
AC_PROG_CC
AC_PROG_RANLIB

# test for win32 {
AC_MSG_CHECKING([for win32])
tmp=`mktemp`
cat << _ACEOF | gcc -xc -o "$tmp" -
#include <stdio.h>

int main()
{ 
#if defined (_WIN32) // {
  puts("yes");
#else // } {
  puts("no");
#endif // }
  return 0;
}
_ACEOF

win32="`$tmp`"
rm -f "$tmp"
AM_CONDITIONAL([WIN32], [test x$win32 = xyes])
AC_MSG_RESULT($win32)
# }

# test for FFmpeg [
unset withval
AC_ARG_WITH(ffmpeg, AS_HELP_STRING([--with-ffmpeg=[DIR]], [path to FFmpeg installation]),
    ffmpeg=$withval, ffmpeg=yes)
if test "x$ffmpeg" != "xno"; then # [
  if test "x$ffmpeg" != "xyes"; then # [
    LDFLAGS="${LDFLAGS} -L$ffmpeg/lib"
    CPPFLAGS="${CPPFLAGS} -I$ffmpeg/include"
	  if test "x$win32" = "xno"; then # [
      export LD_LIBRARY_PATH="$ffmpeg/lib:${LD_LIBRARY_PATH}"
	  else # ] [
      export PATH="$ffmpeg/bin:${PATH}"
    fi # ]
    FFMPEG_PREFIX="$ffmpeg"
    AC_SUBST(FFMPEG_PREFIX)
  fi # ]
fi # ]
if test "x$ffmpeg" != "xno"; then # [
  AC_CHECK_HEADER(libavutil/avutil.h,
    AC_CHECK_LIB(avutil, avutil_version, [ffmpeg=yes LIBS="${LIBS} -lavutil"], ffmpeg=no),
    ffmpeg=no)
fi # ]
if test "x$ffmpeg" != "xno"; then # [
  AC_CHECK_HEADER(libswresample/swresample.h,
    AC_CHECK_LIB(swresample, swresample_version, [ffmpeg=yes LIBS="${LIBS} -lswresample"], ffmpeg=no),
    ffmpeg=no)
fi # ]
if test "x$ffmpeg" != "xno"; then # [
  AC_CHECK_HEADER(libavcodec/avcodec.h,
    AC_CHECK_LIB(avcodec, avcodec_version, [ffmpeg=yes LIBS="${LIBS} -lavcodec"], ffmpeg=no),
    ffmpeg=no)
fi # ]
if test "x$ffmpeg" != "xno"; then # [
  AC_CHECK_HEADER(libavformat/avformat.h,
    AC_CHECK_LIB(avformat, avformat_version, [ffmpeg=yes LIBS="${LIBS} -lavformat"], ffmpeg=no),
    ffmpeg=no)
fi # ]
if test "x$ffmpeg" == "xno" ; then # [
  AC_MSG_ERROR([FFmpeg not found])
fi # ]
# test for FFmpeg ]

# test for SoX [
unset withval
AC_ARG_WITH(sox, AS_HELP_STRING([--with-sox=[DIR]], [path to SoX installation]),
    sox=$withval, sox=yes)
if test "x$sox" != "xno"; then # [
  if test "x$sox" != "xyes"; then
    LDFLAGS="${LDFLAGS} -L$sox/lib"
    CPPFLAGS="${CPPFLAGS} -I$sox/include"
	  if test "$win32" = "xno"; then # [
      export LD_LIBRARY_PATH="$sox/lib:${LD_LIBRARY_PATH}"
    else # ] [
      export PATH="$sox/bin:${PATH}"
    fi # ]
    SOX_PREFIX="$sox"
    AC_SUBST(SOX_PREFIX)
  fi # ]
fi # ]
if test "x$sox" != "xno"; then # [
  AC_CHECK_HEADER(sox.h,
    AC_CHECK_LIB(sox, sox_version, [sox=yes LIBS="${LIBS} -lsox"], sox=no),
    sox=no)
fi # ]
if test "x$sox" == "xno" ; then # [
  AC_MSG_ERROR([SoX not found])
fi # ]
# test for SoX ]

# define avutil major version [
tmp=`mktemp`

cat << _ACEOF | gcc -xc ${CPPFLAGS} -o "$tmp" - ${LDFLAGS} ${LIBS}
#include <stdio.h>
#include <libavutil/avutil.h>

int main() { printf("%u",avutil_version()>>16); return 0; }
_ACEOF

FFSOX_AVUTIL_V="`$tmp`"
AC_SUBST(FFSOX_AVUTIL_V)
AC_DEFINE_UNQUOTED([FFSOX_AVUTIL_V], ["$FFSOX_AVUTIL_V"], [Define to libavutil major version.])
rm -f "$tmp"
# define avutil major version ]

# define swresample major version [
tmp=`mktemp`

cat << _ACEOF | gcc -xc ${CPPFLAGS} -o "$tmp" - ${LDFLAGS} ${LIBS}
#include <stdio.h>
#include <libswresample/swresample.h>

int main() { printf("%u",swresample_version()>>16); return 0; }
_ACEOF

FFSOX_SWRESAMPLE_V="`$tmp`"
AC_SUBST(FFSOX_SWRESAMPLE_V)
AC_DEFINE_UNQUOTED([FFSOX_SWRESAMPLE_V], ["$FFSOX_SWRESAMPLE_V"], [Define to libswresample major version.])
rm -f "$tmp"
# define swresample major version ]

# define avcodec major version [
tmp=`mktemp`

cat << _ACEOF | gcc -xc ${CPPFLAGS} -o "$tmp" - ${LDFLAGS} ${LIBS}
#include <stdio.h>
#include <libavcodec/avcodec.h>

int main() { printf("%u",avcodec_version()>>16); return 0; }
_ACEOF

FFSOX_AVCODEC_V="`$tmp`"
AC_SUBST(FFSOX_AVCODEC_V)
AC_DEFINE_UNQUOTED([FFSOX_AVCODEC_V], ["$FFSOX_AVCODEC_V"], [Define to libavcodec major version.])
rm -f "$tmp"
# define avcodec major version ]

# define avformat major version [
tmp=`mktemp`

cat << _ACEOF | gcc -xc ${CPPFLAGS} -o "$tmp" - ${LDFLAGS} ${LIBS}
#include <stdio.h>
#include <libavformat/avformat.h>

int main() { printf("%u",avformat_version()>>16); return 0; }
_ACEOF

FFSOX_AVFORMAT_V="`$tmp`"
AC_SUBST(FFSOX_AVFORMAT_V)
AC_DEFINE_UNQUOTED([FFSOX_AVFORMAT_V], ["$FFSOX_AVFORMAT_V"], [Define to libavformat major version.])
rm -f "$tmp"
# define avformat major version ]

# define sox major version [
tmp=`mktemp`
cat << _ACEOF | gcc -xc ${CPPFLAGS} -o "$tmp" - ${LDFLAGS} ${LIBS}
#include <stdio.h>
#include <string.h>
#include <sox.h>

int main()
{
  puts(strcmp(sox_version(),"14.4.2")<0?"2":"3");
  return 0;
}
_ACEOF

FFSOX_LIBSOX_V="`$tmp`"
AC_SUBST(FFSOX_LIBSOX_V)
AC_DEFINE_UNQUOTED([FFSOX_LIBSOX_V], ["$FFSOX_LIBSOX_V"], [Define to libsox major version.])
rm -f "$tmp"
# define sox major version ]

# test for dynload [
unset withval
AC_ARG_ENABLE(dynload, AS_HELP_STRING([--enable-dynload], [load FFmpeg and SoX dynamically]),
    dynload=yes, dynload=no)
AM_CONDITIONAL([FFSOX_DYNLOAD], [test x$dynload = xyes])
if test "x$dynload" = "xyes"; then # [
  AC_DEFINE([HAVE_FFSOX_DYNLOAD], [1],
      [Define to 1 if SoX and FFmpeg should be loaded dynamically.])
  # test for dl [
  if test "xyes" != "x$win32"; then # [
    unset withval
    AC_ARG_WITH(dl, AS_HELP_STRING([--with-dl=[DIR]], [path to libdl installation]),
        dl=$withval, dl=yes)
    if test "x$dl" != "xno"; then # [
      if test "x$dl" != "xyes"; then # [
        LDFLAGS="${LDFLAGS} -L$dl/lib"
        CPPFLAGS="${CPPFLAGS} -I$dl/include"
      fi # ]
    fi # ]
    if test "x$dl" != "xno"; then # [
      AC_CHECK_HEADER(dlfcn.h,
        AC_CHECK_LIB(dl, dlopen, [dl=yes LIBS="${LIBS} -ldl"], dl=no),
        dl=no)
    fi # ]
    if test "x$dl" == "xno" ; then # [
      AC_MSG_ERROR([libdl not found])
    fi # ]
  fi # ]
  # test for dl ]
  AC_MSG_NOTICE([loading FFmpeg and SoX dynamically])
else # ] [
  AC_MSG_NOTICE([linking FFmpeg and SoX])
fi # ]
# test for dynload ]

# dealing with shared objects [
if test "x$win32" = "xyes"; then # [
  SODIR='bin'
	AVUTIL_SO=avutil-$FFSOX_AVUTIL_V.dll
	SWRESAMPLE_SO=swresample-$FFSOX_SWRESAMPLE_V.dll
	AVCODEC_SO=avcodec-$FFSOX_AVCODEC_V.dll
	AVFORMAT_SO=avformat-$FFSOX_AVFORMAT_V.dll
	LIBSOX_SO=libsox-$FFSOX_LIBSOX_V.dll
  BS1770GAIN_LDFLAGS="-static-libgcc -static-libstdc++"
else # ] [
  SODIR='lib'
	AVUTIL_SO=libavutil.so.$FFSOX_AVUTIL_V
	SWRESAMPLE_SO=libswresample.so.$FFSOX_SWRESAMPLE_V
	AVCODEC_SO=libavcodec.so.$FFSOX_AVCODEC_V
	AVFORMAT_SO=libavformat.so.$FFSOX_AVFORMAT_V
	LIBSOX_SO=libsox.so.$FFSOX_LIBSOX_V
  BS1770GAIN_LDFLAGS=""
fi # ]

AC_SUBST(SODIR)
AC_SUBST(AVUTIL_SO)
AC_SUBST(SWRESAMPLE_SO)
AC_SUBST(AVCODEC_SO)
AC_SUBST(AVFORMAT_SO)
AC_SUBST(LIBSOX_SO)
AC_SUBST(BS1770GAIN_LDFLAGS)
# dealing with shared objects ]

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_FILES([
  Makefile
  libpbutil/Makefile
  lib1770-2/Makefile
  libffsox-2/Makefile
  bs1770gain/Makefile
])
AC_OUTPUT
