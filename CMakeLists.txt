cmake_minimum_required(VERSION 3.22)
project(lab06 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Проверка существования всех необходимых директорий
macro(check_directory dir)
    if(NOT EXISTS ${CMAKE_SOURCE_DIR}/${dir})
        message(FATAL_ERROR "Directory ${dir} not found at: ${CMAKE_SOURCE_DIR}/${dir}")
    endif()
endmacro()

check_directory(formatter_ex_lib)
check_directory(solver_lib)
check_directory(solver_application)

# Добавление поддиректорий
add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)
add_subdirectory(solver_application)

# Опциональные компоненты
if(EXISTS ${CMAKE_SOURCE_DIR}/formatter_lib})
    add_subdirectory(formatter_lib)
endif()

if(EXISTS ${CMAKE_SOURCE_DIR}/hello_world_application})
    add_subdirectory(hello_world_application)
endif()

# Установка
if(TARGET solver AND TARGET hello_world)
    install(TARGETS solver hello_world
        RUNTIME DESTINATION bin
    )
else()
    message(WARNING "Some targets are missing for installation")
endif()

include(InstallRequiredSystemLibraries)
include(CPack)
