Previous: [Simple Cameras](gltfTutorial_015_SimpleCameras.md) | [Table of Contents](README.md) | Next: [Simple Morph Target](gltfTutorial_017_SimpleMorphTarget.md)

# Cameras

相机

The example in the [Simple Cameras](gltfTutorial_017_SimpleCameras.md) section showed how to define perspective and orthographic cameras, and how they can be integrated into a scene by attaching them to nodes. This section will explain the differences between both types of cameras, and the handling of cameras in general.  

简单摄影机部分中的示例演示了如何定义透视摄影机和正交摄影机，以及如何通过将透视摄影机和正交摄影机附加到节点来将它们集成到场景中。本节将解释两种类型的相机之间的差异，以及相机的一般处理方式。

## Perspective and orthographic cameras

透视和正交相机

There are two kinds of cameras: *Perspective* cameras, where the viewing volume is a truncated pyramid (often referred to as "viewing frustum"), and *orthographic*  cameras, where the viewing volume is a rectangular box. The main difference is that rendering with a *perspective* camera causes a proper perspective distortion, whereas rendering with an *orthographic* camera causes a preservation of lengths and angles.

有两种类型的相机：透视相机，其中观看体积是截断的金字塔（通常称为“视锥”）和正交相机，其中观看体积是矩形框。主要区别在于，使用透视相机渲染会导致适当的透视失真，而使用正交相机渲染会导致长度和角度的保留。

The example in the [Simple Cameras](gltfTutorial_015_SimpleCameras.md) section contains one camera of each type, a perspective camera at index 0 and an orthographic camera at index 1:

简单相机部分中的示例包含每种类型的一个相机，索引为 0 的透视相机和索引 1 处的正交相机：

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

The `type` of the camera is given as a string, which can be `"perspective"` or  `"orthographic"`. Depending on this type, the `camera` object contains a [`camera.perspective`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera-perspective) object or a [`camera.orthographic`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera-orthographic) object. These objects contain additional parameters that define the actual viewing volume.

相机的类型以字符串形式给出，可以是“透视”或“正交”。根据此类型，相机对象包含摄像机透视对象或摄像机正交对象。这些对象包含定义实际查看音量的其他参数。

The `camera.perspective` object contains an `aspectRatio` property that defines the aspect ratio of the viewport. Additionally, it contains a property called `yfov`, which stands for *Field Of View in Y-direction*. It defines the "opening angle" of the camera and is given in radians.

camera.perspective 对象包含一个 aspectRatio 属性，该属性定义视区的纵横比。此外，它还包含一个名为 yfov 的属性，它代表 Y 方向的视野。它定义了相机的“打开角度”，并以弧度为单位。

The `camera.orthographic` object contains `xmag` and `ymag` properties. These define the magnification of the camera in x- and y-direction, and basically describe the width and height of the viewing volume.

camera.orthographic 对象包含 xmag 和 ymag 属性。这些定义了相机在x 和 y方向上的放大倍率，并且基本上描述了观看体积的宽度和高度。

Both camera types additionally contain `znear` and `zfar` properties, which are the coordinates of the near and far clipping plane. For perspective cameras, the `zfar` value is optional. When it is missing, a special "infinite projection matrix" will be used.

这两种相机类型还包含 znear 和 zfar 属性，它们是近剪裁平面和远剪裁平面的坐标。对于透视相机，zfar 值是可选的。当缺少它时，将使用特殊的“无限投影矩阵”。

Explaining the details of cameras, viewing, and projections is beyond the scope of this tutorial. The important point is that most graphics APIs offer methods for defining the viewing configuration that are directly based on these parameters. In general, these parameters can be used to compute a *camera matrix*. The camera matrix can be inverted to obtain the *view matrix*, which will later be post-multiplied with the *model matrix* to obtain the *model-view matrix*, which is required by the renderer.

解释照相机、查看和投影的详细信息超出了本教程的范围。重要的一点是，大多数图形 API 都提供了直接基于这些参数来定义查看配置的方法。通常，这些参数可用于计算相机矩阵。相机矩阵可以反转得到视图矩阵，稍后会与模型矩阵进行后乘法，得到渲染器需要的模型-视图矩阵。

# Camera orientation

相机方向

A `camera` can be transformed to have a certain orientation and viewing direction in the scene. This is accomplished by attaching the camera to a `node`. Each [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node) may contain the index of a `camera` that is attached to it. In the simple camera example, there are two nodes for the cameras. The first node refers to the perspective camera with index 0, and the second one refers to the orthographic camera with index 1:

摄像机可以变换为在场景中具有特定的方向和查看方向。这是通过将相机连接到节点来实现的。每个节点可能包含连接到它的相机的索引。在简单的相机示例中，相机有两个节点。第一个节点引用索引为 0 的透视相机，第二个节点引用索引为 1 的正交相机：

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

As shown in the [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) section, these nodes may have properties that define the transform matrix of the node. The [global transform](gltfTutorial_004_ScenesNodes.md#global-transforms-of-nodes) of a node then defines the actual orientation of the camera in the scene. With the option to apply arbitrary [animations](gltfTutorial_007_Animations.md) to the nodes, it is even possible to define camera flights.

如场景和节点部分所示，这些节点可能具有定义节点转换矩阵的属性。然后，节点的全局变换定义场景中摄像机的实际方向。通过对节点应用任意动画的选项，甚至可以定义相机飞行。

When the global transform of the camera node is the identity matrix, then the eye point of the camera is at the origin, and the viewing direction is along the negative z-axis. In the given example, the nodes both have a `translation` about `(0.5, 0.5, 3.0)`, which causes the camera to be transformed accordingly: it is translated about 0.5 in the x- and y- direction, to look at the center of the unit square, and about 3.0 along the z-axis, to move it a bit away from the object.

当相机节点的全局变换为单位矩阵时，相机的视点在原点，观看方向沿负z轴。在给定的示例中，节点都有一个关于 （0.5， 0.5， 3.0） 的平移，这会导致相机相应地变换：它在 x 和 y 方向上平移约 0.5，以查看单位正方形的中心，沿 z 轴平移约 3.0，使其远离对象。

## Camera instancing and management

摄像机实例化和管理

There may be multiple cameras defined in the JSON part of a glTF. Each camera may be referred to by multiple nodes. Therefore, the cameras as they appear in the glTF asset are really "templates" for actual camera *instances*: Whenever a node refers to one camera, a new instance of this camera is created.

glTF 的 JSON 部分中可能定义了多个摄像头。每个相机可以由多个节点引用。因此，glTF 资产中显示的摄像机实际上是实际摄像机实例的“模板”：每当节点引用一个摄像机时，都会创建该摄像机的新实例。

There is no "default" camera for a glTF asset. Instead, the client application has to keep track of the currently active camera. The client application may, for example, offer a dropdown-menu that allows one to select the active camera and thus to quickly switch between predefined view configurations. With a bit more implementation effort, the client application can also define its own camera and interaction patterns for the camera control (e.g., zooming with the mouse wheel). However, the logic for the navigation and interaction has to be implemented solely by the client application in this case. [Image 15a](gltfTutorial_015_SimpleCameras.md#cameras-png) shows the result of such an implementation, where the user may select either the active camera from the ones that are defined in the glTF asset, or an "external camera" that may be controlled with the mouse.

glTF 资产没有“默认”摄像机。相反，客户端应用程序必须跟踪当前活动的相机。例如，客户端应用程序可能会提供一个下拉菜单，允许人们选择活动摄像机，从而在预定义的视图配置之间快速切换。通过更多的实现工作，客户端应用程序还可以为相机控件定义自己的相机和交互模式（例如，使用鼠标滚轮缩放）。但是，在这种情况下，导航和交互的逻辑必须仅由客户端应用程序实现。图15a显示了这种实现的结果，用户可以从glTF资产中定义的相机中选择活动相机，也可以选择可以用鼠标控制的“外部相机”。

Previous: [Simple Cameras](gltfTutorial_015_SimpleCameras.md) | [Table of Contents](README.md) | Next: [Simple Morph Target](gltfTutorial_017_SimpleMorphTarget.md)
