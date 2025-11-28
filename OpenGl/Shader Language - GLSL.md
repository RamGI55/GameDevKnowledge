
OpenGL shader use the closed and isolated programs which is not allowed to communicated with each others but via their inputs and outputs. Shaders are written in unique language called GLSL, which is C-Like language but only for isolated shader program. 
#### Types of GLSL

- `vecn`: the default vector of `n` floats.
- `bvecn`: a vector of `n` Booleans.
- `ivecn`: a vector of `n` integers.
- `uvecn`: a vector of `n` unsigned integers.
- `dvecn`: a vector of `n` double components.

Order of the component 
.x > .y. > .z > .w
also, allows to use rgba for colours and stpq for texture coordinates. 

#### Swizzling 

```cpp 
vec2 someVec;
vec4 differentVec = someVec.xyxx;
vec3 anotherVec = differentVec.zyw; 
vec4 otherVec = someVec.xxxx + anotherVec.yxzy; 
```
Source : 

Swizzling allows to use any combination of up to 4 letters to create the new vector as long as the original vector has those components. 


#### Ins and Outs 
Vertex shader should receive some form of the input. 

```cpp 
// Vertex Shader 
const char *vertexShaderSource = "#version 330 core\n"  
"layout (location = 0) in vec3 aPos;\n"  
"out vec4 vertexColor;\n"  
"void main()\n"  
"{\n"  
"   gl_Position = vec4(aPos, 1.0);\n"  
"   vertexColor = vec4(0.5, 0.0, 0.0, 1.0);\n"  
"}\0";
```

```cpp 
// Fragment Shader 
const char *fragmentShaderSource = "#version 330 core\n"  
"out vec4 FragColor;\n"  
"in vec4 vertexColor;\n"  
"void main ()\n"  
"{\n"  
"   FragColor = vertexColor;\n"  
"}\0";
```

