cmake_minimum_required(VERSION 3.24)
project(hellolib LANGUAGES CXX)

add_library(hellolib STATIC src/hellolib.cpp)

# be sure that you use the generator expressions
# ($<BUILD_INTERFACE: and $<INSTALL_INTERFACE:) for your include directory setups
target_include_directories(hellolib PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

include(GNUInstallDirs)
install(TARGETS hellolib
  EXPORT hellolibTargets
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  PUBLIC_HEADER DESTINATION include
)

# NAMESPACE sets the namespace that your target will live within.
# you can have multiple targets under the same namespace
install(EXPORT hellolibTargets
  FILE hellolibTargets.cmake
  NAMESPACE hellolib::  
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/hellolib
)

# this looks for your .cmake.in file that you created in step 1.
# if you set the directory differently you may need to handle this differently
include(CMakePackageConfigHelpers)
configure_package_config_file(${CMAKE_CURRENT_LIST_DIR}/cmake/hellolib.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/hellolibConfig.cmake"
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/hellolib
)

install(FILES
  "${CMAKE_CURRENT_BINARY_DIR}/hellolibConfig.cmake"
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/hellolib
)


# actually install your header files
install(
    DIRECTORY include/
    DESTINATION ${CMAKE_INSTALL_PREFIX}/include
    FILES_MATCHING PATTERN "*.h*"
)
