cmake_minimum_required(VERSION 3.15)
project(lcms2 LANGUAGES C VERSION 2.16)

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
set(CONFIG_INSTALL_DIR lib/cmake/lcms2)
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/lcms2Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/lcms2Config.cmake
    INSTALL_DESTINATION ${CONFIG_INSTALL_DIR}
)

install(EXPORT lcms2Targets
    FILE lcms2Targets.cmake
    NAMESPACE lcms2::
    DESTINATION ${CONFIG_INSTALL_DIR}
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/lcms2Config.cmake
    DESTINATION ${CONFIG_INSTALL_DIR}
)

add_library(lcms2 SHARED ${SRC_FILES})

if (WIN32)
    target_compile_definitions(lcms2 PRIVATE CMS_DLL)
    target_compile_definitions(lcms2 PRIVATE CMS_DLL_BUILD)
endif()

target_include_directories(lcms2
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)
set_target_properties(lcms2 PROPERTIES VERSION ${LIBRARY_VERSION} SOVERSION ${LCMS2_VERSION_MAJOR})

install(TARGETS lcms2
        EXPORT lcms2Targets
        LIBRARY DESTINATION lib
        INCLUDES DESTINATION include)

install(FILES include/lcms2.h include/lcms2_plugin.h
    DESTINATION include
)
