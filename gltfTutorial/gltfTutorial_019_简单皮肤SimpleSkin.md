Previous: [Morph Targets](gltfTutorial_018_MorphTargets.md) | [Table of Contents](README.md) | Next: [Skins](gltfTutorial_020_Skins.md)

# A Simple Skin
简单的皮肤

glTF supports *vertex skinning*, which allows the geometry (vertices) of a mesh to be deformed based on the pose of a skeleton. This is essential in order to give animated geometry, for example of virtual characters, a realistic appearance. The core for the definition of vertex skinning in a glTF asset is the [`skin`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-skin), but vertex skinning in general implies several interdependencies between the elements of a glTF asset that have been presented so far.

glTF 支持顶点蒙皮，它允许网格的几何体（顶点）根据骨架的姿势变形。这对于赋予动画几何体（例如虚拟角色）逼真的外观至关重要。glTF 资产中顶点蒙皮定义的核心是蒙皮，但顶点蒙皮通常意味着到目前为止介绍的 glTF 资产元素之间存在多种相互依赖关系。

The following is a glTF asset that shows basic vertex skinning for a simple geometry. The elements of this asset will be summarized quickly in this section, referring to the previous sections where appropriate, and pointing out the new elements that have been added for the vertex skinning functionality. The details and background information for vertex skinning will be given in the next section.

下面是一个 glTF 资产，其中显示了简单几何体的基本顶点蒙皮。本节将快速总结此资产的元素，在适当的情况下参考前面的部分，并指出为顶点蒙皮功能添加的新元素。顶点蒙皮的详细信息和背景信息将在下一节中提供。

```javascript
{
  "scene" : 0,
  "scenes" : [ {
    "nodes" : [ 0, 1 ]
  } ],
  
  "nodes" : [ {
    "skin" : 0,
    "mesh" : 0
  }, {
    "children" : [ 2 ]
  }, {
    "translation" : [ 0.0, 1.0, 0.0 ],
    "rotation" : [ 0.0, 0.0, 0.0, 1.0 ]
  } ],
  
  "meshes" : [ {
    "primitives" : [ {
      "attributes" : {
        "POSITION" : 1,
        "JOINTS_0" : 2,
        "WEIGHTS_0" : 3
      },
      "indices" : 0
    } ]
  } ],

  "skins" : [ {
    "inverseBindMatrices" : 4,
    "joints" : [ 1, 2 ]
  } ],
  
  "animations" : [ {
    "channels" : [ {
      "sampler" : 0,
      "target" : {
        "node" : 2,
        "path" : "rotation"
      }
    } ],
    "samplers" : [ {
      "input" : 5,
      "interpolation" : "LINEAR",
      "output" : 6
    } ]
  } ],
  
  "buffers" : [ {
    "uri" : "data:application/gltf-buffer;base64,AAABAAMAAAADAAIAAgADAAUAAgAFAAQABAAFAAcABAAHAAYABgAHAAkABgAJAAgAAAAAvwAAAAAAAAAAAAAAPwAAAAAAAAAAAAAAvwAAAD8AAAAAAAAAPwAAAD8AAAAAAAAAvwAAgD8AAAAAAAAAPwAAgD8AAAAAAAAAvwAAwD8AAAAAAAAAPwAAwD8AAAAAAAAAvwAAAEAAAAAAAAAAPwAAAEAAAAAA",
    "byteLength" : 168
  }, {
    "uri" : "data:application/gltf-buffer;base64,AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAABAPwAAgD4AAAAAAAAAAAAAQD8AAIA+AAAAAAAAAAAAAAA/AAAAPwAAAAAAAAAAAAAAPwAAAD8AAAAAAAAAAAAAgD4AAEA/AAAAAAAAAAAAAIA+AABAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAA=",
    "byteLength" : 320
  }, {
    "uri" : "data:application/gltf-buffer;base64,AACAPwAAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAAAAAAAAAAACAPwAAgD8AAAAAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAIC/AAAAAAAAgD8=",
    "byteLength" : 128
  }, {
    "uri" : "data:application/gltf-buffer;base64,AAAAAAAAAD8AAIA/AADAPwAAAEAAACBAAABAQAAAYEAAAIBAAACQQAAAoEAAALBAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAkxjEPkSLbD8AAAAAAAAAAPT9ND/0/TQ/AAAAAAAAAAD0/TQ/9P00PwAAAAAAAAAAkxjEPkSLbD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAkxjEvkSLbD8AAAAAAAAAAPT9NL/0/TQ/AAAAAAAAAAD0/TS/9P00PwAAAAAAAAAAkxjEvkSLbD8AAAAAAAAAAAAAAAAAAIA/",
    "byteLength" : 240
  } ],
  
  "bufferViews" : [ {
    "buffer" : 0,
    "byteLength" : 48,
    "target" : 34963
  }, {
    "buffer" : 0,
    "byteOffset" : 48,
    "byteLength" : 120,
    "target" : 34962
  }, {
    "buffer" : 1,
    "byteLength" : 320,
    "byteStride" : 16
  }, {
    "buffer" : 2,
    "byteLength" : 128
  }, {
    "buffer" : 3,
    "byteLength" : 240
  } ],

  "accessors" : [ {
    "bufferView" : 0,
    "componentType" : 5123,
    "count" : 24,
    "type" : "SCALAR"
  }, {
    "bufferView" : 1,
    "componentType" : 5126,
    "count" : 10,
    "type" : "VEC3",
    "max" : [ 0.5, 2.0, 0.0 ],
    "min" : [ -0.5, 0.0, 0.0 ]
  }, {
    "bufferView" : 2,
    "componentType" : 5123,
    "count" : 10,
    "type" : "VEC4"
  }, {
    "bufferView" : 2,
    "byteOffset" : 160,
    "componentType" : 5126,
    "count" : 10,
    "type" : "VEC4"
  }, {
    "bufferView" : 3,
    "componentType" : 5126,
    "count" : 2,
    "type" : "MAT4"
  }, {
    "bufferView" : 4,
    "componentType" : 5126,
    "count" : 12,
    "type" : "SCALAR",
    "max" : [ 5.5 ],
    "min" : [ 0.0 ]
  }, {
    "bufferView" : 4,
    "byteOffset" : 48,
    "componentType" : 5126,
    "count" : 12,
    "type" : "VEC4",
    "max" : [ 0.0, 0.0, 0.707, 1.0 ],
    "min" : [ 0.0, 0.0, -0.707, 0.707 ]
  } ],
 
  "asset" : {
    "version" : "2.0"
  }
}
```



The result of rendering this asset is shown in Image 19a.

渲染此资产的结果如图 19a 所示。

<p align="center">
<img src="images/simpleSkin.gif" /><br>
<a name="simpleSkin-gif"></a>Image 19a: A scene with simple vertex skinning.
</p>


## Elements of the simple skin example

简单外观示例的元素

The elements of the given example are briefly summarized here:

下面简要总结了给定示例的元素：
- The `scenes` and `nodes` elements have been explained in the [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) section. For the vertex skinning, new nodes have been added: the nodes at index 1 and 2 define a new node hierarchy for the *skeleton*. These nodes can be considered the joints between the "bones" that will eventually cause the deformation of the mesh.
- 场景和节点元素已在场景和节点部分中进行了说明。对于顶点蒙皮，添加了新节点：索引 1 和 2 处的节点为骨架定义新的节点层次结构。这些节点可以被认为是最终导致网格变形的“骨骼”之间的关节。
- The new top-level dictionary `skins` contains a single skin in the given example. The properties of this skin object will be explained later.
- 新的顶级字典外观在给定示例中包含一个外观。稍后将解释此外观对象的属性。
- The concepts of `animations` has been explained in the [Animations](gltfTutorial_007_Animations.md) section. In the given example, the animation refers to the *skeleton* nodes so that the effect of the vertex skinning is actually visible during the animation.
- 动画的概念已在动画部分中进行了解释。在给定的示例中，动画引用骨架节点，以便在动画期间实际可以看到顶点外观的效果。
- The [Meshes](gltfTutorial_009_Meshes.md) section already explained the contents of the `meshes` and `mesh.primitive` objects. In this example, new mesh primitive attributes have been added, which are required for vertex skinning, namely the `"JOINTS_0"` and `"WEIGHTS_0"` attributes.
- 网格部分已经解释了网格和网格基元对象的内容。在此示例中，添加了顶点蒙皮所需的新网格基元属性，即“JOINTS_0”和“WEIGHTS_0”属性。
- There are several new `buffers`, `bufferViews`, and `accessors`. Their basic properties have been described in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BufferBufferViewsAccessors.md) section. In the given example, they contain the additional data required for vertex skinning.
- 有几个新的缓冲区、缓冲区视图和访问器。它们的基本属性已在缓冲区、缓冲区视图和访问器部分中进行了描述。在给定的示例中，它们包含顶点蒙皮所需的其他数据。

Details about how these elements are interconnected to achieve the vertex skinning will be explained in the [Skins](gltfTutorial_020_Skins.md) section.

有关这些元素如何互连以实现顶点蒙皮的详细信息将在外观部分中进行说明。


Previous: [Morph Targets](gltfTutorial_018_MorphTargets.md) | [Table of Contents](README.md) | Next: [Skins](gltfTutorial_020_Skins.md)
