project(libtomcrypt)

add_library(libtomcrypt INTERFACE)

target_include_directories(libtomcrypt INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/build/include")
if (WIN32)
    target_link_libraries(libtomcrypt INTERFACE 
        "${CMAKE_CURRENT_SOURCE_DIR}/build/lib/tomcrypt.lib")
elseif (APPLE)
    target_link_libraries(libtomcrypt INTERFACE 
        "${CMAKE_CURRENT_SOURCE_DIR}/build/lib/tomcrypt.a")
elseif (UNIX)
	if (${CMAKE_SYSTEM_PROCESSOR} MATCHES "(amd64)|(AMD64)|(x86_64)")
    	target_link_libraries(libtomcrypt INTERFACE
        	"${CMAKE_CURRENT_SOURCE_DIR}/build/lib/libtomcrypt.a")
        elseif (${CMAKE_SYSTEM_PROCESSOR} MATCHES "(^aarch64)")
	target_link_libraries(libtomcrypt INTERFACE
		"${CMAKE_CURRENT_SOURCE_DIR}/build/lib/aarch64/libtomcrypt.a")
	endif()
endif()

