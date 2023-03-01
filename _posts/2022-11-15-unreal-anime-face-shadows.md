---
layout: post
title: "Cleaner Toon Face Shadows in Unreal Engine"
date: 2022-11-15 16:30:00 +0900
tags: [Unreal, Shaders]
toc: true
---

When it comes to rendering anime style models with cel shading, one of the problems with it are shadowing on the face that looks... uneven, to say the least. It is plaguing some games and renders of 3D anime, and if you're reading this, you might have the problem in your Unreal Engine project.



This is caused by how the normals are making up the shape of the lighting. While something like this works fine for PBR renders (e.g. if you're looking for action figure look, or the Jump Force look), it makes cel shading fall short on its face (teehee~), by banding the shadows, and making messy shapes.

This is in stark contrast on what's shown on your favorite 2D drawn anime, with cleaner shadow shapes and controlled contours.

![](/assets/blogpics/imasanimescreencap.jpg)
*Screenshot from THE IDOLM@STER anime (2011) Character: Makoto Kikuchi*

So how do we tackle this in Unreal Engine? By using some vector math to get cleaner normals, and as a result, cleaner face shadows.

This method is loosely based off of [aVersionOfReality's article](http://www.aversionofreality.com/blog/2021/9/5/clean-toon-shading) on achieving clean toon shading on anime faces in Blender. There is a [Unity adaptation](http://panthavma.com/articles/shader-blender-to-unity/) of the method, but as far as I know, nobody have yet to make an Unreal Engine adaptation of it, so here we are.

## Preface
Before we go into the method, here are few things and caveats to consider before you go ahead and apply this method:

- This article was written with (modified) Unreal Engine 5.1, however, there are no UE5 exclusive material nodes used, so it should work fine in Unreal Engine 4 versions.
- This article assumes you already have some form of cel shading working in your Unreal Engine project. As there are many ways to do it, I won't go into detail into how to implement a cel shading to begin with.
- This method completely replaces the normals of the face mesh. If your cel shading already modifies the normals or abandons the normal data, then this method is very unlikely to work.
- This method relies on the mesh bounding box, therefore it's recommended that the face mesh being its own skeletal mesh and influenced by the head bone, resulting in the bounds only cover the face area. This won't work with full body mesh with the face merged into one skeletal mesh.
- Ideally this method is helped with C++ to handle the transforms and the bounding box, but to keep it approachable to artists, the application in this article will be exclusive to the material graph.
- This method involves some amount of vector math, but rest assured, it's not that intimidating.

## Existing methods
But first, let's go through existing methods of achieving clean toon facial shadows to get the idea and talk about the caveats on trying to implement it in Unreal Engine.

### Normal transfer
The idea of this method is transferring normals from a proxy mesh that have similar general shape (but without the detail contours) into the visual mesh. This works in 3D software and added benefit of allowing artist driven shape. However, it falls short once you bake/apply the transferred normals. For one, the transferred normals will break once you apply facial deformation into the mesh. 

As for Unreal Engine, it is outright impossible to do because there is no way to transfer normals from another source... at least through normal means. (Maybe you could do it with some shader and C++ black magic, but in my opinion, it's not worth the hassle.) You can bake the transferred normals so that it works in Unreal, but again, it breaks when you apply deformations, defeating the whole purpose of doing this.

For the body part, this baked normal transfer method will be more likely to work, as the deformations are less finer in scale, thus less likely to break in a noticeable manner.

### Procedural normals
This is the one proposed in [the first aVersionOfReality's article](http://www.aversionofreality.com/blog/2021/9/5/clean-toon-shading) for Blender, and it works like a charm. But to recap, the idea is simple enough, generate normalized (scaling down numbers into 0 to 1 range) vector position in the span of a bounding box to make up the normals. Using the same normalized vector position, it is then bent along axis to conform intended shape of the face.

As this method deals with vector maths, it is plausible to do in shader for game engines like [Unity](http://panthavma.com/articles/shader-blender-to-unity/), and of course, Unreal Engine. One of the helpful nodes are [Pre-Skinned Local Position](https://docs.unrealengine.com/4.27/en-US/RenderingAndGraphics/Materials/ExpressionReference/Vector/#pre-skinnedlocalposition), which outputs normalized vector position in the span of a bounding box before skeletal deformations. So that's what we're looking for, right?

Unfortunately, this is where we hit our first major snag.

Turns out, Pre-Skinned Local Position is calculated before deformations, not just skeletal deformations, but also morph target/shape keys, which is used by many anime models. This makes it the exact same as normal transfer outlined above, except using generated vectors instead of copying from another source.

In the Blender article, the bounding box is bound to a separate box object covering the head portion and follows the transform of the bone head. As you may know, this is impossible to do in Unreal Engine without some C++ black magic That would be too much hassle, so let's see what can we do about it.

## Our method: Normals from aligned position vector
Rectifying the problem of the Pre-Skinned Local Position calculated before all deformations, the idea of this method is to use Local Position, which aren't affected by deformation, and align Local Position's bounding box to match the bone deformation. Since we're restricting ourselves to material graph, we can use Pre-Skinned Local Position's bounding box in world space.



### Implementation
{% assign id = 'ainr184c' %}
{% include blueprintue.html id='ainr184c' %}
