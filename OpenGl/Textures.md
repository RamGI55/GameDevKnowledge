
#### Texture Wrapping 

- `GL_REPEAT` : The default behaviour of the texture, repeats the texture image. 
- `GL_MIRROED_REPEAT` : Repeats the texture but mirrors the image with each repeat. 
- `GL_CLAMP_TO_EDGE`: Clamps the coordinates between `0` and `1`. The result is that higher coordinates become clamped to the edge, resulting in a stretched edge pattern.
- `GL_CLAMP_TO_BORDER` :Coordinates outside the range are now given a user-specified boarder colour.

#### Texture Filtering 

- `GL_NEAREST` : Selected the Texel that centre is closest to the texture coordinates. Default setting of OpenGL. 
- `GL_LINEAR`(Bilinear filtering) : takes the interpolated value from the texture coordinate's neighbouring texels, approximating the colour between of the texels. 



#### Generating the Texture 

`glGenTextures` (# of the texture takes, store the unsigned int array)

```cpp 
unsigned int texture; 
glGenTextures (1, &texture); 
```

`glTexImage2D` 
	- The texture target; 
```cpp
glTexImage2D(GL_TEXTURE_2D, 0 , GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE,data);
glGenerateMipmap(GL_TEXTURE_2D); 
```