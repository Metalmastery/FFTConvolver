cmake_minimum_required(VERSION 3.10)
project(FFTConvolver VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(SOURCES
    AudioFFT.cpp
    FFTConvolver.cpp
    TwoStageFFTConvolver.cpp
    Utilities.cpp
)

add_library(FFTConvolver STATIC ${SOURCES})

target_include_directories(FFTConvolver PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include/FFTConvolver>
)

# Use Apple Accelerate on macOS for best FFT performance
if(APPLE)
    target_compile_definitions(FFTConvolver PUBLIC AUDIOFFT_APPLE_ACCELERATE)
    target_link_libraries(FFTConvolver PUBLIC "-framework Accelerate")
endif()

set_property(TARGET FFTConvolver PROPERTY POSITION_INDEPENDENT_CODE ON)

install(TARGETS FFTConvolver
    ARCHIVE DESTINATION lib
)
install(FILES AudioFFT.h FFTConvolver.h TwoStageFFTConvolver.h Utilities.h
    DESTINATION include/FFTConvolver
)
