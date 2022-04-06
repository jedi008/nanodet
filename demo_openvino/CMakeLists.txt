cmake_minimum_required(VERSION 3.4.1)
set(CMAKE_CXX_STANDARD 14)

project(nanodet_demo)

MESSAGE(STATUS "current platform: Windows")
#如果设置了环境变量可以省略以下步奏
set(OpenCV_DIR "D:/opencv452/build")
set(InferenceEngine_DIR "C:/Program Files (x86)/Intel/openvino_2022.1.0.643/runtime/cmake")
set(ngraph_DIR "C:/Program Files (x86)/Intel/openvino_2022.1.0.643/runtime/cmake")

find_package(OpenCV REQUIRED)
find_package(InferenceEngine REQUIRED)
find_package(ngraph REQUIRED)

include_directories(
    ${OpenCV_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
)

add_executable(nanodet_demo main.cpp nanodet_openvino.cpp)

target_link_libraries(
    nanodet_demo
    ${InferenceEngine_LIBRARIES}
    ${NGRAPH_LIBRARIES}
    ${OpenCV_LIBS}
)

