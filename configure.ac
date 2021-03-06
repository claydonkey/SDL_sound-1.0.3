dnl#                                               -*- Autoconf -*-
dnl# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([SDL_sound],[1.0.3],[icculus@icculus.org])
AC_CONFIG_SRCDIR([alt_audio_convert.c])

dnl#---------------------------------------------------------------------
dnl# System/version info
dnl#---------------------------------------------------------------------

dnl# Making releases:
dnl#   MICRO_VERSION += 1;
dnl#   INTERFACE_AGE += 1;
dnl#   BINARY_AGE += 1;
dnl# if any functions have been added, set INTERFACE_AGE to 0.
dnl# if backwards compatibility has been broken,
dnl# set BINARY_AGE and INTERFACE_AGE to 0.

MAJOR_VERSION=1
MINOR_VERSION=0
MICRO_VERSION=3
INTERFACE_AGE=2
BINARY_AGE=2
VERSION=$MAJOR_VERSION.$MINOR_VERSION.$MICRO_VERSION

AC_SUBST([MAJOR_VERSION])
AC_SUBST([MINOR_VERSION])
AC_SUBST([MICRO_VERSION])
AC_SUBST([INTERFACE_AGE])
AC_SUBST([BINARY_AGE])
AC_SUBST([VERSION])

# libtool versioning
LT_RELEASE=$MAJOR_VERSION.$MINOR_VERSION
LT_CURRENT=`expr $MICRO_VERSION - $INTERFACE_AGE`
LT_REVISION=$INTERFACE_AGE
LT_AGE=`expr $BINARY_AGE - $INTERFACE_AGE`

AC_SUBST([LT_RELEASE])
AC_SUBST([LT_CURRENT])
AC_SUBST([LT_REVISION])
AC_SUBST([LT_AGE])

dnl# Detect the canonical host and target build environment
AC_CANONICAL_HOST
AC_CANONICAL_TARGET

dnl# Setup for automake
AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE([1.13 foreign dejagnu dist-zip -Wall])
AC_CONFIG_MACRO_DIR([m4])
AM_MAINTAINER_MODE

dnl#---------------------------------------------------------------------
dnl# Compilers and other tools
dnl#---------------------------------------------------------------------

AM_PROG_AR
AM_PROG_CC_C_O
AC_PROG_CPP
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_RANLIB
LT_INIT
LT_LANG([C])

dnl# Macros that I just want to see what autoupdate will do with
dnl# (`autoreconf -fvi --warnings=all` reports warnings about these
dnl# macros when they're used in m4 files in the m4 directory, but
dnl# autoupdate only fixes stuff that is put directly in the configure.ac
dnl# file, so try putting them here to see what will happen)
AC_LINK_IFELSE([AC_LANG_PROGRAM([[
#include <stdio.h>
]], [[
#include <stdlib.h>
]])],[],[])
AC_LANG_PUSH([C])
AC_LANG_POP([C])

dnl#---------------------------------------------------------------------
dnl# Debug mode?
dnl#---------------------------------------------------------------------

AC_ARG_ENABLE([debug],
[AS_HELP_STRING([--enable-debug],[enable debug mode [default=no]])],
              [], [enable_debug=no])
if test x$enable_debug = xyes; then
  if test x$ac_cv_prog_cc_g = xyes; then
    CFLAGS="-g -O0"
  else
    CFLAGS="-O0"
  fi
  AC_DEFINE([DEBUG], [1], [Define for debug builds.])
  AC_DEFINE([DEBUG_CHATTER], [1], [Define for debug build chattering.])
else
  AC_DEFINE([NDEBUG], [1], [Define to disable debugging.])
fi

dnl#---------------------------------------------------------------------
dnl# Checks for libraries.
dnl#---------------------------------------------------------------------

dnl# Check for SDL
SDL_VERSION=1.2.0
AM_PATH_SDL([$SDL_VERSION],
            [:],
 	    [AC_MSG_ERROR([*** SDL version $SDL_VERSION not found!])])
CFLAGS="$CFLAGS $SDL_CFLAGS"
LIBS="$LIBS $SDL_LIBS"

dnl# Check for voc decoder inclusion...
AC_ARG_ENABLE([voc],
[AS_HELP_STRING([--enable-voc],[enable VOC decoding [default=yes]])],
              [], [enable_voc=yes])
if test x$enable_voc = xyes; then
    AC_DEFINE([SOUND_SUPPORTS_VOC], [1], [Define if VOC support is desired.])
fi

dnl# Check for wav decoder inclusion...
AC_ARG_ENABLE([wav],
[AS_HELP_STRING([--enable-wav],[enable WAV decoding [default=yes]])],
              [], [enable_wav=yes])
if test x$enable_wav = xyes; then
    AC_DEFINE([SOUND_SUPPORTS_WAV], [1], [Define if WAV support is desired.])
fi


dnl# Check for raw decoder inclusion...
AC_ARG_ENABLE([raw],
[AS_HELP_STRING([--enable-raw],[enable raw audio "decoding" [default=yes]])],
              [], [enable_raw=yes])
if test x$enable_raw = xyes; then
    AC_DEFINE([SOUND_SUPPORTS_RAW], [1], [Define if RAW support is desired.])
fi


dnl# Check for aiff decoder inclusion...
AC_ARG_ENABLE([aiff],
[AS_HELP_STRING([--enable-aiff],[enable AIFF decoding [default=yes]])],
              [], [enable_aiff=yes])
if test x$enable_aiff = xyes; then
    AC_DEFINE([SOUND_SUPPORTS_AIFF], [1], [Define if AIFF support is desired.])
fi

dnl# Check for au decoder inclusion...
AC_ARG_ENABLE([au],
[AS_HELP_STRING([--enable-au],[enable AU decoding [default=yes]])],
              [], [enable_au=yes])
if test x$enable_au = xyes; then
    AC_DEFINE([SOUND_SUPPORTS_AU], [1], [Define if AU support is desired.])
fi

dnl# Check for shn decoder inclusion...
AC_ARG_ENABLE([shn],
[AS_HELP_STRING([--enable-shn],[enable SHN decoding [default=yes]])],
              [], [enable_shn=yes])
if test x$enable_shn = xyes; then
    AC_DEFINE([SOUND_SUPPORTS_SHN], [1], [Define if SHN support is desired.])
fi

dnl# Check for the MIDI decoder...
AC_ARG_ENABLE([midi],
[AS_HELP_STRING([--enable-midi],[enable software MIDI music [default=yes]])],
              [], [enable_midi=yes])
if test x$enable_midi = xyes; then
  AC_DEFINE([SOUND_SUPPORTS_MIDI], [1], [Define if MIDI support is desired.])
fi

dnl# Check for SMPEG
AC_ARG_ENABLE([smpeg],
[AS_HELP_STRING([--enable-smpeg],[enable MP3 decoding via smpeg [default=yes]])],
              [], [enable_smpeg=yes])
if test x$enable_smpeg = xyes; then
  AC_CHECK_HEADER([smpeg.h], [have_smpeg_hdr=yes])
  AC_CHECK_LIB([smpeg], [SMPEG_new], [have_smpeg_lib=yes])
  if test x$have_smpeg_hdr = xyes -a x$have_smpeg_lib = xyes; then
    LIBS="$LIBS -lsmpeg"
    AC_DEFINE([SOUND_SUPPORTS_SMPEG], [1], [Define if SMPEG support is desired.])
  fi
fi

dnl# Use the official smpeg macro to compare results from above block
AM_PATH_SMPEG

dnl# Check for mpglib
AC_ARG_ENABLE([mpglib],
[AS_HELP_STRING([--enable-mpglib],[enable MP3 decoding internally [default=yes]])],
              [], [enable_mpglib=yes])
if test x$enable_mpglib = xyes; then
  AC_DEFINE([SOUND_SUPPORTS_MPGLIB], [1], [Define if MPGLIB support is desired.])
fi


dnl# Check for libmikmod
AC_ARG_ENABLE([mikmod],
[AS_HELP_STRING([--enable-mikmod],[enable MOD decoding via mikmod [default=yes]])],
              [], [enable_mikmod=yes])
if test x$enable_mikmod = xyes; then
  AC_CHECK_HEADER([mikmod.h], [have_mikmod_hdr=yes])
  AC_CHECK_LIB([mikmod], [MikMod_RegisterDriver], [have_mikmod_lib=yes])
  if test x$have_mikmod_hdr = xyes -a x$have_mikmod_lib = xyes; then
    LIBS="$LIBS -lmikmod"
    AC_DEFINE([SOUND_SUPPORTS_MIKMOD], [1], [Define if MIKMOD support is desired.])
  fi
fi

dnl# Use the official libmikmod macro to compare results from above block
AM_PATH_LIBMIKMOD

dnl# Check for libmodplug
AC_ARG_ENABLE([modplug],
[AS_HELP_STRING([--enable-modplug],[enable MOD decoding via modplug [default=yes]])],
              [], [enable_modplug=yes])
if test x$enable_modplug = xyes; then
  have_modplug_hdr=no
  AC_CHECK_HEADER([modplug.h], [have_modplug_hdr=yes])
  if test x$have_modplug_hdr = xno; then
    AC_CHECK_HEADER([libmodplug/modplug.h], [have_modplug_hdr=yes])
    if test x$have_modplug_hdr = xyes; then
      have_modplug_hdr=yes
      AC_DEFINE([SOUND_MODPLUG_IN_OWN_PATH], [1], [Define if modplug header is in own directory.])
    fi
  fi

  AC_CHECK_LIB([modplug], [ModPlug_Load], [have_modplug_lib=yes])
  if test x$have_modplug_hdr = xyes -a x$have_modplug_lib = xyes; then
    LIBS="$LIBS -lmodplug"
    AC_DEFINE([SOUND_SUPPORTS_MODPLUG], [1], [Define if MODPLUG support is desired.])
  fi
fi

AC_CHECK_HEADER([ogg/ogg.h], [have_ogg_hdr=yes])
AC_CHECK_LIB([ogg], [ogg_sync_init], [have_ogg_lib=yes])
have_ogg=no
if test x$have_ogg_hdr = xyes -a x$have_ogg_lib = xyes; then
  have_ogg=yes
fi

dnl# Use the official ogg macro to compare results from above block
XIPH_PATH_OGG

dnl# !!! FIXME: should be --enable-vorbis.
dnl# ^ OK, fixed (I think) (was originally --enable-ogg)
dnl# Check for Ogg Vorbis
AC_ARG_ENABLE([vorbis],
[AS_HELP_STRING([ --enable-vorbis],[enable OGG decoding via libvorbis [default=yes]])],
              [], [enable_vorbis=yes])
if test x$enable_vorbis = xyes; then
  AC_CHECK_HEADER([vorbis/codec.h], [have_vorbis_hdr=yes])
  AC_CHECK_LIB([vorbis], [vorbis_info_init], [have_vorbis_lib=yes])
  AC_CHECK_HEADER([vorbis/vorbisfile.h], [have_vorbisfile_hdr=yes])
  AC_CHECK_LIB([vorbisfile], [ov_open_callbacks], [have_vorbisfile_lib=yes])
  have_vorbis=no
  if test x$have_ogg = xyes; then
    if test x$have_vorbis_hdr = xyes -a x$have_vorbis_lib = xyes; then
      if test x$have_vorbisfile_hdr = xyes -a x$have_vorbisfile_lib = xyes; then
        have_vorbis=yes
      fi
    fi
  fi

  if test x$have_vorbis = xyes; then
    CFLAGS="$CFLAGS"
    LIBS="$LIBS -logg -lvorbis -lvorbisfile"
    AC_DEFINE([SOUND_SUPPORTS_OGG], [1], [Define if OGG support is desired.])
  fi
fi

dnl# Use the official vorbis macro to compare results from above block
XIPH_PATH_VORBIS

dnl# Note that we intentionally look for a symbol in FLAC 1.0.4, since the
dnl#  FLAC developers tend to break their API with every release, so we're
dnl#  checking for the latest-and-greatest here so we don't have to support
dnl#  obsolete versions.
dnl# Starting with FLAC 1.1.3:
dnl#   libFLAC supports Ogg FLAC (no more libOggFLAC) so we also need -logg
dnl#   the libFLAC .so version is also #defined in FLAC/export.h

dnl# Hooray for shitty autoconf bugs! (Not sure if this bug exists any more?)
x="C__stream_decoder_process_single"
flcsym="FLA$x"
dnl# Check for libFLAC
AC_ARG_ENABLE([flac],
[AS_HELP_STRING([--enable-flac],[enable FLAC decoding via libFLAC [default=yes]])],
              [], [enable_flac=yes])
if test x$enable_flac = xyes; then
  AC_CHECK_HEADER([FLAC/stream_decoder.h], [have_flac_hdr=yes])
  AC_CHECK_LIB([FLAC], [$flcsym], [have_flac_lib=yes])
  if test x$have_ogg = xyes; then
    if test x$have_flac_hdr = xyes -a x$have_flac_lib = xyes; then
      LIBS="$LIBS -lFLAC -logg"
      AC_DEFINE([SOUND_SUPPORTS_FLAC], [1], [Define if FLAC support is desired.])
    fi
  fi
fi

dnl# Check for speex
AC_ARG_ENABLE([speex],
[AS_HELP_STRING([--enable-speex],[enable SPX decoding via libspeex [default=yes]])],
              [], [enable_speex=yes])
if test x$enable_speex = xyes; then
  AC_CHECK_HEADER([ogg/ogg.h], [have_ogg_hdr=yes])
  AC_CHECK_LIB([ogg], [ogg_sync_init], [have_ogg_lib=yes])
  AC_CHECK_HEADER([speex/speex.h], [have_speex_hdr=yes])
  AC_CHECK_LIB([speex], [speex_bits_init], [have_speex_lib=yes])
  if test "x$have_ogg_hdr" = "xyes" -a "x$have_ogg_lib" = "xyes" -a "x$have_speex_hdr" = "xyes" -a "x$have_speex_lib" = "xyes"; then
    LIBS="$LIBS -logg -lspeex"
    AC_DEFINE([SOUND_SUPPORTS_SPEEX], [1], [Define if SPEEX support is desired.])
  fi
fi

dnl# Use the official speex macro to compare results from above block
XIPH_PATH_SPEEX

dnl# Check for PhysicsFS http://icculus.org/physfs/
AC_ARG_ENABLE([physfs],
[AS_HELP_STRING([--enable-physfs],[enable PhysicsFS in playsound [default=yes]])],
              [], [enable_physfs=yes])
if test x$enable_physfs = xyes; then
  AC_CHECK_HEADER([physfs.h], [have_physfs_hdr=yes])
  AC_CHECK_LIB([physfs], [PHYSFS_init], [have_physfs_lib=yes])
  if test x$have_physfs_hdr = xyes -a x$have_physfs_lib = xyes; then
    enable_physfs="yes"
  else
    enable_physfs="no"
  fi
fi

dnl# Check for altcvt
AC_ARG_ENABLE([altcvt],
[AS_HELP_STRING([--enable-altcvt],[enable EXPERIMENTAL audio converter [default=no]])],
              [], [enable_altcvt=no])
if test x$enable_altcvt = xyes; then
  AC_DEFINE([SOUND_USE_ALTCVT], [1], [Define to use alternate audio converter.])
fi

# Checks for header files.
AC_C_PROTOTYPES
AC_PATH_X
AC_PATH_XTRA
AC_FUNC_ALLOCA
AC_CHECK_HEADERS([malloc.h stddef.h stdio.h signal.h assert.h])
AC_CHECK_HEADERS([IOKit/IOKitLib.h IOKit/hidsystem/IOLLEvent.h IOKit/IOTypes.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_C_INLINE
AC_CHECK_HEADER_STDBOOL
AC_TYPE_INT8_T
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT64_T
AC_TYPE_UINT8_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_CHECK_TYPES([ptrdiff_t])

# Checks for library functions.

AC_FUNC_MALLOC
AC_FUNC_MEMCMP
AC_FUNC_REALLOC
AC_CHECK_FUNCS([atexit memset pow sqrt strchr strrchr setbuf])

dnl# Add Makefile conditionals
AM_CONDITIONAL([USE_TIMIDITY], [test x$enable_midi = xyes])
AM_CONDITIONAL([USE_MPGLIB], [test x$enable_mpglib = xyes])
AM_CONDITIONAL([USE_PHYSICSFS], [test x$enable_physfs = xyes])

AC_CONFIG_FILES([
Makefile
PBProjects/exports/Makefile
decoders/Makefile
decoders/timidity/Makefile
decoders/mpglib/Makefile
playsound/Makefile
])
AC_OUTPUT
