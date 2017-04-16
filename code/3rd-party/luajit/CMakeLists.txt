cmake_minimum_required(VERSION 3.8)

include(ExternalProject)

set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/makeluajit.bat
           "call \"${VC_ENV_BAT}\" x86
           cd ${SRC_DIR}
           call msvcbuild.bat
           del luajit.exe
           del lua51.exp")

ExternalProject_Add(
    luajit
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/makeluajit.bat
    INSTALL_COMMAND COMMAND ${CMAKE_COMMAND} -E copy ${SRC_DIR}/lua51.dll ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/debug/lua51.dll
                    COMMAND ${CMAKE_COMMAND} -E copy ${SRC_DIR}/lua51.dll ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/relwithdebinfo/lua51.dll
                    COMMAND ${CMAKE_COMMAND} -E copy ${SRC_DIR}/lua51.lib ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}/debug/lua51.lib
                    COMMAND ${CMAKE_COMMAND} -E copy ${SRC_DIR}/lua51.lib ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}/relwithdebinfo/lua51.lib
                    COMMAND ${CMAKE_COMMAND} -E remove -f ${SRC_DIR}/lua51.dll
                    COMMAND ${CMAKE_COMMAND} -E remove -f ${SRC_DIR}/lua51.lib)