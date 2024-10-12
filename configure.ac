dnl#                                               -*- Autoconf -*-
dnl# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([nodetcl],[1.0.1],[jeff@bovine.net])
AC_CONFIG_SRCDIR([src/nodetcl.cc])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_AUX_DIR([autotools_build-aux])
AC_CONFIG_MACRO_DIR([autotools_m4])
AC_CANONICAL_TARGET
case ${host} in
  *-*-*freebsd*)
    export CPPFLAGS="${CPPFLAGS} -I/usr/local/include/tcl8.5"
    ;;
  *-*-*darwin*)
    if test -d /opt/local/include; then
      export CPPFLAGS="${CPPFLAGS} -I/opt/local/include"
      if test -d /opt/local/include/node; then
        export CPPFLAGS="${CPPFLAGS} -I/opt/local/include/node"
        if test -d /opt/local/include/node/uv-private; then
          export CPPFLAGS="${CPPFLAGS} -I/opt/local/include/node/uv-private"
        fi
      elif test -d /opt/local/include/node8; then
        export CPPFLAGS="${CPPFLAGS} -I/opt/local/include/node8"
        if test -d /opt/local/include/node8/uv-private; then
          export CPPFLAGS="${CPPFLAGS} -I/opt/local/include/node8/uv-private"
        fi
      elif test -d /opt/local/include/node08; then
        export CPPFLAGS="${CPPFLAGS} -I/opt/local/include/node08"
        if test -d /opt/local/include/node08/uv-private; then
          export CPPFLAGS="${CPPFLAGS} -I/opt/local/include/node08/uv-private"
        fi
      elif test -d /opt/local/libexec/v8-8.3/include; then
        export CPPFLAGS="${CPPFLAGS} -I/opt/local/libexec/v8-8.3/include"
      elif test -d /opt/local/libexec/v8-11.6/include; then
        export CPPFLAGS="${CPPFLAGS} -I/opt/local/libexec/v8-11.6/include"
      else
        AC_MSG_WARN([you may need to install some additional node or v8-related headers])
      fi
      if test -d /opt/local/include/spandsp; then
        export CPPFLAGS="${CPPFLAGS} -I/opt/local/include/spandsp"
        if test -d /opt/local/include/spandsp/private; then
          export CPPFLAGS="${CPPFLAGS} -I/opt/local/include/spandsp/private"
        fi
      fi
    elif test -d /opt/sw/include; then
      export CPPFLAGS="${CPPFLAGS} -I/opt/sw/include"
      if test -d /opt/sw/include/node; then
        export CPPFLAGS="${CPPFLAGS} -I/opt/sw/include/node"
      fi
      if test -d /opt/sw/include/nodejs/src; then
        export CPPFLAGS="${CPPFLAGS} -I/opt/sw/include/nodejs/src"
      fi
      if test -d /opt/sw/include/nodejs/deps/v8/include; then
        export CPPFLAGS="${CPPFLAGS} -I/opt/sw/include/nodejs/deps/v8/include"
      fi
    elif test -d /sw/include; then
      export CPPFLAGS="${CPPFLAGS} -I/sw/include"
    elif test -d /opt/homebrew/include; then
      export CPPFLAGS="${CPPFLAGS} -I/opt/homebrew/include"
    elif test -d /usr/local/include; then
      export CPPFLAGS="${CPPFLAGS} -I/usr/local/include"
    elif test -d /opt/la/include; then
      export CPPFLAGS="${CPPFLAGS} -I/opt/la/include"
    else
      AC_MSG_WARN([you may need to install a system package manager])
    fi
    test -n "${CPPFLAGS}" && echo "CPPFLAGS are now: ${CPPFLAGS}"
    export AM_LDFLAGS="-bundle -undefined dynamic_lookup"
    AC_SUBST([AM_LDFLAGS])
    if test -d /opt/local/lib; then
      export LDFLAGS="${LDFLAGS} -L/opt/local/lib"
      if test -d /opt/local/lib/node8; then
        export LDFLAGS="${LDFLAGS} -L/opt/local/lib/node8"
      elif test -d /opt/local/lib/node08; then
        export LDFLAGS="${LDFLAGS} -L/opt/local/lib/node08"
      elif test -d /opt/local/libexec/v8-8.3/lib; then
        export LDFLAGS="${LDFLAGS} -L/opt/local/libexec/v8-8.3/lib"
      elif test -d /opt/local/libexec/v8-11.6/lib; then
        export LDFLAGS="${LDFLAGS} -L/opt/local/libexec/v8-11.6/lib"
      else
        AC_MSG_WARN([you may need to install some additional node or v8-related libraries])
      fi
    fi
    test -n "${LDFLAGS}" && echo "LDFLAGS are now: ${LDFLAGS}"
    ;;
esac
AC_SYS_LARGEFILE
AM_INIT_AUTOMAKE([foreign subdir-objects])
AM_MAINTAINER_MODE

# Checks for programs.
AM_PROG_AR
if test "x${AR}" = "x"; then
  test -z "${AR}" || unset AR
  AC_CHECK_TOOLS([AR],[ar gar])dnl
  ## might want to switch around these checks...
  if test "x${ac_cv_prog_AR}" = "x"; then
    test -z "${ac_cv_prog_AR}" || unset ac_cv_prog_AR
    m4_ifdef([AC_PROG_AR],[
      AC_PROG_AR
    ],[
      test -x "`which ar`" && echo "first usable ar in path is `which ar`"
    ])
  fi
else
  test -n "${AR}" && export AR
fi
AC_PROG_RANLIB
AC_PROG_CPP
AC_LANG([C++])
AC_PROG_CXX
AC_PROG_CXXCPP
AC_PROG_OBJCXX
AC_PROG_OBJCXXCPP
AC_PROG_INSTALL
AM_PROG_INSTALL_STRIP
AM_PATH_PYTHON
# JS Interface
if test "x${srcdir}" != "x" && test -d "${srcdir}"; then
  test -n "${srcdir}" && export npm_path="${srcdir}/node_modules/.bin"
else
  test -z "${srcdir}" && test -d . && export npm_path="./node_modules/.bin"
fi
if test -d "${npm_path}"; then
  # We need an absolute path to the npm programs because they are run
  # from inside the subdirectory.
  npm_path="`cd ${npm_path} && pwd`"
  if test "x${PATH_SEPARATOR}" != "x"; then
    test -n "${PATH_SEPARATOR}" && npm_path="${npm_path}${PATH_SEPARATOR}${PATH}"
  else
    test -z "${PATH_SEPARATOR}" && export npm_path="${npm_path}:${PATH}"
  fi
elif test -n "${PATH}"; then
  export npm_path="${PATH}"
elif test -z "${PATH}"; then
  AC_MSG_WARN([PATH is unset!])
  export npm_path="."
else
  AC_MSG_WARN([npm_path will be empty!])
  export npm_path=""
fi
AC_PATH_PROGS([NODE_BIN],[node node8 node08],[],[${npm_path}:/opt/local/bin])
AC_PATH_PROGS([NODE_WAF],[node-waf node-waf-1.6.2],[],
              [${npm_path}:/opt/local/bin:/usr/local/bin:/opt/sw/bin])
if test "x${NODE_WAF}" = "x"; then
	test -z "${NODE_WAF}" && echo "ERROR! NODE_WAF is unset!" >&2
	AC_MSG_ERROR([node-waf not found; it is required to build])
fi
AC_SUBST([NODE_WAF])dnl
AC_PATH_PROGS([WAF],[waf waf-1.6.2],[],[${npm_path}:.])dnl

# Try using libtool for linking:
LT_INIT([dlopen])
LT_LANG([C++])dnl

# Checks for libraries.
AC_SEARCH_LIBS([Tcl_Init],[Tcl Tcl85 Tcl8.5])

if test "x${acl_cv_wl}" = "x"; then
  test -z "${acl_cv_wl}" && export acl_cv_wl='-Wl,'
fi
AC_SUBST([acl_cv_wl])dnl

AC_MSG_CHECKING([for linker flag to verify linked libraries have the correct architecture])
pre_saved_LDFLAGS="${LDFLAGS}"
LDFLAGS="${LDFLAGS} ${acl_cv_wl}-arch_errors_fatal"
AC_LINK_IFELSE([AC_LANG_PROGRAM([[]],[[]])],
               [LD_ARCHFLAGS="${acl_cv_wl}-arch_errors_fatal"],[])
LDFLAGS="${pre_saved_LDFLAGS}"
if test "x${LD_ARCHFLAGS}" != "x"; then
  test -n "${LD_ARCHFLAGS}"
  AC_MSG_RESULT([${LD_ARCHFLAGS}])
  if test "x${ERROR_ON_WARNING}" = "xyes"; then
    if test "x${WARN_LDFLAGS}" != "x"; then
      test -n "${WARN_LDFLAGS}" && WARN_LDFLAGS="${WARN_LDFLAGS} ${LD_ARCHFLAGS}"
    else
      test -z "${WARN_LDFLAGS}" && WARN_LDFLAGS="${LD_ARCHFLAGS}"
    fi
  fi
  AC_SUBST([LD_ARCHFLAGS])dnl
  ## that should be all we need to do with them for now...
else
  test -z "${LD_ARCHFLAGS}"
  AC_MSG_RESULT([none])
fi

PRE_V8_LDFLAGS="${LDFLAGS}"
LDFLAGS="${LDFLAGS} ${LD_ARCHFLAGS}"
# FIXME: Replace `main' with a function in `-lv8':
AC_CHECK_LIB([v8],[main])
# FIXME: Replace `main' with a function in `-lv8_base':
AC_CHECK_LIB([v8_base],[main])
# FIXME: Replace `main' with a function in `-lv8_nosnapshot':
AC_CHECK_LIB([v8_nosnapshot],[main])
# FIXME: Replace `main' with a function in `-lv8_snapshot':
AC_CHECK_LIB([v8_snapshot],[main])dnl

LDFLAGS="${PRE_V8_LDFLAGS}"
AC_SUBST([PRE_V8_LDFLAGS])dnl

# Checks for header files.
AC_CHECK_HEADERS([node.h tcl.h v8.h])
AC_CHECK_HEADERS_ONCE([node/node.h node/v8.h])
AC_CHECK_HEADERS_ONCE([node8/node.h node8/v8.h])
AC_CHECK_HEADERS_ONCE([node08/node.h node08/v8.h])dnl

# Checks for typedefs, structures, and compiler characteristics.
AC_C_BIGENDIAN
AC_C_CONST
AC_C_PROTOTYPES

# Checks for library functions.
AC_FUNC_MALLOC

# Output.
AC_CONFIG_FILES([Makefile])
AC_CONFIG_COMMANDS([node-waf],[
if test -n "${NODE_WAF}" && test -r "${NODE_WAF}" && test -s "${NODE_WAF}" && test -x "${NODE_WAF}"; then
  echo "NODE_WAF is '${NODE_WAF}'" && ${NODE_WAF} configure
elif test "x${NODE_WAF}" = "x"; then
  test -z "${NODE_WAF}" && echo "NODE_WAF is unset, so skipping this..."
fi
])
AC_OUTPUT
