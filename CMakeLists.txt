cmake_minimum_required(VERSION 3.10)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})

project(MyProject VERSION ${PROJECT_VERSION})

# Создание исполняемого файла из main.cpp
add_executable(my_project main.cpp)

# Установка стандартов C++
set_target_properties(my_project PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

# Установка уровней предупреждений компилятора
if (MSVC)
    target_compile_options(my_project PRIVATE /W4)
else ()
    target_compile_options(my_project PRIVATE -Wall -Wextra -pedantic -Werror)
endif()

# Установка имени пакета для CPack
set(CPACK_PACKAGE_NAME "MyProject")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
set(CPACK_PACKAGE_CONTACT "example@example.com")

# Установка генератора CPack для macOS или Linux
if (APPLE)
    set(CPACK_GENERATOR "Bundle")  # Для создания .app или .pkg на macOS
else()
    set(CPACK_GENERATOR "DEB")  # Для создания .deb на Linux
endif()

include(CPack)

# Включение тестирования
enable_testing()

# Если у вас есть тесты, вы можете добавить их здесь
# add_executable(my_tests test_version.cpp)
# target_link_libraries(my_tests PRIVATE my_project)
# add_test(my_tests my_tests)