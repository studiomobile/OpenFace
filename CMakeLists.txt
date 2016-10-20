cmake_minimum_required (VERSION 2.8)
project (OpenFace)

set(CURRENT_DIR ${CMAKE_CURRENT_LIST_DIR})


# CMake
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin/)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/modules/")


# OpenCV
set(OPENCV_DIR ${CURRENT_DIR}/../OpenCV-android-sdk/sdk/native/jni)
set(OPENCV_LIBRARIES 
		opencv_videostab
		opencv_videoio
		opencv_video
		opencv_superres
		opencv_stitching
		opencv_shape
		opencv_photo
		opencv_objdetect
		opencv_ml
		opencv_imgproc
		opencv_imgcodecs
		opencv_highgui
		opencv_flann
		opencv_features2d
		opencv_core
		opencv_calib3d)

set(OPENCV_INCLUDE_DIRS 
	${CURRENT_DIR}/../OpenCV-android-sdk/sdk/native/jni/include/opencv; 
	${CURRENT_DIR}/../OpenCV-android-sdk/sdk/native/jni/include)

include_directories(${OPENCV_INCLUDE_DIRS})


# Boost
set(BOOST_INCLUDE_DIRS ${CURRENT_DIR}/../boost_1_62_0)
set(BOOST_LIBRARY_DIRS ${CURRENT_DIR}/../boost_1_62_0/android-armeabi/lib)
set(Boost_LIBRARIES 
		libboost_filesystem
		libboost_system)

include_directories(${BOOST_INCLUDE_DIRS})
include_directories(${BOOST_INCLUDE_DIRS}/boost)
link_directories(${BOOST_LIBRARY_DIRS})	

if(WIN32)
	include_directories(lib/3rdParty/boost)
	include_directories(lib/3rdParty/boost/boost)
	link_directories( ${PROJECT_SOURCE_DIR}/lib/3rdParty/boost/lib )
endif()


# TBB 
set(TBB_ROOT_DIR ${CURRENT_DIR}/lib/3rdParty/tbb)
set(TBB_INCLUDE_DIRS ${CURRENT_DIR}/lib/3rdParty/tbb/include)
set(TBB_LIBRARIES 
		tbb
		tbb_malloc)



# Move LandmarkDetector model
file(GLOB files "lib/local/LandmarkDetector/model/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model)
	endif(MSVC)
endforeach()

# Move the hierarchical LandmarkDetector models
file(GLOB files "lib/local/LandmarkDetector/model/model*")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model)
	endif(MSVC)
endforeach()

file(GLOB files "lib/local/LandmarkDetector/model/detection_validation/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model/detection_validation)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model/detection_validation)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model/detection_validation)
	endif(MSVC)
endforeach()

file(GLOB files "lib/local/LandmarkDetector/model/patch_experts/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model/patch_experts)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model/patch_experts)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model/patch_experts)
	endif(MSVC)
endforeach()

file(GLOB files "lib/local/LandmarkDetector/model/pdms/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/model/pdms)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/model/pdms)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/model/pdms)
	endif(MSVC)
endforeach()

# Move OpenCV classifiers
file(GLOB files "lib/3rdParty/OpenCV3.1/classifiers/*.xml")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/classifiers)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/classifiers)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/classifiers)
	endif(MSVC)
endforeach()

# Move AU prediction modules
file(GLOB files "lib/local/FaceAnalyser/AU_predictors/*.txt")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/AU_predictors)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/AU_predictors)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/AU_predictors)
	endif(MSVC)
endforeach()

# Move AU prediction modules
file(GLOB files "lib/local/FaceAnalyser/AU_predictors/svr*")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/AU_predictors)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/AU_predictors)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/AU_predictors)
	endif(MSVC)
endforeach()

# Move AU prediction modules
file(GLOB files "lib/local/FaceAnalyser/AU_predictors/svm*")
foreach(file ${files})
	if (MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Debug/AU_predictors)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/Release/AU_predictors)
	else(MSVC)
		file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/bin/AU_predictors)
	endif(MSVC)
endforeach()

if (${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU")
    execute_process(COMMAND ${CMAKE_CXX_COMPILER} -dumpversion OUTPUT_VARIABLE GCC_VERSION)
    if (GCC_VERSION VERSION_LESS 4.7)
        set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
    else ()
        set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
    endif ()
else ()
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif ()



# DLib
include_directories(lib/3rdParty/dlib/include)
add_subdirectory(lib/3rdParty/dlib)


# LandmarkDetector and FaceAnalyser
add_subdirectory(lib/local/LandmarkDetector)
add_subdirectory(lib/local/FaceAnalyser)
