Previous: [Simple Morph Target](gltfTutorial_017_SimpleMorphTarget.md) | [Table of Contents](README.md) | Next: [SimpleSkin](gltfTutorial_019_SimpleSkin.md)

# Morph Targets

变形目标

The example in the previous section contains a mesh that consists of a single triangle with two morph targets:

上一节中的示例包含一个网格，该网格由具有两个变形目标的单个三角形组成：

```javascript
{
  "meshes":[
    {
      "primitives":[
        {
          "attributes":{
            "POSITION":1
          },
          "targets":[
            {
              "POSITION":2
            },
            {
              "POSITION":3
            }
          ],
          "indices":0
        }
      ],
      "weights":[
        1.0,
        0.5
      ]
    }
  ],
```


The actual base geometry of the mesh, namely the triangle geometry, is defined by the `mesh.primitive` attribute called `"POSITIONS"`. The morph targets of the `mesh.primitive` are dictionaries that map the attribute name `"POSITIONS"` to `accessor` objects that contain the *displacements* for each vertex. Image 18a shows the initial triangle geometry in black, and the displacement for the first morph target in red, and the displacement for the second morph target in green.

网格的实际基本几何体，即三角形几何体，由称为“POSITION”的mesh.primitive属性定义。mesh.primitive 的变形目标是将属性名称“POSITION”映射到包含每个顶点位移的访问器对象的字典。图18a以黑色显示初始三角形几何形状，以红色显示第一个变形目标的位移，以绿色显示第二个变形目标的位移。

<p align="center">
<img src="images/simpleMorphInitial.png" /><br>
<a name="simpleMorphInitial-png"></a>Image 18a: The initial triangle and morph target displacements.
</p>

The `weights` of the mesh determine how these morph target displacements are added to the initial geometry in order to obtain the current state of the geometry. The pseudocode for computing the rendered vertex positions for a mesh `primitive` is as follows:

网格的权重决定了如何将这些变形目标位移添加到初始几何体中，以获得几何体的当前状态。用于计算网格基元的渲染顶点位置的伪代码如下：

```
renderedPrimitive.POSITION = primitive.POSITION + 
  weights[0] * primitive.targets[0].POSITION +
  weights[1] * primitive.targets[1].POSITION;
```

This means that the current state of the mesh primitive is computed by taking the initial mesh primitive geometry and adding a linear combination of the morph target displacements, where the `weights` are the factors for the linear combination.

这意味着网格基元的当前状态是通过采用初始网格基元几何形状并添加变形目标位移的线性组合来计算的，其中权重是线性组合的因子。

The asset additionally contains an `animation` that affects the weights for the morph targets. The following table shows the key frames of the animated weights:

该资产还包含一个动画，该动画会影响变形目标的权重。下表显示了动画权重的关键帧：

| Time | Weights   |
|:----:|:---------:|
|  0.0 | 0.0, 0.0  |
|  1.0 | 0.0, 1.0  |
|  2.0 | 1.0, 1.0  |
|  3.0 | 1.0, 0.0  |
|  4.0 | 0.0, 0.0  |


Throughout the animation, the weights are interpolated linearly, and applied to the morph target displacements. At each point, the rendered state of the mesh primitive is updated accordingly. The following is an example of the state that is computed at 1.25 seconds. The weights that are provided by the animation sampler for this animation time are (0.25, 1.0), and they are used for computing the linear combination of the morph target displacements.

在整个动画中，权重以线性方式插值，并应用于变形目标位移。在每个点，网格基元的渲染状态都会相应地更新。下面是在 1.25 秒处计算的状态示例。动画采样器为此动画时间提供的权重为 （0.25， 1.0），它们用于计算变形目标位移的线性组合。

<p align="center">
<img src="images/simpleMorphIntermediate.png" /><br>
<a name="simpleMorphIntermediate-png"></a>Image 18b: An intermediate state of the morph target animation.
</p>




Previous: [Simple Morph Target](gltfTutorial_017_SimpleMorphTarget.md) | [Table of Contents](README.md) | Next: [SimpleSkin](gltfTutorial_019_SimpleSkin.md)







