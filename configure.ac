
AC_INIT([Fire Hydrant Minecraft Load Balancer], [0.1], [Broken Worlds LLC. <info@brokenworldsmc.com>], [firehydrantd])
AC_COPYRIGHT([(c) 2012 Broken Worlds LLC. All Rights Reserved])

AC_CONFIG_SRCDIR(firehydrantd.c)

AM_INIT_AUTOMAKE
AC_PROG_CC
AC_LANG_C

dnl ---------------------------------------------------------------------------
dnl 
AC_ARG_ENABLE(64bit, [AS_HELP_STRING([--enable-64bit], [build 64bit version])])
if test "x$enable_64bit" = "xyes"
then
    org_cflags=$CFLAGS
    CFLAGS=-m64
    AC_RUN_IFELSE(
      [AC_LANG_PROGRAM([], [dnl
return sizeof(void*) == 8 ? 0 : 1;
      ])
    ],[
      CFLAGS="-m64 $org_cflags"
    ],[
    AC_MSG_ERROR([Dont know how to build a 64-bit object.])
    ])
fi

dnl ---------------------------------------------------------------------------
dnl libevent stuff from memcached

# Issue 213: Search for clock_gettime to help people linking
#            with a static version of libevent
AC_SEARCH_LIBS(clock_gettime, rt)
# Issue 214: Search for the network libraries _before_ searching
#            for libevent (to help people linking with static libevent)
AC_SEARCH_LIBS(socket, socket)
AC_SEARCH_LIBS(gethostbyname, nsl)

trylibeventdir=""
AC_ARG_WITH(libevent,
       [  --with-libevent=PATH     Specify path to libevent installation ],
       [
                if test "x$withval" != "xno" ; then
                        trylibeventdir=$withval
                fi
       ]
)

dnl ---------------------------------------------------------------------------
dnl libevent detection.  swiped from Tor.  modified a bit.

LIBEVENT_URL=http://www.monkey.org/~provos/libevent/

AC_CACHE_CHECK([for libevent directory], ac_cv_libevent_dir, [
  saved_LIBS="$LIBS"
  saved_LDFLAGS="$LDFLAGS"
  saved_CPPFLAGS="$CPPFLAGS"
  le_found=no
  for ledir in $trylibeventdir "" $prefix /usr/local ; do
    LDFLAGS="$saved_LDFLAGS"
    LIBS="-levent $saved_LIBS"

    # Skip the directory if it isn't there.
    if test ! -z "$ledir" -a ! -d "$ledir" ; then
       continue;
    fi
    if test ! -z "$ledir" ; then
      if test -d "$ledir/lib" ; then
        LDFLAGS="-L$ledir/lib $LDFLAGS"
      else
        LDFLAGS="-L$ledir $LDFLAGS"
      fi
      if test -d "$ledir/include" ; then
        CPPFLAGS="-I$ledir/include $CPPFLAGS"
      else
        CPPFLAGS="-I$ledir $CPPFLAGS"
      fi
    fi
    # Can I compile and link it?
    AC_TRY_LINK([#include <sys/time.h>
#include <sys/types.h>
#include <event.h>], [ event_init(); ],
       [ libevent_linked=yes ], [ libevent_linked=no ])
    if test $libevent_linked = yes; then
       if test ! -z "$ledir" ; then
         ac_cv_libevent_dir=$ledir
         _myos=`echo $target_os | cut -f 1 -d .`
         AS_IF(test "$SUNCC" = "yes" -o "x$_myos" = "xsolaris2",
               [saved_LDFLAGS="$saved_LDFLAGS -Wl,-R$ledir/lib"],
               [AS_IF(test "$GCC" = "yes",
                     [saved_LDFLAGS="$saved_LDFLAGS -Wl,-rpath,$ledir/lib"])])
       else
         ac_cv_libevent_dir="(system)"
       fi
       le_found=yes
       break
    fi
  done
  LIBS="$saved_LIBS"
  LDFLAGS="$saved_LDFLAGS"
  CPPFLAGS="$saved_CPPFLAGS"
  if test $le_found = no ; then
    AC_MSG_ERROR([libevent is required.  You can get it from $LIBEVENT_URL

      If its already installed, specify its path using --with-libevent=/dir/
])
  fi
])
LIBS="-levent $LIBS"
if test $ac_cv_libevent_dir != "(system)"; then
  if test -d "$ac_cv_libevent_dir/lib" ; then
    LDFLAGS="-L$ac_cv_libevent_dir/lib $LDFLAGS"
    le_libdir="$ac_cv_libevent_dir/lib"
  else
    LDFLAGS="-L$ac_cv_libevent_dir $LDFLAGS"
    le_libdir="$ac_cv_libevent_dir"
  fi
  if test -d "$ac_cv_libevent_dir/include" ; then
    CPPFLAGS="-I$ac_cv_libevent_dir/include $CPPFLAGS"
  else
    CPPFLAGS="-I$ac_cv_libevent_dir $CPPFLAGS"
  fi
fi

dnl ---------------------------------------------------------------------------
dnl OpenSSL

AC_MSG_CHECKING([for --with-openssl])
AC_ARG_WITH(
    [openssl],
    [AS_HELP_STRING([--with-openssl],[/absolute/path/to/openssl-base])],
    [ LDFLAGS="$LDFLAGS -L$with_openssl/lib";
      CFLAGS="$CFLAGS -I$with_openssl/include/";
      LIBS="-lssl";
      AC_MSG_RESULT([$with_openssl])
    ],
    [AC_MSG_RESULT([no])]
)

AC_CHECK_LIB([crypto],[CRYPTO_new_ex_data], [], [AC_MSG_ERROR([OpenSSL libraries required])])
AC_CHECK_LIB([ssl],[SSL_library_init], [], [AC_MSG_ERROR([OpenSSL libraries required])])

AC_CHECK_HEADERS([openssl/crypto.h openssl/x509.h openssl/pem.h openssl/ssl.h openssl/err.h],[],[AC_MSG_ERROR([OpenSSL headers required])])

dnl ---------------------------------------------------------------------------
dnl Finish Up

AC_PROG_INSTALL
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
