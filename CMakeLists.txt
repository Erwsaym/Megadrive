cmake_minimum_required(VERSION 3.10)

project(Megadrive)

set(HEADER_FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/extlibs/SGDK
        ${CMAKE_CURRENT_SOURCE_DIR}/extlibs/SGDK/inc
        ${CMAKE_CURRENT_SOURCE_DIR}/extlibs/SGDK/res)

set(SRC
        ${CMAKE_CURRENT_SOURCE_DIR}/src/main.c)

IF(CMAKE_BUILD_TYPE MATCHES Debug)
    set(type debug)
ELSE()
    set(type clean release)
ENDIF()

set(GDK ${CMAKE_CURRENT_SOURCE_DIR}/extlibs/SGDK)
set(GDK_WIN ${CMAKE_CURRENT_SOURCE_DIR}\\extlibs\\SGDK)

set(SGDK_COMPILE_COMMAND
        ${CMAKE_CURRENT_SOURCE_DIR}/extlibs/SGDK/bin/make.exe GDK=${GDK} GDK_WIN=${GDK_WIN}
        -f ${CMAKE_CURRENT_SOURCE_DIR}/extlibs/SGDK/makelib.gen ${type})

set(PROJECT_COMPILE_COMMAND
        ${CMAKE_CURRENT_SOURCE_DIR}/extlibs/SGDK/bin/make.exe GDK=${GDK} GDK_WIN=${GDK_WIN}
        -f ${CMAKE_CURRENT_SOURCE_DIR}/extlibs/SGDK/makefile.gen ${type})

add_custom_target(megadrive ALL ${SGDK_COMPILE_COMMAND}
        COMMAND ${PROJECT_COMPILE_COMMAND}
        DEPENDS ${HEADER_FILES}
        SOURCES ${SRC}
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})


add_executable(dummy ${SRC})
target_include_directories(dummy PUBLIC ${HEADER_FILES})