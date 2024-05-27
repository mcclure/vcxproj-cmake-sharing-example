# (I don't know the actual minimum required. Probably much lower than 3.27)
cmake_minimum_required(VERSION 3.27)
project(CmakeSharingExample)

set(CMAKE_CXX_STANDARD 14)

# These are files which are shared with the vcxproj. Edit the file cppfiles.txt to add to them.
file(STRINGS "SharedFiles.txt" EXAMPLE_SRC_SHARED)

# Edit the EXAMPLE_SRC_SHARED list to prepend the path src/ to all files.
list(TRANSFORM EXAMPLE_SRC_SHARED PREPEND ${CMAKE_CURRENT_SOURCE_DIR}/src/)

set(EXAMPLE_SRC
	src/cmake-only.cpp
	${EXAMPLE_SRC_SHARED}
)

add_executable(CmakeSharingExample ${EXAMPLE_SRC})
