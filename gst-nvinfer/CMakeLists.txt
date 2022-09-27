set(MODULE_NAME nvdsgst_infer)
file(GLOB SOURCE_LIST "${CMAKE_CURRENT_SOURCE_DIR}/*[.h, .cc]")

find_package(CUDA)
find_package(spdlog)

add_library(${MODULE_NAME} SHARED ${SOURCE_LIST})
target_include_directories(${MODULE_NAME} PUBLIC ${CUDA_INCLUDE_DIRS})
target_link_libraries(${MODULE_NAME} PUBLIC gst_bundle nvds_bundle ${CUDA_LIBRARIES} nvds_infer opencv_bundle)
target_compile_features(${MODULE_NAME} PUBLIC cxx_std_20)
target_compile_options(${MODULE_NAME} PRIVATE -Wno-deprecated -Wno-deprecated-declarations)

add_custom_command(TARGET ${MODULE_NAME} POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E make_directory "${PROJECT_SOURCE_DIR}/custom_plugins"
  COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:${MODULE_NAME}> "${PROJECT_SOURCE_DIR}/custom_plugins"
)

