cmake_minimum_required(VERSION 3.14)
project(lab07dz VERSION 1.0.0 LANGUAGES CXX)

# Отключаем устаревшие предупреждения
cmake_policy(SET CMP0169 NEW)  # Для FetchContent_MakeAvailable
cmake_policy(SET CMP0135 NEW)  # Для временных меток

# Загрузка Hunter через git (надежный способ)
include(FetchContent)
FetchContent_Declare(
    hunter
    GIT_REPOSITORY "https://github.com/cpp-pm/hunter.git"
    GIT_TAG v0.24.8
    SOURCE_DIR "${CMAKE_BINARY_DIR}/_deps/hunter-src"
    BINARY_DIR "${CMAKE_BINARY_DIR}/_deps/hunter-build"
)

# Явная инициализация Hunter
FetchContent_MakeAvailable(hunter)

# После загрузки добавляем модули Hunter в путь
list(APPEND CMAKE_MODULE_PATH "${hunter_SOURCE_DIR}/cmake/modules")

# Проверяем наличие HunterGate
if(NOT COMMAND HunterGate)
    message(FATAL_ERROR "HunterGate not found after Hunter initialization")
endif()

HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.24.8.tar.gz"
    SHA1 "d8e60ff290dd31553486fd339c5ec036d7ff29ea4eb85f2513ddcf31453f5434"
)

# Ваша основная конфигурация проекта
[...]
# Основная библиотека
add_library(lab07dz
    src/lab07dz.cpp
    include/lab07dz/lab07dz.hpp
)

target_include_directories(lab07dz PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Установка
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

# Тесты
enable_testing()
add_executable(lab07dz_test tests/test.cpp)
target_link_libraries(lab07dz_test lab07dz)
add_test(NAME lab07dz_test COMMAND lab07dz_test)
