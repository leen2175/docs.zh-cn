---
title: 如何：设置笔的宽度和对齐方式
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- pens [Windows Forms], setting width
- pens [Windows Forms], setting alignment
ms.assetid: a202af36-4d31-4401-a126-b232f51db581
ms.openlocfilehash: e82f406b4fdca93df7a811eea5506846d56fda28
ms.sourcegitcommit: 160a88c8087b0e63606e6e35f9bd57fa5f69c168
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2019
ms.locfileid: "57703044"
---
# <a name="how-to-set-pen-width-and-alignment"></a>如何：设置笔的宽度和对齐方式
当你创建<xref:System.Drawing.Pen>，可以作为构造函数的参数之一提供钢笔的宽度。 你可以使用笔的宽度<xref:System.Drawing.Pen.Width%2A>属性的<xref:System.Drawing.Pen>类。  
  
 理论的线条具有宽度为 0。 当绘制 1 个像素宽的行时，像素理论的线条上居中。 如果绘制多个像素宽的行，像素或者在理论的线条上居中或理论的线条的一侧显示。 可以设置的笔对齐方式属性<xref:System.Drawing.Pen>来确定如何用其绘制的像素将相对于理论的线条进行定位。  
  
 值<xref:System.Drawing.Drawing2D.PenAlignment.Center>， <xref:System.Drawing.Drawing2D.PenAlignment.Outset>，并<xref:System.Drawing.Drawing2D.PenAlignment.Inset>出现在下面的代码示例均隶属于<xref:System.Drawing.Drawing2D.PenAlignment>枚举。  
  
 下面的代码示例两次绘制一条线： 一次用黑色钢笔的宽度为 1，一次绿色手写笔的宽度为 10。  
  
### <a name="to-vary-the-width-of-a-pen"></a>若要改变笔的宽度  
  
-   设置的值<xref:System.Drawing.Pen.Alignment%2A>属性设置为<xref:System.Drawing.Drawing2D.PenAlignment.Center>（默认值） 以指定使用绿色笔绘制的像素会位于理论的线条。 下图显示了生成的行。  
  
     ![笔](./media/pens1a.gif "pens1A")  
  
     下面的代码示例绘制一个矩形两次： 一次用黑色钢笔的宽度为 1，一次绿色手写笔的宽度为 10。  
  
     [!code-csharp[System.Drawing.UsingAPen#41](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.UsingAPen/CS/Class1.cs#41)]
     [!code-vb[System.Drawing.UsingAPen#41](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.UsingAPen/VB/Class1.vb#41)]  
  
### <a name="to-change-the-alignment-of-a-pen"></a>若要更改笔的对齐方式  
  
-   设置的值<xref:System.Drawing.Pen.Alignment%2A>属性设置为<xref:System.Drawing.Drawing2D.PenAlignment.Center>指定用绿色笔绘制的像素会位于矩形的边界。  
  
     下图显示了所得矩形的大小。  
  
     ![笔](./media/pens2.gif "pens2")  
  
     [!code-csharp[System.Drawing.UsingAPen#42](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.UsingAPen/CS/Class1.cs#42)]
     [!code-vb[System.Drawing.UsingAPen#42](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.UsingAPen/VB/Class1.vb#42)]  
  
### <a name="to-create-an-inset-pen"></a>若要创建嵌入钢笔  
  
-   通过按如下所示修改前面的代码示例中的第三个语句更改绿色笔对齐方式：  
  
     [!code-csharp[System.Drawing.UsingAPen#43](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.UsingAPen/CS/Class1.cs#43)]
     [!code-vb[System.Drawing.UsingAPen#43](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.UsingAPen/VB/Class1.vb#43)]  
  
     现在宽绿线中的像素显示矩形的内部，如下图中所示。  
  
     ![笔](./media/pens3.gif "pens3")  
  
## <a name="see-also"></a>请参阅
- [使用笔绘制直线和形状](using-a-pen-to-draw-lines-and-shapes.md)
- [Windows 窗体中的图形和绘制](graphics-and-drawing-in-windows-forms.md)
