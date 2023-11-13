Previous: [Simple Animation](gltfTutorial_006_SimpleAnimation.md) | [Table of Contents](README.md) | Next: [Simple Meshes](gltfTutorial_008_SimpleMeshes.md)

# Animations

动画

As shown in the [Simple Animation](gltfTutorial_006_SimpleAnimation.md) example, an [`animation`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation) can be used to describe how the `translation`, `rotation`, or `scale` properties of nodes change over time.

如简单动画示例所示，动画可用于描述节点的平移、旋转或缩放属性如何随时间变化。

The following is another example of an `animation`. This time, the animation contains two channels. One animates the translation, and the other animates the rotation of a node:

下面是动画的另一个示例。这一次，动画包含两个通道。一个对平移进行动画处理，另一个对节点的旋转进行动画处理：

```javascript
  "animations": [
    {
      "samplers" : [
        {
          "input" : 2,
          "interpolation" : "LINEAR",
          "output" : 3
        },
        {
          "input" : 2,
          "interpolation" : "LINEAR",
          "output" : 4
        }
      ],
      "channels" : [ 
        {
          "sampler" : 0,
          "target" : {
            "node" : 0,
            "path" : "rotation"
          }
        },
        {
          "sampler" : 1,
          "target" : {
            "node" : 0,
            "path" : "translation"
          }
        } 
      ]
    }
  ],
```

## Animation samplers

动画采样器

The `samplers` array contains [`animation.sampler`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation-sampler) objects that define how the values that are provided by the accessors have to be interpolated between the key frames, as shown in Image 7a.

采样器数组包含 animation.sampler 对象，这些对象定义如何在关键帧之间插值访问器提供的值，如图 7a 所示。

<p align="center">
<img src="images/animationSamplers.png" /><br>
<a name="animationSamplers-png"></a>Image 7a: Animation samplers.
</p>

In order to compute the value of the translation for the current animation time, the following algorithm can be used:

为了计算当前动画时间的平移值，可以使用以下算法：•

* Let the current animation time be given as `currentTime`.
* 让当前动画时间作为当前时间给出。
* Compute the next smaller and the next larger element of the *times* accessor:
* 计算 time 存取器的下一个较小和下一个较大的元素：

    `previousTime` = The largest element from the *times* accessor that is smaller than the `currentTime`

    previousTime = 时间访问器中小于当前时间的最大元素

    `nextTime`  = The smallest element from the *times* accessor that is larger than the `currentTime`

    nextTime = 时间访问器中大于当前时间的最小元素

* Obtain the elements from the *translations* accessor that correspond to these times:
* 从翻译存取器获取与以下时间对应的元素：

    `previousTranslation` = The element from the *translations* accessor that corresponds to the `previousTime`
    
    previousTranslation = 翻译访问器中与 previousTime 对应的元素

    `nextTranslation` = The element from the *translations* accessor that corresponds to the `nextTime`

    nextTranslation = 翻译访问器中与 nextTime 对应的元素

* Compute the interpolation value. This is a value between 0.0 and 1.0 that describes the *relative* position of the `currentTime`, between the `previousTime` and the `nextTime`:
* 计算插值。这是一个介于 0.0 和 1.0 之间的值，用于描述当前时间的相对位置，介于上一个时间和下一个时间之间：

    `interpolationValue = (currentTime - previousTime) / (nextTime - previousTime)`

* Use the interpolation value to compute the translation for the current time:
* 计算插值。这是一个介于 0.0 和 1.0 之间的值，用于描述当前时间的相对位置，介于上一个时间和下一个时间之间：

    `currentTranslation = previousTranslation + interpolationValue * (nextTranslation - previousTranslation)`


### Example:
例子

Imagine the `currentTime` is **1.2**. The next smaller element from the *times* accessor is **0.8**. The next larger element is **1.6**. So

想象一下当前时间是 1.2。时间存取器中的下一个较小元素是 0.8。下一个较大的元素是 1.6。所以

    previousTime = 0.8
    nextTime     = 1.6

The corresponding values from the *translations* accessor can be looked up:、

可以从翻译访问器中查找相应的值：

    previousTranslation = (14.0, 3.0, -2.0)
    nextTranslation     = (18.0, 1.0,  1.0)

The interpolation value can be computed:

插值可以计算：

    interpolationValue = (currentTime - previousTime) / (nextTime - previousTime)
                       = (1.2 - 0.8) / (1.6 - 0.8)
                       = 0.4 / 0.8         
                       = 0.5

From the interpolation value, the current translation can be computed:

根据插值，可以计算出当前的平移：

    currentTranslation = previousTranslation + interpolationValue * (nextTranslation - previousTranslation)
                       = (14.0, 3.0, -2.0) + 0.5 * ( (18.0, 1.0,  1.0) - (14.0, 3.0, -2.0) )
                       = (14.0, 3.0, -2.0) + 0.5 * (4.0, -2.0, 3.0)
                       = (16.0, 2.0, -0.5)

So when the current time is **1.2**, then the `translation` of the node is **(16.0, 2.0, -0.5)**.

所以当当前时间为 1.2 时，节点的平移为 （16.0， 2.0， -0.5）。

## Animation channels

动画通道

The animations contain an array of [`animation.channel`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation-channel) objects. The channels establish the connection between the input, which is the value that is computed from the sampler, and the output, which is the animated node property. Therefore, each channel refers to one sampler, using the index of the sampler, and contains an [`animation.channel.target`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation-channel-target). The `target` refers to a node, using the index of the node, and contains a `path` that defines the property of the node that should be animated. The value from the sampler will be written into this property.

动画包含动画频道对象的数组。通道在输入（从采样器计算的值）和输出（动画节点属性）之间建立连接。因此，每个通道引用一个采样器，使用采样器的索引，并包含一个animation.channel.target。目标使用节点的索引引用节点，并包含一个路径，该路径定义应进行动画处理的节点的属性。采样器中的值将写入此属性。

In the example above, there are two channels for the animation. Both refer to the same node. The path of the first channel refers to the `translation` of the node, and the path of the second channel refers to the `rotation` of the node. So all objects (meshes) that are attached to the node will be translated and rotated by the animation, as shown in Image 7b.

在上面的示例中，动画有两个通道。两者都引用同一节点。第一通道的路径是指节点的平移，第二通道的路径是指节点的旋转。因此，附加到节点的所有对象（网格）都将通过动画进行平移和旋转，如图 7b 所示。

<p align="center">
<img src="images/animationChannels.png" /><br>
<a name="animationChannels-png"></a>Image 7b: Animation channels.
</p>

## Interpolation

The above example only covers `LINEAR` interpolation. Animations in a glTF asset can use three interpolation modes : 

上面的例子只涉及线性插值。glTF 资产中的动画可以使用三种插值模式：

 - `STEP`
 - `LINEAR`
 - `CUBICSPLINE`
 
### Step

The `STEP` interpolation is not really an interpolation mode, it makes objects jump from keyframe to keyframe *without any sort of interpolation*. When a sampler defines a step interpolation, just apply the transformation from the keyframe corresponding to `previousTime`.

STEP 插值并不是真正的插值模式，它使对象从一个关键帧跳到另一个关键帧，而无需任何插值。当采样器定义步骤插值时，只需应用与 previousTime 对应的关键帧的变换。

### Linear

Linear interpolation exactly corresponds to the above example. The general case is : 

线性插值与上述示例完全对应。一般情况是：

Calculate the `interpolationValue`:

计算插值值：

```
    interpolationValue = (currentTime - previousTime) / (nextTime - previousTime)
```

For scalar and vector types, use a linear interpolation (generally called `lerp` in mathematics libraries). Here's a "pseudo code" implementation for reference

对于标量和向量类型，请使用线性插值（在数学库中通常称为 lerp）。下面是一个“伪代码”实现供参考

```
    Point lerp(previousPoint, nextPoint, interpolationValue)
        return previousPoint + interpolationValue * (nextPoint - previousPoint)
```

In the case of rotations expressed as quaternions, you need to perform a spherical linear interpolation (`slerp`) between the previous and next values:

对于表示为四元数的旋转，您需要在上一个值和下一个值之间执行球形线性插值（slerp）：

```
    Quat slerp(previousQuat, nextQuat, interpolationValue)
        var dotProduct = dot(previousQuat, nextQuat)
        
        //make sure we take the shortest path in case dot Product is negative
        if(dotProduct < 0.0)
            nextQuat = -nextQuat
            dotProduct = -dotProduct
            
        //if the two quaternions are too close to each other, just linear interpolate between the 4D vector
        if(dotProduct > 0.9995)
            return normalize(previousQuat + interpolationValue(nextQuat - previousQuat))

        //perform the spherical linear interpolation
        var theta_0 = acos(dotProduct)
        var theta = interpolationValue * theta_0
        var sin_theta = sin(theta)
        var sin_theta_0 = sin(theta_0)
        
        var scalePreviousQuat = cos(theta) - dotproduct * sin_theta / sin_theta_0
        var scaleNextQuat = sin_theta / sin_theta_0
        return scalePreviousQuat * previousQuat + scaleNextQuat * nextQuat
```

This example implementation is inspired from this [Wikipedia article](https://en.wikipedia.org/wiki/Slerp)

此示例实现的灵感来自这篇维基百科文章

### Cubic Spline interpolation

三次样条插值

Cubic spline interpolation needs more data than just the previous and next keyframe time and values, it also need for each keyframe a couple of tangent vectors that act to smooth out the curve around the keyframe points.

三次样条插值需要的数据不仅仅是上一个和下一个关键帧的时间和值，它还需要为每个关键帧提供几个切向量，用于平滑关键帧点周围的曲线。

These tangent are stored in the animation channel. For each keyframe described by the animation sampler, the animation channel contains 3 elements : 

这些切线存储在动画通道中。对于动画采样器描述的每个关键帧，动画通道包含 3 个元素：
 - The input tangent of the keyframe
 - 关键帧的输入切线
 - The keyframe value
 - 关键帧值
 - The output tangent
 - 输出切线
 The input and output tangents are normalized vectors that will need to be scaled by the duration of the keyframe, we call that the deltaTime

 输入和输出切线是归一化向量，需要按关键帧的持续时间进行缩放，我们称之为 deltaTime
 ```
     deltaTime = nextTime - previousTime
 ```
 To calculate the value for `currentTime`, you will need to fetch from the animation channel : 

 要计算当前时间的值，您需要从动画通道获取：•

 - The output tangent direction of `previousTime` keyframe
 - 上一个时间关键帧的输出切线方向
 - The value of `previousTime` keyframe
 - 上一个时间关键帧的输出切线方向
 - The value of `nextTime` keyframe
 - 上一个时间关键帧的输出切线方向
 - The input tangent direction of `nextTime` keyframe
 - 上一个时间关键帧的输出切线方向
 
*note: the nput tangent of the first keyframe and the output tangent of the last keyframe are totally ignored*

注： 第一个关键帧的输入切线和最后一个关键帧的输出切线将被完全忽略

To calculate the actual tangents of the keyframe, you need to multiply the direction vectors you got from the channel by `deltaTime`

要计算关键帧的实际切线，您需要将从通道获得的方向矢量乘以 deltaTime

```
    previousTangent = deltaTime * previousOutputTangent
    nextTangent = deltaTime * nextInputTangent
```

The mathematical function is described in the [Appendix C](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#appendix-c-interpolation) of the glTF 2.0 specification.

数学函数在 glTF 2.0 规范的附录 C 中进行了描述。

Here's a corresponding pseudocode snippet :

这是一个相应的伪代码片段：

```
    Point cubicSpline(previousPoint, previousTangent, nextPoint, nextTangent, interpolationValue)
        t = interpolationValue
        t2 = t * t
        t3 = t2 * t
                return (2 * t3 - 3 * t2 + 1) * previousPoint + (t3 - 2 * t2 + t) * previousTangent + (-2 * t3 + 3 * t2) * nextPoint + (t3 - t2) * nextTangent;
```
Previous: [Simple Animation](gltfTutorial_006_SimpleAnimation.md) | [Table of Contents](README.md) | Next: [Simple Meshes](gltfTutorial_008_SimpleMeshes.md)
