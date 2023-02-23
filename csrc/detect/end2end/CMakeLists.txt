cmake_minimum_required(VERSION 2.8.12)

set(CMAKE_CUDA_ARCHITECTURES 60 61 62 70 72 75 86)
set(CMAKE_CUDA_COMPILER "C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.7/bin/nvcc.exe")

project(yolov8 LANGUAGES CXX CUDA)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -O3 -g")
set(CMAKE_CXX_STANDARD 17)
#set(CMAKE_BUILD_TYPE Release)
option(CUDA_USE_STATIC_CUDA_RUNTIME OFF)

# CUDA
find_package(CUDA REQUIRED)
message(STATUS "CUDA Libs: \n${CUDA_LIBRARIES}\n")
message(STATUS "CUDA Headers: \n${CUDA_INCLUDE_DIRS}\n")

# OpenCV
find_package(OpenCV REQUIRED)
message(STATUS "OpenCV Libs: \n${OpenCV_LIBS}\n")
message(STATUS "OpenCV Libraries: \n${OpenCV_LIBRARIES}\n")
message(STATUS "OpenCV Headers: \n${OpenCV_INCLUDE_DIRS}\n")

# TensorRT
set(TensorRT_INCLUDE_DIRS "C:/dev/TensorRT-8.5.3.1.Windows10.x86_64.cuda-11.8.cudnn8.6/TensorRT-8.5.3.1/include")
set(TensorRT_LIBRARIES "C:/dev/TensorRT-8.5.3.1.Windows10.x86_64.cuda-11.8.cudnn8.6/TensorRT-8.5.3.1/lib")


message(STATUS "TensorRT Libs: \n${TensorRT_LIBRARIES}\n")
message(STATUS "TensorRT Headers: \n${TensorRT_INCLUDE_DIRS}\n")

list(APPEND INCLUDE_DIRS
        ${CUDA_INCLUDE_DIRS}
        ${OpenCV_INCLUDE_DIRS}
        ${TensorRT_INCLUDE_DIRS}
        include
        )

list(APPEND ALL_LIBS
        ${CUDA_LIBRARIES}
        ${OpenCV_LIBRARIES}
        ${TensorRT_LIBRARIES}
        )

include_directories(${INCLUDE_DIRS})

add_executable(${PROJECT_NAME}
        main.cpp
        include/yolov8.hpp
        include/common.hpp
        )

target_link_directories(${PROJECT_NAME} PUBLIC ${ALL_LIBS})
target_link_libraries(${PROJECT_NAME} PRIVATE nvinfer nvinfer_plugin ${CUDA_LIBRARIES} ${OpenCV_LIBS})
#target_link_libraries(${PROJECT_NAME} PRIVATE nvinfer nvinfer_plugin ${OpenCV_LIBS})
