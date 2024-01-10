# WebGL

## Table of Contents
- [What is WebGL?](#what-is-webgl)
- [WebGL High-Level View](#webgl-high-level-view)
- [How WebGL Works](#how-webgl-works)
  - [WebGL Rendering Pipeline](#webgl-rendering-pipeline)
- [Reference](#reference)

## What is WebGL?
> WebGL (Web Graphics Library) is a JavaScript API for rendering high-performance interactive 3D and 2D graphics within any compatible web browser without the use of plug-ins. WebGL does so by introducing an API that closely conforms to OpenGL ES 2.0 that can be used in HTML `<canvas>` elements. This conformance makes it possible for the API to take advantage of hardware graphics acceleration provided by the user's device. | MDN

> WebGL programs consist of control code written in JavaScript and special effects code (shader code) that is executed on a computer's Graphics Processing Unit (GPU). WebGL elements can be mixed with other HTML elements and composited with other parts of the page or page background. | MDN

> The WebGL 2 API introduces support for much of the OpenGL ES 3.0 feature set; it's provided through the `WebGL2RenderingContext` interface. | MDN

> The `<canvas>` element is also used by the Canvas API to do 2D graphics on web pages. | MDN

- WebGL essentially runs a specific context for the `<canvas>` element, giving us access to hardware-accelerated 3D rendering in JS (i.e. runs directly with your GPU).
- Since WebGL is based on OpenGL ES 2.0, it can run on many devices including computers, phones, TVs.
- WebGL is low-level system that only draws points, lines, and triangles.
  - There are many libraries such as Three.js that use WebGL and handles scenes, lights, shadows, materials, textures, 3D math, etc, making it easier for us to develop.
### OpenGL
- A cross-language and platform to render 2D/3D vector graphics.
- Mainly used in desktop applications.
- C, C++.
- Fixed function pipeline.
### OpenGL ES
- A subset of OpenGL, meaning it is simpler and has fewer capabilities than OpenGL.
- Used on mobile.

## Terminologies
- **Scene:** the environment/space where the 3D objects are positioned and rendered.
  - i.e. the overall context/container that holds all the involved elements (objects, lights, cameras, etc.).
- **Model:** a single 3D object representing some entity in the scene.
  - 3D models consist of vertices, edges, faces that define the shape and structure of the object.
- **Shader:** a program determining "how" you want to draw the object.
  - Vertex Shader: specify all the vertices (essentially, the shape itself).
  - Fragment Shader: define the color, texture, etc (essentially, the decorations).
- **Buffer:** data held on the GPU
  - Vertex Buffer: vertex(geometry) attributes of the model (Ex: position, color, texture coordinates).
  - Frame Buffer: a collection of buffers that serve as a rendering destination. Essentially a collection of attachments.

## WebGL High-Level View
1. Prepare the canvas, get the WebGL rendering context.
2. Specify the geometry (what you want to draw) and store it in buffer objects.
    - Geometry is usually in the form of an array with vertices (Ex: x, y, z coordinates).
3. Create shader programs and compile them.
4. Link the shader programs with the buffer objects.
    - Buffer Object: what you want to draw.
5. Draw the objects.

## How WebGL Works
- Triangles are the basic element with which models are drawn. We need to use JS to generate the information for where and how these triangles should be created, and their appearance (color, shades, textures, etc). This information will be sent to the GPU to process and return a view of the scene.
### WebGL Rendering Pipeline
<div align="center">
  <img src="https://media.geeksforgeeks.org/wp-content/uploads/20220623131006/Group12.png" alt="WebGL Rendering Pipeline" />
  <p>WebGL Introduction - GeeksforGeeks</p>
</div>

1. First, **Vertex Arrays** are created in JS. These can be created by 1) processing 3D model files (Ex: .obj), 2) using a predefined model provided by a library, or 3) creating from scratch.
    - Vertex Arrays: arrays containing vertex information such as the location of the vertex in the 3D space, the vertex's texture, color, and light.
2. The information in the vertex arrays is fed to the GPU in a set of one or more **Vertex Buffers**. When these are preparing to render, another array of indices pointing to the elements in the Vertex Array is needed. These indices will later indicate how each vertex will combine into triangles (step 4).
3. The GPU reads each item in the Vertex Buffer and runs it through the **Vertex Shader**.
    - Vertex Shader: a program that receives a set of vertex attributes and outputs a new set of attributes. It can calculate the projected position of the vertex in screen space, and also generate other attributes (Ex: color/texture coordinates) for each vertex.
4. The GPU creates the triangles by connecting the projected vertices. It takes the vertices in the order specified by the indices array (provided in step 2) and groups them into sets of three.
5. The **Rasterizer** takes each triangle and clips it by discarding portions that are out of bounds of the screen. The visible portion is split into pixel-sized fragments. Other vertex attributes (output by the Vertex Shader) are applied across the rasterized surface of each triangle for a smooth gradient (Ex: if each vertex is assigned a color value, the colors will be blended into a color gradient across the pixelated surface).
    - Rasterizer: a program that outputs pixel-sized fragments (small pieces representing data for generating a screen pixel).
6. The **Fragment Shader** calculates and assigns the color to the pixels.
    - Fragment Shader: a program that outputs color and depth values for every individual pixel (most performance-sensitive step of the graphics pipeline).
      - Ex: texture mapping, and lighting.
7. The values resulting from the Fragment Shader are then drawn into the **Frame Buffer**.
    - FrameBuffer: holds the scene data including width/height of the surface, each pixel's color, depth and stencil buffers.

## Reference
[WebGL: 2D and 3D graphics for the web - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API)  
[Dev.Opera — An Introduction to WebGL — Part 1](https://dev.opera.com/articles/introduction-to-webgl-part-1/)  
[WebGL Introduction - GeeksforGeeks](https://www.geeksforgeeks.org/webgl-introduction/)  
[WebGL Fundamentals](https://webglfundamentals.org/)  
[WebGL tutorial - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/Tutorial)  
