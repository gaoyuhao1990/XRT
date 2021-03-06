set(TESTNAME "kernel_bw.exe")

if (WIN32)
  set(OpenCL_INCLUDE_DIR ${OCL_ROOT}/include)
  find_library(OpenCL_LIBRARY
    NAMES OpenCL
    HINTS "${OCL_ROOT}/lib")
  include_directories(${OpenCL_INCLUDE_DIR})
else()
  find_package(OpenCL)
  find_package(Boost REQUIRED COMPONENTS system filesystem)
endif(WIN32)

include_directories( ${Boost_INCLUDE_DIRS}  ../../../src/include/1_2 ../../../src/runtime_src/core/include src/ ../common/includes/xcl2 )
add_executable(${TESTNAME} ../common/includes/xcl2/xcl2.cpp src/host.cpp)
target_link_libraries(${TESTNAME} PRIVATE ${xrt_coreutil_LIBRARY})

if (NOT WIN32)
  target_link_libraries(${TESTNAME} PRIVATE ${uuid_LIBRARY} pthread ${OpenCL_LIBRARY} ${Boost_FILESYSTEM_LIBRARY} ${Boost_SYSTEM_LIBRARY})
endif(NOT WIN32)

install(TARGETS ${TESTNAME}
  RUNTIME DESTINATION ${XRT_VALIDATE_DIR})
