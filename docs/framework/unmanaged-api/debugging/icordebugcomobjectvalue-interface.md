---
title: "Интерфейс ICorDebugComObjectValue Interface"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name:
- ICorDebugComObjectValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugComObjectValue
helpviewer_keywords:
- ICorDebugComObjectValue interface [.NET Framework debugging]
ms.assetid: 505a7f6c-d92b-42b4-b539-433f5102ea9b
topic_type:
- apiref
caps.latest.revision: 
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.workload:
- dotnet
ms.openlocfilehash: 9adeec14e102ef575f24566980477a285f87ba40
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="icordebugcomobjectvalue-interface"></a>Интерфейс ICorDebugComObjectValue Interface
Предоставляет методы для получения сведений, связанных с вызываемой оболочки времени выполнения (RCW).  
  
## <a name="methods"></a>Методы  
  
|Метод|Описание:|  
|------------|-----------------|  
|[Метод GetCachedInterfacePointers](../../../../docs/framework/unmanaged-api/debugging/icordebugcomobjectvalue-getcachedinterfacepointers-method.md)|Возвращает интерфейс необработанные указатели, кэшируются на текущем вызываемая оболочка времени Выполнения.|  
|[Метод GetCachedInterfaceTypes](../../../../docs/framework/unmanaged-api/debugging/icordebugcomobjectvalue-getcachedinterfacetypes-method.md)|Предоставляет перечислитель для типов интерфейсов, что текущий объект был случаем и или использовать в качестве.|  
  
## <a name="remarks"></a>Примечания  
 Чтобы проверить, представляет ли экземпляр интерфейса «ICorDebugValue» RCW, отладчик вызывает `QueryInterface` на «ICorDebugValue» с `IID_ICorDebugComObjectValue`.  
  
## <a name="requirements"></a>Требования  
 **Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** CorDebug.idl, CorDebug.h  
  
 **Библиотека:** CorGuids.lib  
  
 **Версии платформы .NET framework:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>См. также  
 [Интерфейсы отладки](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)  
 [Отладка](../../../../docs/framework/unmanaged-api/debugging/index.md)
