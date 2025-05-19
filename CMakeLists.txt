cmake_minimum_required(VERSION 3.22)
project(lab06 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Включаем политику CMP0079
if(POLICY CMP0079)
    cmake_policy(SET CMP0079 NEW)
endif()

# Проверка существования поддиректорий перед добавлением
set(FORMATTER_LIB_DIR "${CMAKE_SOURCE_DIR}/formatter_lib")
set(FORMATTER_EX_LIB_DIR "${CMAKE_SOURCE_DIR}/formatter_ex_lib") 
set(SOLVER_LIB_DIR "${CMAKE_SOURCE_DIR}/solver_lib")

# Добавляем поддиректории с проверкой
if(EXISTS ${FORMATTER_LIB_DIR})
    add_subdirectory(${FORMATTER_LIB_DIR} formatter_lib_build)
else()
    message(FATAL_ERROR "Directory formatter_lib not found at: ${FORMATTER_LIB_DIR}")
endif()

if(EXISTS ${FORMATTER_EX_LIB_DIR})
    add_subdirectory(${FORMATTER_EX_LIB_DIR} formatter_ex_lib_build)
else()
    message(FATAL_ERROR "Directory formatter_ex_lib not found at: ${FORMATTER_EX_LIB_DIR}")
endif()

if(EXISTS ${SOLVER_LIB_DIR})
    add_subdirectory(${SOLVER_LIB_DIR} solver_lib_build)
else()
    message(FATAL_ERROR "Directory solver_lib not found at: ${SOLVER_LIB_DIR}")
endif()

# Добавляем приложения
add_subdirectory(hello_world_application hello_world_build)
add_subdirectory(solver_application solver_app_build)

# Установка только если цели существуют
if(TARGET hello_world AND TARGET solver)
    install(TARGETS hello_world solver
        RUNTIME DESTINATION bin
    )
else()
    message(WARNING "Not all targets found for installation")
endif()

include(InstallRequiredSystemLibraries)

# Настройки CPack
set(CPACK_PACKAGE_NAME "lab06")
set(CPACK_PACKAGE_VERSION "1.0.0")
include(CPack)
