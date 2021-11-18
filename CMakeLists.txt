cmake_minimum_required(VERSION 3.0)
project(CITS3003_Labs)

set(CMAKE_CXX_STANDARD 17)

set(OpenGL_GL_PREFERENCE "GLVND")
set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)

set(ONLY_LIBS ON CACHE BOOL "" FORCE)
set(glew-cmake_BUILD_SHARED OFF CACHE BOOL "" FORCE)
set(glew-cmake_BUILD_STATIC ON CACHE BOOL "" FORCE)
include_directories(lib/glew/include)
include_directories(lib/glew/include/GL)
add_compile_definitions(GLEW_INCLUDE=<${CMAKE_SOURCE_DIR}/lib/glew/include/GL/glew.h>)
add_compile_definitions(GLEW_STATIC)
add_subdirectory(lib/glew EXCLUDE_FROM_ALL)

include_directories(lib/freeglut/freeglut/freeglut/include)
include_directories(lib/freeglut/freeglut/freeglut/include/GL)
if (NOT APPLE)
    set(FREEGLUT_BUILD_DEMOS OFF CACHE BOOL "" FORCE)
    set(FREEGLUT_BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
    set(FREEGLUT_BUILD_STATIC_LIBS ON CACHE BOOL "" FORCE)
    add_compile_definitions(FREEGLUT_STATIC)
    add_subdirectory(lib/freeglut/freeglut/freeglut EXCLUDE_FROM_ALL)
endif()

add_subdirectory(lib/angel)
include_directories(lib/angel/include)

MACRO(SUBDIRLIST result curdir)
    FILE(GLOB children RELATIVE ${curdir} ${curdir}/*)
    SET(dirlist "")
    FOREACH(child ${children})
        IF(IS_DIRECTORY ${curdir}/${child})
            LIST(APPEND dirlist ${child})
        ENDIF()
    ENDFOREACH()
    SET(${result} ${dirlist})
ENDMACRO()

MACRO(FILELIST result curdir)
    FILE(GLOB children RELATIVE ${curdir} ${curdir}/*.cpp)
    SET(files "")
    FOREACH(child ${children})
        IF(NOT IS_DIRECTORY ${curdir}/${child})
            LIST(APPEND files ${child})
        ENDIF()
    ENDFOREACH()
    SET(${result} ${files})
ENDMACRO()

SUBDIRLIST(SUBDIRS ${CMAKE_CURRENT_SOURCE_DIR}/src)

FOREACH(subdir ${SUBDIRS})
#    add_subdirectory(src/${subdir})

    FILELIST(SUBFILES  ${CMAKE_CURRENT_SOURCE_DIR}/src/${subdir})

    FOREACH(subfile ${SUBFILES})
        get_filename_component(TARGET_NAME src/${subdir}/${subfile} NAME_WE)
        set(SHORT_TARGET_NAME ${TARGET_NAME})
        set(TARGET_NAME ${subdir}_${TARGET_NAME})
        add_executable(${TARGET_NAME} src/${subdir}/${subfile})

        if (MSVC)
            #    set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT start_scene)
            set_property(TARGET ${TARGET_NAME} PROPERTY VS_DEBUGGER_WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/src/${subdir}")
        endif(MSVC)

        if (APPLE)
            target_link_libraries(${TARGET_NAME} angel libglew_static "-framework GLUT")
        else()
            target_link_libraries(${TARGET_NAME} angel libglew_static freeglut_static)
        endif()

        add_custom_command(TARGET ${TARGET_NAME}
                POST_BUILD
                COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:${TARGET_NAME}> ${CMAKE_CURRENT_SOURCE_DIR}/src/${subdir}/${SHORT_TARGET_NAME}${CMAKE_EXECUTABLE_SUFFIX})
                
	set_property(
		TARGET ${TARGET_NAME}
		APPEND
		PROPERTY ADDITIONAL_CLEAN_FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/${subdir}/${SHORT_TARGET_NAME}${CMAKE_EXECUTABLE_SUFFIX}
	)

    ENDFOREACH()
ENDFOREACH()
