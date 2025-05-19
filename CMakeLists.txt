cmake_minimum_required(VERSION 3.22)
project(lab06 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Включаем политику для линковки между поддиректориями
if(POLICY CMP0079)
    cmake_policy(SET CMP0079 NEW)
endif()

# Проверка существования всех необходимых директорий
set(REQUIRED_DIRS
    "${CMAKE_SOURCE_DIR}/formatter_lib"
    "${CMAKE_SOURCE_DIR}/formatter_ex_lib"
    "${CMAKE_SOURCE_DIR}/solver_lib"
    "${CMAKE_SOURCE_DIR}/hello_world_application"
    "${CMAKE_SOURCE_DIR}/solver_application"
)

foreach(dir ${REQUIRED_DIRS})
    if(NOT EXISTS ${dir})
        message(FATAL_ERROR "Required directory not found: ${dir}")
    endif()
endforeach()

# Добавляем все поддиректории с уникальными бинарными директориями
add_subdirectory(${CMAKE_SOURCE_DIR}/formatter_lib formatter_lib_build)
add_subdirectory(${CMAKE_SOURCE_DIR}/formatter_ex_lib formatter_ex_lib_build)
add_subdirectory(${CMAKE_SOURCE_DIR}/solver_lib solver_lib_build)
add_subdirectory(${CMAKE_SOURCE_DIR}/hello_world_application hello_world_build)
add_subdirectory(${CMAKE_SOURCE_DIR}/solver_application solver_app_build)

# Установка только если цели существуют
set(INSTALL_TARGETS hello_world solver)
foreach(target ${INSTALL_TARGETS})
    if(NOT TARGET ${target})
        list(REMOVE_ITEM INSTALL_TARGETS ${target})
        message(WARNING "Target ${target} not found - skipping installation")
    endif()
endforeach()

if(INSTALL_TARGETS)
    install(TARGETS ${INSTALL_TARGETS}
        RUNTIME DESTINATION bin
    )
endif()

include(InstallRequiredSystemLibraries)

# Настройки CPack
set(CPACK_PACKAGE_NAME "lab06")
set(CPACK_PACKAGE_VERSION "1.0.0")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Lab 06 solution")
include(CPack)
