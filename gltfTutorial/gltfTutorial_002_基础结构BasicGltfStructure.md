Previous: [Introduction](gltfTutorial_001_Introduction.md) | [Table of Contents](README.md) | Next: [A Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md)

# The Basic Structure of glTF

glTF的基本结构

The core of glTF is a JSON file. This file describes the whole contents of the 3D scene. It consists of a description of the scene structure itself, which is given by a hierarchy of nodes that define a scene graph. The 3D objects that appear in the scene are defined using meshes that are attached to the nodes. Materials define the appearance of the objects. Animations describe how the 3D objects are transformed (e.g., rotated or translated) over time, and skins define how the geometry of the objects is deformed based on a skeleton pose. Cameras describe the view configuration for the renderer.

glTF的核心是一个JSON文件。此文件描述了 3D 场景的全部内容，它由场景结构（scene structure）的描述组成，定义了场景图的节点层次结构。场景中出现的 3D 对象由附加到节点的Mesh定义。材质（Material）定义对象的外观。动画（Animation）描述了 3D 对象如何随时间变换（例如，旋转或平移），皮肤（Skin）定义了对象的几何形状如何根据骨架姿势变形。相机（camera）描述渲染器（renderer）的视图配置。

## The JSON structure

JSON 结构

The scene objects are stored in arrays in the JSON file. They can be accessed using the index of the respective object in the array:

场景（Scene）对象存储在 JSON 文件的数组中。可以使用数组中相应对象的索引（脚标/index）访问它们

```javascript
"meshes" :
[
    { ... }
    { ... }
    ...
],
```

These indices are also used to define the *relationships* between the objects. The example above defines multiple meshes, and a node may refer to one of these meshes, using the mesh index, to indicate that the mesh should be attached to this node:

索引还用于定义对象之间的关系。上面的示例定义了多个Mesh，nodes可以使用网格索引引用其中一个mesh，以指示mesh应附加到此节点

```javascript
"nodes":
[
    { "mesh": 0, ... },
    { "mesh": 5, ... },
    ...
]
```

The following image (adapted from the [glTF concepts section](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#concepts)) gives an overview of the top-level elements of the JSON part of a glTF asset:

下图（改编自 glTF 概念部分）概述了 glTF 的 json 描述中的部分顶级元素：

<p align="center">
<img src="images/gltfJsonStructure.png" /><br>
<a name="gltfJsonStructure-png"></a>Image 2a: The glTF JSON structure
</p>

These elements are summarized here quickly, to give an overview, with links to the respective sections of the glTF specification. More detailed explanations of the relationships between these elements will be given in the following sections.

这里概述了这些元素，并提供指向glTF规范相应部分的链接。以下各节将更详细地解释这些元素之间的关系。

- The [`scene`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-scene) is the entry point for the description of the scene that is stored in the glTF. It refers to the `node`s that define the scene graph.
- 场景（Scene）是描述存储在 glTF 中的场景的入口点。它指的是定义场景图的节点。
- The [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node) is one node in the scene graph hierarchy. It can contain a transformation (e.g., rotation or translation), and it may refer to further (child) nodes. Additionally, it may refer to `mesh` or `camera` instances that are "attached" to the node, or to a `skin` that describes a mesh deformation.
- 节点（Node）是场景图层次结构中的一个节点。它包含变换（例如，旋转或平移），并可以引用其他（子）节点。此外，节点还可以引用附加到节点上的Mesh或Camera实例或Skin。
- The [`camera`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera) defines the view configuration for rendering the scene.
- 摄像机（camera）定义用于渲染场景的视图配置。
- A [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh) describes a geometric object that appears in the scene. It refers to `accessor` objects that are used for accessing the actual geometry data, and to `material`s that define the appearance of the object when it is rendered.
- 网格（Mesh）描述出现在场景中的几何对象。它是指用于访问实际几何数据的存取器(accessor)对象，以及定义对象在渲染时外观的材质。（geometry+material？）
- The [`skin`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-skin) defines parameters that are required for vertex skinning, which allows the deformation of a mesh based on the pose of a virtual character. The values of these parameters are obtained from an `accessor`.
- 皮肤（Skin）定义顶点蒙皮所需的参数，该参数允许根据虚拟角色的姿势对Mesh进行变形。这些参数的值是从存取器（accessor）获取的。
- An [`animation`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation) describes how transformations of certain nodes (e.g., rotation or translation) change over time.
- 动画(Animation)描述了某些节点的变换（例如，旋转或平移）如何随时间变化。
- The [`accessor`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-accessor) is used as an abstract source of arbitrary data. It is used by the `mesh`, `skin`, and `animation`, and provides the geometry data, the skinning parameters and the time-dependent animation values. It refers to a [`bufferView`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-bufferview), which is a part of a [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) that contains the actual raw binary data.
- 存取器（Accessor）用作任意数据的抽象源。它可供Mesh、Skin和Animation使用，并提供几何数据、蒙皮参数和随时间变化的动画值。它指的是缓冲区视图，包含部分原始二进制数据的缓冲区。
- The [`material`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-material) contains the parameters that define the appearance of an object. It usually refers to `texture` objects that will be applied to the rendered geometry.
- 材质（Material）包含定义对象外观的参数。它通常是指将应用于渲染几何体的纹理对象。
- The [`texture`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-texture) is defined by a [`sampler`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-sampler) and an [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image). The `sampler` defines how the texture `image` should be placed on the object.
- 纹理（Texture）由采样器和图像定义。采样器定义纹理图像应如何放置在对象上。（sampler ≈ uv？）

## References to external data

对外部数据的引用

The binary data, like geometry and textures of the 3D objects, are usually not contained in the JSON file. Instead, they are stored in dedicated files, and the JSON part only contains links to these files. This allows the binary data to be stored in a form that is very compact and can efficiently be transferred over the web. Additionally, the data can be stored in a format that can be used directly in the renderer, without having to parse, decode, or preprocess the data.    

二进制数据（如 3D 对象的几何图形和纹理）通常不包含在 JSON 文件中。它们存储在专用文件（.bin file）中，JSON 部分仅包含指向这些文件的链接。这使得二进制数据以非常紧凑的形式存储，并且可以有效地通过Web传输。此外，数据可以直接以在渲染器中使用的格式存储，从而无需解析、解码或预处理数据。

<p align="center">
<img src="images/gltfStructure.png" /><br>
<a name="gltfStructure-png"></a>Image 2b: The glTF structure
</p>

As shown in the image above, there are two types of objects that may contain such links to external resources, namely `buffers` and `images`. These objects will later be explained in more detail.

如上图所示，有两类对象可能包含指向外部资源的链接，即缓冲区和图像。后续将更详细地描述。

## Reading and managing external data

读取和管理外部数据

Reading and processing a glTF asset starts with parsing the JSON structure. After the structure has been parsed, the [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) and [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image) objects are available in the top-level `buffers` and `images` arrays, respectively. Each of these objects may refer to blocks of binary data. For further processing, this data is read into memory. Usually, the data will be be stored in an array so that they may be looked up using the same index that is used for referring to the `buffer` or `image` object that they belong to. 

读取和处理 glTF 资产从解析 JSON 结构开始。解析后，缓冲区和图像分别存在顶级缓冲区和图像数组中。这些对象都可以引用二进制数据块。为了进一步处理，此数据会被读入内存。数据将存储在数组中，以便可以通过它们所属的缓冲区或图像数组的索引来查找它们。

## Binary data in `buffers`

缓冲区中的二进制数据

A [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) contains a URI that points to a file containing the raw, binary buffer data:

缓冲区包含一个 URI，该 URI 指向包含原始二进制缓冲区数据的文件：

```javascript
"buffer01": {
    "byteLength": 12352,
    "type": "arraybuffer",
    "uri": "buffer01.bin"
}
```

This binary data is just a raw block of memory that is read from the URI of the `buffer`, with no inherent meaning or structure. The [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section will show how this raw data is extended with information about data types and the data layout. With this information, one part of the data may, for example, be interpreted as animation data, and another part may be interpreted as geometry data. Storing the data in a binary form allows it to be transferred over the web much more efficiently than in the JSON format, and the binary data can be passed directly to the renderer without having to decode or pre-process it.

这个二进制数据只是从缓冲区的URI读取的原始内存块，没有固有的含义或结构。缓冲区、缓冲区视图和访问器部分将描述如何使用有关数据类型和数据布局的信息扩展此原始数据。例如，使用此信息，可以将数据的一部分解释为动画数据，而另一部分可以解释为几何数据。以二进制形式存储数据可以比以 JSON 格式更有效地通过 Web 传输数据，并且二进制数据可以直接传递到渲染器，而无需对其进行解码或预处理。

## Image data in `images`

图像中的图像数据

An [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image) may refer to an external image file that can be used as the texture of a rendered object:

image可以引用外部图像文件作为渲染对象的纹理：

```javascript
"image01": {
    "uri": "image01.png"
}
```

The reference is given as a URI that usually points to a PNG or JPG file. These formats significantly reduce the size of the files so that they may efficiently be transferred over the web. In some cases, the `image` objects may not refer to an external file, but to data that is stored in a `buffer`. The details of this indirection will be explained in the [Textures, Images, and Samplers](gltfTutorial_012_TexturesImagesSamplers.md) section.

引用通常是指向 PNG 或 JPG 格式文件的 URI 。这些格式减小了文件的大小，以便有效地通过 Web 传输。图像对象也可以不引用外部文件，而引用存储在缓冲区中的数据。间接寻址的详细信息在纹理、图像和采样器部分中进行描述。

## Binary data in data URIs

数据 URI 中的二进制数据

Usually, the URIs that are contained in the `buffer` and `image` objects will point to a file that contains the actual data. As an alternative, the data may be *embedded* into the JSON, in binary format, by using a [data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs).

通常，缓冲区和图像对象的 URI 会指向数据文件。有时 URI 也会直接描述二进制格式文件。

Previous: [Introduction](gltfTutorial_001_Introduction.md) | [Table of Contents](README.md) | Next: [A Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md)
