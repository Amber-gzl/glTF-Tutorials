Previous: [Meshes](gltfTutorial_009_Meshes.md) | [Table of Contents](README.md) | Next: [Simple Material](gltfTutorial_011_SimpleMaterial.md)

# Materials

材质

## Introduction

介绍

The purpose of glTF is to define a transmission format for 3D assets. As shown in the previous sections, this includes information about the scene structure and the geometric objects that appear in the scene. But a glTF asset can also contain information about the *appearance* of the objects; that is, how these objects should be rendered on the screen.

glTF 的目的是定义 3D 资产的传输格式。如前面各节所示，这包括有关场景结构和场景中出现的几何对象的信息。但是 glTF 资产也可以包含有关对象外观的信息;也就是说，这些对象应该如何在屏幕上呈现。

There are different possible representations for the properties of a material, and the *shading model* describes how these properties are processed. Simple shading models, like the [Phong](https://en.wikipedia.org/wiki/Phong_reflection_model) or [Blinn-Phong](https://en.wikipedia.org/wiki/Blinn%E2%80%93Phong_shading_model), are directly supported by common graphics APIs like OpenGL or WebGL. These shading models are built on a set of basic material properties. For example, the material properties involve information about the color of diffusely reflected light (often in the form of a texture), the color of specularly reflected light, and a shininess parameter. Many file formats contain exactly these parameters. For example, [Wavefront OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file) files are combined with `MTL` files that contain this texture and color information. Renderers can read this information and render the objects accordingly. But in order to describe more realistic materials, more sophisticated shading and material models are required.

材质的属性有不同的可能表示形式，着色模型描述了如何处理这些属性。简单的着色模型，如Phong或Blinn-Phong，由OpenGL或WebGL等常见的图形API直接支持。这些着色模型建立在一组基本材料属性之上。例如，材质属性涉及有关漫反射光的颜色（通常以纹理的形式）、镜面反射光的颜色和光泽度参数的信息。许多文件格式都包含这些参数。例如，'Wavefront OBJ' 文件与包含此纹理和颜色信息的'MTL'文件组合在一起。渲染器可以读取此信息并相应地渲染对象。但为了描述更逼真的材质，需要更复杂的着色和材质模型。

## Physically-Based Rendering (PBR)

基于物理的渲染 （PBR）

To allow renderers to display objects with a realistic appearance under different lighting conditions, the shading model has to take the *physical* properties of the object surface into account. There are different representations of these physical material properties. One that is frequently used is the *metallic-roughness-model*. Here, the information about the object surface is encoded with three main parameters:

为了允许渲染器在不同光照条件下显示具有真实外观的对象，着色模型必须考虑对象表面的物理属性。这些物理材料属性有不同的表示形式。经常使用的一种是金属粗糙度模型。在这里，有关对象表面的信息使用三个主要参数进行编码：•

- The *base color*, which is the "main" color of the object surface.
- 基色，即对象表面的“主要”颜色。
- The *metallic* value. This is a parameter that describes how much the reflective behavior of the material resembles that of a metal.
- 金属值。这是一个参数，用于描述材料的反射行为与金属的反射行为的相似程度。
- The *roughness* value, indicating how rough the surface is, affecting the light scattering.
- 粗糙度值，表示表面的粗糙程度，影响光散射。

The metallic-roughness model is the representation that is used in glTF. Other material representations, like the *specular-glossiness-model*, are supported via extensions.

金属粗糙度模型是 glTF 中使用的表示形式。其他材质表示（如镜面反射-光泽度-模型）可通过扩展提供支持。

The effects of different metallic- and roughness values are illustrated in this image:

下图说明了不同金属和粗糙度值的影响：

<p align="center">
<img src="images/metallicRoughnessSpheres.png" /><br>
<a name="metallicRoughnessSpheres-png"></a>Image 10a: Spheres with different metallic- and roughness values.
</p>

The base color, metallic, and roughness properties may be given as single values and are then applied to the whole object. In order to assign different material properties to different parts of the object surface, these properties may also be given in the form of textures. This makes it possible to model a wide range of real-world materials with a realistic appearance.

基色、金属和粗糙度属性可以作为单个值给出，然后应用于整个对象。为了将不同的材料属性分配给对象表面的不同部分，这些属性也可以以纹理的形式给出。这使得可以对具有逼真外观的各种真实世界材料进行建模。

Depending on the shading model, additional effects can be applied to the object surface. These are usually given as a combination of a texture and a scaling factor:

根据着色模型，可以将其他效果应用于对象表面。这些通常以纹理和比例因子的组合形式给出：•

- An *emissive* texture describes the parts of the object surface that emit light with a certain color.
- 自发光纹理描述对象表面发出具有特定颜色的光的部分。
- The *occlusion* texture can be used to simulate the effect of objects self-shadowing each other.
- 遮挡纹理可用于模拟对象相互自遮蔽的效果。
- The *normal map* is a texture applied to modulate the surface normal in a way that makes it possible to simulate finer geometric details without the cost of a higher mesh resolution.
- 法线贴图是一种用于调制表面法线的纹理，可以模拟更精细的几何细节，而无需更高的网格分辨率

glTF supports all of these additional properties, and defines sensible default values for the cases that these properties are omitted.

glTF 支持所有这些附加属性，并为省略这些属性的情况定义合理的默认值。

The following sections will show how these material properties are encoded in a glTF asset, including various examples of materials:

以下部分将展示如何在 glTF 资产中对这些材质属性进行编码，包括各种材质示例：
- [A Simple Material](gltfTutorial_011_SimpleMaterial.md)
- [Textures, Images, and Samplers](gltfTutorial_012_TexturesImagesSamplers.md) that serve as a basis for defining material properties
- [A Simple Texture](gltfTutorial_013_SimpleTexture.md) showing an example of how to use a texture for a material
- [An Advanced Material](gltfTutorial_014_AdvancedMaterial.md) combining multiple textures to achieve a sophisticated surface appearance for the objects


Previous: [Meshes](gltfTutorial_009_Meshes.md) | [Table of Contents](README.md) | Next: [Simple Material](gltfTutorial_011_SimpleMaterial.md)
