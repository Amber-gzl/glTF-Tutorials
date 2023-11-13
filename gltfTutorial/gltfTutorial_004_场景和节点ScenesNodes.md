Previous: [A Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md) | [Table of Contents](README.md) | Next: [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md)

# Scenes and Nodes

场景和节点

## Scenes

场景

There may be multiple scenes stored in one glTF file. The `scene` property indicated which of these scenes should be the default scene that is displayed when the asset is loaded. Each scene contains an array of `nodes`, which are the indices of the root nodes of the scene graphs. Again, there may be multiple root nodes, forming different hierarchies, but in many cases, the scene will have a single root node. The most simple possible scene description has already been shown in the previous section, consisting of a single scene with a single node:

一个glTF文件中可能存储了多个场景。scene 属性定义哪个场景是加载资产时显示的默认场景。每个场景都包含一个nodes数组，这些节点是场景图根节点的索引。同样，可能有多个根节点，形成不同的层次结构，但在许多情况下，场景将具有单个根节点。最简单的场景描述已经在上一节中显示过，它由单个场景和单个节点组成：

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

## Nodes forming the scene graph

构成场景图的nodes

Each [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node) can contain an array called `children` that contains the indices of its child nodes. So each node is one element of a hierarchy of nodes, and together they define the structure of the scene as a scene graph.  

每个node可以包含一个名为 child 的数组，该数组包含其子节点的索引。因此，每个node都是nodes层次结构中的一个元素，它们一起将场景的结构定义为场景图。

<p align="center">
<img src="images/sceneGraph.png" /><br>
<a name="sceneGraph-png"></a>Image 4a: The scene graph representation stored in the glTF JSON.
</p>

Each of the nodes that are given in the `scene` can be traversed, recursively visiting all their children, to process all elements that are attached to the nodes. The simplified pseudocode for this traversal may look like the following:

可以遍历场景中给定的每个节点，递归访问其所有子节点，以处理附加到节点的所有元素。此遍历的简化伪代码可能如下所示：

```
traverse(node) {
    // Process the meshes, cameras, etc., that are
    // attached to this node - discussed later
    processElements(node);

    // Recursively process all children
    for each (child in node.children) {
        traverse(child);
    }
}
```

In practice, some additional information will be required for the traversal: the processing of some elements that are attached to nodes will require information about *which* node they are attached to. Additionally, the information about the transforms of the nodes has to be accumulated during the traversal.

在实践中，遍历将需要一些附加信息：处理附加到节点的某些元素将需要有关它们附加到哪个节点的信息。此外，有关节点转换的信息必须在遍历期间了解。

### Local and global transforms

本地和全局转换

Each node can have a transform. Such a transform will define a translation, rotation, and/or scale. This transform will be applied to all elements attached to the node itself and to all its child nodes. The hierarchy of nodes thus allows one to structure the translations, rotations, and scalings that are applied to the scene elements.

每个节点都可以有一个转换。此类转换将定义平移、旋转和/或缩放。此转换将应用于附加到节点本身的所有元素及其所有子节点。因此，节点的层次结构允许人们构建应用于场景元素的平移、旋转和缩放。

#### Local transforms of nodes

节点的本地转换

There are different possible representations for the local transform of a node. The transform can be given directly by the `matrix` property of the node. This is an array of 16 floating point numbers that describe the matrix in column-major order. For example, the following matrix describes a scaling about (2,1,0.5), a rotation about 30 degrees around the x-axis, and a translation about (10,20,30):

节点的局部转换有不同的可能表示形式。转换可以直接由节点的“矩阵”属性给出。这是一个由 16 个浮点数组成的数组，按列主顺序描述矩阵。例如，以下矩阵描述了大约 （2,1,0.5） 的缩放、围绕 x 轴约 30 度的旋转以及大约 （10,20,30） 的平移：

```javascript
"node0": {
    "matrix": [
        2.0,    0.0,    0.0,    0.0,
        0.0,    0.866,  0.5,    0.0,
        0.0,   -0.25,   0.433,  0.0,
       10.0,   20.0,   30.0,    1.0
    ]
}
```

The matrix defined here is as shown in Image 4b.

此处定义的矩阵如图4b所示。

<p align="center">
<img src="images/matrix.png" /><br>
<a name="matrix-png"></a>Image 4b: An example matrix.
</p>

The transform of a node can also be given using the `translation`, `rotation`, and `scale` properties of a node, which is sometimes abbreviated as *TRS*:  

节点的转换也可以使用节点的“平移”、“旋转”和“缩放”属性给出，有时缩写为 *TRS*：

```javascript
"node0": {
    "translation": [ 10.0, 20.0, 30.0 ],
    "rotation": [ 0.259, 0.0, 0.0, 0.966 ],
    "scale": [ 2.0, 1.0, 0.5 ]
}
```

Each of these properties can be used to create a matrix, and the product of these matrices then is the local transform of the node:

这些属性中的每一个都可用于创建矩阵，然后这些矩阵的乘积是节点的局部变换：

- The `translation` just contains the translation in x-, y-, and z-direction. For example, from a translation of `[ 10.0, 20.0, 30.0 ]`, one can create a translation matrix that contains this translation as its last column, as shown in Image 4c.
- “平移”只包含 x、y 和 z 方向的平移。例如，从“[ 10.0， 20.0， 30.0 ]”的变换中，可以创建一个将此变换作为其最后一列的矩阵，如图 4c 所示。

<p align="center">
<img src="images/translationMatrix.png" /><br>
<a name="translationMatrix-png"></a>Image 4c: A translation matrix.
</p>

- The `rotation` is given as a [quaternion](https://en.wikipedia.org/wiki/Quaternion). The mathematical background of quaternions is beyond the scope of this tutorial. For now, the most important information is that a quaternion is a compact representation of a rotation about an arbitrary angle and around an arbitrary axis. It is stored as a tuple `(x,y,z,w)`, where the `w`-component is the cosine of half of the rotation angle. For example, the quaternion `[ 0.259, 0.0, 0.0, 0.966 ]` describes a rotation about 30 degrees, around the x-axis. So this quaternion can be converted into a rotation matrix, as shown in Image 4d.
- rotation以四元数给出。四元数的数学背景超出了本教程的范围。四元数是围绕任意角度和围绕任意轴旋转的表示。它存储为元组 （x，y，z，w），其中 w 分量 = 旋转角度一半的余弦。例如，四元数 [0.259， 0.0， 0.0， 0.966 ] 描述绕 x 轴旋转约 30 度。所以这个四元数可以转换成旋转矩阵，如图4d所示。（0.259是怎么算的？）

<p align="center">
<img src="images/rotationMatrix.png" /><br>
<a name="rotationMatrix-png"></a>Image 4d: A rotation matrix.
</p>

- The `scale` contains the scaling factors along the x-, y-, and z-axes. The corresponding matrix can be created by using these scaling factors as the entries on the diagonal of the matrix. For example, the scale matrix for the scaling factors `[ 2.0, 1.0, 0.5 ]` is shown in Image 4e.
- scale包含沿 x、y 和 z 轴的缩放比例。把缩放比例作为矩阵对角线上的值可创建相应的矩阵。例如，比例因子 [ 2.0， 1.0， 0.5 ] 的比例矩阵如图 4e 所示。

<p align="center">
<img src="images/scaleMatrix.png" /><br>
<a name="scaleMatrix-png"></a>Image 4e: A scale matrix.
</p>

When computing the final, local transform matrix of the node, these matrices are multiplied together. It is important to perform the multiplication of these matrices in the right order. The local transform matrix always has to be computed as `M = T * R * S`, where `T` is the matrix for the `translation` part, `R` is the matrix for the `rotation` part, and `S` is the matrix for the `scale` part. So the pseudocode for the computation is

在计算节点的最终局部变换矩阵时，这些矩阵相乘。以正确的顺序执行这些矩阵的乘法非常重要。局部变换矩阵始终必须计算为 M = T *R* S，其中 T 是平移部分的矩阵，R 是旋转部分的矩阵，S 是比例部分的矩阵。所以计算的伪代码是

```
translationMatrix = createTranslationMatrix(node.translation);
rotationMatrix = createRotationMatrix(node.rotation);
scaleMatrix = createScaleMatrix(node.scale);
localTransform = translationMatrix * rotationMatrix * scaleMatrix;
```

For the example matrices given above, the final, local transform matrix of the node will be as shown in Image 4f.

对于上面给出的示例矩阵，节点的最终局部变换矩阵如图 4f 所示。

<p align="center">
<img src="images/productMatrix.png" /><br>
<a name="produtMatrix-png"></a>Image 4f: The final local transform matrix computed from the TRS properties.
</p>

This matrix will cause the vertices of the meshes to be scaled, then rotated, and then translated according to the `scale`, `rotation`, and `translation` properties that have been given in the node.

此矩阵将导致网格的顶点缩放，然后旋转，然后根据节点中给定的比例、旋转和平移属性进行平移。

When any of the three properties is not given, the identity matrix will be used. Similarly, when a node contains neither a `matrix` property nor TRS-properties, then its local transform will be the identity matrix.

如果未给出这三个属性中的任何一个，则将使用单位矩阵。同样，当节点既不包含矩阵属性也不包含 TRS 属性时，其局部转换将是单位矩阵。（不变）

#### Global transforms of nodes

节点的全局转换

Regardless of the representation in the JSON file, the local transform of a node can be stored as a 4&times;4 matrix. The *global* transform of a node is given by the product of all local transforms on the path from the root to the respective node:

无论 JSON 文件中的表示形式如何，节点的本地转换都可以存储为 4×4 矩阵。节点的全局转换由从根到相应节点的路径上所有本地转换的乘积给出：

    Structure:           local transform      global transform
    root                 R                    R
     +- nodeA            A                    R*A
         +- nodeB        B                    R*A*B
         +- nodeC        C                    R*A*C

It is important to point out that after the file was loaded these global transforms can *not* be computed only once. Later, it will be shown how *animations* may modify the local transforms of individual nodes. And these modifications will affect the global transforms of all descendant nodes. Therefore, when the global transform of a node is required, it has to be computed directly from the current local transforms of all nodes. Alternatively, and as a potential performance improvement, an implementation could cache the global transforms, detect changes in the local transforms of ancestor nodes, and update the global transforms only when necessary. The different implementation options for this will depend on the programming language and the requirements for the client application, and thus are beyond the scope of this tutorial.

需要指出的是，在加载文件后，这些全局转换不能只计算一次。稍后，将展示动画如何修改单个节点的本地转换。这些修改将影响所有后代节点的全局转换。因此，当需要节点的全局变换时，必须直接从所有节点的当前局部变换中计算出来。或者，作为潜在的性能改进，实现可以缓存全局转换，检测祖先节点的本地转换中的更改，并仅在必要时更新全局转换。不同的实现选项将取决于编程语言和客户端应用程序的要求，因此超出了本教程的范围。

Previous: [A Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md) | [Table of Contents](README.md) | Next: [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md)
