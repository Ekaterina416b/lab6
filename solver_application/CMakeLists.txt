cmake_minimum_required(VERSION 3.10)
project(solver_application LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Включаем политику для линковки между поддиректориями
if(POLICY CMP0079)
    cmake_policy(SET CMP0079 NEW)
endif()

# Проверяем, не добавлены ли уже зависимости
if(NOT TARGET formatter_ex)
    # Используем уникальные имена бинарных директорий
    add_subdirectory(
        ../formatter_ex_lib
        ../formatter_ex_lib_build
    )
endif()

if(NOT TARGET solver_lib)
    add_subdirectory(
        ../solver_lib
        ../solver_lib_build
    )
endif()

# Создаем исполняемый файл
add_executable(solver equation.cpp)

# Линкуем с проверкой существования целей
if(TARGET formatter_ex AND TARGET solver_lib)
    target_link_libraries(solver PRIVATE
        formatter_ex
        solver_lib
    )
    
    # Указываем пути к заголовкам
    target_include_directories(solver PRIVATE
        ../formatter_lib
        ../formatter_ex_lib
        ../solver_lib
    )
else()
    message(WARNING "Не удалось найти все необходимые библиотеки для линковки")
endif()
