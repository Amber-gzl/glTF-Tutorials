Previous: [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) | [Table of Contents](README.md) | Next: [Simple Animation](gltfTutorial_006_SimpleAnimation.md)


# Buffers, BufferViews, and Accessors
缓冲区、缓冲区视图和存取器

An example of `buffer`, `bufferView`, and `accessor` objects was already given in the [Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md) section. This section will explain these concepts in more detail.

缓冲区、缓冲区视图和访问器对象的示例已在最小 glTF 文件部分中给出。本节将更详细地解释这些概念。

## Buffers

缓冲区

A [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) represents a block of raw binary data, without an inherent structure or meaning. This data is referred to by a buffer using its `uri`. This URI may either point to an external file, or be a [data URI](gltfTutorial_002_BasicGltfStructure.md#binary-data-in-buffers) that encodes the binary data directly in the JSON file. The [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md) contained an example of a `buffer`, with 44 bytes of data, encoded in a data URI:

缓冲区表示原始二进制数据块，没有固有的结构或含义。此数据由缓冲区使用其 uri 引用。此 URI 可以指向外部文件，也可以是直接在 JSON 文件中对二进制数据进行编码的数据 URI。最小 glTF 文件包含一个缓冲区示例，其中包含 44 字节的数据，以数据 URI 编码：

```javascript
  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    }
  ],
```


<p align="center">
<img src="images/buffer.png" /><br>
<a name="buffer-png"></a>Image 5a: The buffer data, consisting of 44 bytes.
</p>

Parts of the data of a `buffer` may have to be passed to the renderer as vertex attributes, or as indices, or the data may contain skinning information or animation key frames. In order to be able to use this data, additional information about the structure and type of this data is required.

缓冲区的部分数据可能必须作为顶点属性或索引传递给渲染器，或者数据可能包含蒙皮信息或动画关键帧。为了能够使用此数据，需要有关此数据的结构和类型的其他信息。


## BufferViews
缓冲区试图

The first step of structuring the data from a `buffer` is with [`bufferView`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-bufferview) objects. A `bufferView` represents a "slice" of the data of one buffer. This slice is defined using an offset and a length, in bytes. The [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md) defined two `bufferView` objects:

从缓冲区结构化数据的第一步是使用 bufferView 对象。缓冲区视图表示一个缓冲区的数据“切片”。此切片使用偏移量和长度（以字节为单位）定义。最小 glTF 文件定义了两个缓冲区视图对象：
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

The first `bufferView` refers to the first 6 bytes of the buffer data. The second one refers to 36 bytes of the buffer, with an offset of 8 bytes, as shown in this image:

第一个缓冲区视图引用缓冲区数据的前 6 个字节。第二个引用缓冲区的 36 个字节，偏移量为 8 个字节，如下图所示：

<p align="center">
<img src="images/bufferBufferView.png" /><br>
<a name="bufferBufferView-png"></a>Image 5b: The buffer views, referring to parts of the buffer.
</p>

The bytes that are shown in light gray are padding bytes that are required for properly aligning the accessors, as described below. 

以浅灰色显示的字节是正确对齐访问器所需的填充字节，如下所述。

Each `bufferView` additionally contains a `target` property. This property may later be used by the renderer to classify the type or nature of the data that the buffer view refers to. The `target` can be a constant indicating that the data is used for vertex attributes (`34962`, standing for `ARRAY_BUFFER`), or that the data is used for vertex indices (`34963`, standing for `ELEMENT_ARRAY_BUFFER`).

每个缓冲区视图还包含一个目标属性。呈现器稍后可以使用此属性对缓冲区视图引用的数据的类型或性质进行分类。目标可以是指示数据用于顶点属性（34962，代表ARRAY_BUFFER）或数据用于顶点索引（34963，代表ELEMENT_ARRAY_BUFFER）的常量。

At this point, the `buffer` data has been divided into multiple parts, and each part is described by one `bufferView`. But in order to really use this data in a renderer, additional information about the type and layout of the data is required.

此时，缓冲区数据已分为多个部分，每个部分由一个缓冲区视图描述。但是，为了在渲染器中真正使用此数据，需要有关数据类型和布局的其他信息。


## Accessors

存取器

An [`accessor`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-accessor) object refers to a `bufferView` and contains properties that define the type and layout of the data of this `bufferView`.

访问器对象引用缓冲区视图，并包含定义此缓冲区视图的数据的类型和布局的属性。

### Data type

数据类型

The type of an accessor's data is encoded in the `type` and the `componentType` properties. The value of the `type` property is a string that specifies whether the data elements are scalars, vectors, or matrices. For example, the value may be `"SCALAR"` for scalar values, `"VEC3"` for 3D vectors, or `"MAT4"` for 4&times;4 matrices.

访问器对象引用缓冲区视图，并包含定义此缓冲区视图的数据的类型和布局的属性。

The `componentType` specifies the type of the components of these data elements. This is a GL constant that may, for example, be `5126` (`FLOAT`) or `5123` (`UNSIGNED_SHORT`), to indicate that the elements have `float` or `unsigned short` components, respectively.

“组件类型”指定这些数据元素的组件类型。这是一个 GL 常量，例如，可以是“5126”（“FLOAT”）或“5123”（“UNSIGNED_SHORT”），以指示元素分别具有“浮点”或“无符号短”组件。

Different combinations of these properties may be used to describe arbitrary data types. For example, the [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md) contained two accessors:

这些属性的不同组合可用于描述任意数据类型。例如，最小 glTF 文件包含两个访问器：

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

The first accessor refers to the `bufferView` with index 0, which defines the part of the `buffer` data that contains the indices. Its `type` is `"SCALAR"`, and its `componentType` is `5123` (`UNSIGNED_SHORT`). This means that the indices are stored as scalar `unsigned short` values.

第一个访问器引用索引为 0 的 bufferView，它定义包含索引的缓冲区数据部分。其类型为“SCALAR”，其组件类型为 5123 （UNSIGNED_SHORT）。这意味着索引存储为标量无符号短值。

The second accessor refers to the `bufferView` with index 1, which defines the part of the `buffer` data that contains the vertex attributes - particularly, the vertex positions. Its `type` is `"VEC3"`, and its `componentType` is  `5126` (`FLOAT`). So this accessor describes 3D vectors with floating point components.

第二个访问器引用索引为 1 的“bufferView”，它定义了包含顶点属性的“缓冲区”数据部分，特别是顶点位置。它的“类型”是“VEC3”，它的“组件类型”是“5126”（“FLOAT”）。因此，此访问器描述了具有浮点分量的 3D 向量。


### Data layout

数据布局

Additional properties of an `accessor` further specify the layout of the data. The `count` property of an accessor indicates how many data elements it consists of. In the example above, the count has been `3` for both accessors, standing for the three indices and the three vertices of the triangle, respectively. Each accessor also has a `byteOffset` property. For the example above, it has been `0` for both accessors, because there was only one `accessor` for each `bufferView`. But when multiple accessors refer to the same `bufferView`, then the `byteOffset` describes where the data of the accessor starts, relative to the `bufferView` that it refers to.

访问器的其他属性进一步指定数据的布局。访问器的 count 属性指示它由多少个数据元素组成。在上面的示例中，两个访问器的计数均为 3，分别代表三角形的三个索引和三个顶点。每个访问器还有一个 byteOffset 属性。对于上面的示例，两个访问器的值均为 0，因为每个 bufferView 只有一个访问器。但是，当多个访问器引用同一个缓冲区视图时，byteOffset 将描述访问器的数据相对于它所引用的缓冲区视图的起始位置。


### Data alignment

数据对齐

The data that is referred to by an `accessor` may be sent to the graphics card for rendering, or be used at the host side as animation or skinning data. Therefore, the data of an `accessor` has to be aligned based on the *type* of the data. For example, when the `componentType` of an `accessor` is `5126` (`FLOAT`), then the data must be aligned at 4-byte boundaries, because a single `float` value consists of four bytes. This alignment requirement of an `accessor` refers to its `bufferView` and the underlying `buffer`. Particularly, the alignment requirements are as follows:

访问器引用的数据可以发送到图形卡进行渲染，或者在主机端用作动画或外观数据。因此，访问器的数据必须根据数据类型进行对齐。例如，当访问器的 componentType 为 5126 （FLOAT） 时，数据必须在 4 字节边界对齐，因为单个浮点值由四个字节组成。访问器的这种对齐要求是指其缓冲区视图和基础缓冲区。具体而言，对齐要求如下：

- The `byteOffset` of an `accessor` must be divisible by the size of its `componentType`. 
- 访问器的字节偏移量必须能够被其组件类型的大小整除。
- The sum of the `byteOffset` of an accessor and the `byteOffset` of the `bufferView` that it refers to must be divisible by the size of its `componentType`.
- 访问器的 byteOffset 和它引用的 bufferView 的 byteOffset 之和必须能够被其 componentType 的大小整除。

In the example above, the `byteOffset` of the `bufferView` with index 1 (which refers to the vertex attributes) was chosen to be `8`, in order to align the data of the accessor for the vertex positions to 4-byte boundaries. The bytes `6` and `7` of the `buffer` are thus *padding* bytes that do not carry relevant data. 
在上面的示例中，索引为 1（指顶点属性）的 bufferView 的 byteOffset 被选为 8，以便将顶点位置的访问器数据与 4 字节边界对齐。因此，缓冲区的字节 6 和 7 是填充不携带相关数据的字节。

Image 5c illustrates how the raw data of a `buffer` is structured using `bufferView` objects and is augmented with data type information using `accessor` objects.
图 5c 说明了如何使用 bufferView 对象构建缓冲区的原始数据，以及如何使用访问器对象通过数据类型信息进行扩充。

<p align="center">
<img src="images/bufferBufferViewAccessor.png" /><br>
<a name="bufferBufferViewAccessor-png"></a>Image 5c: The accessors defining how to interpret the data of the buffer views.
</p>


### Data interleaving

数据交错

The data of the attributes that are stored in a single `bufferView` may be stored as an *Array-Of-Structures*. A single `bufferView` may, for example, contain the data for vertex positions and for vertex normals in an interleaved fashion. In this case, the `byteOffset` of an accessor defines the start of the first relevant data element for the respective attribute, and the `bufferView` defines an additional `byteStride` property. This is the number of bytes between the start of one element of its accessors, and the start of the next one. An example of how interleaved position and normal attributes are stored inside a `bufferView` is shown in Image 5d.

存储在单个缓冲区视图中的属性数据可以存储为结构数组。例如，单个缓冲区视图可以以交错方式包含顶点位置和顶点法线的数据。在这种情况下，访问器的 byteOffset 定义相应属性的第一个相关数据元素的开头，bufferView 定义一个额外的 byteStride 属性。这是其访问器的一个元素的开始与下一个元素的开始之间的字节数。交错位置和法线属性如何存储在缓冲区视图中的示例如图 5d 所示。
<p align="center">
<img src="images/aos.png" /><br>
<a name="aos-png"></a>Image 5d: Interleaved accessors in one buffer view.
</p>


### Data contents

数据内容

An `accessor` also contains `min` and `max` properties that summarize the contents of their data. They are the component-wise minimum and maximum values of all data elements contained in the accessor. In the case of vertex positions, the `min` and `max` properties thus define the *bounding box* of an object. This can be useful for prioritizing downloads, or for visibility detection. In general, this information is also useful for storing and processing *quantized* data that is dequantized at runtime, by the renderer, but details of this quantization are beyond the scope of this tutorial.

访问器还包含汇总其数据内容的 min 和 max 属性。它们是访问器中包含的所有数据元素的组件最小值和最大值。在顶点位置的情况下，最小值和最大值属性因此定义了对象的边界框。这对于确定下载优先级或可见性检测非常有用。通常，此信息对于存储和处理呈现器在运行时反量化的量化数据也很有用，但此量化的详细信息超出了本教程的范围。



## Sparse accessors

稀疏访问器


With version 2.0, the concept of *sparse accessors* was introduced in glTF. This is a special representation of data that allows very compact storage of multiple data blocks that have only a few different entries. For example, when there is geometry data that contains vertex positions, this geometry data may be used for multiple objects. This may be achieved by referring to the same `accessor` from both objects. If the vertex positions for both objects are mostly the same and differ for only a few vertices, then it is not necessary to store the whole geometry data twice. Instead, it is possible to store the data only once, and use a sparse accessor to store only the vertex positions that differ for the second object. 

在 2.0 版本中，glTF 中引入了稀疏访问器的概念。这是数据的特殊表示形式，允许非常紧凑地存储只有几个不同条目的多个数据块。例如，当存在包含顶点位置的几何数据时，此几何数据可用于多个对象。这可以通过从两个对象引用相同的访问器来实现。如果两个对象的顶点位置基本相同，并且只有少数顶点不同，则无需将整个几何数据存储两次。相反，可以只存储一次数据，并使用稀疏访问器仅存储第二个对象不同的顶点位置。

The following is a complete glTF asset, in embedded representation, that shows an example of sparse accessors:

以下是嵌入式表示形式的完整 glTF 资产，其中显示了稀疏访问器的示例：

```javascript
{
  "scenes" : [ {
    "nodes" : [ 0 ]
  } ],
  
  "nodes" : [ {
    "mesh" : 0
  } ],
  
  "meshes" : [ {
    "primitives" : [ {
      "attributes" : {
        "POSITION" : 1
      },
      "indices" : 0
    } ]
  } ],
  
  "buffers" : [ {
    "uri" : "data:application/gltf-buffer;base64,AAAIAAcAAAABAAgAAQAJAAgAAQACAAkAAgAKAAkAAgADAAoAAwALAAoAAwAEAAsABAAMAAsABAAFAAwABQANAAwABQAGAA0AAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAQAAAAAAAAAAAAABAQAAAAAAAAAAAAACAQAAAAAAAAAAAAACgQAAAAAAAAAAAAADAQAAAAAAAAAAAAAAAAAAAgD8AAAAAAACAPwAAgD8AAAAAAAAAQAAAgD8AAAAAAABAQAAAgD8AAAAAAACAQAAAgD8AAAAAAACgQAAAgD8AAAAAAADAQAAAgD8AAAAACAAKAAwAAAAAAIA/AAAAQAAAAAAAAEBAAABAQAAAAAAAAKBAAACAQAAAAAA=",
    "byteLength" : 284
  } ],
  
  "bufferViews" : [ {
    "buffer" : 0,
    "byteOffset" : 0,
    "byteLength" : 72,
    "target" : 34963
  }, {
    "buffer" : 0,
    "byteOffset" : 72,
    "byteLength" : 168
  }, {
    "buffer" : 0,
    "byteOffset" : 240,
    "byteLength" : 6
  }, {
    "buffer" : 0,
    "byteOffset" : 248,
    "byteLength" : 36
  } ],
  
  "accessors" : [ {
    "bufferView" : 0,
    "byteOffset" : 0,
    "componentType" : 5123,
    "count" : 36,
    "type" : "SCALAR",
    "max" : [ 13 ],
    "min" : [ 0 ]
  }, {
    "bufferView" : 1,
    "byteOffset" : 0,
    "componentType" : 5126,
    "count" : 14,
    "type" : "VEC3",
    "max" : [ 6.0, 4.0, 0.0 ],
    "min" : [ 0.0, 0.0, 0.0 ],
    "sparse" : {
      "count" : 3,
      "indices" : {
        "bufferView" : 2,
        "byteOffset" : 0,
        "componentType" : 5123
      },
      "values" : {
        "bufferView" : 3,
        "byteOffset" : 0
      }
    }
  } ],
  
  "asset" : {
    "version" : "2.0"
  }
}
```

The result of rendering this asset is shown in Image 5e:

渲染此资产的结果如图 5e 所示
<p align="center">
<img src="images/simpleSparseAccessor.png" /><br>
<a name="simpleSparseAccessor-png"></a>Image 5e: The result of rendering the simple sparse accessor asset.
</p>

The example contains two accessors: one for the indices of the mesh, and one for the vertex positions. The one that refers to the vertex positions defines an additional `accessor.sparse` property, which contains the information about the sparse data substitution that should be applied:

该示例包含两个访问器：一个用于网格的索引，另一个用于顶点位置。引用顶点位置的那个定义了一个额外的 accessor.sparse 属性，该属性包含有关应应用的稀疏数据替换的信息：


```javascript
  "accessors" : [ 
  ...
  {
    "bufferView" : 1,
    "byteOffset" : 0,
    "componentType" : 5126,
    "count" : 14,
    "type" : "VEC3",
    "max" : [ 6.0, 4.0, 0.0 ],
    "min" : [ 0.0, 0.0, 0.0 ],
    "sparse" : {
      "count" : 3,
      "indices" : {
        "bufferView" : 2,
        "byteOffset" : 0,
        "componentType" : 5123
      },
      "values" : {
        "bufferView" : 3,
        "byteOffset" : 0
      }
    }
  } ],
```

This `sparse` object itself defines the `count` of elements that will be affected by the substitution. The `sparse.indices` property refers to a `bufferView` that contains the indices of the elements which will be replaced. The `sparse.values` refers to a `bufferView` that contains the actual data. 

此稀疏对象本身定义将受替换影响的元素计数。sparse.indices 属性引用一个缓冲区视图，其中包含将被替换的元素的索引。sparse.values 是指包含实际数据的缓冲区视图。

In the example, the original geometry data is stored in the `bufferView` with index 1. It describes a rectangular array of vertices. The `sparse.indices` refer to the `bufferView` with index 2, which contains the indices `[8, 10, 12]`. The `sparse.values` refers to the `bufferView` with index 3, which contains new vertex positions, namely, `[(1,2,0), (3,3,0), (5,4,0)]`. The effect of applying the corresponding substitution is shown in Image 5f.

在此示例中，原始几何数据存储在索引为 1 的缓冲区视图中。它描述了顶点的矩形数组。sparse.indices 引用索引为 2 的 bufferView，其中包含索引 [8， 10， 12]。sparse.values 是指索引为 3 的缓冲区视图，其中包含新的顶点位置，即 [（1,2,0）、（3,3,0）、（5,4,0）]。应用相应替换的效果如图5f所示。


<p align="center">
<img src="images/simpleSparseAccessorDescription.png" /><br>
<a name="simpleSparseAccessorDescription-png"></a>Image 5f: The substitution that is done with the sparse accessor.
</p>





Previous: [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) | [Table of Contents](README.md) | Next: [Simple Animation](gltfTutorial_006_SimpleAnimation.md)
