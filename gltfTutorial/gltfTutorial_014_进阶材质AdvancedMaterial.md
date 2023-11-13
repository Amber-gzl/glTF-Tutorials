Previous: [Simple Texture](gltfTutorial_013_SimpleTexture.md) | [Table of Contents](README.md) | Next: [Simple Cameras](gltfTutorial_015_SimpleCameras.md)

# An Advanced Material

进阶材质

The [Simple Texture](gltfTutorial_013_SimpleTexture.md) example in the previous section showed a material for which the "base color" was defined using a texture. But in addition to the base color, there are other properties of a material that may be defined via textures. These properties have already been summarized in the [Materials](gltfTutorial_010_Materials.md) section:

上一节中的简单纹理示例显示了使用纹理定义“基色”的材质。但除了基色之外，材质的其他属性可以通过纹理定义。这些属性已在“材料”部分中进行了总结：

- The *base color*,
- 基色，
- The *metallic* value,
- 金属值
- The *roughness* of the surface,
- 表面粗糙度
- The *emissive* properties,
- 自发光性质，
- An *occlusion* texture, and
- 遮挡纹理
- A *normal map*.
- 法线贴图。


The effects of these properties cannot properly be demonstrated with trivial textures. Therefore, they will be shown here using one of the official Khronos PBR sample models, namely, the [WaterBottle](https://github.com/KhronosGroup/glTF-Sample-Models/tree/master/2.0/WaterBottle) model. Image 14a shows an overview of the textures that are involved in this model, and the final rendered object:

这些属性的效果无法用琐碎的纹理正确展示。因此，它们将在此处使用官方Khronos PBR示例模型之一（即WaterBottle模型）进行展示。图 14a 显示了此模型中涉及的纹理的概述，以及最终渲染的对象：

<p align="center">
<img src="images/materials.png" /><br>
<a name="cameras-png"></a>Image 14a: An example of a material where the surface properties are defined via textures.
</p>

Explaining the implementation of physically based rendering is beyond the scope of this tutorial. The official Khronos [glTF Sample Viewer](https://github.com/KhronosGroup/glTF-Sample-Viewer) contains a reference implementation of a PBR renderer based on WebGL, and provides implementation hints and background information. The following images mainly aim at demonstrating the effects of the different material property textures, under different lighting conditions.

解释基于物理的渲染的实现超出了本教程的范围。官方的Khronos glTF Sample Viewer包含基于WebGL的PBR渲染器的参考实现，并提供实现提示和背景信息。以下图像主要旨在演示不同材质属性纹理在不同照明条件下的效果。

Image 14b shows the effect of the roughness texture: the main part of the bottle has a low roughness, causing it to appear shiny, compared to the cap, which has a rough surface structure.

图14b显示了粗糙度纹理的效果：与具有粗糙表面结构的盖子相比，瓶子的主要部分粗糙度较低，使其看起来有光泽。

<p align="center">
<img src="images/advancedMaterial_roughness.png" /><br>
<a name="advancedMaterial_roughness-png"></a>Image 14b: The influence of the roughness texture.
</p>

Image 14c highlights the effect of the metallic texture: the bottle reflects the light from the surrounding environment map.

图14c突出了金属纹理的效果：瓶子反射来自周围环境图的光线。

<p align="center">
<img src="images/advancedMaterial_metallic.png" /><br>
<a name="advancedMaterial_metallic-png"></a>Image 14c: The influence of the metallic texture.
</p>

Image 14d shows the emissive part of the texture: regardless of the dark environment setting, the text, which is contained in the emissive texture, is clearly visible.

图 14d 显示了纹理的自发光部分：无论深色环境设置如何，自发光纹理中包含的文本都清晰可见。

<p align="center">
<img src="images/advancedMaterial_emissive.png" /><br>
<a name="advancedMaterial_emissive-png"></a>Image 14d: The emissive part of the texture.
</p>

Image 14e shows the part of the bottle cap for which a normal map is defined: the text appears to be embossed into the cap. This makes it possible to model finer geometric details on the surface, even though the model itself only has a very coarse geometric resolution.

图 14e 显示了瓶盖中定义法线贴图的部分：文本  **似乎**   浮雕在瓶盖上。这使得在表面上对更精细的几何细节进行建模成为可能，即使模型本身只有非常粗糙的几何分辨率。

<p align="center">
<img src="images/advancedMaterial_normal.png" /><br>
<a name="advancedMaterial_normal-png"></a>Image 14e: The effect of a normal map.
</p>

Together, these textures and maps allow modeling a wide range of real-world materials. Thanks to the common underlying PBR model - namely, the metallic-roughness model - the objects can be rendered consistently by different renderer implementations.

这些纹理和贴图一起允许对各种真实世界的材质进行建模。由于通用的底层PBR模型，即金属粗糙度模型，对象可以通过不同的渲染器实现一致地渲染。


Previous: [Simple Texture](gltfTutorial_013_SimpleTexture.md) | [Table of Contents](README.md) | Next: [Simple Cameras](gltfTutorial_015_SimpleCameras.md)
