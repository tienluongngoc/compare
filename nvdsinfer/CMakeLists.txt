set(MODULE_NAME nvds_infer)
enable_language(CUDA)
file(GLOB SOURCE_LIST "${CMAKE_CURRENT_SOURCE_DIR}/*[.h, .cc, .cu]")

find_package(CUDA)
find_package(spdlog)

add_library(${MODULE_NAME} SHARED ${SOURCE_LIST})
target_include_directories(${MODULE_NAME} PUBLIC ${CUDA_INCLUDE_DIRS} ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(${MODULE_NAME} PUBLIC nvds_bundle opencv_bundle ${CUDA_LIBRARIES} spdlog::spdlog)
target_compile_features(${MODULE_NAME} PUBLIC cxx_std_20)
target_compile_options(${MODULE_NAME} PRIVATE -Wno-deprecated -Wno-deprecated-declarations)

add_custom_command(TARGET ${MODULE_NAME} POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E make_directory "${PROJECT_SOURCE_DIR}/custom_libs"
  COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:${MODULE_NAME}> "${PROJECT_SOURCE_DIR}/custom_libs"
)

