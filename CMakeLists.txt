cmake_minimum_required(VERSION 3.14)
project(lab07dz VERSION 1.0.0 LANGUAGES CXX)

# Установка политик CMake
cmake_policy(SET CMP0135 NEW)
cmake_policy(SET CMP0169 NEW)

# Способ 1: Использование Hunter через ExternalProject (надежнее)
include(ExternalProject)
ExternalProject_Add(
    hunter_project
    GIT_REPOSITORY "https://github.com/cpp-pm/hunter.git"
    GIT_TAG v0.24.8
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ""
    INSTALL_COMMAND ""
)

# Получаем путь к исходникам Hunter
ExternalProject_Get_Property(hunter_project SOURCE_DIR)
set(HUNTER_SOURCE_DIR "${SOURCE_DIR}")
list(APPEND CMAKE_MODULE_PATH "${HUNTER_SOURCE_DIR}/cmake/modules")

# Инициализация Hunter
include("${HUNTER_SOURCE_DIR}/cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.24.8.tar.gz"
    SHA1 "d8e60ff290dd31553486fd339c5ec036d7ff29ea4eb85f2513ddcf31453f5434"
)

# Основная конфигурация проекта
add_library(lab07dz
    src/lab07dz.cpp
    include/lab07dz/lab07dz.hpp
)

target_include_directories(lab07dz PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# ... остальная часть вашего CMakeLists.txt
# 6. Установка
install(TARGETS lab07dz EXPORT lab07dz-targets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

install(DIRECTORY include/ DESTINATION include)
install(EXPORT lab07dz-targets
    FILE lab07dz-config.cmake
    DESTINATION lib/cmake/lab07dz
)

# 7. Тесты
enable_testing()
add_executable(lab07dz_test tests/test.cpp)
target_link_libraries(lab07dz_test lab07dz)
add_test(NAME lab07dz_test COMMAND lab07dz_test)
