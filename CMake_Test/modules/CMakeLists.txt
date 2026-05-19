set(Common_INCLUDE_DIRS ${CMAKE_CURRENT_LIST_DIR}/include CACHE PATH "Common_INCLUDE_DIRS")

execute_process(COMMAND ls ${CMAKE_CURRENT_SOURCE_DIR} OUTPUT_VARIABLE TEST_MODULE_COMMON_ST)
string(REPLACE "\n" ";" TEST_MODULE_COMMON_ST "${TEST_MODULE_COMMON_ST}")
foreach(m ${TEST_MODULE_COMMON_ST})
    if(IS_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${m})
        if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/${m}/CMakeLists.txt)
            add_subdirectory(${m})
        endif()
    endif()
endforeach()

# 编译 A1 和 A2
add_library(A1 A1/src/A11.cpp A1/src/A12.cpp A1/src/A13.cpp)
target_include_directories(A1 PUBLIC A1/include)

add_library(A2 A2/src/A2.cpp)
target_include_directories(A2 PUBLIC A2/include)

# 编译 M2 并指明卡尔曼滤波的头文件路径
add_library(M2 M2/src/M2.cpp)
target_include_directories(M2 PUBLIC M2/include ../common/kalman/include)
target_link_libraries(M2 PUBLIC ${OpenCV_LIBS} A1 A2)
