set(SHARED_OUTPUT "mojoal")
set(STATIC_OUTPUT "mojoal")
set(LIBRARY_OUTPUT_NAME "mojoal")

set(mojoAL_SRCS
    mojoal.c
)

set(mojoAL_HDRS
    ../include/al.h
    ../include/alc.h
)

if(WIN32)
    set (STATIC_OUTPUT ${LIBRARY_OUTPUT_NAME}_s)
else(WIN32)
    set (STATIC_OUTPUT ${LIBRARY_OUTPUT_NAME})
endif(WIN32)

if(MSVC)
    set(OUTPUT_DIR mojoAL-VS)
elseif(MINGW)
    set(OUTPUT_DIR mojoAL-MINGW)
else()
    set(OUTPUT_DIR mojoAL)
endif()

file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/${OUTPUT_DIR})
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/bin)
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/include)
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/lib)
foreach(INC_FILE ${mojoAL_HDRS})
    get_filename_component(BASE ${INC_FILE} NAME)
    configure_file(${INC_FILE} ${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/include/${BASE} COPYONLY)
endforeach(INC_FILE)

if(BUILD_SHARED)
    add_library(
        mojoAL_shared SHARED
        ${mojoAL_HDRS}
        ${mojoAL_SRCS}
    )
    
    set_target_properties(mojoAL_shared PROPERTIES
        OUTPUT_NAME ${SHARED_OUTPUT}
        CLEAN_DIRECT_OUTPUT 1
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/lib"
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/lib"
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/bin"
    )
    
    set_target_properties(mojoAL_shared PROPERTIES LINKER_LANGUAGE C)
endif(BUILD_SHARED)

if(BUILD_STATIC)
    add_library(
        mojoAL_static STATIC
        ${mojoAL_HDRS}
        ${mojoAL_SRCS}
    )
    
    if(NOT CMAKE_VERSION VERSION_LESS "3.1")
        set_property(TARGET mojoAL_static PROPERTY C_STANDARD 99)
    endif()
    set_target_properties(mojoAL_static PROPERTIES
        OUTPUT_NAME ${STATIC_OUTPUT}
        CLEAN_DIRECT_OUTPUT 1
        VERSION "1.1"
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/lib"
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/lib"
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DIR}/bin"
    )
    set_target_properties(mojoAL_static PROPERTIES LINKER_LANGUAGE C)
endif(BUILD_STATIC)

source_group(Common FILES ${mojoAL_SRCS} ${mojoAL_HDRS})