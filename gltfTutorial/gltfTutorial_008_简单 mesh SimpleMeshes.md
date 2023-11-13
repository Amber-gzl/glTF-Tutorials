Previous: [Animations](gltfTutorial_007_Animations.md) | [Table of Contents](README.md) | Next: [Meshes](gltfTutorial_009_Meshes.md)

# Simple Meshes

简单网格

A [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh) represents a geometric object that appears in a scene. An example of a `mesh` has already been shown in the [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md). This example had a single `mesh` attached to a single `node`, and the mesh consisted of a single [`mesh.primitive`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh-primitive) that contained only a single attribute&mdash;namely, the attribute for the vertex positions. But usually, the mesh primitives will contain more attributes. These attributes may, for example, be the vertex normals or texture coordinates.

mesh表示出现在场景中的几何对象。网格的示例已在最小 glTF 文件中显示。此示例将单个网格附加到单个节点，并且网格由仅包含单个属性（即顶点位置的属性）的单个网格组成。但通常，网格基元将包含更多属性。例如，这些属性可以是顶点法线或纹理坐标。

The following is a glTF asset that contains a simple mesh with multiple attributes, which will serve as the basis for explaining the related concepts:

下面是一个 glTF 资产，其中包含一个具有多个属性的简单网格，它将作为解释相关概念的基础：

```javascript
{
  "scene": 0,
  "scenes" : [
    {
      "nodes" : [ 0, 1]
    }
  ],
  "nodes" : [
    {
      "mesh" : 0
    },
    {
      "mesh" : 0,
      "translation" : [ 1.0, 0.0, 0.0 ]
    }
  ],

  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1,
          "NORMAL" : 2
        },
        "indices" : 0
      } ]
    }
  ],

  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAgD8AAAAAAAAAAAAAgD8=",
      "byteLength" : 80
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
      "byteLength" : 72,
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
    },
    {
      "bufferView" : 1,
      "byteOffset" : 36,
      "componentType" : 5126,
      "count" : 3,
      "type" : "VEC3",
      "max" : [ 0.0, 0.0, 1.0 ],
      "min" : [ 0.0, 0.0, 1.0 ]
    }
  ],
  "asset" : {
    "version" : "2.0"
  }
}
```

Image 8a shows the rendered glTF asset.
图 8a 显示了渲染的 glTF 资产。

<p align="center">
<img src="images/simpleMeshes.png" /><br>
<a name="simpleMeshes-png"></a>Image 8a: A simple mesh, attached to two nodes.
</p>


## The mesh definition

网格定义

The given example still contains a single mesh that has a single mesh primitive. But this mesh primitive contains multiple attributes:

给定的示例仍然包含具有单个网格基元的单个网格。但是这个网格基元包含多个属性：

```javascript
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1,
          "NORMAL" : 2
        },
        "indices" : 0
      } ]
    }
  ],
```

In addition to the `"POSITION"` attribute, it has a `"NORMAL"` attribute. This refers to the `accessor` object that provides the vertex normals, as described in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section.

除了“POSITION”属性之外，它还具有“NORMAL”属性。这是指提供顶点法线的访问器对象，如缓冲区、缓冲区视图和访问器部分所述。

## The rendered mesh instances

渲染的网格实例

As can be seen in Image 8a, the mesh is rendered *twice*. This is accomplished by attaching the mesh to two different nodes:

如图8a所示，网格被渲染了两次。这是通过将网格附加到两个不同的节点来实现的：

```javascript
  "nodes" : [
    {
      "mesh" : 0
    },
    {
      "mesh" : 0,
      "translation" : [ 1.0, 0.0, 0.0 ]
    }
  ],
```

The `mesh` property of each node refers to the mesh that is attached to the node, using the index of the mesh. One of the nodes has a `translation` that causes the attached mesh to be rendered at a different position.

每个节点的网格属性是指使用网格的索引附加到节点的网格。其中一个节点具有平移，导致附加的网格在不同的位置渲染。

The [next section](gltfTutorial_009_Meshes.md) will explain meshes and mesh primitives in more detail.

下一节将更详细地解释网格和网格基元。



Previous: [Animations](gltfTutorial_007_Animations.md) | [Table of Contents](README.md) | Next: [Meshes](gltfTutorial_009_Meshes.md)
