---
title: "Übersicht über die Konfiguration von Azure Service Fabric Reliable Actors FabricTransport | Microsoft Docs"
description: "Erfahren Sie mehr über das Konfigurieren der Kommunikationseinstellungen von Actors in Azure Service Fabric."
services: Service-Fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: 
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/22/2016
ms.author: suchia
translationtype: Human Translation
ms.sourcegitcommit: a395d0653a72883b1901824ba25d40f673276d3f
ms.openlocfilehash: 0b43ba67ea30772054c513b47c31642baaa8b9e7


---
# <a name="configuring-fabrictransport-settings-for-reliable-actors"></a>Konfigurieren von FabricTransport-Einstellungen für Reliable Actors

Unter [FabrictTansportSettings](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.fabrictransport.common.fabrictransportsettings) finden Sie die Liste der Einstellungen, die Benutzer konfigurieren können.

Sie können die Standardkonfiguration von FabricTransport auf folgende Weise ändern.

1.  Mithilfe des Assemblyattributs ‒ [FabricTransportActorRemotingProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.actors.remoting.fabrictransport.fabrictransportactorremotingproviderattribute?redirectedfrom=MSDN#microsoft_servicefabric_actors_remoting_fabrictransport_fabrictransportactorremotingproviderattribute)

  Dieses Attribut muss für die Actorclient- und die Actordienstassembly angewendet werden.
  Das folgende Beispiel zeigt, wie der Standardwert der Einstellungen „FabricTransport“ und „OperationTimeoutInSeconds“ geändert werden kann.

  ```csharp
     using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600)]
   ```

   Das zweite Beispiel ändert die Standardwerte der FabricTransport MaxMessageSize und OperationTimeoutInSeconds

    ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600,MaxMessageSize = 134217728)]
    ```

2. Mithilfe des [Konfigurationspakets](service-fabric-application-model.md):

  * Konfigurieren der FabricTransport-Einstellungen für den Actordienst

    Fügen Sie den Abschnitt „TransportSettings“ zur Datei „settings.xml“ hinzu.

    * SectionName: Der Actorcode sucht standardmäßig im Format „&lt;Actorname&gt;TransportSettings“ nach SectionName. Wenn dieser nicht gefunden wird, wird im Format „TransportSettings“ nach SectionName gesucht.

  ```xml
  <Section Name="MyActorServiceTransportSettings">
       <Parameter Name="MaxMessageSize" Value="10000000" />
       <Parameter Name="OperationTimeoutInSeconds" Value="300" />
       <Parameter Name="SecurityCredentialsType" Value="X509" />
       <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
       <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
       <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
       <Parameter Name="CertificateStoreName" Value="My" />
       <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
       <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
   </Section>
  ```

  * Konfigurieren der FabricTransport-Einstellungen für die Actorclientassembly

    Wenn der Client nicht als Teil eines Diensts ausgeführt wird, können Sie die XML-Datei „&lt;Name der ausführbaren Clientdatei&gt;.settings.xml“ am gleichen Speicherort erstellen, an dem sich auch die ausführbare Clientdatei befindet. Fügen Sie dann den Abschnitt „TransportSettings“ zu dieser Datei hinzu. SectionName muss „TransportSettings“ lauten.

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
    <Section Name="TransportSettings">
      <Parameter Name="SecurityCredentialsType" Value="X509" />
       <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
      <Parameter Name="CertificateFindValue" Value="78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf" />
       <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
      <Parameter Name="CertificateStoreName" Value="My" />
      <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Alice" />
    </Section>
  </Settings>
   ```



<!--HONumber=Nov16_HO4-->


