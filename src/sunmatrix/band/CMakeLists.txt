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
# CMakeLists.txt file for the band SUNMatrix library
# ---------------------------------------------------------------

install(CODE "MESSAGE(\"\nInstall SUNMATRIX_BAND\n\")")

# Add variable sunmatrixband_HEADERS with the exported SUNMATRIXBAND header files
set(sunmatrixband_HEADERS
  ${SUNDIALS_SOURCE_DIR}/include/sunmatrix/sunmatrix_band.h
  )

# Add the sunmatrix_band library
sundials_add_library(sundials_sunmatrixband
  SOURCES
    sunmatrix_band.c
  OBJECT_LIBRARIES
    sundials_generic_obj
  OUTPUT_NAME
    sundials_sunmatrixband
  VERSION
    ${sunmatrixlib_VERSION}
  SOVERSION
    ${sunmatrixlib_SOVERSION}
)

# Install the SUNMATRIXBAND header files
install(FILES ${sunmatrixband_HEADERS} DESTINATION include/sunmatrix)

#
message(STATUS "Added SUNMATRIX_BAND module")

# Add F2003 module if the interface is enabled
if(BUILD_FORTRAN_MODULE_INTERFACE)
  add_subdirectory(fmod)
endif()

# If FCMIX is enabled, build and install the Fortran77 library
if(BUILD_FORTRAN77_INTERFACE)
  sundials_add_library(sundials_fsunmatrixband
    SOURCES
      fsunmatrix_band.c
    LINK_LIBRARIES
      PRIVATE sundials_fnvecserial sundials_sunmatrixband
    OUTPUT_NAME
      sundials_fsunmatrixband
    VERSION
      ${sunmatrixlib_VERSION}
    SOVERSION
      ${sunmatrixlib_SOVERSION}
  )
  message(STATUS "Added SUNMATRIX_BAND F77 interface")
endif()
