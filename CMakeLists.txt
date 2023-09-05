cmake_minimum_required(VERSION 3.6...3.15)

project(MAC VERSION 1.0.0)

# include(CheckIncludeFileCXX)
# include(CheckLibraryExists)

add_library(MAC INTERFACE)
add_library(MAC::MAC ALIAS MAC)

# set_property(TARGET MAC APPEND PROPERTY IMPORTED_CONFIGURATIONS RELEASE)

# set_target_properties(MAC PROPERTIES
#   IMPORTED_LINK_INTERFACE_LANGUAGES_RELEASE "CXX"
#   IMPORTED_LOCATION_RELEASE "${CMAKE_CURRENT_SOURCE_DIR}/lib/libMAC.a"
# )

target_include_directories(MAC INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# list(APPEND CEREAL_THREAD_LIBS cereal::cereal)

# if(NOT CMAKE_VERSION VERSION_LESS 3.8)
#     target_compile_features(cereal INTERFACE cxx_std_11)
# endif()

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

# install(TARGETS MAC EXPORT MACTargets)


install(DIRECTORY include/MAC DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

set(configFile ${CMAKE_CURRENT_BINARY_DIR}/MACConfig.cmake)
set(versionFile ${CMAKE_CURRENT_BINARY_DIR}/MACConfigVersion.cmake)
set(configInstallDestination ${CMAKE_INSTALL_LIBDIR}/cmake/MAC)

configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/Config.cmake.in
    ${configFile}
    INSTALL_DESTINATION ${configInstallDestination}
)

include(CMakePackageConfigHelpers)
if(${CMAKE_VERSION} VERSION_GREATER 3.13)
    write_basic_package_version_file(${versionFile} COMPATIBILITY SameMajorVersion ARCH_INDEPENDENT)
else()
    write_basic_package_version_file(${versionFile} COMPATIBILITY SameMajorVersion)
endif()

install(FILES ${configFile} ${versionFile} DESTINATION ${configInstallDestination})

install(TARGETS MAC EXPORT MACTargets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(
    EXPORT MACTargets
    NAMESPACE "MAC::"
    DESTINATION ${configInstallDestination}
)

message(STATUS ""                                                      )
message(STATUS "CMAKE_INSTALL_PREFIX     = ${CMAKE_INSTALL_PREFIX}"    )
message(STATUS "CMAKE_INSTALL_LIBDIR     = ${CMAKE_INSTALL_LIBDIR}"    )
message(STATUS "CMAKE_INSTALL_INCLUDEDIR = ${CMAKE_INSTALL_INCLUDEDIR}")
message(STATUS "CMAKE_INSTALL_DATADIR    = ${CMAKE_INSTALL_DATADIR}"   )
message(STATUS "CMAKE_INSTALL_BINDIR     = ${CMAKE_INSTALL_BINDIR}"    )
