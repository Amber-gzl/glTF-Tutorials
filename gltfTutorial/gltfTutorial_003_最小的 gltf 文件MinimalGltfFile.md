Previous: [Basic glTF Structure](gltfTutorial_002_BasicGltfStructure.md) | [Table of Contents](README.md) | Next: [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)


# A Minimal glTF File
最小的 glTF 文件

The following is a minimal but complete glTF asset, containing a single, indexed triangle. You can copy and paste it into a `gltf` file, and every glTF-based application should be able to load and render it. This section will explain the basic concepts of glTF based on this example.

下面是一个最小但完整的 glTF 资产，包含一个索引三角形。您可以将其复制并粘贴到 gltf 文件中，每个基于 glTF 的应用程序都应该能够加载和渲染它。本节将基于此示例解释 glTF 的基本概念。

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
        "indices" : 0
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
  
  "asset" : {
    "version" : "2.0"
  }
}
```

<p align="center">
<img src="images/triangle.png" /><br>
<a name="triangle-png"></a>Image 3a: A single triangle.
</p>


## The `scene` and `nodes` structure
场景和节点结构

The [`scenes`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-scene) array is the entry point for the description of the scenes that are stored in the glTF. When parsing a glTF JSON file, the traversal of the scene structure will start here. Each scene contains an array called `nodes`, which contains the indices of [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node) objects. These nodes are the root nodes of a scene graph hierarchy.

场景数组是描述存储在 glTF 中的场景的入口点。解析 glTF JSON 文件时，场景结构的遍历将从此处开始。每个场景都包含一个名为节点的数组，其中包含节点对象的索引。这些节点是场景图层次结构的根节点。

The example here consists of a single scene. The `scene` property indicates that this scene is the default scene that should be displayed when the asset is loaded. The scene refers to the only node in this example, which is the node with the index 0. This node, in turn, refers to the only mesh, which has the index 0:

此处的示例由单个场景组成。scene 属性指示此场景是加载资产时应显示的默认场景。场景引用此示例中唯一的节点，即索引为 0 的节点。反过来，此节点引用索引为 0 的唯一网格：


```javascript
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
```

More details about scenes and nodes and their properties will be given in the [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) section.

有关场景和节点及其属性的更多详细信息将在场景和节点部分中提供。


## The `meshes`

A [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh) represents an actual geometric object that appears in the scene. The mesh itself usually does not have any properties, but only contains an array of [`mesh.primitive`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh-primitive) objects, which serve as building blocks for larger models. Each mesh primitive contains a description of the geometry data that the mesh consists of.

网格表示出现在场景中的实际几何对象。网格本身通常没有任何属性，而只包含 mesh.primitive 对象的数组，这些对象充当较大模型的构建块。每个网格基元都包含网格所包含的几何数据的描述。

The example consists of a single mesh, and has a single `mesh.primitive` object. The mesh primitive has an array of `attributes`. These are the attributes of the vertices of the mesh geometry, and in this case, this is only the `POSITION` attribute, describing the positions of the vertices. The mesh primitive describes an *indexed* geometry, which is indicated by the `indices` property. By default, it is assumed to describe a set of triangles, so that three consecutive indices are the indices of the vertices of one triangle.

该示例由单个网格组成，并具有单个网格.基元对象。网格基元具有一系列属性。这些是网格几何体顶点的属性，在本例中，这只是描述顶点位置的 POSITION 属性。网格基元描述索引几何体，该几何体由索引属性指示。默认情况下，假设描述一组三角形，因此三个连续索引是一个三角形顶点的索引。

The actual geometry data of the mesh primitive is given by the `attributes` and the `indices`. These both refer to `accessor` objects, which will be explained below.

网格基元的实际几何数据由属性和索引给出。它们都引用访问器对象，这将在下面解释。

```javascript
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0
      } ]
    }
  ],
```

A more detailed description of meshes and mesh primitives can be found in the [meshes](gltfTutorial_009_Meshes.md) section.

有关网格和网格基元的更详细说明，请参阅网格部分。

## The `buffer`, `bufferView`, and `accessor` concepts

缓冲区、缓冲区视图和访问器概念

The `buffer`, `bufferView`, and `accessor` objects provide information about the geometry data that the mesh primitives consist of. They are introduced here quickly, based on the specific example. A more detailed description of these concepts will be given in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section.

缓冲区、缓冲区视图和访问器对象提供有关网格基元组成的几何数据的信息。根据具体示例，在此处快速介绍它们。有关这些概念的更详细说明，请参阅缓冲区、缓冲区视图和访问器部分。

### Buffers

A [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) defines a block of raw, unstructured data with no inherent meaning. It contains an `uri`, which can either point to an external file that contains the data, or it can be a [data URI](gltfTutorial_002_BasicGltfStructure.md#binary-data-in-data-uris) that encodes the binary data directly in the JSON file.

缓冲区定义一个没有固有含义的原始非结构化数据块。它包含一个 URI，该 URI 可以指向包含数据的外部文件，也可以是直接在 JSON 文件中对二进制数据进行编码的数据 URI。

In the example file, the second approach is used: there is a single buffer, containing 44 bytes, and the data of this buffer is encoded as a data URI:

缓冲区定义一个没有固有含义的原始非结构化数据块。它包含一个 URI，该 URI 可以指向包含数据的外部文件，也可以是直接在 JSON 文件中对二进制数据进行编码的数据 URI。

```javascript
  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    }
  ],
```

This data contains the indices of the triangle, and the vertex positions of the triangle. But in order to actually use this data as the geometry data of a mesh primitive, additional information about the *structure* of this data is required. This information about the structure is encoded in the `bufferView` and `accessor` objects.

此数据包含三角形的索引和三角形的顶点位置。但是，为了实际将此数据用作网格基元的几何数据，需要有关此数据结构的其他信息。有关结构的此信息在 bufferView 和访问器对象中编码。

### Buffer views

A [`bufferView`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-bufferview) describes a "chunk" or a "slice" of the whole, raw buffer data. In the given example, there are two buffer views. They both refer to the same buffer. The first buffer view refers to the part of the buffer that contains the data of the indices: it has a `byteOffset` of 0 referring to the whole buffer data, and a `byteLength` of 6. The second buffer view refers to the part of the buffer that contains the vertex positions. It starts at a `byteOffset` of 8, and has a `byteLength` of 36; that is, it extends to the end of the whole buffer.

bufferView 描述整个原始缓冲区数据的“块”或“切片”。在给定的示例中，有两个缓冲区视图。它们都引用同一个缓冲区。第一个缓冲区视图是指缓冲区中包含索引数据的部分：它的 byteOffset 为 0，表示整个缓冲区数据，byteLength 为 6。第二个缓冲区视图是指包含顶点位置的缓冲区部分。它从字节偏移量 8 开始，字节长度为 36;也就是说，它延伸到整个缓冲区的末尾。

```javascript
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
```


### Accessors

存取器

The second step of structuring the data is accomplished with [`accessor`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-accessor) objects. They define how the data of a `bufferView` has to be interpreted by providing information about the data types and the layout.

结构化数据的第二步是使用访问器对象完成的。它们通过提供有关数据类型和布局的信息来定义如何解释缓冲区视图的数据

In the example, there are two accessor objects.

在此示例中，有两个访问器对象。

The first accessor describes the indices of the geometry data. It refers to the `bufferView` with index 0, which is the part of the `buffer` that contains the raw data for the indices. Additionally, it specifies the `count` and `type` of the elements and their `componentType`. In this case, there are 3 scalar elements, and their component type is given by a constant that stands for the `unsigned short` type.

第一个访问器描述几何数据的索引。它引用具有索引 0 的缓冲区视图，这是包含索引的原始数据的缓冲区部分。此外，它还指定元素及其组件类型的计数和类型。在这种情况下，有 3 个标量元素，它们的组件类型由代表无符号短类型的常量给出。

The second accessor describes the vertex positions. It contains a reference to the relevant part of the buffer data, via the `bufferView` with index 1, and its `count`, `type`, and `componentType` properties say that there are three elements of 3D vectors, each having `float` components.

第二个访问器描述顶点位置。它通过索引为 1 的 bufferView 包含对缓冲区数据相关部分的引用，其计数、类型和组件类型属性表示 3D 向量有三个元素，每个元素都有浮点分量。


```javascript
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
```

As described above, a `mesh.primitive` may now refer to these accessors, using their indices:

如上所述，mesh.primitive 现在可以使用它们的索引来引用这些访问器：

```javascript
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0
      } ]
    }
  ],
```

When this `mesh.primitive` has to be rendered, the renderer can resolve the underlying buffer views and buffers and will send the required parts of the buffer to the renderer, together with the information about the data types and layout. A more detailed description of how the accessor data is obtained and processed by the renderer is given in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section.

当必须渲染此 mesh.primitive 时，渲染器可以解析基础缓冲区视图和缓冲区，并将缓冲区的所需部分以及有关数据类型和布局的信息发送到渲染器。有关呈现器如何获取和处理访问器数据的更详细说明，请参阅缓冲区、缓冲区视图和访问器部分。



## The `asset` description

资产描述

In glTF 1.0, this property is still optional, but in subsequent glTF versions, the JSON file is required to contain an `asset` property that contains the `version` number. The example here says that the asset complies to glTF version 2.0:

在 glTF 1.0 中，此属性仍然是可选的，但在后续 glTF 版本中，JSON 文件需要包含包含版本号的资产属性。此处的示例说明资产符合 glTF 版本 2.0：

```javascript
  "asset" : {
    "version" : "2.0"
  }
```

The `asset` property may contain additional metadata that is described in the [`asset` specification](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-asset).

资产属性可能包含资产规范中描述的其他元数据。




Previous: [Basic glTF Structure](gltfTutorial_002_BasicGltfStructure.md) | [Table of Contents](README.md) | Next: [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)
