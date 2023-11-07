Previous: [Simple Skin](gltfTutorial_019_SimpleSkin.md) | [Table of Contents](README.md)


# Skins

皮肤

The process of vertex skinning is a bit complex. It brings together nearly all elements that are contained in a glTF asset. This section will explain the basics of vertex skinning, based on the example in the [Simple Skin](gltfTutorial_019_SimpleSkin.md) section.

顶点蒙皮的过程有点复杂。它汇集了 glTF 资产中包含的几乎所有元素。本节将根据简单外观部分中的示例解释顶点蒙皮的基础知识。


## The geometry data

几何数据

The geometry of the vertex skinning example is an indexed triangle mesh, consisting of 8 triangles and 10 vertices. They form a rectangle in the xy-plane, with a width of 1.0 and a height of 2.0. The bottom center of the rectangle is at the origin (0,0,0). So the positions of the vertices are

顶点蒙皮示例的几何图形是一个索引三角形网格，由 8 个三角形和 10 个顶点组成。它们在 xy 平面中形成一个矩形，宽度为 1.0，高度为 2.0。矩形的底部中心位于原点 （0,0,0）。所以顶点的位置是

    -0.5, 0.0, 0.0,
     0.5, 0.0, 0.0,
    -0.5, 0.5, 0.0,
     0.5, 0.5, 0.0,
    -0.5, 1.0, 0.0,
     0.5, 1.0, 0.0,
    -0.5, 1.5, 0.0,
     0.5, 1.5, 0.0,
    -0.5, 2.0, 0.0,
     0.5, 2.0, 0.0

and the indices of the triangles are

三角形的索引是

    0, 1, 3,
    0, 3, 2,
    2, 3, 5,
    2, 5, 4,
    4, 5, 7,
    4, 7, 6,
    6, 7, 9,
    6, 9, 8,

The raw data is stored in the first `buffer`. The indices and vertex positions are defined by the `bufferView` objects at index 0 and 1, and the corresponding `accessor` objects at index 0 and 1 offer typed access to these buffer views. Image 20a shows this geometry with outline rendering to better show the structure.

原始数据存储在第一个缓冲区中。索引和顶点位置由索引 0 和 1 处的 bufferView 对象定义，索引 0 和 1 处的相应访问器对象提供对这些缓冲区视图的类型化访问。图像 20a 显示了此几何图形，并进行了轮廓渲染，以更好地显示结构。

<p align="center">
<img src="images/simpleSkinOutline01.png" /><br>
<a name="simpleSkinOutline01-png"></a>Image 20a: The geometry for the skinning example, with outline rendering, in its initial configuration.
</p>

This geometry data is contained in the mesh primitive of the only mesh, which is attached to the main node of the scene. The mesh primitive contains additional attributes, namely the `"JOINTS_0"` and `"WEIGHTS_0"` attributes. The purpose of these attributes will be explained below.

此几何数据包含在唯一网格的网格基元中，该网格附加到场景的主节点。网格基元包含其他属性，即“JOINTS_0”和“WEIGHTS_0”属性。下面将解释这些属性的用途。


## The skeleton structure

骨架结构

In the given example, there are two nodes that define the skeleton. They are referred to as "skeleton nodes", or "joint nodes", because they can be imagined as the joints between the bones of the skeleton. The `skin` refers to these nodes, by listing their indices in its `joints` property.

在给定的示例中，有两个节点定义骨架。它们被称为“骨骼节点”或“关节节点”，因为它们可以被想象成骨骼骨骼之间的关节。皮肤通过在其关节属性中列出它们的索引来引用这些节点。


```javascript
  "nodes" : [ 
   ...
   {
    "children" : [ 2 ]
   }, 
   {
    "translation" : [ 0.0, 1.0, 0.0 ],
    "rotation" : [ 0.0, 0.0, 0.0, 1.0 ]
   }
  ],

```

The first joint node is located at the origin, and does not contain any transformations. The second node has a `translation` property, defining a translation about 1.0 along the y-axis, and a `rotation` property that initially describes a rotation about 0 degrees (thus, no rotation at all). This rotation will later be changed by the animation to let the skeleton bend left and right and show the effect of the vertex skinning.

第一个关节节点位于原点，不包含任何变换。第二个节点具有一个平移属性，该属性定义沿 y 轴大约 1.0 的平移，以及一个最初描述大约 0 度的旋转（因此，根本没有旋转）的旋转属性。动画稍后将更改此旋转，以使骨架左右弯曲并显示顶点蒙皮的效果。

## The skin

皮肤


The `skin` is the core element of the vertex skinning. In the example, there is a single skin:


“皮肤”是顶点蒙皮的核心元素。在示例中，有一个外观：

```javascript
  "skins" : [ 
   {
    "inverseBindMatrices" : 4,
    "joints" : [ 1, 2 ]
   }
  ],

```

The skin contains an array called `joints`, which lists the indices of the nodes that define the skeleton hierarchy. Additionally, the skin contains a reference to an accessor in the property `inverseBindMatrices`. This accessor provides one matrix for each joint. Each of these matrices transforms the geometry into the space of the respective joint. This means that each matrix is the *inverse* of the global transform of the respective joint, in its initial configuration. 

皮肤包含一个名为“joints”的数组，该数组列出了定义骨架层次结构的节点的索引。此外，外观还包含对属性“反向绑定矩阵”中访问器的引用。此访问器为每个关节提供一个矩阵。这些矩阵中的每一个都将几何形状转换为相应关节的空间。这意味着每个矩阵在其初始配置中都是相应关节的全局变换的*逆*。

In the given example, joint `0` does not have an explicit transform, meaning that its global transform is the identity matrix. Therefore, the inverse bind matrix of joint `0` is also the identity matrix. 

在给定的示例中，联合“0”没有显式变换，这意味着其全局变换是单位矩阵。因此，关节'0'的逆绑定矩阵也是单位矩阵。

Joint `1` contains a translation about 1.0 along the y-axis. The inverse bind matrix of joint `1` is therefore

关节 '1' 包含沿 y 轴约 1.0 的平移。因此，关节 '1' 的逆结合矩阵为

    1.0   0.0   0.0    0.0   
    0.0   1.0   0.0   -1.0   
    0.0   0.0   1.0    0.0   
    0.0   0.0   0.0    1.0  

This matrix translates the mesh about -1.0 along the y-axis, as shown Image 20b.

该矩阵沿y轴平移网格约-1.0，如图20b所示。

<p align="center">
<img src="images/skinInverseBindMatrix.png" /><br>
<a name="skinInverseBindMatrix-png"></a>Image 20b: The transformation of the geometry with the inverse bind matrix of joint 1.
</p>

This transformation may look counterintuitive at first glance. But the goal of this transformation is to bring the coordinates of the skinned vertices into the same space as each joint.

乍一看，这种转变可能违反直觉。但此转换的目标是将蒙皮顶点的坐标引入与每个关节相同的空间。


## Vertex skinning implementation

顶点蒙皮实现

Users of existing rendering libraries will hardly ever have to manually process the vertex skinning data contained in a glTF asset: the actual skinning computations usually take place in the vertex shader, which is a low-level implementation detail of the respective library. However, knowing how the vertex skinning data is supposed to be processed may help to create proper, valid models with vertex skinning. So this section will give a short summary of how the vertex skinning is applied, using some pseudocode and examples in GLSL.

现有渲染库的用户几乎不必手动处理 glTF 资产中包含的顶点蒙皮数据：实际的蒙皮计算通常在顶点着色器中进行，这是相应库的低级实现细节。但是，了解顶点蒙皮数据的处理方式可能有助于创建具有顶点蒙皮的适当、有效的模型。因此，本节将使用 GLSL 中的一些伪代码和示例，简要总结如何应用顶点蒙皮。

### The joint matrices

联合矩阵

The vertex positions of a skinned mesh are eventually computed by the vertex shader. During these computations, the vertex shader has to take into account the current pose of the skeleton in order to compute the proper vertex position. This information is passed to the vertex shader as an array of matrices, namely as the *joint matrices*. This is an array - that is, a `uniform` variable - that contains one 4&times;4 matrix for each joint of the skeleton. In the shader, these matrices are combined to compute the actual skinning matrix for each vertex:

蒙皮网格的顶点位置最终由顶点着色器计算。在这些计算过程中，顶点着色器必须考虑骨架的当前姿势，以便计算正确的顶点位置。此信息作为矩阵数组（即“联合矩阵”）传递到顶点着色器。这是一个数组 -即“统一”变量 -对于骨架的每个关节包含一个 4×4 矩阵。在着色器中，这些矩阵被组合起来计算每个顶点的实际蒙皮矩阵


```glsl
...
uniform mat4 u_jointMat[2];

...
void main(void)
{
    mat4 skinMat =
        a_weight.x * u_jointMat[int(a_joint.x)] +
        a_weight.y * u_jointMat[int(a_joint.y)] +
        a_weight.z * u_jointMat[int(a_joint.z)] +
        a_weight.w * u_jointMat[int(a_joint.w)];
    ....
}
```

The joint matrix for each joint has to perform the following transformations to the vertices:

每个关节的关节矩阵必须对顶点执行以下转换：

- The vertices have to be transformed with the `inverseBindMatrix` of the joint node, to bring them into the same coordinate space as the joint.
- 顶点必须使用关节节点的逆绑定矩阵进行变换，以使它们进入与关节相同的坐标空间。
- The vertices have to be transformed with the *current* global transform of the joint node. Together with the transformation from the `inverseBindMatrix`, this will cause the vertices to be transformed only based on the current transform of the node, in the coordinate space of the current joint node.
- 顶点必须使用联合节点的当前全局变换进行变换。与逆绑定矩阵的变换一起，这将导致顶点仅基于节点的当前变换在当前联合节点的坐标空间中进行变换。

So the pseudocode for computing the joint matrix of joint `j` may look as follows:

所以计算关节j的联合矩阵的伪代码可能如下所示：

    jointMatrix(j) =
      globalTransformOfJointNode(j) *
      inverseBindMatrixForJoint(j);
      
Note: Vertex skinning in other contexts often involves a matrix that is called "Bind Shape Matrix". This matrix is supposed to transform the geometry of the skinned mesh into the coordinate space of the joints. In glTF, this matrix is omitted, and it is assumed that this transform is either premultiplied with the mesh data, or postmultiplied to the inverse bind matrices. 

注意：在其他上下文中，顶点蒙皮通常涉及称为“绑定形状矩阵”的矩阵。该矩阵应该将蒙皮网格的几何形状转换为关节的坐标空间。在glTF中，省略了此矩阵，并假设此变换要么与网格数据预乘，要么后乘到逆绑定矩阵。

Image 20c shows the transformations that are done to the geometry in the [Simple Skin](gltfTutorial_019_SimpleSkin.md) example, using the joint matrix of joint 1. The image shows the transformation for an intermediate state of the animation, namely, when the rotation of the joint node has already been modified by the animation, to describe a rotation about 45 degrees around the z-axis.

注意：在其他上下文中，顶点蒙皮通常涉及称为“绑定形状矩阵”的矩阵。该矩阵应该将蒙皮网格的几何形状转换为关节的坐标空间。在glTF中，省略了此矩阵，并假设此变换要么与网格数据预乘，要么后乘到逆绑定矩阵。

<p align="center">
<img src="images/skinJointMatrices.png" /><br>
<a name="skinJointMatrices-png"></a>Image 20c: The transformation of the geometry done for joint 1.
</p>

The last panel of Image 20c shows how the geometry would look like if it were *only* transformed with the joint matrix of joint 1. This state of the geometry is never really visible: The *actual* geometry that is computed in the vertex shader will *combine* the geometries as they are created from the different joint matrices, based on the joints- and weights that are explained below.

图像 20c 的最后一个面板显示了如果*仅*使用关节 1 的联合矩阵进行变换，几何形状的外观。几何体的这种状态永远不会真正可见：在顶点着色器中计算的*实际*几何体将根据下面解释的关节和权重，*组合*从不同的关节矩阵创建的几何体。


### The skinning joints and weights

蒙皮关节和配重

As mentioned above, the mesh primitive contains new attributes that are required for the vertex skinning. Particularly, these are the `"JOINTS_0"` and the `"WEIGHTS_0"` attributes. Each attribute refers to an accessor that provides one data element for each vertex of the mesh.

如上所述，网格基元包含顶点蒙皮所需的新属性。特别是，这些是“JOINTS_0”和“WEIGHTS_0”属性。每个属性都引用一个访问器，该访问器为网格的每个顶点提供一个数据元素。

The `"JOINTS_0"` attribute refers to an accessor that contains the indices of the joints that should have an influence on the vertex during the skinning process. For simplicity and efficiency, these indices are usually stored as 4D vectors, limiting the number of joints that may influence a vertex to 4. In the given example, the joints information is very simple:

“JOINTS_0”属性是指一个访问器，其中包含在蒙皮过程中应该对顶点产生影响的关节索引。为了简单和高效，这些索引通常存储为 4D 向量，将可能影响顶点的关节数量限制为 4。在给定的示例中，关节信息非常简单：

    Vertex 0:  0, 0, 0, 0,
    Vertex 1:  0, 0, 0, 0,
    Vertex 2:  0, 1, 0, 0,
    Vertex 3:  0, 1, 0, 0,
    Vertex 4:  0, 1, 0, 0,
    Vertex 5:  0, 1, 0, 0,
    Vertex 6:  0, 1, 0, 0,
    Vertex 7:  0, 1, 0, 0,
    Vertex 8:  0, 1, 0, 0,
    Vertex 9:  0, 1, 0, 0,

This means that every vertex may be influenced by joint 0 and joint 1, except the first two vertices are influenced only by joint 0, and the last two vertices are influenced only by joint 1. The last 2 components of each vector are ignored here. If there were multiple joints, then one entry of this accessor could, for example, contain

这意味着每个顶点都可能受到关节 0 和关节 1 的影响，除了前两个顶点仅受关节 0 的影响，最后两个顶点仅受关节 1 的影响。此处忽略每个向量的最后 2 个分量。如果有多个关节，则此访问器的一个条目可以包含，例如，包含

    3, 1, 8, 4,

meaning that the corresponding vertex should be influenced by the joints 3, 1, 8, and 4.

这意味着相应的顶点应受关节 3、1、8 和 4 的影响。

The `"WEIGHTS_0"` attribute refers to an accessor that provides information about how strongly each joint should influence each vertex. In the given example, the weights are as follows:

“WEIGHTS_0”属性是指一个访问器，该访问器提供有关每个关节对每个顶点的影响程度的信息。在给定的示例中，权重如下：

    Vertex 0:  1.00,  0.00,  0.0, 0.0,
    Vertex 1:  1.00,  0.00,  0.0, 0.0,
    Vertex 2:  0.75,  0.25,  0.0, 0.0,
    Vertex 3:  0.75,  0.25,  0.0, 0.0,
    Vertex 4:  0.50,  0.50,  0.0, 0.0,
    Vertex 5:  0.50,  0.50,  0.0, 0.0,
    Vertex 6:  0.25,  0.75,  0.0, 0.0,
    Vertex 7:  0.25,  0.75,  0.0, 0.0,
    Vertex 8:  0.00,  1.00,  0.0, 0.0,
    Vertex 9:  0.00,  1.00,  0.0, 0.0,

Again, the last two components of each entry are not relevant, because there are only two joints.

同样，每个条目的最后两个组件不相关，因为只有两个关节。

Combining the `"JOINTS_0"` and `"WEIGHTS_0"` attributes yields exact information about the influence that each joint has on each vertex. For example, the given data means that vertex 6 should be influenced to 25% by joint 0 and to 75% by joint 1.

组合“JOINTS_0”和“WEIGHTS_0”属性可生成有关每个关节对每个顶点影响的确切信息。例如，给定的数据意味着顶点 6 应受关节 0 影响 25%，受关节 1 影响至 75%。

In the vertex shader, this information is used to create a linear combination of the joint matrices. This matrix is called the *skin matrix* of the respective vertex. Therefore, the data of the `"JOINTS_0"` and `"WEIGHTS_0"` attributes are passed to the shader. In this example, they are given as the `a_joint` and `a_weight` attribute variable, respectively:

在顶点着色器中，此信息用于创建联合矩阵的线性组合。该矩阵称为相应顶点的*皮肤矩阵*。因此，“JOINTS_0”和“WEIGHTS_0”属性的数据将传递给着色器。在此示例中，它们分别作为“a_joint”和“a_weight”属性变量给出：

```glsl
...
attribute vec4 a_joint;
attribute vec4 a_weight;

uniform mat4 u_jointMat[2];

...
void main(void)
{
    mat4 skinMat =
        a_weight.x * u_jointMat[int(a_joint.x)] +
        a_weight.y * u_jointMat[int(a_joint.y)] +
        a_weight.z * u_jointMat[int(a_joint.z)] +
        a_weight.w * u_jointMat[int(a_joint.w)];
    vec4 worldPosition = skinMat * vec4(a_position,1.0);
    vec4 cameraPosition = u_viewMatrix * worldPosition;
    gl_Position = u_projectionMatrix * cameraPosition;
}
```
The skin matrix is then used to transform the original position of the vertex into the world space. The transform of the node that the skin is attached to is ignored. The result of this transformation can be imagined as a weighted transformation of the vertices with the respective joint matrices, as shown in Image 20d.

然后使用皮肤矩阵将顶点的原始位置转换为世界空间。将忽略外观附加到的节点的转换。这种变换的结果可以想象为顶点与各个联合矩阵的加权变换，如图 20d 所示。

<p align="center">
<img src="images/skinSkinMatrix.png" /><br>
<a name="skinSkinMatrix-png"></a>Image 20d: Computation of the skin matrix.
</p>

The result of applying this skin matrix to the vertices for the given example is shown in Image 20e.

将此皮肤矩阵应用于给定示例的顶点的结果如图 20e 所示。

<p align="center">
<img src="images/simpleSkinOutline02.png" /><br>
<a name="simpleSkinOutline02-png"></a>Image 20e: The geometry for the skinning example, with outline rendering, during the animation.
</p>







Previous: [Simple Skin](gltfTutorial_019_SimpleSkin.md) | [Table of Contents](README.md)
