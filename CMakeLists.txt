cmake_minimum_required(VERSION 3.22)
project(lab06 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Включаем политику для линковки между поддиректориями
if(POLICY CMP0079)
    cmake_policy(SET CMP0079 NEW)
endif()

# Проверяем и добавляем библиотеки с абсолютными путями
set(FORMATTER_EX_LIB_PATH "${CMAKE_SOURCE_DIR}/formatter_ex_lib")
set(SOLVER_LIB_PATH "${CMAKE_SOURCE_DIR}/solver_lib")

if(NOT EXISTS ${FORMATTER_EX_LIB_PATH})
    message(FATAL_ERROR "formatter_ex_lib not found at: ${FORMATTER_EX_LIB_PATH}")
endif()

if(NOT EXISTS ${SOLVER_LIB_PATH})
    message(FATAL_ERROR "solver_lib not found at: ${SOLVER_LIB_PATH}")
endif()

# Добавляем поддиректории
add_subdirectory(${FORMATTER_EX_LIB_PATH} formatter_ex_build)
add_subdirectory(${SOLVER_LIB_PATH} solver_lib_build)

# Добавляем приложения
add_subdirectory(hello_world_application hello_world_build)
add_subdirectory(solver_application solver_app_build)

# Установка (только если цели существуют)
if(TARGET hello_world AND TARGET solver)
    install(TARGETS hello_world solver
        RUNTIME DESTINATION bin
    )
else()
    message(WARNING "Installation targets not found")
endif()

include(InstallRequiredSystemLibraries)

# Настройка CPack
set(CPACK_PACKAGE_NAME "lab06")
set(CPACK_PACKAGE_VERSION "1.0.0")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Lab 06 solution")
include(CPack)
