cmake_minimum_required(VERSION 3.22)
project(lab06)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Включаем политику CMP0079 (разрешаем линковку между поддиректориями)
if(POLICY CMP0079)
    cmake_policy(SET CMP0079 NEW)
endif()

add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)
add_subdirectory(hello_world_application)
add_subdirectory(solver_application)
# Инструкции для инсталляции (установка исполняемых файлов в /usr/bin)
install(TARGETS hello_world solver
    RUNTIME DESTINATION bin
)

include(InstallRequiredSystemLibraries)
# Включение CPack для создания пакетов (DEB, RPM и т. д.)
include(CPack)
