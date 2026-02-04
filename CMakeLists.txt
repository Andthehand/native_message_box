# ========================================================
# Copyright (c) 2020 Gabriel Schmitz. All rights reserved.
# ========================================================

cmake_minimum_required( VERSION 3.20 )
project( native_message_box )

# ===================
# LIBRARY
# ===================

add_library( nmb INTERFACE )
target_include_directories( nmb INTERFACE include )

# =======
# WINDOWS
# =======

if( WIN32 )
    message( STATUS "Platform: Windows" )

# =====
# MACOS
# =====

elseif( APPLE )
    message( STATUS "Platform: MacOS" )

    find_library( CORE_FOUNDATION_LIBRARY CoreFoundation )
    if( NOT CORE_FOUNDATION_LIBRARY )
        message( FATAL_ERROR ": Framework CoreFoundation not found" )
    else()
        message( STATUS "CoreFoundation: ${CORE_FOUNDATION_LIBRARY}" )
        target_link_libraries( nmb INTERFACE ${CORE_FOUNDATION_LIBRARY} )
    endif()

# =====
# LINUX
# =====

elseif( UNIX )
    message( STATUS "Platform: Linux" )

    find_package( PkgConfig REQUIRED )
    pkg_check_modules( GTK REQUIRED gtk+-3.0 )
    
    target_include_directories( nmb INTERFACE ${GTK_INCLUDE_DIRS} )
    target_link_libraries( nmb INTERFACE ${GTK_LIBRARIES} )
    target_link_directories( nmb INTERFACE ${GTK_LIBRARY_DIRS} )
    target_compile_options( nmb INTERFACE ${GTK_CFLAGS_OTHER} )

# ===========
# UNSUPPORTED
# ===========

else()
    message( FATAL_ERROR "Platform: Not supported" )

endif()
