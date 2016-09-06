cmake_minimum_required(VERSION 2.8)
project(Ini C)
 
set(INI_MAJOR_VERSION 0)
set(INI_MINOR_VERSION 1)
set(INI_PATCH_VERSION 0)
set(INI_VERSION ${INI_MAJOR_VERSION}.${INI_MINOR_VERSION}.${INI_PATCH_VERSION})
 
# Offer the user the choice of overriding the installation directories
set(INSTALL_LIB_DIR lib CACHE PATH "Installation directory for libraries")
set(INSTALL_BIN_DIR bin CACHE PATH "Installation directory for executables")
set(INSTALL_INCLUDE_DIR include CACHE PATH
  "Installation directory for header files")
if(WIN32 AND NOT CYGWIN)
  set(DEF_INSTALL_CMAKE_DIR CMake)
else()
  set(DEF_INSTALL_CMAKE_DIR lib/CMake/Ini)
endif()
set(INSTALL_CMAKE_DIR ${DEF_INSTALL_CMAKE_DIR} CACHE PATH
  "Installation directory for CMake files")
 
# Make relative paths absolute (needed later on)
foreach(p LIB BIN INCLUDE CMAKE)
  set(var INSTALL_${p}_DIR)
  if(NOT IS_ABSOLUTE "${${var}}")
    set(${var} "${CMAKE_INSTALL_PREFIX}/${${var}}")
  endif()
endforeach()
 
# set up include-directories
include_directories(
  "${PROJECT_SOURCE_DIR}"   # to find foo/foo.h
  "${PROJECT_BINARY_DIR}")  # to find foo/config.h
 
# Add sub-directories
add_subdirectory(ini)
 
# The interesting stuff goes here
# ===============================
 
# Add all targets to the build-tree export set
export(TARGETS ini
  FILE "${PROJECT_BINARY_DIR}/IniTargets.cmake")
 
# Export the package for use from the build-tree
# (this registers the build-tree with a global CMake-registry)
export(PACKAGE Ini)
 
# Create the FooBarConfig.cmake and FooBarConfigVersion files
file(RELATIVE_PATH REL_INCLUDE_DIR "${INSTALL_CMAKE_DIR}"
   "${INSTALL_INCLUDE_DIR}")
# ... for the build tree
set(CONF_INCLUDE_DIRS "${PROJECT_SOURCE_DIR}" "${PROJECT_BINARY_DIR}")
configure_file(IniConfig.cmake.in
  "${PROJECT_BINARY_DIR}/IniConfig.cmake" @ONLY)
# ... for the install tree
set(CONF_INCLUDE_DIRS "\${INI_CMAKE_DIR}/${REL_INCLUDE_DIR}")
configure_file(IniConfig.cmake.in
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/IniConfig.cmake" @ONLY)
# ... for both
configure_file(IniConfigVersion.cmake.in
  "${PROJECT_BINARY_DIR}/IniConfigVersion.cmake" @ONLY)
 
# Install the FooBarConfig.cmake and FooBarConfigVersion.cmake
install(FILES
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/IniConfig.cmake"
  "${PROJECT_BINARY_DIR}/IniConfigVersion.cmake"
  DESTINATION "${INSTALL_CMAKE_DIR}" COMPONENT dev)
 
# Install the export set for use with the install-tree
install(EXPORT IniTargets DESTINATION
  "${INSTALL_CMAKE_DIR}" COMPONENT dev)
