---
title: "Unterstützung der sicheren Kommunikation für Dienste in Azure Service Fabric | Microsoft-Dokumentation"
description: "Übersicht über die Möglichkeiten zur Unterstützung der sicheren Kommunikation für Reliable Services, die in einem Azure Service Fabric-Cluster ausgeführt werden."
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: c4634e3d8efb1745fffcfe3e647e43d867038716
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017


---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Unterstützung der Kommunikationssicherung für Dienste in Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java unter Linux](service-fabric-reliable-services-secure-communication-java.md)
>
>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Unterstützung der Sicherung eines Diensts bei der Verwendung von Dienstremoting
Wir verwenden ein vorhandenes [Beispiel](service-fabric-reliable-services-communication-remoting-java.md) , um zu erläutern, wie das Remoting für Reliable Services eingerichtet wird. Um die Sicherung eines Diensts bei der Verwendung von Dienstremoting zu unterstützen, befolgen Sie diese Schritte:

1. Erstellen Sie eine Schnittstelle, `HelloWorldStateless`, die definiert, welche Methoden für einen Remoteprozeduraufruf Ihres Diensts verfügbar sind. Ihr Dienst verwendet `FabricTransportServiceRemotingListener`, der im Paket `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` deklariert wird. Dies ist eine `CommunicationListener` -Implementierung, die Remotingfunktionen bereitstellt.

    ```java
    public interface HelloWorldStateless extends Service {
        CompletableFuture<String> getHelloWorld();
    }

    class HelloWorldStatelessImpl extends StatelessService implements HelloWorldStateless {
        @Override
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
        return listeners;
        }

        public CompletableFuture<String> getHelloWorld() {
            return CompletableFuture.completedFuture("Hello World!");
        }
    }
    ```
2. Fügen Sie Listenereinstellungen und Sicherheitsanmeldeinformationen hinzu.

    Stellen Sie sicher, dass das Zertifikat, das Sie zum Schützen Ihrer Dienstkommunikation verwenden möchten, auf allen Knoten im Cluster installiert wird. Es gibt zwei Möglichkeiten, wie Sie Listenereinstellungen und Sicherheitsanmeldeinformationen bereitstellen können:

   1. Bereitstellung mithilfe eines [Konfigurationspakets](service-fabric-application-model.md):

       Fügen Sie in der Datei „settings.xml“ den Abschnitt `TransportSettings` hinzu.

       ```xml
       <!--Section name should always end with "TransportSettings".-->
       <!--Here we are using a prefix "HelloWorldStateless".-->
        <Section Name="HelloWorldStatelessTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509_2" />
            <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
            <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
        </Section>

       ```

       In diesem Fall sieht die `createServiceInstanceListeners` -Methode wie folgt aus:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        Wenn Sie in der Datei „settings.xml“ den Abschnitt `TransportSettings` ohne Präfix hinzufügen, lädt `FabricTransportListenerSettings` standardmäßig alle Einstellungen aus diesem Abschnitt.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        In diesem Fall sieht die `CreateServiceInstanceListeners` -Methode wie folgt aus:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Wenn Sie Methoden für einen gesicherten Dienst mit dem Remotingstapel aufrufen, statt mit der `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase`-Klasse einen Dienstproxy zu erstellen, verwenden Sie `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.

    Wenn der Clientcode als Teil eines Diensts ausgeführt wird, können Sie das `FabricTransportSettings` -Element aus der Datei „settings.xml“ laden. Erstellen Sie einen TransportSettings-Abschnitt, der dem obigen Dienstcode ähnelt. Nehmen Sie die folgenden Änderungen am Clientcode vor:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```

