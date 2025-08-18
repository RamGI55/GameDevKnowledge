

```cpp
cmake_minimum_required(VERSION 3.31)  
project(YOURPROEJCTNAME)  
  
set(CMAKE_CXX_STANDARD 20)  
  
if(NOT DEFINED OPENGL_PATH)  
    message(FATAL_ERROR "OPENGL_PATH is not set. Pass it via -DOPENGL_PATH=...")  
endif()  
  
if(NOT DEFINED GLAD_PATH)  
    message(FATAL_ERROR "GLAD_PATH is not set. Pass it via -DGLAD_PATH=...")  
endif()  

// This includes the library directories to the project. 
include_directories(${GLAD_PATH}/include)  // Use the reletive directory for compatitbility.  
include_directories(${OPENGL_PATH}/include)  
link_directories(${OPENGL_PATH}/lib)  


add_executable(
		YOURPROEJCTNAME 
		{...}
        // Must add the glad.c on the executable
        ${GLAD_PATH}/src/glad.c )  
  
target_link_libraries(  
        YOUPROJECTNAME  
        // must add those on the target_link_libraries.
        ${OPENGL_PATH}/lib/libglfw3.a  
        opengl32  
        gdi32  
        user32  
        kernel32  
)

```