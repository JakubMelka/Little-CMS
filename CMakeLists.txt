cmake_minimum_required(VERSION 3.5)
project(lcms2 VERSION 2.16)

# Nastavení verzí
set(LCMS2_VERSION_MAJOR 2)
set(LCMS2_VERSION_MINOR 16)
set(LCMS2_VERSION_MICRO 0)
set(LIBRARY_VERSION "${LCMS2_VERSION_MAJOR}.${LCMS2_VERSION_MINOR}.${LCMS2_VERSION_MICRO}")

# Build type (default na Release)
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()

# Zajištění správných instalačních cest (pomocí GNUInstallDirs)
include(GNUInstallDirs)

# Seznam zdrojových souborů
set(SRC_FILES
    include/lcms2.h
    include/lcms2_plugin.h
    src/cmsalpha.c
    src/cmscam02.c
    src/cmscgats.c
    src/cmscnvrt.c
    src/cmserr.c
    src/cmsgamma.c
    src/cmsgmt.c
    src/cmshalf.c
    src/cmsintrp.c
    src/cmsio0.c
    src/cmsio1.c
    src/cmslut.c
    src/cmsmd5.c
    src/cmsmtrx.c
    src/cmsnamed.c
    src/cmsopt.c
    src/cmspack.c
    src/cmspcs.c
    src/cmsplugin.c
    src/cmsps2.c
    src/cmssamp.c
    src/cmssm.c
    src/cmstypes.c
    src/cmsvirt.c
    src/cmswtpnt.c
    src/cmsxform.c
    src/lcms2_internal.h
)

# Přidání knihovny
add_library(lcms2 SHARED ${SRC_FILES} src/lcms2.def)

# Kompilační definice
target_compile_definitions(lcms2
    PUBLIC CMS_DLL CMS_NO_REGISTER_KEYWORD
    PRIVATE CMS_DLL_BUILD UNICODE _UNICODE)

# Zahrnutí hlaviček
target_include_directories(lcms2
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>)

# Nastavení vlastností targetu
set_target_properties(lcms2 PROPERTIES
    PUBLIC_HEADER "include/lcms2.h;include/lcms2_plugin.h"
    VERSION ${LIBRARY_VERSION}
    SOVERSION ${LCMS2_VERSION_MAJOR})

# Instalace targetu
install(TARGETS lcms2
    EXPORT lcms2-targets
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# Export targetů pro použití v CMake
install(EXPORT lcms2-targets
    FILE lcms2Targets.cmake
    NAMESPACE lcms2::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lcms2)

# Vytvoření konfigurace CMake pro použití find_package
include(CMakePackageConfigHelpers)

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/lcms2ConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion)

configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/lcms2Config.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/lcms2Config.cmake"
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lcms2)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/lcms2Config.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/lcms2ConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lcms2)
