cmake_minimum_required(VERSION 3.6...3.15)

project(cereal LANGUAGES CXX VERSION 1.3.2)

add_library(cereal STATIC)
add_library(cereal::cereal ALIAS cereal)
target_include_directories(cereal INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
list(APPEND CEREAL_THREAD_LIBS cereal::cereal)

if(NOT CMAKE_VERSION VERSION_LESS 3.8)
    target_compile_features(cereal INTERFACE cxx_std_11)
endif()



include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

install(TARGETS cereal EXPORT ${PROJECT_NAME}Targets)

install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}_Targets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(DIRECTORY include/cereal DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

set(configFile ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake)
set(versionFile ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake)
set(configInstallDestination ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
    ${configFile}
    INSTALL_DESTINATION ${configInstallDestination}
)

include(CMakePackageConfigHelpers)
if(${CMAKE_VERSION} VERSION_GREATER 3.13)
    write_basic_package_version_file("${versionFile}" COMPATIBILITY SameMajorVersion ARCH_INDEPENDENT)
else()
    write_basic_package_version_file("${versionFile}" COMPATIBILITY SameMajorVersion)
endif()

install(FILES ${configFile} ${versionFile} DESTINATION ${configInstallDestination})
install(
    EXPORT ${PROJECT_NAME}Targets
    NAMESPACE "cereal::"
    DESTINATION ${configInstallDestination}
)

