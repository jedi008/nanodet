cmake_minimum_required(VERSION 3.4.1)
set(CMAKE_CXX_STANDARD 14)

project(nanodet_demo)


MESSAGE(STATUS "operation system is ${CMAKE_SYSTEM}")

IF (CMAKE_SYSTEM_NAME MATCHES "Linux")

    MESSAGE(STATUS "current platform: Linux ")
    #如果设置了环境变量可以省略以下步奏
    set(Qt5_DIR "/home/jedi/Qt5.14.2/5.14.2/gcc_64/lib/cmake/Qt5")
    set(Torch_DIR "/home/jedi/libtorch/share/cmake/Torch")

ELSEIF (CMAKE_SYSTEM_NAME MATCHES "Windows")

    MESSAGE(STATUS "current platform: Windows")
    #如果设置了环境变量可以省略以下步奏
    set(Qt5_DIR "D:/Qt/Qt5.13.2/5.13.2/msvc2017_64/lib/cmake/Qt5")
    set(OpenCV_DIR "D:/opencv452/build")
    set(Torch_DIR "D:/LibTorch_1.8.2.R/share/cmake/Torch")

ELSEIF (CMAKE_SYSTEM_NAME MATCHES "FreeBSD")

    MESSAGE(STATUS "current platform: FreeBSD")

ELSE ()

    MESSAGE(STATUS "other platform: ${CMAKE_SYSTEM_NAME}")

ENDIF (CMAKE_SYSTEM_NAME MATCHES "Linux")

MESSAGE(STSTUS "###################################")



find_package(OpenCV REQUIRED)
find_package(Torch REQUIRED)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")



include_directories(
    ${OpenCV_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
)

add_executable(nanodet_demo main.cpp nanodet_libtorch.cpp)

target_link_libraries(
    nanodet_demo
    ${TORCH_LIBRARIES}
    ${OpenCV_LIBS}
)

if (MSVC)
  file(GLOB TORCH_DLLS "${TORCH_INSTALL_PREFIX}/lib/*.dll")
  add_custom_command(TARGET nanodet_demo
                     POST_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy_if_different
                     ${TORCH_DLLS}
                     $<TARGET_FILE_DIR:nanodet_demo>)
endif (MSVC)
