---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: 创建自定义 HTML 帮助器 (C#) |Microsoft 文档
author: microsoft
description: 本教程的目标是演示如何创建自定义 HTML 帮助程序，你可以使用您的 MVC 视图中。 通过利用 HTML 帮助程序...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ebc9aa2aa8dbc02dc01833d671c3bfd19141ba74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-html-helpers-c"></a>创建自定义 HTML 帮助器 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下载 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> 本教程的目标是演示如何创建自定义 HTML 帮助程序，你可以使用您的 MVC 视图中。 通过利用 HTML 帮助器，可以减少的你必须执行来创建标准的 HTML 页面的 HTML 标记的类型设置需要很长时间。


本教程的目标是演示如何创建自定义 HTML 帮助程序，你可以使用您的 MVC 视图中。 通过利用 HTML 帮助器，可以减少的你必须执行来创建标准的 HTML 页面的 HTML 标记的类型设置需要很长时间。

在本教程的第一部分，我介绍一些现有 ASP.NET MVC framework 附带的 HTML 帮助器。 接下来，我介绍了两种方法可以创建自定义 HTML 帮助： 我介绍如何创建自定义 HTML 帮助器，通过创建静态方法，并创建扩展方法。

## <a name="understanding-html-helpers"></a>了解 HTML 帮助器

HTML 帮助程序是仅返回一个字符串的方法。 字符串可以表示任何类型的所需的内容。 例如，你可以使用 HTML 帮助器来呈现类似于 HTML 的标准 HTML 标记`<input>`和`<img>`标记。 此外可以使用 HTML 帮助器来呈现更复杂的内容，如选项卡条或一个 HTML 表的数据库数据。

ASP.NET MVC framework 包括以下一组标准 HTML 帮助器 （这不是完整列表）：

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

例如，考虑列表 1 中的窗体。 此窗体将呈现的两个标准 HTML 帮助器 （请参见图 1） 的帮助。 此窗体使用`Html.BeginForm()`和`Html.TextBox()`帮助器方法来呈现一个简单的 HTML 窗体。


[![页呈现的 HTML 帮助器](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**图 01**： 页上呈现的 HTML 帮助器 ([单击以查看实际尺寸的图像](creating-custom-html-helpers-cs/_static/image3.png))


**列表 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Html.BeginForm() 帮助器方法用于创建的开始和结束 HTML`<form>`标记。 请注意，`Html.BeginForm()`方法调用在中使用语句。 使用语句可确保`<form>`标记使用的结尾处获取结束块。

如果你愿意，而不是创建 using 块中，你可以调用 Html.EndForm() 帮助器方法来关闭`<form>`标记。 使用任何方法来创建打开和关闭`<form>`看起来最直观的标记。

`Html.TextBox()`帮助器方法列表 1 中使用以呈现 HTML`<input>`标记。 如果在你的浏览器中选择查看源你看到列出 2 中的 HTML 源文件。 请注意源包含标准的 HTML 标记。

> [!IMPORTANT]
> 请注意，`Html.TextBox()`帮助器呈现的 HTML`<%= %>`标记而不是`<% %>`标记。 如果不包含等号，然后执行任何操作获取呈现到浏览器。

ASP.NET MVC framework 包含一小部分的帮助器。 最有可能，你将需要扩展的自定义 HTML 帮助器 MVC 框架。 在本教程的其余部分中，你了解两种方法可以创建自定义 HTML 帮助器。

**列出 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>使用静态方法创建 HTML 帮助器

创建新的 HTML 帮助器的最简单方法是创建返回的字符串的静态方法。 例如，假设你决定创建新的 HTML 帮助器上呈现 HTML`<label>`标记。 可以使用在列出 2 中的类来呈现`<label>`。

**列出 2 – `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

无需进行任何特殊有关列出 2 中的类。 `Label()`方法只需返回的字符串。

已修改的索引视图，列出 3 中使用`LabelHelper`呈现 HTML`<label>`标记。 请注意，则视图包括`<%@ imports %>`导入的指令`Application1.Helpers`命名空间。

**列出 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>使用扩展方法中创建 HTML 帮助器

如果你想要创建 HTML 帮助程序，只需工作诸如标准的 HTML 帮助器，它们包含在 ASP.NET MVC framework 中，则需要创建扩展方法。 扩展方法使你能够添加到现有类的新方法。 在创建一个 HTML 帮助器方法时，你将新方法添加到由视图的 Html 属性表示 HtmlHelper 类。

列出 3 中的此类添加到扩展方法`HtmlHelper`类名为`Label()`。 有一些你应注意到有关此类的内容。 首先，请注意此类是静态类。 你必须定义一个具有静态类扩展方法。

其次，请注意，第一个参数`Label()`方法前面加上关键字`this`。 扩展方法的第一个参数指示该扩展方法所扩展的类。

**列出 3 – `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

创建扩展方法，并在成功生成你的应用程序后，该扩展方法将出现在 Visual Studio Intellisense，就像所有其他方法的类 （请参见图 2）。 唯一的区别是方法用特殊符号旁边 （图标的向下箭头） 显示该扩展。


[![使用 Html.Label() 扩展方法](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**图 02**： 使用 Html.Label() 扩展方法 ([单击以查看实际尺寸的图像](creating-custom-html-helpers-cs/_static/image6.png))


已修改的索引视图，列出 4 中使用 Html.Label() 扩展方法来呈现的所有其`<label>`标记。

**列出 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>总结

在本教程中，您学习了两种方法可以创建自定义 HTML 帮助器。 首先，您学习了如何创建自定义`Label()`通过创建静态方法的 HTML 帮助程序返回的字符串。 接下来，您学习了如何创建自定义`Label()`通过上创建的扩展方法的 HTML 帮助器方法`HtmlHelper`类。

在本教程中，我侧重于生成一个非常简单的 HTML 帮助器方法。 请注意，可根据需要一样复杂 HTML 帮助器。 你可以构建呈现如树视图、 菜单或表的数据库数据的丰富内容的 HTML 帮助器。

> [!div class="step-by-step"]
> [上一页](asp-net-mvc-views-overview-cs.md)
> [下一页](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
