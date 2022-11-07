cmake_minimum_required(VERSION 3.23)
project(test)

set(CMAKE_CXX_STANDARD 14)

include(FetchContent)
find_package(SDL2 REQUIRED CONFIG)

if(GITHUB_ACTIONS)
    message("NOTE: GITHUB_ACTIONS defined")
    target_compile_definitions(${PROJECT_NAME} PRIVATE GITHUB_ACTIONS)
endif()

message("Compiler: ${CMAKE_CXX_COMPILER_ID} version ${CMAKE_CXX_COMPILER_VERSION}")
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE /W4 /permissive- /wd4244 /wd4267 /external:anglebrackets /external:W0)
    if(WARNINGS_AS_ERRORS)
        target_compile_options(${PROJECT_NAME} PRIVATE /WX)
    endif()
else()
    target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -pedantic)
    if(WARNINGS_AS_ERRORS)
        target_compile_options(${PROJECT_NAME} PRIVATE -Werror)
    endif()

    if("${CMAKE_CXX_COMPILER_ID}" MATCHES "GNU")
    elseif("${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang")
        target_compile_options(${PROJECT_NAME} PRIVATE -stdlib=libc++)
        target_link_options(${PROJECT_NAME} PRIVATE -stdlib=libc++)
    endif()
endif()

add_executable(test
        main.cpp)

install(TARGETS ${PROJECT_NAME} DESTINATION bin)
