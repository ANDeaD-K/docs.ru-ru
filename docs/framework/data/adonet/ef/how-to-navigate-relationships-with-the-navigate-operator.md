---
title: "Практическое руководство. Переход по отношениям с помощью оператора Navigate"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-ado
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
ms.assetid: 79996d2d-9b03-4a9d-82cc-7c5e7c2ad93d
caps.latest.revision: "3"
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: dotnet
ms.openlocfilehash: 20451f661bd4bcd2338af81bef0cf1d07b0a5637
ms.sourcegitcommit: c0dd436f6f8f44dc80dc43b07f6841a00b74b23f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-navigate-relationships-with-the-navigate-operator"></a>Практическое руководство. Переход по отношениям с помощью оператора Navigate
В этом подразделе показано выполнение команды для концептуальной модели с помощью объекта <xref:System.Data.EntityClient.EntityCommand>, а также получение результатов <xref:System.Data.Metadata.Edm.RefType> с помощью <xref:System.Data.EntityClient.EntityDataReader>.  
  
### <a name="to-run-the-code-in-this-example"></a>Выполнение кода в этом примере  
  
1.  Добавить [модели AdventureWorks Sales](http://msdn.microsoft.com/library/f16cd988-673f-4376-b034-129ca93c7832) в проект и настроить проект для использования [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]. Дополнительные сведения см. в разделе [как: использовать мастер моделей EDM](http://msdn.microsoft.com/library/dadb058a-c5d9-4c5c-8b01-28044112231d).  
  
2.  На странице кода приложения добавьте следующие инструкции `using` (`Imports` в Visual Basic):  
  
     [!code-csharp[DP EntityServices Concepts#Namespaces](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts/cs/source.cs#namespaces)]
     [!code-vb[DP EntityServices Concepts#Namespaces](../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp entityservices concepts/vb/source.vb#namespaces)]  
  
## <a name="example"></a>Пример  
 В следующем примере показано, как для перехода по связям в [!INCLUDE[esql](../../../../../includes/esql-md.md)] с помощью [NAVIGATE](../../../../../docs/framework/data/adonet/ef/language-reference/navigate-entity-sql.md) оператор. `Navigate` Оператор принимает следующие параметры: экземпляр сущности, тип связи, конец связи и начало связи. При необходимости можно передать только экземпляр сущности и тип связи `Navigate` оператор.  
  
 [!code-csharp[DP EntityServices Concepts#NavigateWithNavOperatorWithEntityCommand](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts/cs/source.cs#navigatewithnavoperatorwithentitycommand)]
 [!code-vb[DP EntityServices Concepts#NavigateWithNavOperatorWithEntityCommand](../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp entityservices concepts/vb/source.vb#navigatewithnavoperatorwithentitycommand)]  
  
## <a name="see-also"></a>См. также  
 [Поставщик EntityClient для Entity Framework](../../../../../docs/framework/data/adonet/ef/entityclient-provider-for-the-entity-framework.md)  
 [Язык Entity SQL](../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-language.md)
