dnl require autoconf 2.60 (AS_ECHO/AS_ECHO_N)
AC_PREREQ([2.60])
define(_CLIENT_VERSION_MAJOR, 0)
define(_CLIENT_VERSION_MINOR, 7)
define(_CLIENT_VERSION_REVISION, 3)
define(_CLIENT_VERSION_BUILD, 01)
define(_CLIENT_VERSION_IS_RELEASE, false)
define(_COPYRIGHT_YEAR, 2019)
AC_INIT([Stronghands],[_CLIENT_VERSION_MAJOR._CLIENT_VERSION_MINOR._CLIENT_VERSION_REVISION],[info@stronghands.info],[stronghands])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_MACRO_DIR([build-aux/m4])
LT_INIT([disable-shared])
AC_CANONICAL_HOST
AH_TOP([#ifndef BITCOIN_CONFIG_H])
AH_TOP([#define BITCOIN_CONFIG_H])
AH_BOTTOM([#endif //BITCOIN_CONFIG_H])

# This m4 will only be used if a system copy cannot be found. This is helpful
# on systems where autotools are installed but the pkg-config macros are not in
# a default location. It is currently used for building on OSX where autotools
# are preinstalled but pkg-config comes from macports or homebrew. It should
# probably be removed when building on <= 10.6 is no longer supported.
m4_include([pkg.m4])

dnl reset CXXFLAGS if empty (autoconf adds its own defaults otherwise)
: ${CFLAGS=""}
: ${CXXFLAGS=""}

dnl faketime breaks configure and is only needed for make. Disable it here.
unset FAKETIME

dnl ==============================================================
dnl Setup for automake
dnl ==============================================================

AM_INIT_AUTOMAKE([no-define subdir-objects foreign])

dnl faketime messes with timestamps and causes configure to be re-run.
dnl --disable-maintainer-mode can be used to bypass this.
AM_MAINTAINER_MODE([enable])

dnl make the compilation flags quiet unless V=1 is used
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

# Enable wallet
AC_ARG_ENABLE([wallet],
  [AS_HELP_STRING([--enable-wallet],
  [enable wallet (default is yes)])],
  [enable_wallet=$enableval],
  [enable_wallet=yes])

AC_ARG_ENABLE([mining],
  [AS_HELP_STRING([--disable-mining],
  [disable mining and minting (default is no)])],
  [disable_mining=$enableval],
  [disable_mining=no])

AC_ARG_WITH([miniupnpc],
  [AS_HELP_STRING([--with-miniupnpc],
  [enable UPNP (default is yes if libminiupnpc is found)])],
  [use_upnp=$withval],
  [use_upnp=auto])

AC_ARG_ENABLE([upnp-default],
  [AS_HELP_STRING([--enable-upnp-default],
  [if UPNP is enabled, turn it on at startup (default is no)])],
  [use_upnp_default=$enableval],
  [use_upnp_default=no])

dnl enable ipv6 support
AC_ARG_ENABLE([ipv6],
  [AS_HELP_STRING([--enable-ipv6],
  [enable ipv6 (default is yes)])],
  [use_ipv6=$enableval],
  [use_ipv6=yes])

dnl enable qt support
AC_ARG_WITH([qt],
  [AS_HELP_STRING([--with-qt],
  [enable qt (default is yes)])],
  [use_qt=$withval],
  [use_qt=yes])

dnl compile with full warnings and debugging symbols
AC_ARG_ENABLE(debug,
    AS_HELP_STRING([--enable-debug],[compile with debug symbols (default is yes)]),
    [use_debug=$enableval],
    [use_debug=yes])
    AC_MSG_CHECKING([if compiling with debugging symbols])

AC_ARG_ENABLE(tests,
    AS_HELP_STRING([--enable-tests],[compile tests (default is yes)]),
    [use_tests=$enableval],
    [use_tests=yes])

AC_ARG_WITH([comparison-tool],
    AS_HELP_STRING([with-comparison-tool],[path to java comparison tool (requires --enable-tests)]),
    [use_comparison_tool=$withval],
    [use_comparison_tool=no])

AC_ARG_WITH([qrencode],
  [AS_HELP_STRING([--with-qrencode],
  [enable QR code support (default is yes if qt is enabled and libqrencode is found)])],
  [use_qr=$withval],
  [use_qr=auto])

AC_ARG_WITH([qtdbus],
  [AS_HELP_STRING([--with-qtdbus],
  [enable DBus support (default is yes if qt is enabled and QtDBus is found)])],
  [use_dbus=$withval],
  [use_dbus=auto])

AC_ARG_ENABLE([hardening],
  [AS_HELP_STRING([--enable-hardening],
  [attempt to harden the resulting executables (default is yes)])],
  [use_hardening=$enableval],
  [use_hardening=yes])

AC_ARG_ENABLE([ccache],
  [AS_HELP_STRING([--enable-ccache],
  [enable building with ccache (default is yes if ccache is found)])],
  [use_ccache=$enableval],
  [use_ccache=auto])

AC_ARG_ENABLE([lcov],
  [AS_HELP_STRING([--enable-lcov],
  [enable lcov testing (default is no)])],
  [use_lcov=yes],
  [use_lcov=no])

AC_ARG_WITH([qt-incdir],[AS_HELP_STRING([--with-qt-incdir=INC_DIR],[specify qt include path (overridden by pkgconfig)])], [qt_include_path=$withval], [])
AC_ARG_WITH([qt-libdir],[AS_HELP_STRING([--with-qt-libdir=LIB_DIR],[specify qt lib path (overridden by pkgconfig)])], [qt_lib_path=$withval], [])
AC_ARG_WITH([qt-bindir],[AS_HELP_STRING([--with-qt-bindir=BIN_DIR],[specify qt bin path])], [qt_bin_path=$withval], [])
AC_ARG_WITH([qt-plugindir],[AS_HELP_STRING([--with-qt-plugindir=PLUGIN_DIR],[specify qt plugin path (overridden by pkgconfig)])], [qt_plugin_path=$withval], [])
AC_ARG_WITH([protoc-bindir],[AS_HELP_STRING([--with-protoc-bindir=BIN_DIR],[specify protoc bin path])], [protoc_bin_path=$withval], [])


AC_CONFIG_SRCDIR([src])
AC_CONFIG_HEADERS([src/bitcoin-config.h])

dnl Checks for programs.
AC_PROG_CXX
AC_PROG_CC
AC_PROG_CPP
AC_PROG_CXXCPP
AC_PROG_INSTALL
AC_PROG_OBJC
m4_ifdef([AC_PROG_OBJCXX],[AC_PROG_OBJCXX])
AC_PROG_MKDIR_P
AC_PROG_SED
AC_PATH_TOOL(AR, ar)
AC_PATH_TOOL(RANLIB, ranlib)
AC_PATH_TOOL(WINDRES, windres)
AC_PATH_TOOL(STRIP, strip)
AC_PATH_TOOL(GCOV, gcov)
AC_PATH_PROG(LCOV, lcov)
AC_PATH_PROG(JAVA, java)
AC_PATH_PROG(GENHTML, genhtml)
AC_PATH_PROG([GIT], [git])
AC_PATH_PROG(CCACHE,ccache)
AC_PATH_PROG(XGETTEXT,xgettext)
AC_PATH_PROG(HEXDUMP,hexdump)
PKG_PROG_PKG_CONFIG

# Enable wallet
AC_ARG_ENABLE([wallet],
  [AS_HELP_STRING([--enable-wallet],
  [enable wallet (default is yes)])],
  [enable_wallet=$enableval],
  [enable_wallet=yes])


## TODO: Remove these hard-coded paths and flags. They are here for the sake of
##       compatibility with the legacy buildsystem.
##
CXXFLAGS="$CXXFLAGS -Wall -Wextra -Wformat -Wformat-security -Wno-unused-parameter"
CPPFLAGS="$CPPFLAGS -DBOOST_SPIRIT_THREADSAFE -DBOOST_NO_CXX11_SCOPED_ENUMS -DHAVE_BUILD_INFO"
DEBUG_CXXFLAGS="-g -ggdb"
RELEASE_CXXFLAGS="-O2"

AC_LANG_PUSH([C++])

use_pkgconfig=yes
case $host in
  *mingw*)

     #pkgconfig does more harm than good with mingw
     use_pkgconfig=no

     TARGET_OS=windows
     AC_CHECK_LIB([mingwthrd],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([kernel32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([user32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([gdi32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([comdlg32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([winspool],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([winmm],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([shell32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([comctl32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([ole32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([oleaut32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([uuid],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([rpcrt4],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([advapi32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([ws2_32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([mswsock],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([shlwapi],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([iphlpapi],      [main],, AC_MSG_ERROR(lib missing))

     AC_PATH_PROG([MAKENSIS], [makensis], none)
     if test x$MAKENSIS = xnone; then
       AC_MSG_WARN("makensis not found. Cannot create installer.")
     fi

     CPPFLAGS="$CPPFLAGS -D_MT -DWIN32 -D_WINDOWS -DBOOST_THREAD_USE_LIB"
     LEVELDB_TARGET_FLAGS="TARGET_OS=OS_WINDOWS_CROSSCOMPILE"
     CXXFLAGS="$CXXFLAGS -w"
     ;;
  *darwin*)
     TARGET_OS=darwin
     LEVELDB_TARGET_FLAGS="TARGET_OS=Darwin"
     if  test x$cross_compiling != xyes; then
       BUILD_OS=darwin
       AC_CHECK_PROG([PORT],port, port)
       if test x$PORT = xport; then
         dnl add default macports paths
         CPPFLAGS="$CPPFLAGS -I/opt/local/include -I/opt/local/include/db48"
         LIBS="$LIBS -L/opt/local/lib -L/opt/local/lib/db48"
       fi

       AC_CHECK_PROG([BREW],brew, brew)
       if test x$BREW = xbrew; then
         dnl add default homebrew paths
         openssl_prefix=`$BREW --prefix openssl`
         bdb_prefix=`$BREW --prefix berkeley-db4`
         export PKG_CONFIG_PATH="$openssl_prefix/lib/pkgconfig:$PKG_CONFIG_PATH"
         CPPFLAGS="$CPPFLAGS -I$bdb_prefix/include"
         LIBS="$LIBS -L/$bdb_prefix/lib"
       fi
     else
       case $build_os in
         *darwin*)
           BUILD_OS=darwin
           ;;
         *)
           AC_PATH_TOOL([INSTALLNAMETOOL], [install_name_tool], install_name_tool)
           AC_PATH_TOOL([OTOOL], [otool], otool)
           AC_PATH_PROGS([GENISOIMAGE], [genisoimage mkisofs],genisoimage)
           ;;
       esac
     fi

     if test x$use_qt = xyes; then
       MOC_DEFS="-DQ_OS_MAC"
       base_frameworks="-framework Foundation -framework ApplicationServices -framework AppKit"
       AX_CHECK_LINK_FLAG([[$base_frameworks]],[LIBS="$LIBS $base_frameworks"],[AC_MSG_ERROR(could not find base frameworks)])
     fi

     AX_CHECK_COMPILE_FLAG([-mmacosx-version-min=10.5],[RELEASE_CXXFLAGS="$RELEASE_CXXFLAGS -mmacosx-version-min=10.5"])
     AX_CHECK_COMPILE_FLAG([-arch i386],[RELEASE_CXXFLAGS="$RELEASE_CXXFLAGS -arch i386"])
     RELEASE_CXXFLAGS="$RELEASE_CXXFLAGS -O3"
     CPPFLAGS="$CPPFLAGS -DMAC_OSX"
     TESTDEFS="-DBOOST_TEST_DYN_LINK"
     ;;
   *)
     TESTDEFS="-DBOOST_TEST_DYN_LINK"
     ;;
esac

if test x$use_debug == xyes; then
  CXXFLAGS="$DEBUG_CXXFLAGS $CXXFLAGS"
else
  CXXFLAGS="$RELEASE_CXXFLAGS $CXXFLAGS"
fi

if test x$use_comparison_tool != xno; then
  AC_SUBST(JAVA_COMPARISON_TOOL, $use_comparison_tool)
fi

if test x$use_lcov == xyes; then
  if test x$LCOV == x; then
    AC_MSG_ERROR("lcov testing requested but lcov not found")
  fi
  if test x$GCOV == x; then
    AC_MSG_ERROR("lcov testing requested but gcov not found")
  fi
  if test x$JAVA == x; then
    AC_MSG_ERROR("lcov testing requested but java not found")
  fi
  if test x$GENHTML == x; then
    AC_MSG_ERROR("lcov testing requested but genhtml not found")
  fi
  if test x$use_comparison_tool == x; then
    AC_MSG_ERROR("lcov testing requested but comparison tool was not specified")
  fi
  LCOV="$LCOV --gcov-tool=$GCOV"
  AX_CHECK_COMPILE_FLAG([--coverage],[CXXFLAGS="$CXXFLAGS --coverage"],
    [AC_MSG_ERROR("lcov testing requested but --coverage flag does not work")])
fi

dnl Require little endian
AC_C_BIGENDIAN([AC_MSG_ERROR("Big Endian not supported")])

dnl Check for pthread compile/link requirements
AX_PTHREAD
INCLUDES="$INCLUDES $PTHREAD_CFLAGS"

# The following macro will add the necessary defines to bitcoin-config.h, but
# they also need to be passed down to any subprojects. Pull the results out of
# the cache and add them to CPPFLAGS.
AC_SYS_LARGEFILE

if test x$ac_cv_sys_file_offset_bits != x &&
   test x$ac_cv_sys_file_offset_bits != xno &&
   test x$ac_cv_sys_file_offset_bits != xunknown; then
  CPPFLAGS="$CPPFLAGS -D_FILE_OFFSET_BITS=$ac_cv_sys_file_offset_bits"
fi

if test x$ac_cv_sys_large_files != x &&
   test x$ac_cv_sys_large_files != xno &&
   test x$ac_cv_sys_large_files != xunknown; then
  CPPFLAGS="$CPPFLAGS -D_LARGE_FILES=$ac_cv_sys_large_files"
fi

AX_CHECK_LINK_FLAG([[-Wl,--large-address-aware]], [LDFLAGS="$LDFLAGS -Wl,--large-address-aware"])

if test x$use_glibc_compat != xno; then

  #__fdelt_chk's params and return type have changed from long unsigned int to long int.
  # See which one is present here.
  AC_MSG_CHECKING(__fdelt_chk type)
  AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[#ifdef _FORTIFY_SOURCE
                    #undef _FORTIFY_SOURCE
                  #endif
                  #define _FORTIFY_SOURCE 2
                  #include <sys/select.h>
     extern "C" long unsigned int __fdelt_warn(long unsigned int);]],[[]])],
    [ fdelt_type="long unsigned int"],
    [ fdelt_type="long int"])
  AC_MSG_RESULT($fdelt_type)
  AC_DEFINE_UNQUOTED(FDELT_TYPE, $fdelt_type,[parameter and return value type for __fdelt_chk])

fi

if test x$TARGET_OS != xwindows; then
  # All windows code is PIC, forcing it on just adds useless compile warnings
  AX_CHECK_COMPILE_FLAG([-fPIC],[PIC_FLAGS="-fPIC"])
fi

if test x$use_hardening != xno; then
  AX_CHECK_COMPILE_FLAG([-Wstack-protector],[HARDENED_CXXFLAGS="$HARDENED_CXXFLAGS -Wstack-protector"])
  AX_CHECK_COMPILE_FLAG([-fPIE],[HARDENED_CXXFLAGS="$HARDENED_CXXFLAGS -fPIE"])

  AX_CHECK_PREPROC_FLAG([-D_FORTIFY_SOURCE=2],[HARDENED_CPPFLAGS="$HARDENED_CPPFLAGS -D_FORTIFY_SOURCE=2"])

  AX_CHECK_LINK_FLAG([[-Wl,--dynamicbase]], [HARDENED_LDFLAGS="$HARDENED_LDFLAGS -Wl,--dynamicbase"])
  AX_CHECK_LINK_FLAG([[-Wl,--nxcompat]], [HARDENED_LDFLAGS="$HARDENED_LDFLAGS -Wl,--nxcompat"])
  AX_CHECK_LINK_FLAG([[-Wl,-z,relro]], [LDFLAGS="-Wl,-z,relro"])
  AX_CHECK_LINK_FLAG([[-Wl,-z,now]], [LDFLAGS="-Wl,-z,now"])

  if test x$TARGET_OS != xwindows; then
    # -fstack-protector-all can produce broken binaries with mingw
    AX_CHECK_COMPILE_FLAG([-fno-stack-protector],[HARDENED_CXXFLAGS="$HARDENED_CXXFLAGS -fno-stack-protector"])
    AX_CHECK_COMPILE_FLAG([-fstack-protector-all],[HARDENED_CXXFLAGS="$HARDENED_CXXFLAGS -fstack-protector-all"])

    # -pie will link successfully with mingw, but it's unsupported and leads to undeterministic binaries
    AX_CHECK_LINK_FLAG([[-pie]], [HARDENED_LDFLAGS="$HARDENED_LDFLAGS -pie"])
  fi
fi

dnl this flag screws up non-darwin gcc even when the check fails. special-case it.
if test x$TARGET_OS = xdarwin; then
  AX_CHECK_LINK_FLAG([[-Wl,-dead_strip]], [LDFLAGS="$LDFLAGS -Wl,-dead_strip"])
fi

AC_CHECK_HEADERS([stdio.h stdlib.h unistd.h strings.h sys/types.h sys/stat.h])

dnl Check for MSG_NOSIGNAL
AC_MSG_CHECKING(for MSG_NOSIGNAL)
AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[#include <sys/socket.h>]],
 [[ int f = MSG_NOSIGNAL; ]])],
 [ AC_MSG_RESULT(yes); AC_DEFINE(HAVE_MSG_NOSIGNAL, 1,[Define this symbol if you have MSG_NOSIGNAL]) ],
 [ AC_MSG_RESULT(no)]
)

LEVELDB_CPPFLAGS=
LIBLEVELDB=
LIBMEMENV=
AM_CONDITIONAL([EMBEDDED_LEVELDB],[true])
AC_SUBST(LEVELDB_CPPFLAGS)
AC_SUBST(LIBLEVELDB)
AC_SUBST(LIBMEMENV)

if test x$enable_wallet != xno; then
    dnl Check for libdb_cxx only if wallet enabled
    BITCOIN_FIND_BDB48
fi

dnl Check for libminiupnpc (optional)
if test x$use_upnp != xno; then
  AC_CHECK_LIB([miniupnpc], [main],, [have_miniupnpc=no])
fi

dnl Check for boost libs
AX_BOOST_BASE
AX_BOOST_SYSTEM
AX_BOOST_FILESYSTEM
AX_BOOST_PROGRAM_OPTIONS
AX_BOOST_THREAD
AX_BOOST_CHRONO

if test x$use_tests = xyes; then
  AX_BOOST_UNIT_TEST_FRAMEWORK
fi

BOOST_LIBS="$BOOST_LDFLAGS $BOOST_SYSTEM_LIB $BOOST_FILESYSTEM_LIB $BOOST_PROGRAM_OPTIONS_LIB $BOOST_THREAD_LIB"
BOOST_INCLUDES="$BOOST_CPPFLAGS"

dnl Boost >= 1.50 uses sleep_for rather than the now-deprecated sleep, however
dnl it was broken from 1.50 to 1.52 when backed by nanosleep. Use sleep_for if
dnl a working version is available, else fall back to sleep. sleep was removed
dnl after 1.56.
dnl If neither is available, abort.
dnl If sleep_for is used, boost_chrono becomes a requirement.
if test x$ax_cv_boost_chrono = xyes; then
TEMP_LIBS="$LIBS"
LIBS="$BOOST_LIBS $BOOST_CHRONO_LIB $LIBS"
TEMP_CPPFLAGS="$CPPFLAGS"
CPPFLAGS="$CPPFLAGS $BOOST_CPPFLAGS"
AC_LINK_IFELSE([AC_LANG_PROGRAM([[
  #include <boost/thread/thread.hpp>
  #include <boost/version.hpp>
  ]],[[
  #if BOOST_VERSION >= 105000 && (!defined(BOOST_HAS_NANOSLEEP) || BOOST_VERSION >= 105200)
      boost::this_thread::sleep_for(boost::chrono::milliseconds(0));
  #else
   choke me
  #endif
  ]])],
  [boost_sleep=yes; BOOST_LIBS="$BOOST_LIBS $BOOST_CHRONO_LIB";
     AC_DEFINE(HAVE_WORKING_BOOST_SLEEP_FOR, 1, [Define this symbol if boost sleep_for works])],
  [boost_sleep=no])
LIBS="$TEMP_LIBS"
fi

if test x$boost_sleep != xyes; then
TEMP_LIBS="$LIBS"
LIBS="$BOOST_LIBS $LIBS"
TEMP_CPPFLAGS="$CPPFLAGS"
CPPFLAGS="$CPPFLAGS $BOOST_CPPFLAGS"
AC_LINK_IFELSE([AC_LANG_PROGRAM([[
  #include <boost/version.hpp>
  #include <boost/thread.hpp>
  #include <boost/date_time/posix_time/posix_time_types.hpp>
  ]],[[
  #if BOOST_VERSION <= 105600
      boost::this_thread::sleep(boost::posix_time::milliseconds(0));
  #else
   choke me
  #endif
  ]])],
  [boost_sleep=yes; AC_DEFINE(HAVE_WORKING_BOOST_SLEEP, 1, [Define this symbol if boost sleep works])],
  [boost_sleep=no])
LIBS="$TEMP_LIBS"
fi

if test x$boost_sleep != xyes; then
  AC_MSG_ERROR(No working boost sleep implementation found)
fi

build_bitcoind=yes
build_bitcoin_cli=yes
BITCOIN_QT_INIT

if test x$use_pkgconfig = xyes; then

  if test x$PKG_CONFIG == x; then
    AC_MSG_ERROR(pkg-config not found.)]
  fi

  : #NOP
  m4_ifdef(
    [PKG_CHECK_MODULES],
    [
      PKG_CHECK_MODULES([SSL], [libssl], [INCLUDES="$INCLUDES $SSL_CFLAGS"; LIBS="$LIBS $SSL_LIBS"], [AC_MSG_ERROR(openssl  not found.)])
      PKG_CHECK_MODULES([CRYPTO], [libcrypto], [INCLUDES="$INCLUDES $CRYPTO_CFLAGS"; LIBS="$LIBS $CRYPTO_LIBS"], [AC_MSG_ERROR(libcrypto  not found.)])
      if test x$use_qr != xno; then
        BITCOIN_QT_CHECK([PKG_CHECK_MODULES([QR], [libqrencode], [have_qrencode=yes], [have_qrencode=no])])
      fi
    ]
  )
else
  AC_CHECK_HEADER([openssl/crypto.h],,AC_MSG_ERROR(libcrypto headers missing))
  AC_CHECK_LIB([crypto],      [main],, AC_MSG_ERROR(libcrypto missing))

  AC_CHECK_HEADER([openssl/ssl.h],, AC_MSG_ERROR(libssl headers missing),)
  AC_CHECK_LIB([ssl],         [main],, AC_MSG_ERROR(libssl missing))

  if test x$use_qr != xno; then
    BITCOIN_QT_CHECK([AC_CHECK_LIB([qrencode], [main],, [have_qrencode=no])])
    BITCOIN_QT_CHECK([AC_CHECK_HEADER([qrencode.h],, have_qrencode=no)])
  fi
fi

AC_MSG_CHECKING([if bitcoind should be enabled])
AM_CONDITIONAL([BUILD_BITCOIND], [test x$build_bitcoind = xyes])
AC_MSG_RESULT($build_bitcoind)

AC_MSG_CHECKING([if bitcoin-cli should be enabled])
AM_CONDITIONAL([BUILD_BITCOIN_CLI], [test x$build_bitcoin_cli = xyes])
AC_MSG_RESULT($build_bitcoin_cli)

dnl sets $bitcoin_enable_qt, $bitcoin_enable_qt_test, $bitcoin_enable_qt_dbus
BITCOIN_QT_CONFIGURE([$use_pkgconfig], [qt5])

if test x$use_ipv6 = xyes; then
  dnl Check for ipv6 build requirements
  AC_MSG_CHECKING(for IPV6 build support)
  AC_TRY_LINK([
  #if defined(_WINDOWS)
  #include <winsock2.h>
  #else
  #include <sys/types.h>
  #include <sys/socket.h>
  #include <netinet/in.h>
  #endif
  ],[
  #if !defined(_WINDOWS)
  struct in6_addr ipv6Addr;
  struct sockaddr_in6 addr;
  #endif
  int temp = socket(AF_INET6, SOCK_STREAM, 0);],
  [AC_MSG_RESULT(yes); have_ipv6=yes; AC_DEFINE(HAVE_IPV6, 1, [Define this symbol if you have ipv6 build support])],
  [AC_MSG_RESULT(no)]; have_ipv6=no)
fi
AC_LANG_POP

if test "x$use_ccache" != "xno"; then
  AC_MSG_CHECKING(if ccache should be enabled)
  if test x$CCACHE = x; then
    if test "x$use_ccache" = "xyes"; then
      AC_MSG_ERROR([ccache not found.]);
    else
      AC_MSG_NOTICE([ccache not found. Falling back to default CC])
      use_ccache=no
    fi
  else
    use_ccache=yes
    CC="$ac_cv_path_CCACHE $CC"
    CXX="$ac_cv_path_CCACHE $CXX"
  fi
  AC_MSG_RESULT($use_ccache)
fi

dnl enable wallet
AC_MSG_CHECKING([if wallet should be enabled])
if test x$enable_wallet != xno; then
  AC_MSG_RESULT(yes)
  AC_DEFINE_UNQUOTED([ENABLE_WALLET],[1],[Define to 1 to enable wallet functions])

else
  AC_MSG_RESULT(no)

  if test "x$use_qt" != "xno"; then
    AC_MSG_ERROR([Cannot currently build Qt GUI with wallet disabled. Use --without-qt.])
  fi
fi

dnl enable mining
AC_MSG_CHECKING([if mining/minting should be disabled])
if test x$disable_mining != xno; then
  AC_MSG_RESULT(yes)
  AC_DEFINE_UNQUOTED([DISABLE_MINING],[1],[Define to 1 to disable mining/minting functions])

else
  AC_MSG_RESULT(no)
fi

dnl enable ipv6 support
AC_MSG_CHECKING([if ipv6 should be enabled])
if test x$have_ipv6 = xno; then
  if test x$use_ipv6 = xyes; then
     AC_MSG_ERROR("ipv6 requested but cannot be built. use --disable-ipv6")
  fi
  AC_MSG_RESULT(no)
else
  if test x$use_ipv6 = xyes; then
      AC_MSG_RESULT(yes)
      AC_DEFINE([USE_IPV6],[1],[Define if ipv6 support should be compiled in])
  else
      AC_MSG_RESULT(no)
  fi
fi

dnl enable upnp support
AC_MSG_CHECKING([if upnp should be enabled])
if test x$have_miniupnpc = xno; then
  if test x$use_upnp = xyes; then
     AC_MSG_ERROR("upnp requested but cannot be built. use --without-miniupnpc")
  fi
  AC_MSG_RESULT(no)
else
  if test x$use_upnp != xno; then
    AC_MSG_RESULT(yes)
    AC_MSG_CHECKING([if upnp should be on by default])
    use_upnp=yes
    upnp_setting=0
    if test x$use_upnp_default != xno; then
      use_upnp_default=yes
      upnp_setting=1
    fi
    AC_MSG_RESULT($use_upnp_default)
    AC_DEFINE_UNQUOTED([USE_UPNP],[$upnp_setting],[Define to 1 for upnp runtime support])
    if test x$TARGET_OS = xwindows; then
      CPPFLAGS="$CPPFLAGS -DSTATICLIB"
    fi
  else
    AC_MSG_RESULT(no)
  fi
fi

dnl these are only used when qt is enabled
if test x$bitcoin_enable_qt != xno; then
  CPPFLAGS="$CPPFLAGS -DQT_GUI"
  BUILD_QT=qt
  dnl enable dbus support
  AC_MSG_CHECKING([if dbus should be enabled])
  if test x$bitcoin_enable_qt_dbus != xno; then
    AC_DEFINE([USE_DBUS],[1],[Define if dbus support should be compiled in])
  fi
  AC_MSG_RESULT($bitcoin_enable_qt_dbus)

  dnl enable qr support
  AC_MSG_CHECKING([if qr should be enabled])
  if test x$have_qrencode = xno; then
    if test x$use_qr == xyes; then
     AC_MSG_ERROR("QR support requested but cannot be built. use --without-qrencode")
    fi
    AC_MSG_RESULT(no)
  else
    if test x$use_qr != xno; then
      AC_MSG_RESULT(yes)
      AC_DEFINE([USE_QRCODE],[1],[Define if QR support should be compiled in])
      use_qr=yes
    else
      AC_MSG_RESULT(no)
    fi
  fi

  if test x$XGETTEXT == x; then
    AC_MSG_WARN("xgettext is required to update qt translations")
  fi

  if test x$use_tests = xyes; then
    BUILD_TEST_QT="test"
    if test x$bitcoin_enable_qt_test != xyes; then
      AC_MSG_ERROR("Qt Test lib not found. Use --disable-tests or --without-gui.")
    fi
  fi
  if test x$enable_wallet == xno; then
    AC_MSG_ERROR([Cannot currently build Qt GUI with wallet disabled. Use --without-qt.])
  fi
fi

if test x$use_tests = xyes; then
  BUILD_TEST="test"
fi

if test "x$use_tests$build_bitcoind$use_qt" = "xnonono"; then
  AC_MSG_ERROR([No targets! Please specify at least one of: --enable-cli --enable-daemon --enable-gui or --enable-tests])
fi

AM_CONDITIONAL([TARGET_DARWIN], [test x$TARGET_OS = xdarwin])
AM_CONDITIONAL([BUILD_DARWIN], [test x$BUILD_OS = xdarwin])
AM_CONDITIONAL([TARGET_WINDOWS], [test x$TARGET_OS = xwindows])
AM_CONDITIONAL([ENABLE_WALLET],[test x$enable_wallet == xyes])
AM_CONDITIONAL([DISABLE_MINING],[test x$disable_mining == xyes])
AM_CONDITIONAL([ENABLE_TESTS],[test x$use_tests == xyes])
AM_CONDITIONAL([ENABLE_QT],[test x$bitcoin_enable_qt == xyes])
AM_CONDITIONAL([ENABLE_QT_TESTS],[test x$use_tests$bitcoin_enable_qt_test = xyesyes])
AM_CONDITIONAL([USE_QRCODE], [test x$use_qr = xyes])
AM_CONDITIONAL([USE_LCOV],[test x$use_lcov == xyes])
AM_CONDITIONAL([USE_COMPARISON_TOOL],[test x$use_comparison_tool != xno])

AC_DEFINE(CLIENT_VERSION_MAJOR, _CLIENT_VERSION_MAJOR, [Major version])
AC_DEFINE(CLIENT_VERSION_MINOR, _CLIENT_VERSION_MINOR, [Minor version])
AC_DEFINE(CLIENT_VERSION_REVISION, _CLIENT_VERSION_REVISION, [Build revision])
AC_DEFINE(CLIENT_VERSION_BUILD, _CLIENT_VERSION_BUILD, [Version Build])
AC_DEFINE(CLIENT_VERSION_IS_RELEASE, _CLIENT_VERSION_IS_RELEASE, [Version is release])
AC_DEFINE(COPYRIGHT_YEAR, _COPYRIGHT_YEAR, [Version is release])
AC_SUBST(CLIENT_VERSION_MAJOR, _CLIENT_VERSION_MAJOR)
AC_SUBST(CLIENT_VERSION_MINOR, _CLIENT_VERSION_MINOR)
AC_SUBST(CLIENT_VERSION_REVISION, _CLIENT_VERSION_REVISION)
AC_SUBST(CLIENT_VERSION_BUILD, _CLIENT_VERSION_BUILD)
AC_SUBST(CLIENT_VERSION_IS_RELEASE, _CLIENT_VERSION_IS_RELEASE)
AC_SUBST(COPYRIGHT_YEAR, _COPYRIGHT_YEAR)

AC_SUBST(RELDFLAGS)
AC_SUBST(HARDENED_CXXFLAGS)
AC_SUBST(HARDENED_CPPFLAGS)
AC_SUBST(HARDENED_LDFLAGS)
AC_SUBST(CXXFLAGS)
AC_SUBST(CPPFLAGS)
AC_SUBST(LDFLAGS)
AC_SUBST(PIC_FLAGS)
AC_SUBST(PIE_FLAGS)

AC_SUBST(USE_UPNP)
AC_SUBST(USE_QRCODE)
AC_SUBST(USE_IPV6)
AC_SUBST(INCLUDES)
AC_SUBST(BOOST_LIBS)
AC_SUBST(MOC_DEFS)
AC_SUBST(QT_INCLUDES)
AC_SUBST(QT_TEST_LIBS)
AC_SUBST(QT_LIBS)
AC_SUBST(QT_DBUS_LIBS)
AC_SUBST(QT_DBUS_INCLUDES)
AC_SUBST(QT_TEST_INCLUDES)
AC_SUBST(TESTDEFS)
AC_SUBST(LEVELDB_TARGET_FLAGS)
AC_SUBST(BUILD_QT)
AC_SUBST(BUILD_TEST)
AC_SUBST(BUILD_TEST_QT)
AC_CONFIG_FILES([Makefile src/Makefile share/setup.nsi share/qt/Info.plist])
AC_CONFIG_FILES([qa/pull-tester/run-bitcoind-for-test.sh],[chmod +x qa/pull-tester/run-bitcoind-for-test.sh])
AC_CONFIG_FILES([qa/pull-tester/build-tests.sh],[chmod +x qa/pull-tester/build-tests.sh])
AC_OUTPUT
