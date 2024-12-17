cmake_minimum_required(VERSION 3.10)
project(yolov8n_demo)

# 设置C++标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 设置调试标志
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g")

# 环境变量设置（原build_vx.sh中的设置）
set(OPENCV_ROOT "/usr/include/opencv2" CACHE PATH "OpenCV root directory")
set(VIVANTE_SDK_INC "/usr/share/npu/sdk/include" CACHE PATH "Vivante SDK include directory")
set(VIVANTE_SDK_LIB "/usr/lib" CACHE PATH "Vivante SDK library directory")
set(OVXLIB_DIR "${CMAKE_SOURCE_DIR}/ovxlib" CACHE PATH "OVXLIB directory")

# 查找依赖包
find_package(OpenCV REQUIRED)

# 添加包含目录
include_directories(
    ${VIVANTE_SDK_INC}
    ${OVXLIB_DIR}/include
    ${CMAKE_SOURCE_DIR}/include
    ${OPENCV_ROOT}/modules
    ${OPENCV_ROOT}/modules/highgui/include
    ${OPENCV_ROOT}/modules/core/include
    ${OPENCV_ROOT}/modules/imgproc/include
    ${OPENCV_ROOT}/modules/objdetect/include
    ${OPENCV_ROOT}/modules/imgcodecs/include
    ${OPENCV_ROOT}/modules/videoio/include
    ${CMAKE_SOURCE_DIR}
)

# 添加源文件
set(SOURCES
    main.cpp
    vnn_yolov8n.c
    yolov8n_process.c
)

# 创建可执行文件
add_executable(yolov8n_demo_x11_usb ${SOURCES})

# 链接库
target_link_libraries(yolov8n_demo_x11_usb
    ${OpenCV_LIBS}
    -L${OVXLIB_DIR}/lib -lovxlib
    -L${VIVANTE_SDK_LIB} -lOpenVX -lOpenVXU -lGAL -lovxlib -lArchModelSw -lNNArchPerf
    -lm -ldl
)