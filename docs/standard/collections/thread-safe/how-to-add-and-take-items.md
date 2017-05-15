---
title: "Практическое руководство. Добавление и удаление отдельных элементов коллекции BlockingCollection | Документация Майкрософт"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- thread-safe collections, blocking dictionary
ms.assetid: 38f2f3d8-15e5-4bf4-9c83-2b5b6f22bad1
caps.latest.revision: 7
author: mairaw
ms.author: mairaw
manager: wpickett
translationtype: Human Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: 442e2f9a80c4c0624c90e8a7509cd22f6be9a4a0
ms.lasthandoff: 04/18/2017

---
# <a name="how-to-add-and-take-items-individually-from-a-blockingcollection"></a>Практическое руководство. Добавление и удаление отдельных элементов коллекции BlockingCollection
В этом примере показано, как добавлять и удалять элементы в <xref:System.Collections.Concurrent.BlockingCollection%601> с блокировкой и без блокировки. Дополнительные сведения о классе <xref:System.Collections.Concurrent.BlockingCollection%601>, см. в статье, которая содержит [общие сведения о коллекции BlockingCollection](../../../../docs/standard/collections/thread-safe/blockingcollection-overview.md).  
  
 Пример перечисления коллекции <xref:System.Collections.Concurrent.BlockingCollection%601>, пока она не станет пустой и никакие элементы не будут добавляться, см. в статье [Практическое руководство. Использование оператора ForEach для удаления элементов в коллекции BlockingCollection](../../../../docs/standard/collections/thread-safe/how-to-use-foreach-to-remove.md).  
  
## <a name="example"></a>Пример  
 В первом примере показано, как добавлять и удалять элементы, чтобы операции блокировались, если коллекция временно пуста (при извлечении) или имеет максимальную заполненность (при добавлении), или если заданное время ожидания истекло. Обратите внимание, что блокирование при максимальной заполненности доступно, только если BlockingCollection создана с указанием максимальной емкости в конструкторе.  
  
 [!code-csharp[CDS_BlockingCollection#01](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example01.cs#01)]
 [!code-vb[CDS_BlockingCollection#01](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/simpleblocking.vb#01)]  
  
## <a name="example"></a>Пример  
 Во втором примере показано, как добавлять и извлекать элементы без блокирования операций. Если элементы отсутствуют, достигнута максимальная емкость для связанной коллекции или истекло время ожидания, операция <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> или <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> возвращает значение false. Это позволяет потоку короткое время выполнять некоторые другие полезные действия, а затем повторить попытку позднее, чтобы либо получить новый элемент, либо попытаться добавить тот же элемент, который не удалось добавить ранее. Программа также демонстрирует, как реализовать отмену при доступе к коллекции <xref:System.Collections.Concurrent.BlockingCollection%601>.  
  
 [!code-csharp[CDS_BlockingCollection#02](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example02.cs#02)]
 [!code-vb[CDS_BlockingCollection#02](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/nonblockingbc.vb#02)]  
  
## <a name="see-also"></a>См. также  
 <xref:System.Collections.Concurrent?displayProperty=fullName>   
 [Общие сведения о коллекции BlockingCollection](../../../../docs/standard/collections/thread-safe/blockingcollection-overview.md)