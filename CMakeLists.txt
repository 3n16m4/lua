project (lua LANGUAGES CXX)

file(GLOB_RECURSE CFILES "${CMAKE_SOURCE_DIR}/*.c")
list(REMOVE_ITEM CFILES "${CMAKE_SOURCE_DIR}/lua.c" "${CMAKE_SOURCE_DIR}/luac.c")
SET_SOURCE_FILES_PROPERTIES(${CFILES} PROPERTIES LANGUAGE CXX)

add_library(lua STATIC ${lua_sources})

if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    # using Clang
    add_compile_options(-Wno-deprecated)
    add_compile_definitions(LUA_USE_POSIX)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    # using GCC
    add_compile_definitions(LUA_USE_POSIX)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    # using Visual Studio C++
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
    set(BUILD_SHARED_LIBS TRUE)
    add_compile_definitions(LUA_USE_WINDOWS)
    add_compile_options(/wd4334)
endif()

add_library(lua SHARED ${CFILES})
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
