cmake_minimum_required(VERSION 3.18)

project(ClangTools VERSION 22.1.8 LANGUAGES NONE)

if (CMAKE_HOST_SYSTEM_NAME STREQUAL "Windows" AND CMAKE_HOST_SYSTEM_PROCESSOR MATCHES "AMD64|x86_64")
    set(ClangToolsPlatform "windows-x64")
    set(ClangToolsSuffix ".exe")
elseif (CMAKE_HOST_SYSTEM_NAME STREQUAL "Linux" AND CMAKE_HOST_SYSTEM_PROCESSOR MATCHES "x86_64|AMD64")
    set(ClangToolsPlatform "linux-x64")
elseif (CMAKE_HOST_SYSTEM_NAME STREQUAL "Darwin" AND CMAKE_HOST_SYSTEM_PROCESSOR MATCHES "arm64|aarch64")
    set(ClangToolsPlatform "macos-arm64")
else()
    message(WARNING "ClangTools: no binaries for ${CMAKE_HOST_SYSTEM_NAME} ${CMAKE_HOST_SYSTEM_PROCESSOR}")
    return()
endif()

set(ClangToolsArchive "${CMAKE_CURRENT_SOURCE_DIR}/${ClangToolsPlatform}.tar.xz")
set(ClangToolsDir "${CMAKE_CURRENT_BINARY_DIR}/${ClangToolsPlatform}")

file(ARCHIVE_EXTRACT INPUT "${ClangToolsArchive}" DESTINATION "${ClangToolsDir}")

set(CLANG_TIDY_EXECUTABLE "${ClangToolsDir}/bin/clang-tidy${ClangToolsSuffix}"
    CACHE FILEPATH "clang-tidy ${PROJECT_VERSION}" FORCE)
set(CLANG_FORMAT_EXECUTABLE "${ClangToolsDir}/bin/clang-format${ClangToolsSuffix}"
    CACHE FILEPATH "clang-format ${PROJECT_VERSION}" FORCE)
set(CLANG_APPLY_REPLACEMENTS_EXECUTABLE "${ClangToolsDir}/bin/clang-apply-replacements${ClangToolsSuffix}"
    CACHE FILEPATH "clang-apply-replacements ${PROJECT_VERSION}" FORCE)
