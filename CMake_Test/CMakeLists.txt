cmake_minimum_required(VERSION 3.10)
project(Test)

# 寻找 OpenCV
find_package(OpenCV 4.5.3 REQUIRED)

# 执行写好脚本的子目录
add_subdirectory(common)
add_subdirectory(modules)

# 生成可执行文件
add_executable(test main.cpp)

# 链接所有模块和 OpenCV
target_link_libraries(test PUBLIC math A1 A2 M1 M2 ${OpenCV_LIBS})
