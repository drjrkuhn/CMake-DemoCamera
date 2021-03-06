cmake_minimum_required(VERSION 3.8.0)
project(DemoCamera VERSION 0.1.0)

MESSAGE( STATUS "${PROJECT_NAME} folder CMakeLists.txt")
get_filename_component(MM_MMDEVICE_INCLUDEDIR_ABSOLUTE_PATH "${MM_MMDEVICE_INCLUDEDIR}" ABSOLUTE)
MESSAGE( STATUS "MM_MMDEVICE_INCLUDEDIR:   " ${MM_MMDEVICE_INCLUDEDIR_ABSOLUTE_PATH} )

set(MMDEVICE_DEFINITIONS _CRT_SECURE_NO_WARNINGS _SCL_SECURE_NO_WARNINGS WIN32 NDEBUG _WINDOWS _USRDLL MODULE_EXPORTS)


set(MMDEVICE_SRC 
    ${MM_MMDEVICE_INCLUDEDIR}/Debayer.cpp
    ${MM_MMDEVICE_INCLUDEDIR}/DeviceUtils.cpp
    ${MM_MMDEVICE_INCLUDEDIR}/ImgBuffer.cpp
    ${MM_MMDEVICE_INCLUDEDIR}/MMDevice.cpp
    ${MM_MMDEVICE_INCLUDEDIR}/ModuleInterface.cpp
    ${MM_MMDEVICE_INCLUDEDIR}/Property.cpp
)

add_library(MMDevice-Shared ${MMDEVICE_SRC})
target_include_directories(MMDevice-Shared PRIVATE ${MM_MMDEVICE_INCLUDEDIR})
target_compile_definitions(MMDevice-Shared PUBLIC ${MMDEVICE_DEFINITIONS})

set(TARGET_NAME "mmgr_dal_${PROJECT_NAME}")
add_library(${TARGET_NAME} SHARED DemoCamera.cpp DemoCamera.h WriteCompactTiffRGB.h)
target_include_directories(${TARGET_NAME} PRIVATE ${MM_MMDEVICE_INCLUDEDIR})
target_link_libraries(${TARGET_NAME} MMDevice-Shared)
target_compile_definitions(${TARGET_NAME} PUBLIC ${MMDEVICE_DEFINITIONS})

install(TARGETS ${TARGET_NAME} DESTINATION ${MM_INSTALLDIR})