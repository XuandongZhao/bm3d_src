#cmake_minimum_required(VERSION 3.14)
project(bm3d)
# set(CMAKE_CXX_STANDARD 10)

# Are we using gcc?
if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    # GCC on MacOs needs this option to use the clang assembler
    if (APPLE)
        set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wa,-q")
        set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wa,-q")
    endif ()
    # Optimize to the current CPU and enable warnings
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=native -Wall -Wextra")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native -Wall -Wextra")
endif ()

find_package (PNG REQUIRED)
find_path (FFTW_INCLUDES fftw3.h)
find_library (FFTWF_LIBRARIES NAMES fftw3f)
include_directories (PUBLIC ${PNG_INCLUDE_DIRS} PUBLIC ${FFTW_INCLUDES})
link_libraries (${PNG_LIBRARIES} ${FFTWF_LIBRARIES})
set(SOURCE_FILES bm3d.cpp bm3d.h io_png.c io_png.h lib_transforms.cpp lib_transforms.h mt19937ar.c mt19937ar.h utilities.cpp utilities.h main.cpp)
set_property(SOURCE iio.c PROPERTY COMPILE_FLAGS "-Wno-unused-variable -Wno-unused-parameter -Wno-pointer-sign -Wno-parentheses -Wno-deprecated-declarations -Wno-unused-function")
add_executable(bm3d ${SOURCE_FILES})