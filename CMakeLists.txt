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

# 根据 BUILD_SHARED_LIBS 决定库类型
if(BUILD_SHARED_LIBS)
    set(LIB_TYPE SHARED)
else()
    set(LIB_TYPE STATIC)
endif()

# 添加库
add_library(crypto_algorithms ${LIB_TYPE} ${SOURCES})

# 设置库的包含目录
target_include_directories(crypto_algorithms PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# 安装目标
install(TARGETS crypto_algorithms
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

if(WIN32 AND ${CMAKE_BUILD_TYPE} MATCHES "Debug")
    install(FILES $<TARGET_PDB_FILE:crypto_algorithms>
        DESTINATION bin
        CONFIGURATIONS Debug
    )
endif()

# 安装头文件
install(FILES ${HEADERS} DESTINATION include)