
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
