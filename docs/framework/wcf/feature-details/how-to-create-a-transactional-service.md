---
title: "Как создавать транзакционные службы | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 1bd2e4ed-a557-43f9-ba98-4c70cb75c154
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# Как создавать транзакционные службы
В этом примере показаны различные аспекты создания транзакционной службы и использования инициируемых клиентом транзакций для координации операций службы.  
  
### Создание транзакционной службы  
  
1.  Создайте контракт службы и аннотируйте операции с выбранным параметром из перечисления <xref:System.ServiceModel.TransactionFlowOption>, чтобы задать требования входящих транзакций.Обратите внимание, что в реализуемый класс службы также можно включить атрибут <xref:System.ServiceModel.TransactionFlowAttribute>.Это позволит одной реализации интерфейса, а не всем реализациям, использовать эти параметры.  
  
    ```  
    [ServiceContract]  
    public interface ICalculator  
    {  
        [OperationContract]  
        // Use this to require an incoming transaction  
        [TransactionFlow(TransactionFlowOption.Mandatory)]  
        double Add(double n1, double n2);  
        [OperationContract]  
        // Use this to permit an incoming transaction  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        double Subtract(double n1, double n2);  
    }  
    ```  
  
2.  Создайте класс реализации и воспользуйтесь атрибутом <xref:System.ServiceModel.ServiceBehaviorAttribute>, чтобы задать значения свойств <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> и <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> \(необязательно\).Обратите внимание, что во многих случаях можно использовать значения по умолчанию для свойств <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> \(60 секунд\) и <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> \(`Unspecified`\).Для каждой операции можно с помощью атрибута <xref:System.ServiceModel.OperationBehaviorAttribute> определить, должны ли операции, расположенные внутри метода, выполняться в области действия транзакции в соответствии со значением атрибута <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A>.В этом случае транзакция, используемая для метода `Add`, будет совпадать с обязательной входящей транзакцией, которая поступает от клиента, а транзакция, используемая для метода `Subtract`, либо совпадает со входящей транзакцией, если она поступила от клиента, либо является новой созданной явно локальной транзакцией.  
  
    ```  
    [ServiceBehavior(  
        TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable,  
        TransactionTimeout = "00:00:45")]  
    public class CalculatorService : ICalculator  
    {  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Add(double n1, double n2)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Adding {0} to {1}", n1, n2));  
            return n1 + n2;  
        }  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Subtract(double n1, double n2)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Subtracting {0} from {1}", n2, n1));  
            return n1 - n2;  
        }  
  
        private static void RecordToLog(string recordText)  
        {  
            // Database operations omitted for brevity  
            // This is where the transaction provides specific benefit  
            // - changes to the database will be committed only when  
            // the transaction completes.  
        }  
    }  
    ```  
  
3.  Настройте привязки в файле конфигурации, указав, что контекст транзакций должен передаваться, и необходимые для этого протоколы.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][Конфигурация транзакции ServiceModel](../../../../docs/framework/wcf/feature-details/servicemodel-transaction-configuration.md).В частности тип привязки задается в атрибуте `binding` элемента конечной точки.Элемент [\<endpoint\>](http://msdn.microsoft.com/ru-ru/13aa23b7-2f08-4add-8dbf-a99f8127c017) содержит атрибут `bindingConfiguration`, который ссылается на конфигурацию привязки с именем `transactionalOleTransactionsTcpBinding`, как показано в следующем примере конфигурации.  
  
    ```  
    <service name="CalculatorService">  
      <endpoint address="net.tcp://localhost:8008/CalcService"  
        binding="netTcpBinding"  
        bindingConfiguration="transactionalOleTransactionsTcpBinding"  
        contract="ICalculator"  
        name="OleTransactions_endpoint" />  
    </service>  
    ```  
  
     Поток транзакций настраивается на уровне конфигурации с помощью атрибута `transactionFlow`, а протокол транзакций задается с помощью атрибута `transactionProtocol`, как показано в следующей конфигурации.  
  
    ```  
    <bindings>  
      <netTcpBinding>  
        <binding name="transactionalOleTransactionsTcpBinding"  
          transactionFlow="true"  
          transactionProtocol="OleTransactions"/>  
      </netTcpBinding>  
    </bindings>  
    ```  
  
### Поддержка нескольких протоколов транзакций  
  
1.  Для достижения оптимальной производительности в сценариях, включающих клиенты и серверы, написанные с помощью [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)], следует использовать протокол OleTransactions.Однако в сценариях, требующих взаимодействия со сторонним стеком протоколов, удобно использовать протокол WS\-AtomicTransaction \(WS\-AT\).Службы [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] можно настроить таким образом, чтобы они поддерживали оба протокола. Для этого необходимо задать несколько конечных точек с соответствующими привязками, как показано в следующем примере конфигурации.  
  
    ```  
    <service name="CalculatorService">  
      <endpoint address="http://localhost:8000/CalcService"  
        binding="wsHttpBinding"  
        bindingConfiguration="transactionalWsatHttpBinding"  
        contract="ICalculator"  
        name="WSAtomicTransaction_endpoint" />  
      <endpoint address="net.tcp://localhost:8008/CalcService"  
        binding="netTcpBinding"  
        bindingConfiguration="transactionalOleTransactionsTcpBinding"  
        contract="ICalculator"  
        name="OleTransactions_endpoint" />  
    </service>  
    ```  
  
     Протокол транзакций задается с помощью атрибута `transactionProtocol`.Однако в предоставляемой системой привязке `wsHttpBinding` этого атрибута нет, поскольку эта привязка может использовать только протокол WS\-AT.  
  
    ```  
    <bindings>  
      <wsHttpBinding>  
        <binding name="transactionalWsatHttpBinding"  
          transactionFlow="true" />  
      </wsHttpBinding>  
      <netTcpBinding>  
        <binding name="transactionalOleTransactionsTcpBinding"  
          transactionFlow="true"  
          transactionProtocol="OleTransactions"/>  
      </netTcpBinding>  
    </bindings>  
    ```  
  
### Управление завершением транзакций  
  
1.  По умолчанию операции [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] автоматически завершают транзакции, если не создаются необработанные исключения.Такое поведение можно изменить с помощью свойства <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> и метода <xref:System.ServiceModel.OperationContext.SetTransactionComplete%2A>.Если операция должна произойти в той же транзакции, что и другая операция \(например, операции дебета и кредита\), можно отключить автоматическое завершение, задав для свойства <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> значение `false`, как показано в приведенном ниже примере операции `Debit`.Транзакция, используемая операцией `Debit`, остается незавершенной, пока не будет вызван метод, у которого свойство <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> имеет значение `true`, как показано в операции `Credit1`, или пока не будет вызван метод <xref:System.ServiceModel.OperationContext.SetTransactionComplete%2A>, явным образом показывающий, что транзакция завершена, как показано в операции `Credit2`.Обратите внимание, что две операции кредита показаны в этом примере для иллюстрации, и в реальной ситуации обычно используется одна операция кредита.  
  
    ```  
    [ServiceBehavior]  
    public class CalculatorService : IAccount  
    {  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = false)]  
        public void Debit(double n)  
        {  
            // Perform debit operation  
  
            return;  
        }  
  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = true)]  
        public void Credit1(double n)  
        {  
            // Perform credit operation  
  
            return;  
        }  
  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = false)]  
        public void Credit2(double n)  
        {  
            // Perform alternate credit operation  
  
            OperationContext.Current.SetTransactionComplete();  
            return;  
        }  
    }  
    ```  
  
2.  Если для согласования транзакций свойству <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> присваивается значение `false`, необходимо использовать привязку с отслеживанием состояния.Это требование задается с помощью свойства `SessionMode` класса <xref:System.ServiceModel.ServiceContractAttribute>.  
  
    ```  
    [ServiceContract(SessionMode = SessionMode.Required)]  
    public interface IAccount  
    {  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Debit(double n);  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Credit1(double n);  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Credit2(double n);  
    }  
    ```  
  
### Управление временем существования экземпляра транзакционной службы  
  
1.  Чтобы указать, нужно ли освобождать соответствующий экземпляр службы при завершении транзакции, в [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] используется свойство <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A>.Поскольку, если не задано иное значение, это свойство по умолчанию имеет значение `true`, в [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] реализован эффективный и предсказуемый механизм активации, срабатывающий в нужное время.При вызове службы в последующей транзакции используется новый экземпляр службы, который не содержит признаков состояний предыдущих транзакций.Хотя такой подход зачастую бывает удобным, иногда может возникнуть необходимость сохранения состояния службы после завершения транзакции.Например, такая ситуация возникает, если извлечение или восстановление требуемого состояния или дескриптора требует большого объема ресурсов.В этом случае можно задать для свойства <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A> значение `false`.Если этот параметр задан, экземпляр и связанное состояние будут доступны в последующих вызовах.При использовании этого сценария необходимо внимательно следить за тем, как и когда происходят очистка и завершение состояний и транзакций.В следующем примере показано, как реализовать это, поддерживая экземпляр с помощью переменной `runningTotal`.  
  
    ```  
    [ServiceBehavior(TransactionIsolationLevel = [ServiceBehavior(  
        ReleaseServiceInstanceOnTransactionComplete = false)]  
    public class CalculatorService : ICalculator  
    {  
        double runningTotal = 0;  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Add(double n)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Adding {0} to {1}", n, runningTotal));  
            runningTotal = runningTotal + n;  
            return runningTotal;  
        }  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Subtract(double n)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Subtracting {0} from {1}", n, runningTotal));  
            runningTotal = runningTotal - n;  
            return runningTotal;  
        }  
  
        private static void RecordToLog(string recordText)  
        {  
            // Database operations omitted for brevity  
        }  
    }  
    ```  
  
    > [!NOTE]
    >  Поскольку время существования экземпляра определяется внутри службы и управляется через свойство <xref:System.ServiceModel.ServiceBehaviorAttribute>, чтобы задать поведение экземпляра, изменять конфигурацию службы или контракт службы не требуется.Кроме того, это не отразится на сети.