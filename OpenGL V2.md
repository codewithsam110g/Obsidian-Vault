# Section 1: OpenGL Basics
## Requirements

1. **GLFW** – Cross-platform windowing and input library.
2. **GLM** – Math library for vector and matrix operations.
3. **OpenGL Loader** – I use **GLAD** for loading OpenGL functions.
4. **stb_image** – Library for image loading.
5. **Model Loading** – Use either **Assimp** or **tinyobjloader** for loading 3D wireframe models.
## Chapter 1: Setup and Window
### Setup:
1. Link **GLFW** and **GLM** to the project
2. Add the **GLAD** files to the source code of the project
3. Make sure to compile the **glad.c** file from Previous step
### Window:
1. Include **GLAD** headers then include **GLFW** headers
2. Initialize **GLFW** and configure it
3. Create a window using **glfwCreateWindow** 
4. Start the OpenGL Context for that window using **glfwMakeContextCurrent**
5. Now create a main loop and poll events, swap buffers in the that loop
6. after loop destroy the window and terminate the glfw
Now if you run this code it should spawn a window with the properties you have set like the width, height, title etc in a black colored background

## Chapter 2: Drawing a Triangle
To draw a simple triangle with three vertices, it requires some setup and many concepts.
We will take a look at each one in brief before drawing the triangle. Fortunately we can draw many triangles if we can draw a single one with some simple changes. So, let's get into it.
### OpenGL Setup:
1. Now that we have a Window Ready, we should set up the OpenGL
2. just call **gladLoadGL** after the **glfwMakeContextCurrent**
3. Now after that point we can use any **OpenGL** function we want
### Buffers
- Buffer is a place to hold some amount of data which we can define as we like
- It is used to send data from the **CPU** to the **GPU** which will render or draw with that data to the screen or window
- There are many types of buffers, we will take at each one when necessary
#### Vertex Buffer
- This is used to send the **VERTEX** data to the **GPU**
- Vertex data is the data we need to draw the vertex like the position, color, etc
- currently we only care about the position, but we will implement the rest as the time goes
### Vertex Array
- This is used to hold many Vertex Buffers Objects (VBO's) at once along with other necessary buffers
- So we would create all the buffers we need and attach them to a Vertex Array Object (VAO)
- This would simplify the attachment and removal of all the Buffers
- We have specified the data in the vbo, but we haven't told the **GPU** how to use it
- This is done with Vertex Attribute Binding in the vao
- there we specify the length of each part of the data
Let's get to the implementation
1. Generate, Store and Bind the vao
2. Generate, Store and Bind the vbo after vao is bound
3. load the Data into the vbo
4. set the vertex attributes for each data (eg: position, color etc)
5. bind the vertex attributes to unique location
6. To draw the triangle, we use the main loop
7. in the loop, bind the vao, and call the draw command
8. the draw command for vao with vbo is **glDrawArrays**
Now we should see a triangle centered on the screen with white color

## Chapter 3: Shaders and Colored Triangle
A plain white triangle is boring right, so I want to color it. To do that we have to learn and implement a concept called Shaders. They will bring a whole load of things like lighting, Realistic Lighting and Other things. But for now lets just try to color our little tiny triangle
### Shaders & Pipeline:
- These are the pieces of code that we send to **GPU** along with the Buffers to do a specific Task
- For eg: Rendering a Triangle, coloring a Triangle, etc.
- Then why did the code in previous chapter run or render a triangle without a shader?
- That is due to the default or basic shaders that are available in the **GPU** beforehand.
- Before you can ask, no there no default shaders that draw colors or do the lighting, we have to implement them on our own
- Shaders are a part of the Rendering Pipeline, Essentially the data in the buffers have to go through a lot of steps to render on screen
- Those steps are called Pipeline, it consists of several shaders
- Some can be changed via code, some can be only configure via properties, some cant be changed
- Changeable ones are: Vertex Shader, Fragment Shader and Geometry Shader
- We don't use the last one currently, we will use that later on
### Stages of the Rendering Pipeline
![Rendering PipeLine](https://www.khronos.org/opengl/wiki_opengl/images/RenderingPipeline.png)
Out of the above Stages, we will implement the Vertex and Fragment Shaders
### Shader Programming:
- It is written in a Language Called **GLSL** which stands for OpenGL Shading Language
- This is extremely similar to Programming in C
- It has extra things like matrices, vectors, images etc

### Implementation
- Firstly add the necessary color data to the vertices
- and modify the **VAO** accordingly
- Now create a function to read from a file and output to a **std::string**
- we will create two files one for vertex and other for fragment shader
- Now we will implement a basic shader that will take the position and color in vertex shader, then give that to fragment shader which will show that color
- Now read from each file and store it in a string
- Generate a shader similar to vbo, vao for each shader stage, currently two needed
- add the code to the respective shader
- now compile the shader and check for errors
- repeat this another time for the fragment shader
- Now, we will create a Shader Program
- and attach the two shaders we have created and link the program
- We can check of errors after the linking of the program
- This is required to link the shader app to the actual application
- Now we can safely delete the shaders as we already linked them to the program
- Now we just call the **glUseProgram** and pass the program as argument whenever we want to use that set of shaders
- Note we can't modify a pipeline while the app is running, but we can swap it with other one we have created now or beforehand at the cost of perfomance.
- Essentially switching pipelines in middle of something will cause performance issues in the long run
Now we should see the triangle with colors interpolated beautifully between all the three vertices

## Chapter 3.5: Culling and Index / Element Buffer

Till now, we have implemented a triangle with colors, what if we want to draw a rectangle, a square, or the epitome of all the evil the **CIRCLE**. For that we have to convert everything to triangles and render them, that is the reason every 3D model has the triangle count. So for now lets draw a simple rectangle, by splitting it into 2 triangles along a diagonal.

So let's add the remain triangle data to the vertices and it will take care of the rest, since we have set up it up to draw the data in accordance with position and color.

A rectangle has 2 triangles so each triangle has 6 vertices, but a rectangle needs only 4 vertices. So the two in the diagonal are duplicated resulting in 6 vertices for 2 triangles. Currently, it is fine with extra 2 vertices, but for many models we have thousand of triangles with shared vertices, with duplication that will be huge amount of unnecessary data in the buffer. Instead of that we can send another new buffer to the **GPU** that will be the order of the vertices we want to use to draw a triangle that will reduce the data in vertex buffer by huge margins. Let's take a look at the Buffer

**NOTE:** This is not the only approach, for keen observers, the draw call takes a type as the first argument. The others are Triangle Fan, Triangle Strip, but these require the **VBO** data in a specific way for respective method.

### Element Array Buffer (EBO):
- This buffer contains the index of the vertex to be used to draw triangle
- so for a rectangle we need only the 4 vertices, but we will mention the order of the drawing of the triangle
- the order would look like this 0, 1, 3 for the first triangle and 1,2,3 for the second one
- But there is catch, why we have to order it in this way?

**NOTE:** This EBO is also called as Index Buffer commonly as this contains the indices of vertex of the triangles that need to be drawn

### Face Culling:
- As there are 3 vertices, there are two orders or winding of the vertices, either they are arranged in Clock Wise order (CW) or Counter Clock Wise order (CCW)
- In Graphics, the CCW order is treated as the front facing order and the CW order as the back facing order
- So we can reduce the triangles being drawn based on the winding or the order
- This is can get good performance, but can and will induce artifacts like missing triangles and invisible models or objects from the back
- So we can enable this if the **VBO** data and the scene orientation matches
- This extremely error-prone as the order is independent of the orientation of the camera
- So if we enable back face or CW culling and look at the back we won't see anything

### Implementation
- Enough of that lets implement the EBO, and turn the Culling off which is by default
- Implementation of EBO is extremely similar to VBO
- generate a buffer after we bind the VAO
- bind it to **GL_ELEMENT_ARRAY_BUFFER**
- insert the indices' data similar to VBO using **glBufferData**
- that's it for the buffer, when we want to render
- we bind the associated VAO
- render via other draw call which is **glDrawElements**

Now we can see a colorful rectangle
**Fun Fact:** To see wireframe mode, we can set the rendering method to wireframe using **glPolygonMode**

## Chapter 4: Textures

I don't know about you, but I feel bored with those simple colors, I want something more colorful, more sparkly, more textured like an **IMAGE**. So, let's take look into how we can get the image onto the triangle or the rectangle we have implemented in last chapter

Similar to how we read the shader code from file and made a shader, we have to first read the image store the image data, then give to the OpenGL to show that data

### Reading and Storing the Image Data:
- Images come in all shapes and sizes, some are in color, some have transparency like **.png's**
- So we can't just read from the file using binary as reading mode, instead we will use stb library, it has the following file "stb_image.h", that we will be using to read and store the image
- first we create a slot in the texture system of the OpenGL to send the data to.
- similar to buffers, first we generate a texture
- bind the texture to **GL_TEXTURE_2D**
- Set the wrapping and filtering properties
- Now that we have a slot to keep the image data, we use the stb_image.h to read from the image
- first create empty variables for width, height, no. of channels (Colors : r, g, b; Transparency: a)
- then use the stbi_load and save the data in the form of unsigned char *
- now we will check if the data exists and load that data into texture we created earlier using **glTexImage2D**
- and generate mipmaps while we are at it
- and finally clear the data from image since we have already loaded it using **stbi_free**
### Using Textures:
- since we have the texture ready to use with the simple bind function similar to a VAO
- we have to use, but we can't just bind it before drawing and expect a texture
- that won't work since we didn't mention how to use that data in the shaders
- so first we add another vertex parameter called texture coordinates or TexCoords in short
- so now our vertex is 8 values long 3 for position, 3 for the color, 2 for the TexCoords
- now we also have to change the shader code to implement this
- we just the pass the vec2 TexCoords to the fragment shader from the vertex shader
- in fragment shader we take the image and TexCoords to show the texture using **texture()** function
Now we should be able to see the texture applied to the rectangle, for fun we can mix both the color from the vertex data and the texture with 20% of the color coming through the texture

## Chapter 5: Coordinate Systems
Till now we have rendered only a single triangle or a rectangle, but we have always done that with the vertices being in range of -1 to 1, and we have never known why?

Now we will try to understand that. Currently, the coordinates that we specify are in a Coordinate System called NDC or Normalized Device Coordinates. The OpenGL NDC consists of 3 axes, namely: X, Y, Z. They all have a range of -1 to 1. The positive X axis is towards the right, Y axis is towards the up, Z axis is toward the user or out from the screen.

That is the reason we are using the coordinates in -1 to 1 range till now, OpenGL expects all the vertices to be in the -1 to 1 range cube otherwise it will discard that part. In future, we would want to load all kinds of 3D Models from outside into our OpenGL Application.

But if we look at any 3D model the vertices of that model are relative to the origin of that model. We call this the Local Space or Object Space. At the end, we want everything to be in the NDC, so we have to convert this Local Space coordinates into NDC. We do this in multiple steps.

### Different Coordinate Systems:
1. Local or Object Space
2. World Space
3. View Space
4. Clip Space
5. Screen Space

#### Local Space:
- This space contains the vertices relative the models origin
- All the vertices in 3D models are defined in this way

#### World Space:
- We apply a matrix transformation of the Local Space Coordinates to get them into the World Space Coordinates called **model** matrix
- Here we think about the Model as whole instead of the vertices in that model
- So we take the origin of the Model is now relatively positioned with respect to a Global Origin which is (0, 0, 0).
- This space contains many models including the camera we are seeing the world from.

#### View Space:
- This space only contains the part of the world visible to the camera
- Since we only need to render the parts visible to the camera and discard the rest
- We do this by applying a matrix transformation called **view** matrix

### Clip Space:
- Still the vertices are not in NDC so, we convert them to NDC using a matrix transformation with **projection** matrix
- The projection matrix is responsible for the Projection of the World
- There two types of projection
	- Orthographic
	- Perspective
- Orthographic makes all the lines same size irrespective of the distance from the camera
- Perspective is the one we experience in real life. It makes the farther objects look small.

#### Screen Space:
- The screen is made up of pixels, for example a 1080p screen would have the dimensions of 1920×1080 given the aspect ratio is 16:9.
- In the screen space we the need coordinates from 0 to 1920 and 0 to 1080
- Thankfully we don't need to deal with this as the rasterizer in the pipeline will take care of this

### Going 3D:

Why all this you ask? Cause when we want to build an actual scene we will use the world space coordinates, so we have to convert them into NDC and give that data to the shaders. For that we have to implement matrices, their multiplications, and functions for the perspective and the 3D stuff like moving an object, rotating and scaling it. We can go an and implement this stuff, but this we can use a library that can do all of that stuff for us. So, we use the **GLM** library. It stands for OpenGL Mathematics. Including this library is as simple as copying the header files into the include folder.


Now let's implement the perspective, as we learned before we need 3 matrices, namely model, view, projection. We also have to send this data to the shader using uniforms.

### Implementation
1. Include GLM headers
2. make 3 matrices, namely model, view, and projection
3. set them to appropriate values
4. send them to the shaders via uniform
5. update the vertex shader to include the three uniform matrices
6. calculate the new position using projection \* view \* model \* vec4(vertices.xyz, 1)

Now we can see the 3D perspective the rectangle, for fun we can change the model using **glm::rotate** function to see it rotating on the screen in real time. No more motionless scenes I guess.

