# CMake File for MojoShader Branch
# Written by Ethan "flibitijibibo" Lee

CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
PROJECT(mojoshader)

# Build Options
OPTION(MS_DEBUG "Build MojoShader with debugging symbols" ON)

# Architecture Flags
IF(APPLE)
	SET(CMAKE_OSX_ARCHITECTURES i386 x86_64)
	SET(CMAKE_OSX_DEPLOYMENT_TARGET 10.6)
ENDIF()

# Compiler Flags
IF(MS_DEBUG)
	SET(CMAKE_C_FLAGS "-g -Wall")
ELSE()
	SET(CMAKE_C_FLAGS "-O2 -Wall")
ENDIF()

# Linker Flags
IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	SET(CMAKE_SHARED_LINKER_FLAGS "-Wl,--no-undefined")
	SET(MS_LINKLIBS m)
ELSEIF(APPLE)
	FIND_LIBRARY(CARBON NAMES Carbon)
	SET(MS_LINKLIBS ${CARBON})
ENDIF()

# Defines
ADD_DEFINITIONS(-DMOJOSHADER_NO_VERSION_INCLUDE -DMOJOSHADER_NO_VSNPRINTF -DMOJOSHADER_XNA4_VERTEX_TEXTURES)

# Source Lists
SET(MOJOSHADER_SRC
	mojoshader.c
	mojoshader_common.c
	mojoshader_opengl.c
	mojoshader_effects.c
	SDL_string.c
)

# Targets
ADD_LIBRARY(mojoshader SHARED ${MOJOSHADER_SRC})
TARGET_LINK_LIBRARIES(mojoshader ${MS_LINKLIBS})
