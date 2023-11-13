Previous: [Simple Material](gltfTutorial_011_SimpleMaterial.md) | [Table of Contents](README.md) | Next: [Simple Texture](gltfTutorial_013_SimpleTexture.md)

# Textures, Images, and Samplers

纹理、图像和采样器

Textures are an important aspect of giving objects a realistic appearance. They make it possible to define the main color of the objects, as well as other characteristics that are used in the material definition in order to precisely describe what the rendered object should look like.

纹理是赋予对象真实外观的一个重要方面。它们可以定义对象的主要颜色，以及材料定义中使用的其他特征，以便精确描述渲染对象的外观。

A glTF asset may define multiple [`texture`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-texture) objects, which can be used as the textures of geometric objects during rendering, and which can be used to encode different material properties. Depending on the graphics API, there may be many features and settings that influence the process of texture mapping. Many of these details are beyond the scope of this tutorial. There are dedicated tutorials that explain the exact meaning of all the texture mapping parameters and settings; for example, on [webglfundamentals.org](https://webglfundamentals.org/webgl/lessons/webgl-3d-textures.html),  [open.gl](https://open.gl/textures), and others. This section will only summarize how the information about textures is encoded in a glTF asset.

glTF 资产可以定义多个纹理对象，这些对象可以在渲染期间用作几何对象的纹理，并可用于编码不同的材质属性。根据图形 API，可能有许多功能和设置会影响纹理映射过程。其中许多详细信息超出了本教程的范围。有专门的教程来解释所有纹理映射参数和设置的确切含义;例如，在 webglfundamentals.org、open.gl 等。本节将仅总结如何在 glTF 资产中对纹理相关信息进行编码。

There are three top-level arrays for the definition of textures in the glTF JSON. The `textures`, `samplers`, and `images` dictionaries contain  [`texture`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-texture),  [`sampler`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#_texture_sampler), and [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image) objects, respectively. The following is an excerpt from the [Simple Texture](gltfTutorial_013_SimpleTexture.md) example, which will be presented in the next section:

有三个顶级数组用于定义 glTF JSON 中的纹理。textures、samplers和images字典分别包含纹理、采样器和图像对象。以下是简单纹理示例的摘录，将在下一节中介绍：

```javascript
"textures": [
  {
    "source": 0,
    "sampler": 0
  }
],
"images": [
  {
    "uri": "testTexture.png"
  }
],
"samplers": [
  {
     "magFilter": 9729,
     "minFilter": 9987,
     "wrapS": 33648,
     "wrapT": 33648
   }
],
```

The `texture` itself uses indices to refer to one `sampler` and one `image`. The most important element here is the reference to the `image`. It contains a URI that links to the actual image file that will be used for the texture. Information about how to read this image data can be found in the section about [image data in `images`](gltfTutorial_002_BasicGltfStructure.md#image-data-in-images).

纹理本身使用索引来引用一个采样器和一个图像。这里最重要的元素是对图像的引用。它包含一个链接到将用于纹理的实际图像文件的 URI。有关如何读取此图像数据的信息，请参阅有关图像中的图像数据的部分。
（texture的 source 指向的就是图像索引吗）

The next section will show how such a texture definition may be used inside a material.

下一节将展示如何在材质中使用此类纹理定义。

Previous: [Simple Material](gltfTutorial_011_SimpleMaterial.md) | [Table of Contents](README.md) | Next: [Simple Texture](gltfTutorial_013_SimpleTexture.md)
