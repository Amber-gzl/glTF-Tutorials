[Table of Contents](README.md) | Next: [Basic glTF Structure](gltfTutorial_002_BasicGltfStructure.md)





# Introduction to glTF using WebGL

使用 WebGL 的 glTF 简介

An increasing number of applications and services are based on 3D content. Online shops offer product configurators with a 3D preview. Museums digitize their artifacts with 3D scans and allow visitors to explore their collections in virtual galleries. City planners use 3D city models for planning and information visualization. Educators create interactive, animated 3D models of the human body. Many of these applications run directly in the web browser, which is possible because all modern browsers support efficient rendering with WebGL.

越来越多的应用程序和服务基于 3D 内容。在线商店提供具有3D预览的产品配置器。博物馆通过3D扫描将他们的文物数字化，并允许参观者在虚拟画廊中探索他们的藏品。城市规划者使用 3D 城市模型进行规划和信息可视化。教育工作者创建人体的交互式动画3D模型。其中许多应用程序直接在 Web 浏览器中运行，这是可能的，因为所有现代浏览器都支持使用 WebGL 进行高效渲染。

<p align="center">
<img src="images/applications.png" /><br>
<a name="applications-png"></a>Image 1a: Screenshots of various websites and applications showing 3D models.
</p>

Demand for 3D content in various applications is constantly increasing. In many cases, the 3D content has to be transferred over the web, and it has to be rendered efficiently on the client side. But until now, there has been a gap between the 3D content creation and efficient rendering of that 3D content in the runtime applications.

各种应用中对3D内容的需求不断增加。在许多情况下，3D 内容必须通过 Web 传输，并且必须在客户端高效渲染。但到目前为止，在运行时应用程序中，3D 内容创建和 3D 内容的有效渲染之间存在差距。


## 3D content pipelines
3D 内容管线

3D content that is rendered in client applications comes from different sources and is stored in different file formats. The [list of 3D graphics file formats on Wikipedia](https://en.wikipedia.org/wiki/List_of_file_formats#3D_graphics) shows an overwhelming number, with more than 70 different file formats for 3D data, serving different purposes and application cases.  

在客户端应用程序中呈现的 3D 内容来自不同的源，并以不同的文件格式存储。维基百科上的3D图形文件格式列表显示了压倒性的数量，3D数据有70多种不同的文件格式，服务于不同的目的和应用案例。

For example, raw 3D data may be obtained with a 3D scanner. These scanners usually provide the geometry data of a single object, which is stored in [OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file), [PLY](https://en.wikipedia.org/wiki/PLY_(file_format)), or [STL](https://en.wikipedia.org/wiki/STL_(file_format)) files. These file formats do not contain information about the scene structure or how the objects should be rendered.

例如，可以使用3D扫描仪获得原始3D数据。这些扫描仪通常提供单个物体的几何数据，这些数据存储在OBJ、PLY或STL文件中。这些文件格式不包含有关场景结构或对象应如何渲染的信息

More sophisticated 3D scenes can be created with authoring tools. These tools allow one to edit the structure of the scene, the light setup, cameras, animations, and, of course, the 3D geometry of the objects that appear in the scene. Applications store this information in their own, custom file formats. For example, [Blender](https://www.blender.org/) stores the scenes in `.blend` files, [LightWave3D](https://www.lightwave3d.com/) uses the `.lws` file format, [3ds Max](https://www.autodesk.com/3dsmax) uses the `.max` file format, and [Maya](https://www.autodesk.com/maya) uses `.ma` files.

可以使用创作工具创建更复杂的 3D 场景。这些工具允许人们编辑场景的结构、灯光设置、摄像机、动画，当然还有场景中出现的对象的 3D 几何体。应用程序以自己的自定义文件格式存储此信息。例如，Blender 将场景存储在 .blend 文件中，LightWave3D 使用 .lws 文件格式，3ds Max 使用 .max 文件格式，Maya 使用 .ma 文件。

In order to render such 3D content, the runtime application must be able to read different input file formats. The scene structure has to be parsed, and the 3D geometry data has to be converted into the format required by the graphics API. The 3D data has to be transferred to the graphics card memory, and then the rendering process can be described with sequences of graphics API calls. Thus, each runtime application has to create importers, loaders, or converters for all file formats that it will support, as shown in [Image 1b](#contentPipeline-png).

为了呈现此类 3D 内容，运行时应用程序必须能够读取不同的输入文件格式。必须解析场景结构，并且必须将 3D 几何数据转换为图形 API 所需的格式。必须将 3D 数据传输到显卡内存，然后可以使用一系列图形 API 调用来描述渲染过程。因此，每个运行时应用程序都必须为其支持的所有文件格式创建导入程序、加载程序或转换器，如图 1b 所示。

<p align="center">
<img src="images/contentPipeline.png" /><br>
<a name="contentPipeline-png"></a>Image 1b: The 3D content pipeline today.
</p>


## glTF: A transmission format for 3D scenes

glTF：3D 场景的传输格式

The goal of glTF is to define a standard for representing 3D content, in a form that is suitable for use in runtime applications. The existing file formats are not appropriate for this use case: some of do not contain any scene information, but only geometry data; others have been designed for exchanging data between authoring applications, and their main goal is to retain as much information about the 3D scene as possible, resulting in files that are usually large, complex, and hard to parse. Additionally, the geometry data may have to be preprocessed so that it can be rendered with the client application.

glTF 的目标是定义一个标准，以适合在运行时应用程序中使用的形式表示 3D 内容。现有的文件格式不适合此用例：有些不包含任何场景信息，而只包含几何数据;其他文件设计用于在创作应用程序之间交换数据，其主要目标是保留尽可能多的有关 3D 场景的信息，从而生成通常较大、复杂且难以解析的文件。此外，可能必须对几何数据进行预处理，以便可以使用客户端应用程序呈现。

None of the existing file formats were designed for the use case of efficiently transferring 3D scenes over the web and rendering them as efficiently as possible. But glTF is not "yet another file format." It is the definition of a *transmission* format for 3D scenes:

现有的文件格式都不是为通过 Web 高效传输 3D 场景并尽可能高效地渲染它们的用例而设计的。但glTF并不是“另一种文件格式”。它是3D场景传输格式的定义：

- The scene structure is described with JSON, which is very compact and can easily be parsed.
- 场景结构用 JSON 描述，非常紧凑，可以轻松解析。
- The 3D data of the objects are stored in a form that can be directly used by the common graphics APIs, so there is no overhead for decoding or pre-processing the 3D data.
- 对象的 3D 数据存储在可由常见图形 API 直接使用的形式中，因此没有解码或预处理 3D 数据的开销。

Different content creation tools may now provide 3D content in the glTF format. And an increasing number of client applications are able to consume and render glTF. Some of these applications are shown in [Image 1a](#applications-png). So glTF may help to bridge the gap between content creation and rendering, as shown in [Image 1c](#contentPipelineWithGltf-png).

不同的内容创建工具现在可以提供 glTF 格式的 3D 内容。越来越多的客户端应用程序能够使用和呈现 glTF。其中一些应用如图1a所示。因此，glTF 可能有助于弥合内容创建和渲染之间的差距，如图 1c 所示。

<p align="center">
<img src="images/contentPipelineWithGltf.png" /><br>
<a name="contentPipelineWithGltf-png"></a>Image 1c: The 3D content pipeline with glTF.
</p>

An increasing number of content creation tools provide glTF import and export directly. For example, the Blender Manual documents [how to import and export PBR materials](https://docs.blender.org/manual/en/latest/addons/import_export/scene_gltf2.html) using glTF.  Alternatively, other file formats can be used to create glTF assets, using one of the open-source conversion utilities listed in the [glTF Project Explorer](https://github.khronos.org/glTF-Project-Explorer/). The output of converters and exporters can be validated using the [Khronos glTF Validator](https://github.khronos.org/glTF-Validator/).

越来越多的内容创建工具直接提供 glTF 导入和导出。例如，Blender手册记录了如何使用glTF导入和导出PBR材质。或者，可以使用 glTF 项目资源管理器中列出的开源转换实用程序之一使用其他文件格式来创建 glTF 资产。转换器和导出器的输出可以使用 Khronos glTF 验证器进行验证。


[Table of Contents](README.md) | Next: [Basic glTF Structure](gltfTutorial_002_BasicGltfStructure.md)
