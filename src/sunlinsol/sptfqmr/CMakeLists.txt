# ---------------------------------------------------------------
# Programmer(s): Daniel R. Reynolds @ SMU
# ---------------------------------------------------------------
# SUNDIALS Copyright Start
# Copyright (c) 2002-2020, Lawrence Livermore National Security
# and Southern Methodist University.
# All rights reserved.
#
# See the top-level LICENSE and NOTICE files for details.
#
# SPDX-License-Identifier: BSD-3-Clause
# SUNDIALS Copyright End
# ---------------------------------------------------------------
# CMakeLists.txt file for the SPTFQMR SUNLinearSolver library
# ---------------------------------------------------------------

install(CODE "MESSAGE(\"\nInstall SUNLINSOL_SPTFQMR\n\")")

# Exported header files
set(sunlinsolsptfqmr_HEADERS
  ${SUNDIALS_SOURCE_DIR}/include/sunlinsol/sunlinsol_sptfqmr.h)

# Add the sunlinsol_sptfqmr library
sundials_add_library(sundials_sunlinsolsptfqmr
  SOURCES
    sunlinsol_sptfqmr.c
  OBJECT_LIBRARIES
    sundials_generic_obj
  OUTPUT_NAME
    sundials_sunlinsolsptfqmr
  VERSION
    ${sunlinsollib_VERSION}
  SOVERSION
    ${sunlinsollib_VERSION}
)

# Install the header files
install(FILES ${sunlinsolsptfqmr_HEADERS} DESTINATION include/sunlinsol)

#
message(STATUS "Added SUNLINSOL_SPTFQMR module")

# Add F90 module if F2003 interface is enabled
if(BUILD_FORTRAN_MODULE_INTERFACE)
  add_subdirectory(fmod)
endif()

# If FCMIX is enabled, build and install the Fortran77 library
if(BUILD_FORTRAN77_INTERFACE)
  sundials_add_library(sundials_fsunlinsolsptfqmr
    SOURCES
      fsunlinsol_sptfqmr.c
    LINK_LIBRARIES
      PRIVATE sundials_sunlinsolsptfqmr
    OUTPUT_NAME
      sundials_fsunlinsolsptfqmr
    VERSION
      ${sunlinsollib_VERSION}
    SOVERSION
      ${sunlinsollib_SOVERSION}
  )
  message(STATUS "Added SUNLINSOL_SPTFQMR F77 interface")
endif()
