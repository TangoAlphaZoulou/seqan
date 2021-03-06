# ===========================================================================
#                  SeqAn - The Library for Sequence Analysis
# ===========================================================================
# File: /core/apps/stellar/CMakeLists.txt
#
# CMakeLists.txt file for STELLAR.
# ===========================================================================

cmake_minimum_required (VERSION 2.8.2)
project (core_apps_stellar)
message (STATUS "Configuring core/apps/stellar")

# ----------------------------------------------------------------------------
# Dependencies
# ----------------------------------------------------------------------------

# Search SeqAn and select dependencies.
set (SEQAN_FIND_DEPENDENCIES NONE)
find_package (SeqAn REQUIRED)

# ----------------------------------------------------------------------------
# Build Setup
# ----------------------------------------------------------------------------

# Add include directories.
include_directories (${SEQAN_INCLUDE_DIRS})

# Add definitions set by find_package (SeqAn).
add_definitions (${SEQAN_DEFINITIONS})

# Update the list of file names below if you add source files to your application.
add_executable (stellar stellar.cpp
                       stellar.h
                       stellar_extension.h
                       stellar_output.h
                       stellar_types.h)

# Add dependencies found by find_package (SeqAn).
target_link_libraries (stellar ${SEQAN_LIBRARIES})

# Add CXX flags found by find_package (SeqAn).
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${SEQAN_CXX_FLAGS}")

# ----------------------------------------------------------------------------
# Installation
# ----------------------------------------------------------------------------

# Set variables for installing, depending on the selected build type.
if (NOT SEQAN_PREFIX_SHARE_DOC)
  seqan_setup_install_vars (stellar)
endif (NOT SEQAN_PREFIX_SHARE_DOC)

# Install stellar in ${PREFIX}/bin directory
install (TARGETS stellar
         DESTINATION bin)

# Install non-binary files for the package to "." for app builds and
# ${PREFIX}/share/doc/stellar for SeqAn release builds.
install (FILES LICENSE
               README
         DESTINATION ${SEQAN_PREFIX_SHARE_DOC})
install (FILES example/NC_001474.fasta
               example/NC_001477.fasta
               example/reads.fasta
         DESTINATION ${SEQAN_PREFIX_SHARE_DOC}/example)

# ----------------------------------------------------------------------------
# App Test
# ----------------------------------------------------------------------------

seqan_add_app_test (stellar)

# ---------------------------------------------------------------------------- 
# Setup Common Tool Description for Generic Workflow Nodes 
# ---------------------------------------------------------------------------- 

# Include executable stellar in CTD structure. 
set (SEQAN_CTD_EXECUTABLES ${SEQAN_CTD_EXECUTABLES} stellar CACHE INTERNAL "")

# ----------------------------------------------------------------------------
# CPack Install
# ----------------------------------------------------------------------------

if (SEQAN_BUILD_SYSTEM STREQUAL "APP:stellar")
  set (CPACK_PACKAGE_NAME "stellar")
  set (CPACK_PACKAGE_DESCRIPTION_SUMMARY "STELLAR - SwifT Exact LocaL AligneR")
  set (CPACK_DEBIAN_PACKAGE_MAINTAINER "David Weese <david.weese@fu-berlin.de>")
  set (CPACK_PACKAGE_VENDOR "SeqAn Team, FU Berlin")

  seqan_configure_cpack_app (stellar "Stellar")
endif (SEQAN_BUILD_SYSTEM STREQUAL "APP:stellar")

