cmake_minimum_required(VERSION 3.2)

project(just_for_fun VERSION 0.10.$ENV{TRAVIS_BUILD_NUMBER})

configure_file(version.h.in version.h)

find_package(GTest REQUIRED)
find_package(Threads REQUIRED)
include_directories(${GTEST_INCLUDE_DIR})
include_directories(googlemock/include)


add_executable(helloworld_cli main.cpp)
add_library(hellolib lib.cpp)
add_executable(hellotest tests/test_version.cpp)

set_target_properties(helloworld_cli hellolib hellotest PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

target_include_directories(helloworld_cli
    PRIVATE "${CMAKE_BINARY_DIR}"
)
target_include_directories(hellolib
    PRIVATE "${CMAKE_BINARY_DIR}"
)

target_include_directories(hellolib
    PRIVATE "${CMAKE_BINARY_DIR}"
)

target_link_libraries(helloworld_cli PRIVATE
    hellolib
)

link_directories(/usr/local/lib)
target_link_libraries(hellotest PRIVATE
    hellolib
    ${GTEST_BOTH_LIBRARIES}
    ${CMAKE_THREAD_LIBS_INIT}
    gmock
)

target_compile_options(helloworld_cli PRIVATE
        -Wall -Wextra -pedantic -Werror
)
target_compile_options(hellolib PRIVATE
        -Wall -Wextra -pedantic -Werror
)
target_compile_options(hellotest PRIVATE
        -Wall -Wextra -pedantic -Werror
)

install(TARGETS helloworld_cli RUNTIME DESTINATION bin)

set (CPACK_GENERATOR DEB)
set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
set(CPACK_PACKAGE_CONTACT "Developer den4ik")
include(CPack)

enable_testing()

add_test(hellotest test_version)
