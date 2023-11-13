Previous: [Textures, Images, and Samplers](gltfTutorial_012_TexturesImagesSamplers.md) | [Table of Contents](README.md) | Next: [Advanced Material](gltfTutorial_014_AdvancedMaterial.md)

# A Simple Texture

简单的纹理

As shown in the previous sections, the material definition in a glTF asset contains different parameters for the color of the material or the overall appearance of the material under the influence of light. These properties may be given via single values, for example, defining the color or the roughness of the object as a whole. Alternatively, these values may be provided via textures that are mapped on the object surface. The following is a glTF asset that defines a material with a simple, single texture:

如前面几节所示，glTF 资产中的材质定义包含材质颜色或材质在光影响下的整体外观的不同参数。这些属性可以通过单个值给出，例如，定义整个对象的颜色或粗糙度。或者，这些值可以通过映射在对象表面上的纹理来提供。以下是定义具有简单单一纹理的材质的 glTF 资源：

```javascript
{
  "scene": 0,
  "scenes" : [ {
    "nodes" : [ 0 ]
  } ],
  "nodes" : [ {
    "mesh" : 0
  } ],
  "meshes" : [ {
    "primitives" : [ {
      "attributes" : {
        "POSITION" : 1,
        "TEXCOORD_0" : 2
      },
      "indices" : 0,
      "material" : 0
    } ]
  } ],

  "materials" : [ {
    "pbrMetallicRoughness" : {
      "baseColorTexture" : {
        "index" : 0
      },
      "metallicFactor" : 0.0,
      "roughnessFactor" : 1.0
    }
  } ],

  "textures" : [ {
    "sampler" : 0,
    "source" : 0
  } ],
  "images" : [ {
    "uri" : "testTexture.png"
  } ],
  "samplers" : [ {
    "magFilter" : 9729,
    "minFilter" : 9987,
    "wrapS" : 33648,
    "wrapT" : 33648
  } ],

  "buffers" : [ {
    "uri" : "data:application/gltf-buffer;base64,AAABAAIAAQADAAIAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAACAPwAAgD8AAAAAAAAAAAAAgD8AAAAAAACAPwAAgD8AAAAAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAA",
    "byteLength" : 108
  } ],
  "bufferViews" : [ {
    "buffer" : 0,
    "byteOffset" : 0,
    "byteLength" : 12,
    "target" : 34963
  }, {
    "buffer" : 0,
    "byteOffset" : 12,
    "byteLength" : 96,
    "byteStride" : 12,
    "target" : 34962
  } ],
  "accessors" : [ {
    "bufferView" : 0,
    "byteOffset" : 0,
    "componentType" : 5123,
    "count" : 6,
    "type" : "SCALAR",
    "max" : [ 3 ],
    "min" : [ 0 ]
  }, {
    "bufferView" : 1,
    "byteOffset" : 0,
    "componentType" : 5126,
    "count" : 4,
    "type" : "VEC3",
    "max" : [ 1.0, 1.0, 0.0 ],
    "min" : [ 0.0, 0.0, 0.0 ]
  }, {
    "bufferView" : 1,
    "byteOffset" : 48,
    "componentType" : 5126,
    "count" : 4,
    "type" : "VEC2",
    "max" : [ 1.0, 1.0 ],
    "min" : [ 0.0, 0.0 ]
  } ],

  "asset" : {
    "version" : "2.0"
  }
}
```

The actual image that the texture consists of is stored as a PNG file called `"testTexture.png"` (see Image 15a).

纹理包含的实际图像存储为名为“testTexture.png”的PNG文件（请参阅图像15a）。

<p align="center">
<img src="images/testTexture.png" /><br>
<a name="testTexture-png"></a>Image 15a: The image for the simple texture example.
</p>

Bringing this all together in a renderer will result in the scene rendered in Image 15b.

在渲染器中将所有内容结合在一起将产生图像 15b 中渲染的场景。

<p align="center">
<img src="images/simpleTexture.png" /><br>
<a name="simpleTexture-png"></a>Image 15b: A simple texture on a unit square.
</p>

## The Textured Material Definition

纹理材质定义

The material definition in this example differs from the [Simple Material](gltfTutorial_011_SimpleMaterial.md) that was shown earlier. While the simple material only defined a single color for the whole object, the material definition now refers to the newly added texture:

此示例中的材料定义与前面显示的简单材料不同。虽然简单材质只为整个对象定义了一种颜色，但材质定义现在指的是新添加的纹理：

```javascript
"materials" : [ {
  "pbrMetallicRoughness" : {
    "baseColorTexture" : {
      "index" : 0
    },
    "metallicFactor" : 0.0,
    "roughnessFactor" : 1.0
  }
} ],
```

The `baseColorTexture` is the index of the texture that will be applied to the object surface. The `metallicFactor` and `roughnessFactor` are still single values. A more complex material where these properties are also given via textures will be shown in the next section.

baseColorTexture 是将应用于对象表面的纹理的索引。金属因子和粗糙度因子仍然是单个值。下一节将展示更复杂的材质，其中这些属性也通过纹理给出。

In order to apply a texture to a mesh primitive, there must be information about the texture coordinates that should be used for each vertex. The texture coordinates are only another attribute for the vertices defined in the `mesh.primitive`. By default, a texture will use the texture coordinates that have the attribute name `TEXCOORD_0`. If there are multiple sets of texture coordinates, the one that should be used for one particular texture may be selected by adding a `texCoord` property to the texture reference:

为了将纹理应用于网格基元，必须有关于每个顶点应使用的纹理坐标的信息。纹理坐标只是 mesh.primitive 中定义的顶点的另一个属性。默认情况下，纹理将使用属性名称TEXCOORD_0的纹理坐标。如果存在多组纹理坐标，则可以通过向纹理引用添加 texCoord 属性来选择应用于一个特定纹理的坐标：

```javascript
"baseColorTexture" : {
  "index" : 0,
  "texCoord": 2
},
```
In this case, the texture would use the texture coordinates that are contained in the attribute called `TEXCOORD_2`.

在这种情况下，纹理将使用名为 TEXCOORD_2 的属性中包含的纹理坐标。


Previous: [Textures, Images, and Samplers](gltfTutorial_012_TexturesImagesSamplers.md) | [Table of Contents](README.md) | Next: [Advanced Material](gltfTutorial_014_AdvancedMaterial.md)
