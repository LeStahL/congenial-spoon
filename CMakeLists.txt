# congenial-spoon - Automatic VJ tool by Team210 
# Copyright (C) 2019 Alexander Kraus <nr4@z10.info>
# 
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.

cmake_minimum_required(VERSION 3.12)

project(congenial-spoon)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Find the right python interpreter version
find_package(OpenGL REQUIRED COMPONENTS OpenGL)

add_subdirectory("${PROJECT_SOURCE_DIR}/thirdparty/fftw-3.3.8/")

if(MSVC)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /ENTRY:demo /SUBSYSTEM:Windows  /OPT:REF  /OPT:ICF=10 /VERBOSE /NODEFAULTLIB ")
endif()

add_executable(congenial-spoon cs.c "${PROJECT_SOURCE_DIR}/thirdparty/cJSON/cJSON.c")

target_include_directories(congenial-spoon SYSTEM PRIVATE "${OPENGL_INCLUDE_DIR}")
target_include_directories(congenial-spoon SYSTEM PRIVATE "${PROJECT_SOURCE_DIR}/thirdparty/cJSON/")
target_include_directories(congenial-spoon SYSTEM PRIVATE "${PROJECT_SOURCE_DIR}/thirdparty/fftw-3.3.8/api")

target_link_directories(congenial-spoon PRIVATE "${PROJECT_BINARY_DIR}/thirdparty/fftw-3.3.8/Release")

target_link_libraries(congenial-spoon OpenGL::GL fftw3)

if(MSVC)
    target_compile_options(congenial-spoon PRIVATE /Gy /O1 /fp:fast /GR- /GS- /MT )

    if (MSVC_VERSION GREATER_EQUAL 1910) # VS2017
        target_link_libraries(congenial-spoon Winmm.lib User32.lib ucrt.lib msvcrt.lib vcruntime.lib ucrt.lib Vfw32.lib)
    else()
        # for debug output: User32.lib ucrt.lib msvcrt.lib
        target_link_libraries(congenial-spoon User32.lib Winmm.lib ucrt.lib msvcrt.lib)
    endif()
endif()
