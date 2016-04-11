<properties
   pageTitle="Sichere Kommunikation für Dienste in Service Fabric | Microsoft Azure"
   description="Übersicht über die Möglichkeiten zum Schützen der Kommunikation für Reliable Services, die im Service Fabric-Cluster ausgeführt werden."
   services="service-fabric"
   documentationCenter=".net"
   authors="punewa"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="03/22/2016"
   ms.author="punewa"/>

# Sichere Kommunikation für Dienste in Service Fabric
Sicherheit ist einer der wichtigsten Aspekte der Kommunikation. Mit dem Reliable Services-Anwendungsframework werden einige fertige Kommunikationsstapel und Tools bereitgestellt, die Sie verwenden können. In diesem Artikel wird beschrieben, wie Sie die Sicherheit einrichten, wenn Sie das Dienstremoting und den WCF-Kommunikationsstapel verwenden.

## Schützen des Diensts beim Verwenden des Dienstremotings
Führen Sie zum Einrichten des sicheren Remotings für einen Dienst die folgenden Schritte aus:

1. Wir verwenden ein vorheriges [Beispiel](service-fabric-reliable-services-communication-remoting.md), in dem es darum geht, wie zuverlässiges Dienstremoting eingerichtet wird. Wir beginnen mit der Schnittstelle `IHelloWorldStateful`, mit der die Methoden definiert werden, die für den Remoteprozeduraufruf für Ihren Dienst verfügbar sind. Außerdem wird für den Dienst ein `FabricTransportServiceRemotingListener`-Element verwendet, das im Namespace `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` deklariert wird. Dies ist eine `ICommunicationListener`-Implementierung, die Remotingfunktionen bereitstellt.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Fügen Sie Listener-Einstellungen und Sicherheitsanmeldeinformationen hinzu.

    Das Zertifikat, das Sie zum Schützen Ihrer Dienstkommunikation verwenden möchten, sollte auf allen Knoten im Cluster installiert werden. Es gibt zwei Möglichkeiten, wie Sie Listener-Einstellungen und Sicherheitsanmeldeinformationen bereitstellen können.

    1. Direkt im Dienstcode:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listnerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listnerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Mit dem [Konfigurationspaket](service-fabric-application-model.md):

        Fügen Sie in „settings.xml“ den Abschnitt `TransportSettings` hinzu.

        ```xml
        <!--Section name should always end with "TransportSettings"-->
        <!--Here we are using a prefix "HelloWorldStateful"-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        In diesem Fall sieht die `CreateServiceReplicaListeners`-Methode wie folgt aus:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Wenn Sie in der Datei „settings.xml“ den Abschnitt `TransportSettings` ohne Präfix hinzufügen, lädt `FabricTransportListenerSettings` standardmäßig alle Einstellungen aus diesem Abschnitt.

         ```xml
         <!--"TransportSettings" section without any prefix-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         In diesem Fall sieht die `CreateServiceReplicaListeners`-Methode wie folgt aus:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Beim Aufrufen von Methoden für einen sicheren Dienst mit dem Remotingstapel verwenden wir anstelle der `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy`-Klasse zum Erstellen eines Dienstproxys `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory` und übergeben das `FabricTransportSettings`-Element, das die `SecurityCredentials` enthält.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Wenn der Clientcode als Teil eines Diensts ausgeführt wird, können Sie das `FabricTransportSettings`-Element aus der Datei „settings.xml“ laden. Erstellen Sie einen TransportSettings-Abschnitt, der dem obigen Dienstcode ähnelt. Nehmen Sie die folgenden Änderungen am Clientcode vor.

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Wenn der Client nicht als Teil eines Diensts ausgeführt wird, können Sie die Datei „client\_name.settings.xml“ an demselben Speicherort erstellen, an dem sich auch die Datei „client\_name.exe“ befindet, und in dieser Datei dann einen Abschnitt „TransportSettings“ erstellen.

    Ähnlich wie beim Dienst gilt Folgendes: Wenn Sie in der Datei „client settings.xml“/„client\_name.settings.xml“ auf dem Client den Abschnitt `TransportSettings` ohne *Präfix* hinzufügen, werden mit `FabricTransportSettings` standardmäßig alle Einstellungen aus diesem Abschnitt geladen.

    In diesem Fall wird der obige Code noch weiter vereinfacht.

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## Schützen des Diensts bei Verwendung eines WCF-basierten Kommunikationsstapels

Wir verwenden ein vorheriges [Beispiel](service-fabric-reliable-services-communication-wcf.md), mit dem veranschaulicht wird, wie Sie eine WCF-basierte Kommunikation für Reliable Services einrichten und erweitern, um sie sicher zu machen.

1. Für den Dienst müssen wir ein `WcfCommunicationListener`-Element erstellen, das sicher ist. Hierfür müssen wir die `CreateServiceReplicaListeners`-Methode ändern.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListner = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example we will be using NetTcpBinding
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the servicehost credentials.
        wcfCommunicationListner.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListner;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```
    
2. Auf dem Client bleibt die `WcfCommunicationClient`-Klasse, die wir im vorherigen [Beispiel](service-fabric-reliable-services-communication-wcf.md) erstellt haben, unverändert. Wir müssen aber die `CreateClientAsync`-Methode von `WcfCommunicationClientFactory` überschreiben.

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by the
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Verwenden Sie `SecureWcfCommunicationClientFactory`, um ein `WcfCommunicationClient`-Element zu erstellen. Verwenden Sie den Client, um Dienstmethoden aufzurufen.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## Nächste Schritte

* [Web-API mit OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)

<!---HONumber=AcomDC_0330_2016-->