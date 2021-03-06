# Copyright 2018 Miklos Vajna. All rights reserved.
# Use of this source code is governed by a BSD-style license that can be
# found in the LICENSE file.

if (ODFSIG_INTERNAL_ZLIB)
    include(ExternalProject)

    ExternalProject_Add(extern-zlib-project
        URL http://zlib.net/zlib-1.2.11.tar.gz
        URL_HASH SHA256=c3e5e9fdd5004dcb542feda5ee4f0ff0744628baf8ed2dd5d66f8ca1197cb1a1
        DOWNLOAD_DIR ${CMAKE_SOURCE_DIR}/extern/tarballs
        BUILD_IN_SOURCE ON
        CONFIGURE_COMMAND ${CMAKE_COMMAND}
            -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/install
        )
    add_library(z STATIC IMPORTED)
    if (WIN32)
        set_property(TARGET z PROPERTY IMPORTED_LOCATION ${CMAKE_CURRENT_BINARY_DIR}/install/lib/zlibstatic.lib)
    else ()
        set_property(TARGET z PROPERTY IMPORTED_LOCATION ${CMAKE_CURRENT_BINARY_DIR}/install/lib/libz.a)
    endif ()
    add_dependencies(z extern-zlib-project)

    set(ZLIB_INCLUDE_DIRS
        ${CMAKE_CURRENT_BINARY_DIR}/install/include
        )
    set(ZLIB_LIBRARIES
        z
        )
endif ()

add_library(zlib INTERFACE)
target_include_directories(zlib INTERFACE ${ZLIB_INCLUDE_DIRS})
target_link_libraries(zlib INTERFACE ${ZLIB_LIBRARIES})

# vim:set shiftwidth=4 softtabstop=4 expandtab:
