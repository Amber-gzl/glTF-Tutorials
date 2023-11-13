Previous: [Cameras](gltfTutorial_016_Cameras.md) | [Table of Contents](README.md) | Next: [Morph Targets](gltfTutorial_018_MorphTargets.md)

# A Simple Morph Target

一个简单的变形目标

Starting with version 2.0, glTF supports the definition of *morph targets* for meshes. A morph target stores displacements or differences for certain mesh attributes. At runtime, these differences may be added to the original mesh, with different weights, in order to animate parts of the mesh. This is often used in character animations, for example, to encode different facial expressions of a virtual character.

从版本 2.0 开始，glTF 支持定义网格的变形目标。变形目标存储某些网格属性的位移或差异。在运行时，可以将这些差异添加到具有不同权重的原始网格中，以便对网格的各个部分进行动画处理。这通常用于角色动画，例如，对虚拟角色的不同面部表情进行编码。

The following is a minimal example that shows a mesh with two morph targets. The new elements will be summarized here, and the broader concept of morph targets and how they are applied at runtime will be explained in the next section.

下面是一个最小示例，显示了具有两个变形目标的网格。此处将总结新元素，下一节将解释 morph 目标的更广泛概念以及如何在运行时应用它们。

```javascript
{
  "scene": 0,
  "scenes":[
    {
      "nodes":[
        0
      ]
    }
  ],
  "nodes":[
    {
      "mesh":0
    }
  ],
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

  "animations":[
    {
      "samplers":[
        {
          "input":4,
          "interpolation":"LINEAR",
          "output":5
        }
      ],
      "channels":[
        {
          "sampler":0,
          "target":{
            "node":0,
            "path":"weights"
          }
        }
      ]
    }
  ],

  "buffers":[
    {
      "uri":"data:application/gltf-buffer;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAA/AAAAPwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIC/AACAPwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIA/AACAPwAAAAA=",
      "byteLength":116
    },
    {
      "uri":"data:application/gltf-buffer;base64,AAAAAAAAgD8AAABAAABAQAAAgEAAAAAAAAAAAAAAAAAAAIA/AACAPwAAgD8AAIA/AAAAAAAAAAAAAAAA",
      "byteLength":60
    }
  ],
  "bufferViews":[
    {
      "buffer":0,
      "byteOffset":0,
      "byteLength":6,
      "target":34963
    },
    {
      "buffer":0,
      "byteOffset":8,
      "byteLength":108,
      "byteStride":12,
      "target":34962
    },
    {
      "buffer":1,
      "byteOffset":0,
      "byteLength":20
    },
    {
      "buffer":1,
      "byteOffset":20,
      "byteLength":40
    }
  ],
  "accessors":[
    {
      "bufferView":0,
      "byteOffset":0,
      "componentType":5123,
      "count":3,
      "type":"SCALAR",
      "max":[
        2
      ],
      "min":[
        0
      ]
    },
    {
      "bufferView":1,
      "byteOffset":0,
      "componentType":5126,
      "count":3,
      "type":"VEC3",
      "max":[
        1.0,
        0.5,
        0.0
      ],
      "min":[
        0.0,
        0.0,
        0.0
      ]
    },
    {
      "bufferView":1,
      "byteOffset":36,
      "componentType":5126,
      "count":3,
      "type":"VEC3",
      "max":[
        0.0,
        1.0,
        0.0
      ],
      "min":[
        -1.0,
        0.0,
        0.0
      ]
    },
    {
      "bufferView":1,
      "byteOffset":72,
      "componentType":5126,
      "count":3,
      "type":"VEC3",
      "max":[
        1.0,
        1.0,
        0.0
      ],
      "min":[
        0.0,
        0.0,
        0.0
      ]
    },
    {
      "bufferView":2,
      "byteOffset":0,
      "componentType":5126,
      "count":5,
      "type":"SCALAR",
      "max":[
        4.0
      ],
      "min":[
        0.0
      ]
    },
    {
      "bufferView":3,
      "byteOffset":0,
      "componentType":5126,
      "count":10,
      "type":"SCALAR",
      "max":[
        1.0
      ],
      "min":[
        0.0
      ]
    }
  ],

  "asset":{
    "version":"2.0"
  }
}

```

The asset contains an animation that interpolates between the different morph targets for a single triangle. A screenshot of this asset is shown in Image 17a.

该资产包含一个动画，用于在单个三角形的不同变形目标之间进行插值。此资产的屏幕截图如图 17a 所示。

<p align="center">
<img src="images/simpleMorph.png" /><br>
<a name="simpleMorph-png"></a>Image 17a: A triangle with two morph targets.
</p>


Most of the elements of this asset have already been explained in the previous sections: It contains a `scene` with a single `node` and a single `mesh`. There are two `buffer` objects, one storing the geometry data and one storing the data for the `animation`, and several `bufferView` and `accessor` objects that provide access to this data.

此资产的大多数元素已在前面的部分中进行了说明： 它包含一个具有单个节点和单个网格的场景。有两个缓冲区对象，一个存储几何数据，一个存储动画数据，以及多个提供对此数据的访问的 bufferView 和访问器对象。

The new elements that have been added in order to define the morph targets are contained in the `mesh` and the `animation`:

为了定义变形目标而添加的新元素包含在网格和动画中：


```javascript
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
        0.5,
        0.5
      ]
    }
  ],

```

The `mesh.primitive` contains an array of [morph `targets`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#_mesh_primitive_targets). Each morph target is a dictionary that maps attribute names to `accessor` objects. In the example, there are two morph targets, both mapping the `"POSITION"` attribute to accessors that contain the morphed vertex positions. The mesh also contains an array of `weights` that defines the contribution of each morph target to the final, rendered mesh. These weights are also the `channel.target` of the `animation` that is contained in the asset:

mesh.primitive 包含一个变形目标数组。每个形态目标都是一个字典，用于将属性名称映射到访问器对象。在此示例中，有两个变形目标，它们都将“POSITION”属性映射到包含变形顶点位置的访问器。网格还包含一个权重数组，用于定义每个变形目标对最终渲染网格的贡献。这些权重也是资产中包含的动画的 channel.target：

```javascript
  "animations":[
    {
      "samplers":[
        {
          "input":4,
          "interpolation":"LINEAR",
          "output":5
        }
      ],
      "channels":[
        {
          "sampler":0,
          "target":{
            "node":0,
            "path":"weights"
          }
        }
      ]
    }
  ],

```

This means that the animation will modify the `weights` of the mesh that is referred to by the `target.node`. The result of applying the animation to these weights, and the computation of the final, rendered mesh will be explained in more detail in the next section about [Morph Targets](gltfTutorial_018_MorphTargets.md).

这意味着动画将修改 target.node 引用的网格的权重。将动画应用于这些权重的结果以及最终渲染网格的计算将在下一节关于变形目标的部分中更详细地解释。



Previous: [Cameras](gltfTutorial_016_Cameras.md) | [Table of Contents](README.md) | Next: [Morph Targets](gltfTutorial_018_MorphTargets.md)
