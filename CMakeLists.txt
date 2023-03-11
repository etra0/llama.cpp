cmake_minimum_required(VERSION 3.8)
project("llama.cpp")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED true)
set(CMAKE_C_STANDARD 11)

add_executable(llama
    main.cpp
    utils.cpp
    utils.h)

add_executable(quantize
    quantize.cpp
    utils.cpp
    utils.h)

add_library(ggml
    ggml.c
    ggml.h)

target_include_directories(ggml PUBLIC .)
target_link_libraries(llama PRIVATE ggml)
target_link_libraries(quantize PRIVATE ggml)


if (MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /arch:AVX")
    target_compile_options(ggml PUBLIC /W1)
    target_compile_options(llama PUBLIC /W1)
    target_compile_options(quantize PUBLIC /W1)
endif()
