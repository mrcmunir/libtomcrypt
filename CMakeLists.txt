project(libtomcrypt)

add_library(libtomcrypt INTERFACE)

option(USE_SYSTEM_TOMCRYPT "Dynamically link against system tomcrypt" OFF)

target_include_directories(libtomcrypt INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/build/include")
if (WIN32 AND NOT MINGW)
    target_link_libraries(libtomcrypt INTERFACE 
        "${CMAKE_CURRENT_SOURCE_DIR}/build/lib/tomcrypt.lib")
elseif (APPLE)
    target_link_libraries(libtomcrypt INTERFACE 
        "${CMAKE_CURRENT_SOURCE_DIR}/build/lib/tomcrypt.a")
elseif (UNIX)
    target_link_libraries(libtomcrypt INTERFACE
        "${CMAKE_CURRENT_SOURCE_DIR}/build/lib/libtomcrypt.a")
else ()
    set(USE_SYSTEM_TOMCRYPT ON)
endif ()

if (USE_SYSTEM_TOMCRYPT)
	find_package(PkgConfig REQUIRED)
	pkg_check_modules(TOMCRYPT REQUIRED IMPORTED_TARGET libtomcrypt)
	target_link_libraries(libtomcrypt INTERFACE "${TOMCRYPT_LIBRARIES}")
	target_include_directories(libtomcrypt INTERFACE "${TOMCRYPT_INCLUDE_DIRS}")
endif ()
