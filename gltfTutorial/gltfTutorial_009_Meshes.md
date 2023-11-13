Previous: [Simple Meshes](gltfTutorial_008_SimpleMeshes.md) | [Table of Contents](README.md) | Next: [Materials](gltfTutorial_010_Materials.md)

# Meshes

The [Simple Meshes](gltfTutorial_008_SimpleMeshes.md) example from the previous section showed a basic example of a [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh) with a [`mesh.primitive`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh-primitive) object that contained several attributes. This section will explain the meaning and usage of mesh primitives, how meshes may be attached to nodes of the scene graph, and how they can be rendered with different materials.

上一节中的简单网格示例显示了具有包含多个属性的 mesh.primitive 对象的网格的基本示例。本节将解释网格基元的含义和用法，网格如何附加到场景图的节点，以及如何使用不同的材质渲染它们。

## Mesh primitives

网格基元

Each `mesh` contains an array of `mesh.primitive` objects. These mesh primitive objects are smaller parts or building blocks of a larger object. A mesh primitive summarizes all information about how the respective part of the object will be rendered.

每个网格都包含一个网格基元对象数组。这些网格基元对象是较大对象的较小部分或构建块。网格基元汇总了有关如何渲染对象相应部分的所有信息。

### Mesh primitive attributes

网格基元属性

A mesh primitive defines the geometry data of the object using its `attributes` dictionary. This geometry data is given by references to `accessor` objects that contain the data of vertex attributes. The details of the `accessor` concept are explained in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section.

网格基元使用其属性字典定义对象的几何数据。此几何数据由对包含顶点属性数据的访问器对象的引用给出。有关访问器概念的详细信息，请参阅缓冲区、缓冲区视图和访问器部分。

In the given example, there are two entries in the `attributes` dictionary. The entries refer to the `positionsAccessor` and the `normalsAccessor`:

在给定的示例中，属性字典中有两个条目。这些条目引用了位置访问器和法线访问器：

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

Together, the elements of these accessors define the attributes that belong to the individual vertices, as shown in Image 9a.

这些存取器的元素共同定义了属于各个顶点的属性，如图 9a 所示。

<p align="center">
<img src="images/meshPrimitiveAttributes.png" /><br>
<a name="meshPrimitiveAttributes-png"></a>Image 9a: Mesh primitive accessors containing the data of vertices.
</p>

### Indexed and non-indexed geometry

索引和非索引几何图形

The geometry data of a `mesh.primitive` may be either *indexed* geometry or geometry without indices. In the given example, the `mesh.primitive` contains *indexed* geometry. This is indicated by the `indices` property, which refers to the accessor with index 0, defining the data for the indices. For non-indexed geometry, this property is omitted.

mesh.primitive 的几何数据可以是索引几何，也可以是没有索引的几何。在给定的示例中，mesh.primitive 包含索引几何体。这由 indices 属性指示，该属性引用具有索引 0 的访问器，用于定义索引的数据。对于非索引几何图形，将省略此属性。

### Mesh primitive mode

网格基元模式

By default, the geometry data is assumed to describe a triangle mesh. For the case of *indexed* geometry, this means that three consecutive elements of the `indices` accessor are assumed to contain the indices of a single triangle. For non-indexed geometry, three elements of the vertex attribute accessors are assumed to contain the attributes of the three vertices of a triangle.

默认情况下，假定几何数据描述三角形网格。对于索引几何的情况，这意味着假定索引存取器的三个连续元素包含单个三角形的索引。对于非索引几何，假定顶点属性访问器的三个元素包含三角形的三个顶点的属性。

Other rendering modes are possible: the geometry data may also describe individual points, lines, or triangle strips. This is indicated by the `mode` that may be stored in the mesh primitive. Its value is a constant that indicates how the geometry data has to be interpreted. The mode may, for example, be `0` when the geometry consists of points, or `4` when it consists of triangles. These constants correspond to the GL constants `POINTS` or `TRIANGLES`, respectively. See the [`primitive.mode` specification](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#_mesh_primitive_mode) for a list of available modes.

其他渲染模式也是可能的：几何数据还可以描述单个点、线或三角形条带。这由可能存储在网格基元中的模式指示。其值是一个常量，指示必须如何解释几何数据。例如，当几何图形由点组成时，模式可能为 0，当几何图形由三角形组成时，模式可能为 4。这些常量分别对应于 GL 常量点或三角形。有关可用模式的列表，请参阅 primitive.mode 规范。

### Mesh primitive material

网格基元材质

The mesh primitive may also refer to the `material` that should be used for rendering, using the index of this material. In the given example, no `material` is defined, causing the objects to be rendered with a default material that just defines the objects to have a uniform 50% gray color. A detailed explanation of materials and the related concepts will be given in the [Materials](gltfTutorial_010_Materials.md) section.

网格基元也可以引用应该用于渲染的材质，使用该材质的索引。在给定的示例中，未定义任何材质，导致使用默认材质渲染对象，该材质仅定义对象具有统一的 50% 灰色。材料和相关概念的详细说明将在材料部分给出。

## Meshes attached to nodes

附加到节点的网格

In the example from the [Simple Meshes](gltfTutorial_008_SimpleMeshes.md) section, there is a single `scene`, which contains two nodes, and both nodes refer to the same `mesh` instance, which has the index 0:

在“简单网格”部分中的示例中，有一个场景，其中包含两个节点，并且两个节点都引用索引为 0 的同一网格实例：

```javascript
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
    { ... }
  ],
```

The second node has a `translation` property. As shown in the [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) section, this will be used to compute the local transform matrix of this node. In this case, the matrix will cause a translation of 1.0 along the x-axis. The product of all local transforms of the nodes will yield the [global transform](gltfTutorial_004_ScenesNodes.md#global-transforms-of-nodes). And all elements that are attached to the nodes will be rendered with this global transform.

第二个节点具有转换属性。如场景和节点部分所示，这将用于计算此节点的局部变换矩阵。在这种情况下，矩阵将导致沿 x 轴平移 1.0。节点的所有本地转换的乘积将产生全局转换。附加到节点的所有元素都将使用此全局转换进行渲染。

So in this example, the mesh will be rendered twice because it is attached to two nodes: once with the global transform of the first node, which is the identity transform, and once with the global transform of the second node, which is a translation of 1.0 along the x-axis.

因此，在此示例中，网格将呈现两次，因为它附加到两个节点：一次是第一个节点的全局转换，这是标识转换，一次是第二个节点的全局转换，这是沿 x 轴的 1.0 平移。



Previous: [Simple Meshes](gltfTutorial_008_SimpleMeshes.md) | [Table of Contents](README.md) | Next: [Materials](gltfTutorial_010_Materials.md)
