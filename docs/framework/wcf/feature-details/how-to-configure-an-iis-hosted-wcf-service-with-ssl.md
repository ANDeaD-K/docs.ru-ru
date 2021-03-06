---
title: "Как настраивать протокол SSL в службе WCF, размещенной в IIS"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: df2fe31f-a4bb-4024-92ca-b74ba055e038
caps.latest.revision: "3"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: b16ca5b4cfe615eedd9e532b12f61394806829bd
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="how-to-configure-an-iis-hosted-wcf-service-with-ssl"></a>Как настраивать протокол SSL в службе WCF, размещенной в IIS
В этом разделе описано, как настроить размещенную в IIS службу WCF для использования безопасности транспорта HTTP. Для безопасности транспорта HTTP требуется, чтобы SSL-сертификат был зарегистрирован в службах IIS. Если SSL-сертификат не установлен, для создания тестового сертификата можно использовать службы IIS. Затем необходимо добавить SSL-привязку для проекта веб-сайта и установить свойства проверки подлинности веб-сайта. Наконец, необходимо настроить службу WCF на использование протокола HTTPS.  
  
### <a name="creating-a-self-signed-certificate"></a>Создание самозаверяющего сертификата  
  
1.  Откройте диспетчер служб IIS (inetmgr.exe) и выберите имя компьютера в левой части представления в виде дерева. В правой части экрана выберите сертификаты сервера  
  
     ![Главный экран диспетчера IIS](../../../../docs/framework/wcf/feature-details/media/mg-inetmgrhome.jpg "mg_INetMgrHome")  
  
2.  В окне сертификатов сервера щелкните **создать самозаверяющий сертификат...** Связь.  
  
     ![Создание самозаверяющего &#45; подписанных сертификатов с IIS](../../../../docs/framework/wcf/feature-details/media/mg-createselfsignedcert.jpg "mg_CreateSelfSignedCert")  
  
3.  Введите понятное имя самозаверяющего сертификата и нажмите кнопку **ОК**.  
  
     ![Создать собственное &#45; Диалоговое окно сертификата подписи](../../../../docs/framework/wcf/feature-details/media/mg-mycert.jpg "mg_MyCert")  
  
     Теперь созданный самозаверяющий сертификат сведения отображаются в **сертификаты сервера** окна.  
  
     ![Окно сертификата сервера](../../../../docs/framework/wcf/feature-details/media/mg-servercertificatewindow.jpg "mg_ServerCertificateWindow")  
  
     Созданный сертификат устанавливается в хранилище доверенных корневых центров сертификации.  
  
### <a name="add-ssl-binding"></a>Добавление привязки SSL  
  
1.  В диспетчере служб IIS разверните **сайтов** папки и затем **веб-сайт по умолчанию** папки в представлении дерева в левой части экрана.  
  
2.  Нажмите кнопку **привязки...** Связать **действия** раздел в правой верхней части окна.  
  
     ![Добавление привязки SSL](../../../../docs/framework/wcf/feature-details/media/mg-addsslbinding.jpg "mg_AddSSLBinding")  
  
3.  В окне «привязки сайта» выберите **добавить** кнопки.  
  
     ![Диалоговое окно привязок сайта](../../../../docs/framework/wcf/feature-details/media/mg-sitebindingsdialog.jpg "mg_SiteBindingsDialog")  
  
4.  В **Добавление привязки сайта** диалоговое окно, выберите https для типа и понятное имя самозаверяющего сертификата, только что создан.  
  
     ![Пример привязки сайта](../../../../docs/framework/wcf/feature-details/media/mg-mycertbinding.jpg "mg_MyCertBinding")  
  
### <a name="configure-virtual-directory-for-ssl"></a>Настройка виртуального каталога для SSL  
  
1.  В диспетчере служб IIS выберите виртуальный каталог, содержащий безопасную службу WCF.  
  
2.  В центральной области окна выберите **параметры SSL** в разделе IIS.  
  
     ![Параметры SSL для виртуального каталога](../../../../docs/framework/wcf/feature-details/media/mg-sslsettingsforvdir.jpg "mg_SSLSettingsForVDir")  
  
3.  На панели параметров SSL установите **Требовать SSL** флажок и нажмите кнопку **применить** ссылку в **действия** раздел на правую часть экрана.  
  
     ![Параметры SSL виртуального каталога](../../../../docs/framework/wcf/feature-details/media/mg-vdirsslsettings.JPG "mg_VDirSSLSettings")  
  
### <a name="configure-wcf-service-for-http-transport-security"></a>Настройка службы WCF для безопасности транспорта HTTP  
  
1.  В файле web.config службы WCF настройте привязку HTTP на использование безопасности транспорта, как показано в следующем фрагменте XML.  
  
    ```xml  
    <bindings>  
          <basicHttpBinding>  
            <binding name="secureHttpBinding">  
              <security mode="Transport">  
                <transport clientCredentialType="None"/>  
              </security>  
            </binding>  
          </basicHttpBinding>  
    </bindings>  
    ```  
  
2.  Укажите службу и конечную точку службы, как показано в следующем фрагменте XML.  
  
    ```xml  
    <services>  
          <service name="MySecureWCFService.Service1">  
            <endpoint address=""  
                      binding="basicHttpBinding"  
                      bindingConfiguration="secureHttpBinding"  
                      contract="MySecureWCFService.IService1"/>  
  
            <endpoint address="mex"  
                      binding="mexHttpsBinding"  
                      contract="IMetadataExchange" />  
          </service>  
    </services>  
    ```  
  
## <a name="example"></a>Пример  
 Ниже приведен полный пример файла web.config для службы WCF, использующей безопасность транспорта HTTP  
  
```xml  
<?xml version="1.0"?>  
<configuration>  
  
  <system.web>  
    <compilation debug="true" targetFramework="4.0" />  
  </system.web>  
  <system.serviceModel>  
    <services>  
      <service name="MySecureWCFService.Service1">  
        <endpoint address=""  
                  binding="basicHttpBinding"  
                  bindingConfiguration="secureHttpBinding"  
                  contract="MySecureWCFService.IService1"/>  
  
        <endpoint address="mex"  
                  binding="mexHttpsBinding"  
                  contract="IMetadataExchange" />  
      </service>  
    </services>  
    <bindings>  
      <basicHttpBinding>  
        <binding name="secureHttpBinding">  
          <security mode="Transport">  
            <transport clientCredentialType="None"/>  
          </security>  
        </binding>  
      </basicHttpBinding>  
    </bindings>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <!-- To avoid disclosing metadata information, set the value below to false and remove the metadata endpoint above before deployment -->  
          <serviceMetadata httpsGetEnabled="true"/>  
          <!-- To receive exception details in faults for debugging purposes, set the value below to true.  Set to false before deployment to avoid disclosing exception information -->  
          <serviceDebug includeExceptionDetailInFaults="false"/>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <serviceHostingEnvironment multipleSiteBindingsEnabled="true" />  
  </system.serviceModel>  
  <system.webServer>  
    <modules runAllManagedModulesForAllRequests="true"/>  
  </system.webServer>  
  
</configuration>  
```  
  
## <a name="see-also"></a>См. также  
 [Размещение в службах IIS](../../../../docs/framework/wcf/feature-details/hosting-in-internet-information-services.md)  
 [Инструкции по размещению в службах IIS](../../../../docs/framework/wcf/samples/internet-information-service-hosting-instructions.md)  
 [Рекомендации по размещению в службах IIS](../../../../docs/framework/wcf/feature-details/internet-information-services-hosting-best-practices.md)  
 [Размещение в службах IIS с использованием встроенного кода](../../../../docs/framework/wcf/samples/iis-hosting-using-inline-code.md)
