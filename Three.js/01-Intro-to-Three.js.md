# Intro to Three.js

## Table of Contents
- [What Is It?](#what-is-it)
  - [WebGL vs Three.js](#webgl-vs-threejs)
- [Basics of 3D Modelling Programs](#basics-of-3d-modelling-programs)
  - [Concepts](#concepts)
  - [Modelling vs Rendering](#modelling-vs-rendering)
- [Building Blocks of Three.js Applications](#building-blocks-of-threejs-applications)

## What Is It?
> Three.js is a 3D library that tries to make it as easy as possible to get 3D content on a webpage. | three.js
### WebGL vs Three.js
> Three.js is often confused with WebGL since more often than not, but not always, three.js uses WebGL to draw 3D. WebGL is a very low-level system that only draws points, lines, and triangles. To do anything useful with WebGL generally requires quite a bit of code and that is where three.js comes in. It handles stuff like scenes, lights, shadows, materials, textures, 3d math, all things that you'd have to write yourself if you were to use WebGL directly. | three.js

## Basics of 3D Modelling Programs
### Concepts
#### Object Geometry
- Any object's shape/geometry is separate from other properties such as color and texture.
#### Camera/Eye
- The focal length and position of the camera to the desired view.
#### Light
- The color of an object is determined by the amount of light it absorbs and the amount that is reflected back.
- Light sources need to be manually defined.
- There are two main categories of light:
  - Ambient Light (Ex: light from the Sun).
  - Point Light (Ex: spot lights, flashlight).
#### Texture and Color
- Texture and color are not inherent to an object.
#### Axes and Coordinate Systems
- Objects are on a coordinate with 3 axes: x, y, z.
### Modelling vs Rendering
- Modelling: creating the object itself (only the shape without any other characteristics).
- Rendering: generating an image of an object with its specified materials, colors, lighting, shadows, etc.

## Building Blocks of Three.js Applications
- Requires creating and connecting objects together.
### Example
<p align="center">
  <img src="https://threejs.org/manual/resources/images/threejs-structure.svg" alt="Structure of a Three.js app" width="60%"/>
</p>

- **Renderer** Object
  - The main object of three.js.
  - The Renderer receives a **Scene** object and a **Camera** object and renders/draws the 3D scene that is inside the *frustum* of the camera as a 2D image to a canvas.
- **Scenegraph**
  - A tree-like structure that contains a **Scene** object, **Mesh** objects, **Light** objects, **Group**, **Object3D**, and **Camera** objects.
  - This tree-like structure represents a hierarchical parent/child relationship and indicates where objects appear and how they are oriented.
    - Child objects are positioned and oriented relative to their parent.
    - Ex: wheels on a bike can be children of the bike so that moving the bike's object automatically moves the wheels.
- **Scene** Object
  - The root of the **Scenegraph** containing properties such as background color, fog.
- **Camera** Object
  - Unlike other objects, the **Camera** object does not have to be in the **Scenegraph**.
  - Just like other objects, the **Camera** object moves and orients relative to its parent object.
  - There can be multiple **Camera** objects in a **Scenegraph**.
- **Mesh** Object
  - Represents drawing a specific **Geometry** with a specific **Material**.
  - Multiple **Mesh** objects can reference the same **Geometry** and **Material** objects.
    - Example
      - Grey Cube at location 1(MeshA) = Vertex Data(GeometryA) + Grey(MaterialA)
      - Grey Cube at location 2(MeshB) = Vertex Data(GeometryA) + Grey(MaterialA).
- **Geometry** Object
  - The vertex data of some piece of geometry (Ex: cube, plane, dog, human, tree, building).
- **Material** Object
  - The surface properties used to draw geometry (Ex: color, shininess).
  - **Material** objects can reference **Texture** objects as well.
    - Ex: wrapping an image onto the surface of a geometry.
- **Texture** Object
  - Images either loaded from image files, generated from a canvas, or rendered from another scene.
- **Light** Object
  - Different kinds of lights.


## Reference
[Fundamentals - three.js manual](https://threejs.org/manual/#en/fundamentals)  
