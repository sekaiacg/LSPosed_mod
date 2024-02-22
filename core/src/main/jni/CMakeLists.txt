project(core)
cmake_minimum_required(VERSION 3.4.1)

add_subdirectory(${EXTERNAL_ROOT} external)

aux_source_directory(src SRC_LIST)
aux_source_directory(src/jni SRC_LIST)

add_library(${PROJECT_NAME} STATIC ${SRC_LIST})
add_library(liblzma STATIC IMPORTED)

execute_process(
    COMMAND find /home/runner/work/LSPosed_mod/LSPosed_mod/ -name liblzma.a
    RESULT_VARIABLE result
    OUTPUT_VARIABLE output
    ERROR_VARIABLE error_output
)
message(WARNING ${result})
# Search for liblzma.a
set(LIBLZMA_DIR ${CMAKE_CURRENT_LIST_DIR}/../../../.cxx/${CMAKE_BUILD_TYPE}/)
file(GLOB_RECURSE LZMA_FILES "${LIBLZMA_DIR}/liblzma.a")

foreach(LIB_FILE ${LZMA_FILES})
    get_filename_component(LIB_NAME ${LIB_FILE} NAME_WE)
    find_library(LIBRARY_${LIB_NAME} ${LIB_NAME} PATHS ${LIBRARY_DIR})
    string(FIND ${LIB_FILE} "/${ANDROID_ABI}/" ARCH_INDEX)
    message(WARNING ${LIB_FILE})
    if(NOT ${ARCH_INDEX} EQUAL -1)
        set_target_properties(liblzma PROPERTIES IMPORTED_LOCATION ${LIB_FILE})
    endif()
endforeach()

target_include_directories(${PROJECT_NAME} PUBLIC include)
target_include_directories(${PROJECT_NAME} PUBLIC xz/src/liblzma/api)
target_include_directories(${PROJECT_NAME} PRIVATE src)

target_link_libraries(${PROJECT_NAME} PUBLIC dobby lsplant_static log fmt-header-only liblzma)
target_link_libraries(${PROJECT_NAME} PRIVATE dex_builder_static)
