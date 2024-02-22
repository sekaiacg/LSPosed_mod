project(core)
cmake_minimum_required(VERSION 3.4.1)

add_subdirectory(${EXTERNAL_ROOT} external)

aux_source_directory(src SRC_LIST)
aux_source_directory(src/jni SRC_LIST)

add_library(${PROJECT_NAME} STATIC ${SRC_LIST})
add_library(liblzma STATIC IMPORTED)
set_target_properties(liblzma PROPERTIES IMPORTED_LOCATION_DEBUG ${CMAKE_CURRENT_LIST_DIR}/../../../.cxx/${CMAKE_BUILD_TYPE}/3i36rr5j/${ANDROID_ABI}/liblzma.a)
set_target_properties(liblzma PROPERTIES IMPORTED_LOCATION_RELEASE ${CMAKE_CURRENT_LIST_DIR}/../../../.cxx/${CMAKE_BUILD_TYPE}/6x4l1o1y/${ANDROID_ABI}/liblzma.a)

target_include_directories(${PROJECT_NAME} PUBLIC include)
target_include_directories(${PROJECT_NAME} PUBLIC xz/src/liblzma/api)
target_include_directories(${PROJECT_NAME} PRIVATE src)

target_link_libraries(${PROJECT_NAME} PUBLIC dobby lsplant_static log fmt-header-only liblzma)
target_link_libraries(${PROJECT_NAME} PRIVATE dex_builder_static)
