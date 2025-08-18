
#### The Graphic Pipeline of OpenGL 
![[pipeline.png]]
*Source: Hello Triangle - Learn OpenGL Webpage* 

Graphic Pipeline take as input a set of 3D coordinates and transforms these to coloured 2D pixels on the screen. 

**Vertex** is the collection data per 3D coordinates, it works as the reference point to make the shapes. 
**Vertex Shader** is the basic processing on the vertex attributes, takes a single vertex and transform 3d coordinates into the different 3D coordinates 
One the vertex shader made, engine tried to make **Geometry Shader**, takes the actual input a collection of vertices that form a primitive and has ability to generate other shapes by emitting new vertices to from new primitives. 
Then 

#### Vertex Input 

```cpp 
float vertices[] = {  
    -0.9f, -0.5f, 0.0f,     // bottom left  
    -0.0f, -0.5f, 0.0f,      // bottom right  
    -0.45f, 0.5f, 0.0f     // top right  
    };
// those are the vertices for one triangle shape. 
```

Need 3 points for the vertex input. 

#### Vertex Array Object 


#### Vertex Shader 



#### Programming Shader 

##### Compiling Shader 

```c
const char *vertexShaderSource = "#version 330 core\n"  
"layout (location = 0) in vec3 aPos;\n"  
"out vec4 FragColor;\n"  
"void main ()\n"  
"{\n"  
"   gl_Position = vec4 (aPos.x, aPos.y, aPos.z, 1.0);\n"  
"}\0";

```