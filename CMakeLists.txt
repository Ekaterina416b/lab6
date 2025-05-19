cmake_minimum_required(VERSION 3.22)
project(lab06 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Включаем политику для линковки между поддиректориями
if(POLICY CMP0079)
    cmake_policy(SET CMP0079 NEW)
endif()

# Определяем ожидаемую структуру проекта
set(ROOT_DIR ${CMAKE_SOURCE_DIR})
set(FORMATTER_EX_LIB_DIR ${ROOT_DIR}/formatter_ex_lib)
set(SOLVER_LIB_DIR ${ROOT_DIR}/solver_lib)

# Проверяем существование критически важных директорий
foreach(dir 
    ${FORMATTER_EX_LIB_DIR}
    ${SOLVER_LIB_DIR}
    ${ROOT_DIR}/solver_application
)
    if(NOT EXISTS ${dir})
        message(FATAL_ERROR "Directory not found: ${dir}")
    endif()
endforeach()

# Добавляем поддиректории с явными путями
add_subdirectory(${FORMATTER_EX_LIB_DIR} formatter_ex_build)
add_subdirectory(${SOLVER_LIB_DIR} solver_lib_build)
add_subdirectory(${ROOT_DIR}/solver_application solver_app_build)

# Добавляем остальные компоненты (если есть)
if(EXISTS ${ROOT_DIR}/formatter_lib})
    add_subdirectory(${ROOT_DIR}/formatter_lib formatter_lib_build)
endif()

if(EXISTS ${ROOT_DIR}/hello_world_application})
    add_subdirectory(${ROOT_DIR}/hello_world_application hello_world_build)
endif()

# Установка только существующих целей
set(INSTALL_TARGETS "")
if(TARGET solver)
    list(APPEND INSTALL_TARGETS solver)
endif()
if(TARGET hello_world)
    list(APPEND INSTALL_TARGETS hello_world)
endif()

if(INSTALL_TARGETS)
    install(TARGETS ${INSTALL_TARGETS}
        RUNTIME DESTINATION bin
    )
else()
    message(WARNING "No valid targets found for installation")
endif()

include(InstallRequiredSystemLibraries)

# Настройки CPack
set(CPACK_PACKAGE_NAME "lab06")
set(CPACK_PACKAGE_VERSION "1.0.0")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Lab 06 solution")
include(CPack)
