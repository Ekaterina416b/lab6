cmake_minimum_required(VERSION 3.10)
project(formatter_ex LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Основная библиотека formatter_ex с защитой от повторного создания
if(NOT TARGET formatter_ex)
    add_library(formatter_ex STATIC formatter_ex.cpp)
    
    # Инклюд-директории
    target_include_directories(formatter_ex PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
    )
    
    # Добавление formatter как подпроекта с уникальной бинарной директорией
    if(NOT TARGET formatter)
        add_subdirectory(
            ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib 
            ${CMAKE_BINARY_DIR}/formatter_build
        )
    endif()
    
    # Линковка с formatter (если он существует)
    if(TARGET formatter)
        target_link_libraries(formatter_ex PRIVATE formatter)
    endif()
endif()
