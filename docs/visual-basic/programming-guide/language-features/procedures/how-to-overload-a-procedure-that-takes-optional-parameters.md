---
title: "Практическое руководство. Перегрузка процедуры, которая принимает один необязательный параметр (Visual Basic)"
ms.custom: 
ms.date: 07/20/2015
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: devlang-visual-basic
ms.topic: article
helpviewer_keywords:
- procedures [Visual Basic], parameters
- procedure overloading [Visual Basic], optional parameters
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
ms.assetid: 825f9d56-4cde-43fd-993a-b9171717e2eb
caps.latest.revision: "17"
author: dotnet-bot
ms.author: dotnetcontent
ms.openlocfilehash: b4a863944d4f9ab265aab52578fbb704ca376de5
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2017
---
# <a name="how-to-overload-a-procedure-that-takes-optional-parameters-visual-basic"></a>Практическое руководство. Перегрузка процедуры, которая принимает один необязательный параметр (Visual Basic)
Если процедура имеет один или несколько [необязательно](../../../../visual-basic/language-reference/modifiers/optional.md) параметров, нельзя определить перегруженную версию, соответствующую любой из ее неявных перегрузок. Дополнительные сведения см. в разделе «Неявные перегрузки для необязательные параметры» в [вопросы, связанные с перегрузкой процедур](./considerations-in-overloading-procedures.md).  
  
## <a name="one-optional-parameter"></a>Один необязательный параметр  
  
#### <a name="to-overload-a-procedure-that-takes-one-optional-parameter"></a>Перегрузка процедуры, которая принимает один необязательный параметр  
  
1.  Запись `Sub` или `Function` оператора объявления, который содержит дополнительный параметр в списке параметров. Не используйте `Optional` ключевое слово в данной перегруженной версии.  
  
2.  Перед `Sub` или `Function` ключевого слова with [перегрузки](../../../../visual-basic/language-reference/modifiers/overloads.md) ключевое слово.  
  
3.  Напишите код процедуры, который должен выполняться, когда вызывающий код не предоставляет необязательный аргумент.  
  
4.  Завершите процедуру `End Sub` или `End Function` инструкцию соответствующим образом.  
  
5.  Напишите второй оператор объявления, идентичный первому объявлению, за исключением того, что он не включает необязательный параметр в списке параметров.  
  
6.  Напишите код процедуры, который должен выполняться, когда вызывающий код не предоставляет необязательный аргумент. Завершите процедуру `End Sub` или `End Function` инструкцию соответствующим образом.  
  
     В следующем примере показано процедура, определенная с необязательным параметром, аналогичный набор из двух перегруженных процедур и, наконец, примеры и недопустимых перегруженных версий.  
  
     [!code-vb[VbVbcnProcedures#59](./codesnippet/VisualBasic/how-to-overload-a-procedure-that-takes-optional-parameters_1.vb)]  
  
     [!code-vb[VbVbcnProcedures#60](./codesnippet/VisualBasic/how-to-overload-a-procedure-that-takes-optional-parameters_2.vb)]  
  
     [!code-vb[VbVbcnProcedures#61](./codesnippet/VisualBasic/how-to-overload-a-procedure-that-takes-optional-parameters_3.vb)]  
  
## <a name="multiple-optional-parameters"></a>Несколько необязательных параметров  
 Для процедуры с более чем один необязательный параметр обычно необходимо более двух перегруженных версий. Например если имеются два необязательных параметра, а вызывающий код может использовать или опустить каждый из них независимо от другого, необходимо четыре перегруженных версий, один для каждой возможной комбинации предоставленных аргументов.  
  
 С увеличением числа необязательных параметров увеличивает сложность перегрузки. Если некоторые сочетания предоставленных аргументов неприемлемы, для необязательных параметров N требуется 2 ^ N перегруженных версий. В зависимости от процедуры может оказаться, что ясности логики, японской и корейской лишние усилия для определения всех перегруженных версий.  
  
#### <a name="to-overload-a-procedure-that-takes-more-than-one-optional-parameter"></a>Перегрузка процедуры, которая принимает более чем один необязательный параметр  
  
1.  Определите, какие комбинации предоставленных необязательные аргументы, приемлемы для логику процедуры. Недопустимое сочетание может возникнуть, если один необязательный параметр зависит от другого. Например если один параметр принимает имя супруга, а другой принимает возраст супруга, учитывать аргументы возраста, но опустить имя недопустима.  
  
2.  Для каждой допустимой комбинации предоставленных необязательных аргументов напишите `Sub` или `Function` инструкцию объявления, определяющую соответствующий список параметров. Не используйте `Optional` ключевое слово.  
  
3.  В каждое объявление перед `Sub` или `Function` ключевого слова with [перегрузки](../../../../visual-basic/language-reference/modifiers/overloads.md) ключевое слово.  
  
4.  После каждого объявления напишите код процедуры, который должен выполняться, когда вызывающий код передает список аргументов, соответствующий списку параметров в объявлении.  
  
5.  Завершите выполнение каждой процедуры с `End Sub` или `End Function` инструкцию соответствующим образом.  
  
## <a name="see-also"></a>См. также  
 [Процедуры](./index.md)  
 [Параметры и аргументы процедуры](./procedure-parameters-and-arguments.md)  
 [Необязательные параметры](./optional-parameters.md)  
 [Массивы параметров](./parameter-arrays.md)  
 [Перегрузка процедур](./procedure-overloading.md)  
 [Рекомендации по устранению неполадок](./troubleshooting-procedures.md)  
 [Практическое руководство. Определение различных версий процедуры](./how-to-define-multiple-versions-of-a-procedure.md)  
 [Практическое руководство. Вызов перегруженной процедуры](./how-to-call-an-overloaded-procedure.md)  
 [Практическое руководство. Перегрузка процедуры, принимающей неопределенное число параметров](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)  
 [Разрешение перегрузки](./overload-resolution.md)
