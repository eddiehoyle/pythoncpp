include_directories(include)

add_library(${TARGET_LIB} SHARED src/Node.cc include/Node.hh)

pybind11_add_module(${TARGET_PYLIB} src/PyNode.cc)
target_link_libraries(${TARGET_PYLIB} PRIVATE ${TARGET_LIB})


# Copy our Python module to ${PYTHON_DIST_DIR}, which must be in unix env $PYTHONPATH
add_custom_command(TARGET ${TARGET_PYLIB} POST_BUILD
        COMMAND
        COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:${TARGET_PYLIB}>  ${PYTHON_DIST_DIR})


# Also copy our C++ shared lib to ${PYTHON_DIST_DIR}, which must be in unix env $LD_LIBRARY_PATH
add_custom_command(TARGET ${TARGET_LIB}  POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:${TARGET_LIB}>  ${PYTHON_DIST_DIR})

