cmake_minimum_required(VERSION 3.14)
project(lab07dz VERSION 1.0.0 LANGUAGES CXX)

# Отключение предупреждения о временных метках
cmake_policy(SET CMP0135 NEW)

# Используем актуальные хеш-суммы
include(FetchContent)
FetchContent_Declare(
    hunter
    URL "https://github.com/cpp-pm/hunter/archive/v0.24.8.tar.gz"
    URL_HASH SHA256=d8e60ff290dd31553486fd339c5ec036d7ff29ea4eb85f2513ddcf31453f5434
    DOWNLOAD_EXTRACT_TIMESTAMP TRUE
)
FetchContent_MakeAvailable(hunter)

include(hunter)
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.24.8.tar.gz"
    SHA1 "d8e60ff290dd31553486fd339c5ec036d7ff29ea4eb85f2513ddcf31453f5434"
)

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
