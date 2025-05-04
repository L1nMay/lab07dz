cmake_minimum_required(VERSION 3.14)
project(lab07dz VERSION 1.0.0 LANGUAGES CXX)

# Современный способ подключения Hunter
include(FetchContent)
FetchContent_Declare(
    hunter
    URL "https://github.com/cpp-pm/hunter/archive/v0.24.8.tar.gz"
    URL_HASH SHA256=5e4c9408aad4c1c0e3a25fc2e5d3ca3881a6a6a3a7a2a2a2a2a2a2a2a2a2a2a
)
FetchContent_MakeAvailable(hunter)

include(hunter)
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.24.8.tar.gz"
    SHA1 "5e4c9408aad4c1c0e3a25fc2e5d3ca3881a6a6a3a7a2a2a2a2a2a2a2a2a2a2a"
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
