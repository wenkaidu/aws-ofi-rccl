#
# Copyright (c) 2018-2019, Amazon.com, Inc. or its affiliates. All rights reserved.
# Modifications Copyright (c) 2022, Advanced Micro Devices, Inc. All rights reserved.
#
# See LICENSE.txt for license information
#

AC_PREREQ([2.69])
AC_INIT([aws-ofi-rccl], [1.2.0], [rashika@amazon.com], , [https://github.com/ROCmSoftwarePlatform/aws-ofi-rccl])
AC_CONFIG_SRCDIR([src/nccl_ofi_net.c])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIRS([m4])
AM_SILENT_RULES([yes])

# Checks for programs.
AC_PROG_CC
AC_PROG_MAKE_SET
AM_PROG_AR

# Checks for typedefs, structures, and compiler characteristics.
AC_CHECK_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T
AC_TYPE_UINT64_T
AC_FUNC_MALLOC
AC_CHECK_FUNCS([memset realpath])

# Checks for libraries.
AC_ARG_WITH([libfabric],
            AC_HELP_STRING([--with-libfabric=PATH], [Path to non-standard libfabric installation]),
            [AS_IF([test -d $withval/lib64], [ofi_libdir="lib64"], [ofi_libdir="lib"])
             CFLAGS="-I$withval/include $CFLAGS"
             LDFLAGS="-L$withval/$ofi_libdir $LDFLAGS"],
            [])

AC_DEFINE([__HIP_PLATFORM_HCC__], [], [Use HCC Platform])
AC_ARG_WITH([hip],
            AC_HELP_STRING([--with-hip=PATH], [Path to non-standard HIP installation]),
            [AS_IF([test -d $withval/lib], [hip_libdir="lib"], [hip_libdir="lib64"])
             CFLAGS="-I$withval/include $CFLAGS"
             LDFLAGS="-L$withval/$hip_libdir $LDFLAGS"],
            [CFLAGS="-I/opt/rocm/include $CFLAGS"
             LDFLAGS="-L/opt/rocm/lib $LDFLAGS"],
            [])

AC_ARG_WITH([rccl],
            AC_HELP_STRING([--with-rccl=PATH], [Path to non-standard NCCL installation]),
            [AS_IF([test -d $withval/lib64], [rccl_libdir="lib64"], [rccl_libdir="lib"])
             CFLAGS="-I/opt/rocm/include $CFLAGS"
             LDFLAGS="-L/opt/rocm/lib $LDFLAGS"],
            [])

AC_ARG_WITH([mpi],
            AC_HELP_STRING([--with-mpi=PATH], [Path to non-standard MPI installation]),
            [AS_IF([test -d $withval/lib64], [mpi_libdir="lib64"], [mpi_libdir="lib"])
             CFLAGS="-I$withval/include $CFLAGS"
             LDFLAGS="-L$withval/$mpi_libdir $LDFLAGS"],
            [])

AC_ARG_ENABLE(trace, [AS_HELP_STRING([--enable-trace], [Enable printing trace messages])], [], [enable_trace=no])
AC_MSG_CHECKING([whether to enable trace messages])
AS_IF([test "x${enable_trace}" = "xyes" ],
      [trace=1
       AC_MSG_RESULT(yes)],
       [trace=0
       AC_MSG_RESULT(no)])
AC_DEFINE_UNQUOTED([OFI_NCCL_TRACE], [${trace}], [Defined to 1 if aws-ofi-rccl was configured with --enable-trace, 0 otherwise])

# Checks for header files.
AC_CHECK_HEADERS([limits.h stdlib.h string.h unistd.h rdma/fabric.h hip/hip_runtime.h], [],[
	AC_MSG_ERROR([unable to find required headers])
])

AC_SEARCH_LIBS([dlopen], [dl], [], [
  AC_MSG_ERROR([unable to find libdl])
])
AC_SEARCH_LIBS([fi_getinfo], [fabric], [], [
  AC_MSG_ERROR([unable to find libfabric])
])

AM_INIT_AUTOMAKE([-Wall -Werror foreign subdir-objects])
AC_CONFIG_FILES([Makefile
                 src/Makefile
                 tests/Makefile])
LT_INIT([shared disable-static])
AC_OUTPUT
echo "*"
echo "* AWS OFI RCCL plugin has been configured."
echo "*"
