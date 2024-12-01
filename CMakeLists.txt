cmake_minimum_required(VERSION 3.5)
project(lcms2 VERSION 2.16)

set(LCMS2_VERSION_MAJOR 2)
set(LCMS2_VERSION_MINOR 16)
set(LCMS2_VERSION_MICRO 0)

# Create version string
set(LIBRARY_VERSION "${LCMS2_VERSION_MAJOR}.${LCMS2_VERSION_MINOR}.${LCMS2_VERSION_MICRO}")

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()

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

include(CMakePackageConfigHelpers)

SET(PACKAGE "lcms2")

add_library(lcms2 SHARED ${SRC_FILES} src/lcms2.def)

target_compile_definitions(lcms2 PUBLIC CMS_DLL)
target_compile_definitions(lcms2 PRIVATE CMS_DLL_BUILD)
target_compile_definitions(lcms2 PRIVATE UNICODE _UNICODE)
target_compile_definitions(lcms2 PUBLIC CMS_NO_REGISTER_KEYWORD)

target_include_directories(lcms2
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>)

set_target_properties(lcms2 PROPERTIES
                      PUBLIC_HEADER "${CMAKE_CURRENT_LIST_DIR}/include/lcms2.h;${CMAKE_CURRENT_LIST_DIR}/include/lcms2_plugin.h"
                      VERSION ${LIBRARY_VERSION} SOVERSION ${LCMS2_VERSION_MAJOR})


SET(prefix "${CMAKE_INSTALL_PREFIX}")
SET(exec_prefix "\${prefix}")
SET(libdir "\${exec_prefix}/${CMAKE_INSTALL_LIBDIR}")
SET(includedir "\${prefix}/include")
CONFIGURE_FILE(lcms2.pc.in "${PROJECT_BINARY_DIR}/lcms2.pc" @ONLY)

install(TARGETS lcms2
        EXPORT lcms2Config
        RUNTIME DESTINATION "bin"
        LIBRARY DESTINATION "lib"
        ARCHIVE DESTINATION "lib"
        PUBLIC_HEADER DESTINATION "include")

INSTALL(FILES ${PROJECT_BINARY_DIR}/lcms2.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)

install(EXPORT lcms2Config
        DESTINATION "share/lcms2"
        NAMESPACE lcms2::
)

