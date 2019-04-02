project(Amphibian CXX CUDA)

file(GLOB LIB_CU_SRCS ${CMAKE_CURRENT_SOURCE_DIR}/Amphibian/*.cu)
file(GLOB LIB_CU_HDRS ${CMAKE_CURRENT_SOURCE_DIR}/Amphibian/*.cuh)
file(GLOB LIB_TESTS_CU_SRCS ${CMAKE_CURRENT_SOURCE_DIR}/Amphibian/Tests/*.cu)
file(GLOB LIB_TESTS_CU_HDRS ${CMAKE_CURRENT_SOURCE_DIR}/Amphibian/Tests/*.cuh)
file(GLOB MAIN_CU_SRCS kernel.cu)
#file(GLOB DUMMY_SRCS dummy.cpp)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/Amphibian
    ${CMAKE_CURRENT_SOURCE_DIR}/Amphibian/Tests
)

message(${CMAKE_CUDA_COMPILER})
message(${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})
message(${CMAKE_SOURCE_DIR})
message(${CMAKE_CURRENT_SOURCE_DIR})

add_library(amphibian-lib ${LIB_CU_SRCS} ${LIB_CU_HDRS})
add_library(amphibian-lib-tests ${LIB_TESTS_CU_SRCS} ${LIB_TESTS_CU_HDRS})
#add_library(amphibian-lib-tests STATIC ${LIB_TESTS_CU_SRCS} ${LIB_TESTS_CU_HDRS})
set_target_properties(amphibian-lib PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
set_target_properties(amphibian-lib-tests PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
#target_link_libraries(amphibian-lib -lpthread)

#add_executable(amphibian ${MAIN_CU_SRCS} ${DUMMY_SRCS})
add_executable(amphibian ${MAIN_CU_SRCS})
target_link_libraries(amphibian amphibian-lib)
target_link_libraries(amphibian amphibian-lib-tests)
set_target_properties(amphibian PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
target_compile_features(amphibian PUBLIC cxx_std_11)

