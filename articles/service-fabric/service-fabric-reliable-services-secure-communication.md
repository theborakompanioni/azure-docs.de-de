<properties
   pageTitle="Unterstützung der sicheren Kommunikation für Dienste in Service Fabric | Microsoft Azure"
   description="Übersicht über die Möglichkeiten zur Unterstützung der sicheren Kommunikation für Reliable Services, die in einem Azure Service Fabric-Cluster ausgeführt werden."
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

# Unterstützung der Kommunikationssicherung für Dienste in Azure Service Fabric

Sicherheit ist einer der wichtigsten Aspekte der Kommunikation. Das Reliable Services-Anwendungsframework stellt einige fertige Kommunikationsstapel und Tools bereit, die Sie verwenden können, um die Sicherheit zu verbessern. In diesem Artikel wird erläutert, wie Sie die Sicherheit verbessern, wenn Sie Dienstremoting und den Windows Communication Foundation-Kommunikationsstapel (WCF) verwenden.

## Unterstützung der Sicherung eines Diensts bei der Verwendung von Dienstremoting

Wir verwenden ein vorhandenes [Beispiel](service-fabric-reliable-services-communication-remoting.md), um zu erläutern, wie das Remoting für Reliable Services eingerichtet wird. Um die Sicherung eines Diensts bei der Verwendung von Dienstremoting zu unterstützen, befolgen Sie diese Schritte:

1. Erstellen Sie eine Schnittstelle, `IHelloWorldStateful`, die definiert, welche Methoden für einen Remoteprozeduraufruf Ihres Diensts verfügbar sind. Außerdem verwendet Ihr Dienst `FabricTransportServiceRemotingListener`, der im Namespace `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` deklariert wird. Dies ist eine `ICommunicationListener`-Implementierung, die Remotingfunktionen bereitstellt.

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

2. Fügen Sie Listenereinstellungen und Sicherheitsanmeldeinformationen hinzu.

    Stellen Sie sicher, dass das Zertifikat, das Sie zum Schützen Ihrer Dienstkommunikation verwenden möchten, auf allen Knoten im Cluster installiert wird. Es gibt zwei Möglichkeiten, wie Sie Listenereinstellungen und Sicherheitsanmeldeinformationen bereitstellen können:

    1. Direkte Bereitstellung im Dienstcode:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
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
    2. Bereitstellung mithilfe eines [Konfigurationspakets](service-fabric-application-model.md):

        Fügen Sie in der Datei „settings.xml“ den Abschnitt `TransportSettings` hinzu.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
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
         <!--"TransportSettings" section without any prefix.-->
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

3. Wenn Sie Methoden für einen gesicherten Dienst mit dem Remotingstapel aufrufen, statt mit der `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy`-Klasse einen Dienstproxy zu erstellen, verwenden Sie `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Übergeben Sie `FabricTransportSettings`, worin `SecurityCredentials` enthalten ist.

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

    Wenn der Clientcode als Teil eines Diensts ausgeführt wird, können Sie das `FabricTransportSettings`-Element aus der Datei „settings.xml“ laden. Erstellen Sie einen TransportSettings-Abschnitt, der dem obigen Dienstcode ähnelt. Nehmen Sie die folgenden Änderungen am Clientcode vor:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Wenn der Client nicht als Teil eines Diensts ausgeführt wird, können Sie die Datei „client\_name.settings.xml“ an dem gleichen Speicherort erstellen, an dem sich auch die Datei „client\_name.exe“ befindet. Erstellen Sie dann einen Abschnitt „TransportSettings“ in dieser Datei.

    Ähnlich wie beim Dienst gilt Folgendes: Wenn Sie in der Datei „client settings.xml“/„client\_name.settings.xml“ den Abschnitt `TransportSettings` ohne Präfix hinzufügen, lädt `FabricTransportSettings` standardmäßig alle Einstellungen aus diesem Abschnitt.

    In diesem Fall wird der frühere Code noch weiter vereinfacht:

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## Unterstützung der Sicherung eines Diensts, wenn Sie einen WCF-basierten Kommunikationsstapel verwenden

Wir verwenden ein vorhandenes [Beispiel](service-fabric-reliable-services-communication-wcf.md), um zu erläutern, wie der WCF-basierte Kommunikationsstapel für Reliable Services eingerichtet wird. Um die Sicherung eines Diensts zu unterstützen, wenn Sie einen WCF-basierten Kommunikationsstapel verwenden, befolgen Sie diese Schritte:

1. Für den Dienst müssen Sie die Sicherung des WCF-Kommunikationslisteners (`WcfCommunicationListener`) unterstützen, den Sie erstellen. Ändern Sie dazu die `CreateServiceReplicaListeners`-Methode.

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
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. Auf dem Client bleibt die `WcfCommunicationClient`-Klasse, die wir im vorherigen [Beispiel](service-fabric-reliable-services-communication-wcf.md) erstellt haben, unverändert. Sie müssen aber die `CreateClientAsync`-Methode von `WcfCommunicationClientFactory` überschreiben:

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
            // These credentials will be used by the clients created by
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

    Verwenden Sie `SecureWcfCommunicationClientFactory`, um einen WCF-Kommunikationsclient (`WcfCommunicationClient`) zu erstellen. Verwenden Sie den Client, um Dienstmethoden aufzurufen.

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

<!---HONumber=AcomDC_0420_2016-->