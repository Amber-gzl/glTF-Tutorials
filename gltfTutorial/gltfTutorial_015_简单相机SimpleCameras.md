Previous: [Advanced Material](gltfTutorial_014_AdvancedMaterial.md) | [Table of Contents](README.md) | Next: [Cameras](gltfTutorial_016_Cameras.md)

# Simple Cameras

简单的相机

The previous sections showed how a basic scene structure with geometric objects is represented in a glTF asset, and how different materials can be applied to these objects. This did not yet include information about the view configuration that should be used for rendering the scene. This view configuration is usually described as a virtual *camera* that is contained in the scene, at a certain position, and pointing in a certain direction.

前面的部分介绍了如何在 glTF 资产中表示具有几何对象的基本场景结构，以及如何将不同的材质应用于这些对象。这尚未包括有关应用于渲染场景的视图配置的信息。此视图配置通常被描述为包含在场景中、位于特定位置并指向特定方向的虚拟摄像机。

The following is a simple, complete glTF asset. It is similar to the assets that have already been shown: it defines a simple `scene` containing `node` objects and a single geometric object that is given as a `mesh`, attached to one of the nodes. But this asset additionally contains two [`camera`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera) objects:

下面是一个简单、完整的 glTF 资产。它类似于已经显示的资产：它定义了一个简单的场景，其中包含节点对象和单个几何对象，该对象以网格形式给出，附加到其中一个节点。但此资产还包含两个摄像机对象：

```javascript
{
  "scene": 0,
  "scenes" : [
    {
      "nodes" : [ 0, 1, 2 ]
    }
  ],
  "nodes" : [
    {
      "rotation" : [ -0.383, 0.0, 0.0, 0.924 ],
      "mesh" : 0
    },
    {
      "translation" : [ 0.5, 0.5, 3.0 ],
      "camera" : 0
    },
    {
      "translation" : [ 0.5, 0.5, 3.0 ],
      "camera" : 1
    }
  ],

  "cameras" : [
    {
      "type": "perspective",
      "perspective": {
        "aspectRatio": 1.0,
        "yfov": 0.7,
        "zfar": 100,
        "znear": 0.01
      }
    },
    {
      "type": "orthographic",
      "orthographic": {
        "xmag": 1.0,
        "ymag": 1.0,
        "zfar": 100,
        "znear": 0.01
      }
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
      "uri" : "data:application/octet-stream;base64,AAABAAIAAQADAAIAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAACAPwAAgD8AAAAA",
      "byteLength" : 60
    }
  ],
  "bufferViews" : [
    {
      "buffer" : 0,
      "byteOffset" : 0,
      "byteLength" : 12,
      "target" : 34963
    },
    {
      "buffer" : 0,
      "byteOffset" : 12,
      "byteLength" : 48,
      "target" : 34962
    }
  ],
  "accessors" : [
    {
      "bufferView" : 0,
      "byteOffset" : 0,
      "componentType" : 5123,
      "count" : 6,
      "type" : "SCALAR",
      "max" : [ 3 ],
      "min" : [ 0 ]
    },
    {
      "bufferView" : 1,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 4,
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

The geometry in this asset is a simple unit square. It is rotated by -45 degrees around the x-axis, to emphasize the effect of the different cameras. Image 15a shows three options for rendering this asset. The first examples use the cameras from the asset. The last example shows how the scene looks from an external, user-defined viewpoint.

此资产中的几何图形是一个简单的单位平方。它绕x轴旋转-45度，以强调不同相机的效果。图 15a 显示了用于渲染此资产的三个选项。第一个示例使用资产中的摄像机。最后一个示例显示了从用户定义的外部视点看场景的外观。
<p align="center">
<img src="images/cameras.png" /><br>
<a name="cameras-png"></a>Image 15a: The effect of rendering the scene with different cameras.
</p>

## Camera definitions

相机定义

The new top-level element of this glTF asset is the `cameras` array, which contains the  [`camera`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera) objects:

此 glTF 资产的新顶级元素是摄像机数组，其中包含摄像机对象：

```javascript
"cameras" : [
  {
    "type": "perspective",
    "perspective": {
      "aspectRatio": 1.0,
      "yfov": 0.7,
      "zfar": 100,
      "znear": 0.01
    }
  },
  {
    "type": "orthographic",
    "orthographic": {
      "xmag": 1.0,
      "ymag": 1.0,
      "zfar": 100,
      "znear": 0.01
    }
  }
],
```

When a camera object has been defined, it may be attached to a `node`. This is accomplished by assigning the index of the camera to the `camera` property of a node. In the given example, two new nodes have been added to the scene graph, one for each camera:

定义相机对象后，可以将其附加到节点。这是通过将相机的索引分配给节点的相机属性来实现的。在给定的示例中，场景图中添加了两个新节点，每个摄像机一个节点：

```javascript
"nodes" : {
  ...
  {
    "translation" : [ 0.5, 0.5, 3.0 ],
    "camera" : 0
  },
  {
    "translation" : [ 0.5, 0.5, 3.0 ],
    "camera" : 1
  }
},
```

The differences between perspective and orthographic cameras and their properties, the effect of attaching the cameras to the nodes, and the management of multiple cameras will be explained in detail in the [Cameras](gltfTutorial_016_Cameras.md) section.

透视相机和正交相机之间的差异及其属性、将相机附加到节点的效果以及多个相机的管理将在相机部分中详细解释。

Previous: [Advanced Material](gltfTutorial_014_AdvancedMaterial.md) | [Table of Contents](README.md) | Next: [Cameras](gltfTutorial_016_Cameras.md)
