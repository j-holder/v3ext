cmake_minimum_required(VERSION 3.5)

project(v3ext)

# Set cmake variables
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,--no-undefined")
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)
set(CMAKE_CXX_STANDARD 14)

# Move build binary output to a single directory
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Werror -Wno-unused-result -Wno-long-long -pedantic -Wno-variadic-macros")

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    # Suppress the warning about using gnu specific features
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-gnu-zero-variadic-macro-arguments")

    # Uncomment this line if trying to run scan-build
    #string(REPLACE "-g" "" ${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS})
endif()

include(FindOpenSSL)
if (NOT ${OPENSSL_FOUND})
    message(ERROR " No openssl lib found")
endif()

set(SOURCES
	src/BasicConstraints.cpp
    )

add_library(v3ext ${SOURCES})
target_link_libraries(v3ext
    PRIVATE OpenSSL::Crypto
	PRIVATE OpenSSL::SSL
    )

target_include_directories(v3ext
	PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
	)

