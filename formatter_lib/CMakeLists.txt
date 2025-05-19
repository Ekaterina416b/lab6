cmake_minimum_required(VERSION 3.29.2)
project(formatter LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Защита от повторного создания цели + современный стиль
if(NOT TARGET formatter)
    add_library(formatter STATIC 
        ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp
    )
    
    # Современная замена include_directories()
    target_include_directories(formatter PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}
    )
endif()
