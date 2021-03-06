dnl Process this file with autoconf to produce a configure script.

AC_PREREQ(2.57)
AC_INIT(libibmad, 1.3.13, linux-rdma@vger.kernel.org)
AC_CONFIG_SRCDIR([src/sa.c])
AC_CONFIG_AUX_DIR(config)
AC_CONFIG_MACRO_DIR(config)
AM_CONFIG_HEADER(config.h)
AM_INIT_AUTOMAKE([subdir-objects])

AC_SUBST(RELEASE, ${RELEASE:-unknown})
AC_SUBST(TARBALL, ${TARBALL:-${PACKAGE}-${VERSION}.tar.gz})

dnl the library version info is available in the file: libibmad.ver
ibmad_api_version=`grep LIBVERSION $srcdir/libibmad.ver | sed 's/LIBVERSION=//'`
if test -z $ibmad_api_version; then
   ibmad_api_version=1:0:0
fi
AC_SUBST(ibmad_api_version)

AC_ARG_ENABLE(libcheck, [  --disable-libcheck      do not test for presence of ib libraries],
[       if test x$enableval = xno ; then
                disable_libcheck=yes
        fi
])

AM_PROG_LIBTOOL

dnl Checks for programs
AC_PROG_CC

dnl Checks for libraries
if test "$disable_libcheck" != "yes"
then
AC_CHECK_LIB(ibumad, umad_init, [],
	AC_MSG_ERROR([umad_init() not found. libibmad requires libibumad.]))
fi

dnl Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([netinet/in.h stdlib.h string.h sys/time.h unistd.h])
if test "$disable_libcheck" != "yes"
then
AC_CHECK_HEADER(infiniband/umad.h, [],
	AC_MSG_ERROR([<infiniband/umad.h> not found. libibmad requires libibumad.])
)
fi

dnl Checks for library functions
AC_CHECK_FUNCS([memset strrchr strtol])

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_C_INLINE

AC_CACHE_CHECK(whether ld accepts --version-script, ac_cv_version_script,
    if test -n "`$LD --help < /dev/null 2>/dev/null | grep version-script`"; then
        ac_cv_version_script=yes
    else
        ac_cv_version_script=no
    fi)

AM_CONDITIONAL(HAVE_LD_VERSION_SCRIPT, test "$ac_cv_version_script" = "yes")

AC_CONFIG_FILES([Makefile libibmad.spec])
AC_OUTPUT
