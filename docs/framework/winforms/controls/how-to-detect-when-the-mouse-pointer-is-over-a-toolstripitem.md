---
title: "Практическое руководство. Отслеживание наведения указателя мыши на элемент ToolStripItem"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-winforms
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- toolbars [Windows Forms], detecting mouse movement
- ToolStrip control [Windows Forms], detecting mouse movement
- ToolStripItem class [Windows Forms], detecting mouse movement
- mouse [Windows Forms], detecting movement on toolbars
ms.assetid: d38b5082-aba7-4f6c-841b-bd9714e307fd
caps.latest.revision: "7"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: b04d0ef38a8946abc43da2672ade0723f5f2446f
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="how-to-detect-when-the-mouse-pointer-is-over-a-toolstripitem"></a>Практическое руководство. Отслеживание наведения указателя мыши на элемент ToolStripItem
Используйте следующую процедуру, чтобы обнаружить, когда указатель мыши находится над <xref:System.Windows.Forms.ToolStripItem>.  
  
### <a name="to-detect-when-the-pointer-is-over-a-toolstripitem"></a>Чтобы обнаружить, когда указатель находится над ToolStripItem  
  
-   Используйте <xref:System.Windows.Forms.ToolStripItem.Selected%2A> свойство для элементов, в котором <xref:System.Windows.Forms.ToolStripItem.CanSelect%2A> — `true`.  
  
     Это избавит вас от необходимости синхронизации <xref:System.Windows.Forms.ToolStripItem.MouseEnter> и <xref:System.Windows.Forms.ToolStripItem.MouseLeave> события.  
  
## <a name="see-also"></a>См. также  
 <xref:System.Windows.Forms.ToolStripItem>  
 <xref:System.Windows.Forms.ToolStripItem.Selected%2A>  
 [Общие сведения об элементе управления ToolStrip](../../../../docs/framework/winforms/controls/toolstrip-control-overview-windows-forms.md)
