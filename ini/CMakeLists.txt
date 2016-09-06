cmake_minimum_required(VERSION 2.8)
configure_file(config.h.in "${CMAKE_CURRENT_BINARY_DIR}/config.h" @ONLY)

add_library(ini STATIC ini.c ini.h config.h.in)

set_target_properties(ini PROPERTIES
  PUBLIC_HEADER "ini.h;${CMAKE_CURRENT_BINARY_DIR}/config.h")

install(TARGETS ini
  # IMPORTANT: Add the foo library to the "export-set"
  EXPORT IniTargets
  RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin
  LIBRARY DESTINATION "${INSTALL_LIB_DIR}" COMPONENT shlib
  ARCHIVE DESTINATION "${INSTALL_LIB_DIR}"
  PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/ini"
    COMPONENT dev)
