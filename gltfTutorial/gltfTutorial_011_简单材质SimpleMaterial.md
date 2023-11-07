Previous: [Materials](gltfTutorial_010_Materials.md) | [Table of Contents](README.md) | Next: [Textures, Images, Samplers](gltfTutorial_012_TexturesImagesSamplers.md)

# A Simple Material
简单的材料

The examples of glTF assets that have been given in the previous sections contained a basic scene structure and simple geometric objects. But they did not contain information about the appearance of the objects. When no such information is given, viewers are encouraged to render the objects with a "default" material. And as shown in the screenshot of the [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md), depending on the light conditions in the scene, this default material causes the object to be rendered with a uniformly white or light gray color.

前面部分中给出的 glTF 资产示例包含基本场景结构和简单的几何对象。但它们不包含有关对象外观的信息。如果没有提供此类信息，则鼓励查看者使用“默认”材质渲染对象。如最小 glTF 文件的屏幕截图所示，根据场景中的光照条件，此默认材质会导致对象呈现为均匀的白色或浅灰色。

This section will start with an example of a very simple material and explain the effect of the different material properties.

本节将从非常简单的材料示例开始，并解释不同材料属性的影响。

This is a minimal glTF asset with a simple material:

这是一个最小的glTF资产，具有简单的材料：

```javascript
{
  "scene": 0,
  "scenes" : [
    {
      "nodes" : [ 0 ]
    }
  ],

  "nodes" : [
    {
      "mesh" : 0
    }
  ],

  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0,
        "material" : 0
      } ]
    }
  ],

  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    }
  ],
  "bufferViews" : [
    {
      "buffer" : 0,
      "byteOffset" : 0,
      "byteLength" : 6,
      "target" : 34963
    },
    {
      "buffer" : 0,
      "byteOffset" : 8,
      "byteLength" : 36,
      "target" : 34962
    }
  ],
  "accessors" : [
    {
      "bufferView" : 0,
      "byteOffset" : 0,
      "componentType" : 5123,
      "count" : 3,
      "type" : "SCALAR",
      "max" : [ 2 ],
      "min" : [ 0 ]
    },
    {
      "bufferView" : 1,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 3,
      "type" : "VEC3",
      "max" : [ 1.0, 1.0, 0.0 ],
      "min" : [ 0.0, 0.0, 0.0 ]
    }
  ],

  "materials" : [
    {
      "pbrMetallicRoughness": {
        "baseColorFactor": [ 1.000, 0.766, 0.336, 1.0 ],
        "metallicFactor": 0.5,
        "roughnessFactor": 0.1
      }
    }
  ],
  "asset" : {
    "version" : "2.0"
  }
}
```      

When rendered, this asset will show the triangle with a new material, as shown in Image 11a.

渲染时，此资产将显示带有新材质的三角形，如图 11a 所示。

<p align="center">
<img src="images/simpleMaterial.png" /><br>
<a name="simpleMaterial-png"></a>Image 11a: A triangle with a simple material.
</p>


## Material definition

材料定义


A new top-level array has been added to the glTF JSON to define this material: The `materials` array contains a single element that defines the material and its properties:

一个新的顶级数组已添加到glTF JSON中以定义此材料： 材料数组包含一个定义材料及其属性的元素：


```javascript
  "materials" : [
    {
      "pbrMetallicRoughness": {
        "baseColorFactor": [ 1.000, 0.766, 0.336, 1.0 ],
        "metallicFactor": 0.5,
        "roughnessFactor": 0.1
      }
    }
  ],
```

The actual definition of the [`material`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-material) here only consists of the [`pbrMetallicRoughness`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-material-pbrmetallicroughness) object, which defines the basic properties of a material in the *metallic-roughness-model*. (All other material properties will therefore have default values, which will be explained later.) The `baseColorFactor` contains the red, green, blue, and alpha components of the main color of the material - here, a bright orange color. The `metallicFactor` of 0.5 indicates that the material should have reflection characteristics between that of a metal and a non-metal material. The `roughnessFactor` causes the material to not be perfectly mirror-like, but instead scatter the reflected light a bit.

此处材质的实际定义仅包含 pbrMetallicRoughness 对象，该对象定义了金属粗糙度模型中材质的基本属性。（因此，所有其他材料属性都将具有默认值，稍后将对此进行解释。baseColorFactor 包含此处材质主色的红色、绿色、蓝色和 alpha 分量，即明亮的橙色。金属因子0.5表示材料应具有介于金属和非金属材料之间的反射特性。粗糙度因子导致材料不是完全像镜面的，而是稍微散射反射光。

## Assigning the material to objects

将材质指定给对象

The material is assigned to the triangle, namely to the `mesh.primitive`, by referring to the material using its index:

材质被分配给三角形，即 mesh.primitive，通过使用其索引引用材质：

```javascript
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0,
        "material" : 0
      } ]
    }
```

The next section will give a short introduction to how textures are defined in a glTF asset. The use of textures will then allow the definition of more complex and realistic materials.

下一节将简要介绍如何在 glTF 资产中定义纹理。然后，纹理的使用将允许定义更复杂和逼真的材质。

Previous: [Materials](gltfTutorial_010_Materials.md) | [Table of Contents](README.md) | Next: [Textures, Images, Samplers](gltfTutorial_012_TexturesImagesSamplers.md)
