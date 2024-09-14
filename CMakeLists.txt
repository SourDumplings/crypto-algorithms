cmake_minimum_required(VERSION 3.10)

# 项目名称
project(CryptoAlgorithms)

# 检查是否作为子项目
if(CMAKE_SOURCE_DIR STREQUAL PROJECT_SOURCE_DIR)
    # 作为主项目
    if(NOT CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
        set(CMAKE_INSTALL_PREFIX "${CMAKE_PREFIX_PATH}" CACHE PATH "Install path prefix" FORCE)
    else()
        set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/dist" CACHE PATH "Install path prefix" FORCE)
    endif()
else()
    # 作为子项目
    if(NOT CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
        set(CMAKE_INSTALL_PREFIX "${CMAKE_PREFIX_PATH}" CACHE PATH "Install path prefix" FORCE)
    endif()
endif()

message("CMAKE_INSTALL_PREFIX: " ${CMAKE_INSTALL_PREFIX})

# 设置源文件列表
set(SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/aes.c
    ${CMAKE_CURRENT_SOURCE_DIR}/arcfour.c
    ${CMAKE_CURRENT_SOURCE_DIR}/base64.c
    ${CMAKE_CURRENT_SOURCE_DIR}/blowfish.c
    ${CMAKE_CURRENT_SOURCE_DIR}/des.c
    ${CMAKE_CURRENT_SOURCE_DIR}/md2.c
    ${CMAKE_CURRENT_SOURCE_DIR}/md5.c
    ${CMAKE_CURRENT_SOURCE_DIR}/rot-13.c
    ${CMAKE_CURRENT_SOURCE_DIR}/sha1.c
    ${CMAKE_CURRENT_SOURCE_DIR}/sha256.c
)

file(GLOB HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/*.h)

add_library(crypto_algorithms_static STATIC ${SOURCES})
target_include_directories(crypto_algorithms_static PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

if(${BUILD_CRYPTO_ALGORITHMS_TEST})
    add_executable(crypto_algorithms_test aes_test.c)
    target_link_libraries(crypto_algorithms_test PRIVATE crypto_algorithms_static)
endif()

# 安装目标
install(TARGETS crypto_algorithms_static crypto_algorithms_test
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

# 安装头文件
install(FILES ${HEADERS} DESTINATION include/crypto_algorithms)