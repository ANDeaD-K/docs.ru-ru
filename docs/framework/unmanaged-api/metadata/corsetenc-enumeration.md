---
title: "Перечисление CorSetENC"
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
- CorSetENC
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorSetENC
helpviewer_keywords:
- CorSetENC enumeration [.NET Framework metadata]
ms.assetid: fe4150e8-071d-43fb-8e06-c3c616dbeed2
topic_type:
- apiref
caps.latest.revision: 
author: mairaw
ms.author: mairaw
manager: wpickett
ms.workload:
- dotnet
ms.openlocfilehash: 0f39a1481361377fce3f6b55cf6c7daf8c075ce5
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="corsetenc-enumeration"></a>Перечисление CorSetENC
Содержит значения, используемые для оказания влияния на поведение во время создания метаданных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
typedef enum CorSetENC {  
  
    MDSetENCOn                  = 0x00000001,  
    MDSetENCOff                 = 0x00000002,  
  
    MDUpdateENC                 = 0x00000001,  
    MDUpdateFull                = 0x00000002,  
    MDUpdateExtension           = 0x00000003,  
    MDUpdateIncremental         = 0x00000004,  
    MDUpdateDelta               = 0x00000005,  
    MDUpdateMask                = 0x00000007  
  
} CorSetENC;  
```  
  
## <a name="members"></a>Участники  
  
|Член|Описание:|  
|------------|-----------------|  
|`MDSetENCOn`|Является устаревшей.|  
|`MDSetENCOff`|Является устаревшей.|  
|`MDUpdateENC`|Указывает, тогда как метаданные могут быть обновлены, маркеров не может быть перемещен.|  
|`MDUpdateFull`|Указывает, что токены можно переместить, во время обновления.|  
|`MDUpdateExtension`|Указывает, что обновления могут состоять только из добавлений. Маркеры не могут быть перемещены.|  
|`MDUpdateIncremental`|Указывает добавочной компиляции.|  
|`MDUpdateDelta`|Указывает, что только измененные метаданные должны сохраняться.|  
|`MDUpdateMask`|Включает в себя `MDUpdateENC`, `MDUpdateFull` и `MDUpdateIncremental`.|  
  
## <a name="requirements"></a>Требования  
 **Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** CorHdr.h  
  
 **Версии платформы .NET framework:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>См. также  
 [Перечисления метаданных](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
