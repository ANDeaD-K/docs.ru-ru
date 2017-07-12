---
title: "Одноранговые распознаватели | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: d86d12a1-7358-450f-9727-b6afb95adb9c
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# Одноранговые распознаватели
Чтобы подключиться к сетке, одноранговому узлу требуются IP\-адреса других узлов.  Получение IP\-адресов обычно происходит в результате обращения к службе арбитра, которая принимает идентификатор сетки и возвращает список адресов, соответствующих узлам, зарегистрированным для конкретного идентификатора сетки.  Арбитр сохраняет список зарегистрированных адресов, которые он создает при регистрации в службе всех узлов сетки.  
  
 С помощью свойства `Resolver` привязки <xref:System.ServiceModel.NetPeerTcpBinding> можно задать конкретную службу однорангового распознавателя для использования.  
  
## Поддерживаемые одноранговые распознаватели  
 Одноранговые каналы поддерживают распознаватели двух типов: протокол PNRP и пользовательские службы распознавателя.  
  
 По умолчанию для обнаружения одноранговых и соседних узлов в сетке используется служба однорангового распознавателя PNRP.  В ситуациях и на платформах, где служба PNRP недоступна или нереализуема, [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] предоставляет альтернативную службу обнаружения на базе сервера \- <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService>.  Кроме того, можно явным образом определить пользовательскую службу распознавателя, написав класс, который реализует интерфейс <xref:System.ServiceModel.PeerResolvers.IPeerResolverContract>.  
  
### Протокол PNRP  
 Протокол PNRP, являющийся распознавателем по умолчанию для [!INCLUDE[wv](../../../../includes/wv-md.md)], представляет собой распределенную бессерверную службу распознавания имен.  Протокол PNRP также можно использовать в [!INCLUDE[wxpsp2](../../../../includes/wxpsp2-md.md)], установив пакет Advanced Networking Pack.  Любые два клиента, выполняющие одну и ту же версию PNRP, могут находить друг друга с помощью этого протокола, если соблюдаются определенные условия \(например, между клиентами нет брандмауэра\).  Обратите внимание, что версия протокола PNRP, входящая в состав [!INCLUDE[wv](../../../../includes/wv-md.md)], новее версии, включенной в пакет Advanced Networking Pack.  Проверьте Центр загрузки Майкрософт на наличие обновлений протокола PNRP для [!INCLUDE[wxpsp2](../../../../includes/wxpsp2-md.md)].  
  
### Пользовательские службы распознавателя  
 Если служба PNRP недоступна, или если требуются возможности управления структурой сетки, можно применять пользовательские службы распознавателя на базе сервера.  Можно явным образом определить эту службу путем написания класса распознавателя, реализующего интерфейс <xref:System.ServiceModel.PeerResolvers.IPeerResolverContract>, или с помощью готовой реализации по умолчанию <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService>.  
  
 При использовании реализации службы по умолчанию срок регистрации клиента истекает через заданный промежуток времени, если клиент явным образом не обновляет регистрацию.  Клиенты, использующие службу распознавателя, должны учитывать максимальное время задержки при взаимодействии между клиентом и сервером, чтобы вовремя обновлять регистрацию.  Для этого необходимо установить в службе распознавателя соответствующее значение времени ожидания обновления \(`RefreshInterval`\).  \(Дополнительные сведения см. в разделе [Подробная информация о CustomPeerResolverService: регистрация клиентов](../../../../docs/framework/wcf/feature-details/inside-the-custompeerresolverservice-client-registrations.md).\)  
  
 Кроме того, разработчик приложения должен подумать о защите подключений между клиентами и пользовательской службой распознавателя.  Для этого можно воспользоваться параметрами безопасности привязки <xref:System.ServiceModel.NetTcpBinding>, которую клиенты используют для связи со службой распознавателя.  Необходимо задать учетные данные \(если они используются\) в объекте `ChannelFactory`, который используется для создания однорангового канала.  Эти учетные данные передаются в объект `ChannelFactory`, который используется для создания каналов пользовательского распознавателя.  
  
> [!NOTE]
>  При использовании локальных или импровизированных сетей с пользовательским распознавателем настоятельно рекомендуется включать в приложения, которые используют или поддерживают локальные и импровизированные сети, механизм для выбора при подключении одного локального адреса.  Это позволит избежать ошибок, связанных с компьютерами с несколькими локальными адресами.  Поэтому одноранговый канал поддерживает одновременное использование только одного локального адреса.  Этот адрес можно указать в свойстве `ListenIpAddress` привязки <xref:System.ServiceModel.NetPeerTcpBinding>.  
  
 Пример реализации пользовательского распознавателя см. в разделе [Peer Channel Custom Peer Resolver](http://msdn.microsoft.com/ru-ru/5b75a2bb-7ff1-4a14-abe7-3debf0537d23).  
  
## Содержание  
 [Подробная информация о CustomPeerResolverService: регистрация клиентов](../../../../docs/framework/wcf/feature-details/inside-the-custompeerresolverservice-client-registrations.md)  
  
## См. также  
 [Основные понятия одноранговых каналов](../../../../docs/framework/wcf/feature-details/peer-channel-concepts.md)   
 [Безопасность одноранговых каналов](../../../../docs/framework/wcf/feature-details/peer-channel-security.md)   
 [Создание приложения одноранговых каналов](../../../../docs/framework/wcf/feature-details/building-a-peer-channel-application.md)