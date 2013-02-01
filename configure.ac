
AC_INIT([OpenGLSandbox], [0.1], [AegonTheMad], [openglsandbox])
AC_COPYRIGHT([(c) 2013 AegonTheMad])

AC_CONFIG_SRCDIR(src)

AM_INIT_AUTOMAKE
AC_PROG_CC
AC_LANG_C

dnl ---------------------------------------------------------------------------
dnl 
dnl AC_ARG_ENABLE(64bit, [AS_HELP_STRING([--enable-64bit], [build 64bit version])])
dnl if test "x$enable_64bit" = "xyes"
dnl then
dnl     org_cflags=$CFLAGS
dnl     CFLAGS=-m64
dnl     AC_RUN_IFELSE(
dnl       [AC_LANG_PROGRAM([], [dnl
dnl return sizeof(void*) == 8 ? 0 : 1;
dnl       ])
dnl     ],[
dnl       CFLAGS="-m64 $org_cflags"
dnl     ],[
dnl     AC_MSG_ERROR([Dont know how to build a 64-bit object.])
dnl     ])
dnl fi
dnl 
dnl ---------------------------------------------------------------------------
dnl libevent stuff from memcached

dnl # Issue 213: Search for clock_gettime to help people linking
dnl #            with a static version of libevent
dnl AC_SEARCH_LIBS(clock_gettime, rt)
dnl # Issue 214: Search for the network libraries _before_ searching
dnl #            for libevent (to help people linking with static libevent)
dnl AC_SEARCH_LIBS(socket, socket)
dnl AC_SEARCH_LIBS(gethostbyname, nsl)

dnl trylibeventdir=""
dnl AC_ARG_WITH(libevent,
dnl        [  --with-libevent=PATH     Specify path to libevent installation ],
dnl        [
dnl                 if test "x$withval" != "xno" ; then
dnl                         trylibeventdir=$withval
dnl                 fi
dnl        ]
dnl )

dnl ---------------------------------------------------------------------------
dnl libevent detection.  swiped from Tor.  modified a bit.

dnl LIBEVENT_URL=http://www.monkey.org/~provos/libevent/

dnl AC_CACHE_CHECK([for libevent directory], ac_cv_libevent_dir, [
dnl   saved_LIBS="$LIBS"
dnl   saved_LDFLAGS="$LDFLAGS"
dnl   saved_CPPFLAGS="$CPPFLAGS"
dnl   le_found=no
dnl   for ledir in $trylibeventdir "" $prefix /usr/local ; do
dnl     LDFLAGS="$saved_LDFLAGS"
dnl     LIBS="-levent $saved_LIBS"
dnl 
dnl     # Skip the directory if it isn't there.
dnl     if test ! -z "$ledir" -a ! -d "$ledir" ; then
dnl        continue;
dnl     fi
dnl     if test ! -z "$ledir" ; then
dnl       if test -d "$ledir/lib" ; then
dnl         LDFLAGS="-L$ledir/lib $LDFLAGS"
dnl       else
dnl         LDFLAGS="-L$ledir $LDFLAGS"
dnl       fi
dnl       if test -d "$ledir/include" ; then
dnl         CPPFLAGS="-I$ledir/include $CPPFLAGS"
dnl       else
dnl         CPPFLAGS="-I$ledir $CPPFLAGS"
dnl       fi
dnl     fi
dnl     # Can I compile and link it?
dnl     AC_TRY_LINK([#include <sys/time.h>
dnl #include <sys/types.h>
dnl #include <event.h>], [ event_init(); ],
dnl        [ libevent_linked=yes ], [ libevent_linked=no ])
dnl     if test $libevent_linked = yes; then
dnl        if test ! -z "$ledir" ; then
dnl          ac_cv_libevent_dir=$ledir
dnl          _myos=`echo $target_os | cut -f 1 -d .`
dnl          AS_IF(test "$SUNCC" = "yes" -o "x$_myos" = "xsolaris2",
dnl                [saved_LDFLAGS="$saved_LDFLAGS -Wl,-R$ledir/lib"],
dnl                [AS_IF(test "$GCC" = "yes",
dnl                      [saved_LDFLAGS="$saved_LDFLAGS -Wl,-rpath,$ledir/lib"])])
dnl        else
dnl          ac_cv_libevent_dir="(system)"
dnl        fi
dnl        le_found=yes
dnl        break
dnl     fi
dnl   done
dnl   LIBS="$saved_LIBS"
dnl   LDFLAGS="$saved_LDFLAGS"
dnl   CPPFLAGS="$saved_CPPFLAGS"
dnl   if test $le_found = no ; then
dnl     AC_MSG_ERROR([libevent is required.  You can get it from $LIBEVENT_URL
dnl 
dnl       If its already installed, specify its path using --with-libevent=/dir/
dnl ])
dnl   fi
dnl ])
dnl LIBS="-levent $LIBS"
dnl if test $ac_cv_libevent_dir != "(system)"; then
dnl   if test -d "$ac_cv_libevent_dir/lib" ; then
dnl     LDFLAGS="-L$ac_cv_libevent_dir/lib $LDFLAGS"
dnl     le_libdir="$ac_cv_libevent_dir/lib"
dnl   else
dnl     LDFLAGS="-L$ac_cv_libevent_dir $LDFLAGS"
dnl     le_libdir="$ac_cv_libevent_dir"
dnl   fi
dnl   if test -d "$ac_cv_libevent_dir/include" ; then
dnl     CPPFLAGS="-I$ac_cv_libevent_dir/include $CPPFLAGS"
dnl   else
dnl     CPPFLAGS="-I$ac_cv_libevent_dir $CPPFLAGS"
dnl   fi
dnl fi

dnl ---------------------------------------------------------------------------
dnl OpenSSL

dnl AC_MSG_CHECKING([for --with-openssl])
dnl AC_ARG_WITH(
dnl     [openssl],
dnl     [AS_HELP_STRING([--with-openssl],[/absolute/path/to/openssl-base])],
dnl     [ LDFLAGS="$LDFLAGS -L$with_openssl/lib";
dnl       CFLAGS="$CFLAGS -I$with_openssl/include/";
dnl       LIBS="-lssl";
dnl       AC_MSG_RESULT([$with_openssl])
dnl     ],
dnl     [AC_MSG_RESULT([no])]
dnl )
dnl 
dnl AC_CHECK_LIB([crypto],[CRYPTO_new_ex_data], [], [AC_MSG_ERROR([OpenSSL libraries required])])
dnl AC_CHECK_LIB([ssl],[SSL_library_init], [], [AC_MSG_ERROR([OpenSSL libraries required])])
dnl 
dnl AC_CHECK_HEADERS([openssl/crypto.h openssl/x509.h openssl/pem.h openssl/ssl.h openssl/err.h],[],[AC_MSG_ERROR([OpenSSL dnl headers required])])

dnl ---------------------------------------------------------------------------
dnl Finish Up

AC_PROG_INSTALL
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
