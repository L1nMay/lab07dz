cmake_minimum_required(VERSION 3.14)
project(lab07dz VERSION 1.0.0 LANGUAGES CXX)

# 1. Установка политик CMake
set(CMAKE_POLICY_DEFAULT_CMP0135 NEW)
set(CMAKE_POLICY_DEFAULT_CMP0169 NEW)

# 2. Загрузка Hunter через git clone (самый надежный способ)
find_package(Git REQUIRED)
set(HUNTER_GIT_DIR "${CMAKE_BINARY_DIR}/hunter")
set(HUNTER_GATE_FILE "${HUNTER_GIT_DIR}/cmake/HunterGate.cmake")

if(NOT EXISTS "${HUNTER_GATE_FILE}")
    message(STATUS "Cloning Hunter...")
    execute_process(
        COMMAND ${GIT_EXECUTABLE} clone --depth 1 --branch v0.24.8
                https://github.com/cpp-pm/hunter.git "${HUNTER_GIT_DIR}"
        RESULT_VARIABLE result
    )
    if(NOT result EQUAL 0)
        message(FATAL_ERROR "Failed to clone Hunter repository")
    endif()
endif()

# 3. Включение HunterGate
list(APPEND CMAKE_MODULE_PATH "${HUNTER_GIT_DIR}/cmake/modules")
include("${HUNTER_GATE_FILE}")

# 4. Инициализация Hunter
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.24.8.tar.gz"
    SHA1 "d8e60ff290dd31553486fd339c5ec036d7ff29ea4eb85f2513ddcf31453f5434"
)

# 5. Основная конфигурация проекта
add_library(lab07dz
    src/lab07dz.cpp
    include/lab07dz/lab07dz.hpp
)

target_include_directories(lab07dz PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# ... (остальная часть вашего CMakeLists.txt)
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
