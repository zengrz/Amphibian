project(Amphibian
VERSION 1.0
DESCRIPTION "CUDA Lib"
LANGUAGES CXX CUDA
)

include(CTest)

set(PROJECT_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/Amphibian/)
set(TESTS ${CMAKE_CURRENT_SOURCE_DIR}/Amphibian/Tests)
message(${PROJECT_ROOT})
message(${TESTS})

# GLOB
file(
GLOB AMPHIBIAN_HASHER
#${PROJECT_ROOT}/Hasher.cuh
${PROJECT_ROOT}/Hasher.cu
)
file(
GLOB AMPHIBIAN_STRING
#${PROJECT_ROOT}/String.cuh
${PROJECT_ROOT}/String.cu
)
file(
GLOB AMPHIBIAN_LINKEDLIST
#${PROJECT_ROOT}/LinkedList.cuh
#${PROJECT_ROOT}/dummy.cu
)
file(
GLOB AMPHIBIAN_SET
#${PROJECT_ROOT}/Set.cuh
#${PROJECT_ROOT}/dummy.cu
)
file(
GLOB AMPHIBIAN_MAP
#${PROJECT_ROOT}/Map.cuh
#${PROJECT_ROOT}/dummy.cu
)

# lib
add_library(amphibian-hasher ${AMPHIBIAN_HASHER})
target_include_directories(amphibian-hasher PUBLIC ${PROJECT_ROOT})
set_target_properties(amphibian-hasher PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
target_compile_features(amphibian-hasher PUBLIC cxx_std_11)

add_library(amphibian-string ${AMPHIBIAN_STRING})
target_include_directories(amphibian-string PUBLIC ${PROJECT_ROOT})
set_target_properties(amphibian-string PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
target_compile_features(amphibian-string PUBLIC cxx_std_11)

add_library(amphibian-interface-lib INTERFACE)
target_include_directories(amphibian-interface-lib INTERFACE ${PROJECT_ROOT})
target_sources(amphibian-interface-lib INTERFACE ${AMPHIBIAN_LINEDLIST} ${AMPHIBIAN_SET} ${AMPHIBIAN_MAP})

# tests
if(BUILD_TESTING)
    file(
    GLOB AMPHIBIAN_TEST_HASHER
    #${TESTS}/HasherTest.cuh
    ${TESTS}/HasherTest.cu
    )
    file(
    GLOB AMPHIBIAN_TEST_LINKEDLIST
    #${TESTS}/LinkedListTest.cuh
    ${TESTS}/LinkedListTest.cu
    )
    file(
    GLOB AMPHIBIAN_TEST_STRING
    #${TESTS}/StringTest.cuh
    ${TESTS}/StringTest.cu
    )
    file(
    GLOB AMPHIBIAN_TEST_SET
    #${TESTS}/SetTest.cuh
    ${TESTS}/SetTest.cu
    )
    file(
    GLOB AMPHIBIAN_TEST_MAP
    #${TESTS}/MapTest.cuh
    ${TESTS}/MapTest.cu
    )
    file(GLOB MAIN_SRCS kernel.cu)
    #file(GLOB DUMMY_SRCS dummy.cpp)

    add_library(amphibian-test-hasher ${AMPHIBIAN_TEST_HASHER})
    target_include_directories(amphibian-test-hasher PUBLIC ${PROJECT_ROOT})
    target_include_directories(amphibian-test-hasher PUBLIC ${TESTS})
    set_target_properties(amphibian-test-hasher PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
    target_compile_features(amphibian-test-hasher PUBLIC cxx_std_11)

    add_library(amphibian-test-string ${AMPHIBIAN_TEST_STRING})
    target_include_directories(amphibian-test-string PUBLIC ${PROJECT_ROOT})
    target_include_directories(amphibian-test-string PUBLIC ${TESTS})
    set_target_properties(amphibian-test-string PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
    target_compile_features(amphibian-test-string PUBLIC cxx_std_11)

    add_library(amphibian-test-linkedlist ${AMPHIBIAN_TEST_LINKEDLIST})
    target_include_directories(amphibian-test-linkedlist PUBLIC ${PROJECT_ROOT})
    target_include_directories(amphibian-test-linkedlist PUBLIC ${TESTS})
    set_target_properties(amphibian-test-linkedlist PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
    target_compile_features(amphibian-test-linkedlist PUBLIC cxx_std_11)

    add_library(amphibian-test-set ${AMPHIBIAN_TEST_SET})
    target_include_directories(amphibian-test-set PUBLIC ${PROJECT_ROOT})
    target_include_directories(amphibian-test-set PUBLIC ${TESTS})
    set_target_properties(amphibian-test-set PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
    target_compile_features(amphibian-test-set PUBLIC cxx_std_11)

    add_library(amphibian-test-map ${AMPHIBIAN_TEST_MAP})
    target_include_directories(amphibian-test-map PUBLIC ${PROJECT_ROOT})
#    target_link_libraries(amphibian-test-map PRIVATE amphibian-hasher)
#    target_link_libraries(amphibian-test-map PRIVATE amphibian-string)
#    target_link_libraries(amphibian-test-map PRIVATE amphibian-interface-lib)
    target_include_directories(amphibian-test-map PUBLIC ${TESTS})
    set_target_properties(amphibian-test-map PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
    target_compile_features(amphibian-test-map PUBLIC cxx_std_11)

    #add_library(amphibian-lib-tests ${LIB_TESTS_CU_SRCS} ${LIB_TESTS_CU_HDRS})
    #add_library(amphibian-lib-tests STATIC ${LIB_TESTS_CU_SRCS} ${LIB_TESTS_CU_HDRS})
    #target_link_libraries(amphibian-lib -lpthread)

    #add_executable(amphibian ${MAIN_SRCS} ${DUMMY_SRCS})
    # exe
    add_executable(amphibian ${MAIN_SRCS})
    set_target_properties(amphibian PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
    #target_link_libraries(amphibian amphibian-lib)
    target_link_libraries(amphibian amphibian-hasher)
    target_link_libraries(amphibian amphibian-string)
    target_link_libraries(amphibian amphibian-interface-lib)
    target_link_libraries(amphibian amphibian-test-hasher)
    target_link_libraries(amphibian amphibian-test-string)
    target_link_libraries(amphibian amphibian-test-linkedlist)
    target_link_libraries(amphibian amphibian-test-set)
    target_link_libraries(amphibian amphibian-test-map)
    target_compile_features(amphibian PUBLIC cxx_std_11)

    #  add_test(NAME particles_10k COMMAND particle_test 10000 )
    #  add_test(NAME particles_256k COMMAND particle_test 256000 )

    #  if(APPLE)
        # We need to add the default path to the driver (libcuda.dylib) as an rpath,
        # so that the static cuda runtime can find it at runtime.
    #    set_property(TARGET amphibian-lib-test PROPERTY BUILD_RPATH #${CMAKE_CUDA_IMPLICIT_LINK_DIRECTORIES})
    #  endif()
endif()

