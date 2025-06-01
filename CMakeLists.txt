cmake_minimum_required(VERSION 3.22)
project(lab06
    LANGUAGES CXX
    VERSION 1.0.0 
)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(POLICY CMP0079)
    cmake_policy(SET CMP0079 NEW)
endif()

foreach(dir IN ITEMS formatter_ex_lib solver_lib solver_application)
    if(NOT EXISTS "${CMAKE_SOURCE_DIR}/${dir}")
        message(FATAL_ERROR "Required directory '${dir}' not found!")
    endif()
endforeach()

add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)
add_subdirectory(solver_application)

foreach(opt_dir IN ITEMS formatter_lib hello_world_application)
    if(EXISTS "${CMAKE_SOURCE_DIR}/${opt_dir}")
        add_subdirectory(${opt_dir})
    endif()
endforeach()

set(INSTALL_TARGETS "")
foreach(target IN ITEMS solver hello_world)
    if(TARGET ${target})
        list(APPEND INSTALL_TARGETS ${target})
    else()
        message(WARNING "Target '${target}' not found - skipping installation")
    endif()
endforeach()

if(INSTALL_TARGETS)
    install(TARGETS ${INSTALL_TARGETS}
        RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
    )
endif()

set(CPACK_PACKAGE_NAME "${PROJECT_NAME}")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
set(CPACK_PACKAGE_DESCRIPTION "Solver application for quadratic equations")
set(CPACK_PACKAGE_VENDOR "Your Organization")
set(CPACK_PACKAGE_CONTACT "maintainer@example.com")

# Настройки для Debian
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "${CPACK_PACKAGE_CONTACT}")
set(CPACK_DEBIAN_PACKAGE_DEPENDS "libc6 (>= 2.31), libstdc++6 (>= 10.2)")
set(CPACK_DEBIAN_FILE_NAME "DEB-DEFAULT")

# Настройки для RPM
set(CPACK_RPM_PACKAGE_REQUIRES "libstdc++ >= 10.2")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")

# Настройки для WiX (Windows MSI) - без иконок
if(WIN32)
    list(APPEND CPACK_GENERATOR "WIX")
    set(CPACK_WIX_PRODUCT_GUID "YABCDEFGH-1234-5678-9012-345678901234")  # Обязательно замените на реальный GUID
    set(CPACK_WIX_UPGRADE_GUID "ZYXWVUTS-9876-5432-1098-765432109876")  # Другой GUID
    # Убрали ссылку на иконку
endif()

# Настройки для DMG (macOS) - без фона
if(APPLE)
    list(APPEND CPACK_GENERATOR "DragNDrop")
    set(CPACK_DMG_VOLUME_NAME "${PROJECT_NAME}")
    # Убрали ссылку на background.png
endif()

# Основные форматы пакетов
set(CPACK_GENERATOR "TGZ;DEB;RPM")

include(InstallRequiredSystemLibraries)
include(CPack)
