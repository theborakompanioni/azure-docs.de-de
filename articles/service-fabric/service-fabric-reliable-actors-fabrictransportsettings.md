---
title: "Ändern der FabricTransport-Einstellungen in Azure-Microservices | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über das Konfigurieren der Kommunikationseinstellungen von Akteuren in Azure Service Fabric."
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
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: 4cbca6e496135a312bf4704dd0989f45dcccfc00
ms.lasthandoff: 04/14/2017


---
# <a name="configure-fabrictransport-settings-for-reliable-actors"></a>Konfigurieren von FabricTransport-Einstellungen für Reliable Actors

Hier folgen Einstellungen, die Sie konfigurieren können:

- C#: [FabricTansportSettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.fabrictransport.common.fabrictransportsettings)
- Java: [FabricTransportRemotingSettings](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.fabrictransport._fabric_transport_remoting_settings)

Sie können die Standardkonfiguration von FabricTransport auf folgende Weise ändern.

## <a name="assembly-attribute"></a>Assembly-Attribut

Das [FabricTransportActorRemotingProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.actors.remoting.fabrictransport.fabrictransportactorremotingproviderattribute?redirectedfrom=MSDN#microsoft_servicefabric_actors_remoting_fabrictransport_fabrictransportactorremotingproviderattribute)-Attribut muss auf die Akteurclient- und Akteurdienstassemblys angewendet werden.

Das folgende Beispiel zeigt, wie der Standardwert der Einstellungen „FabricTransport“ und „OperationTimeout“ geändert werden kann:

  ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600)]
   ```

Das folgende Beispiel zeigt, wie die Standardwerte der Einstellungen „FabricTransport“, „MaxMessageSize“ und „OperationTimeoutInSeconds“ geändert werden kann:

  ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600,MaxMessageSize = 134217728)]
   ```

## <a name="config-package"></a>Konfigurationspaket

Sie können ein [Konfigurationspaket](service-fabric-application-model.md) verwenden, um die Standardkonfiguration zu ändern.

### <a name="configure-fabrictransport-settings-for-the-actor-service"></a>Konfigurieren der FabricTransport-Einstellungen für den Akteurdienst

Fügen Sie den Abschnitt „TransportSettings“ zur Datei „settings.xml“ hinzu.

Der Akteurcode sucht standardmäßig im Format „&lt;Akteurname&gt;TransportSettings“ nach „SectionName“. Wenn es nicht gefunden wird, wird im Format „TransportSettings“ nach „SectionName“ gesucht.

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

### <a name="configure-fabrictransport-settings-for-the-actor-client-assembly"></a>Konfigurieren der FabricTransport-Einstellungen für die Akteurclientassembly

Wenn der Client nicht als Teil eines Diensts ausgeführt wird, können Sie die Datei „&lt;Name der ausführbaren Clientdatei&gt;.settings.xml“ am gleichen Speicherort erstellen, an dem sich auch die ausführbare Clientdatei befindet. Fügen Sie dann den Abschnitt „TransportSettings“ zu dieser Datei hinzu. „SectionName“ muss „TransportSettings“ lauten.

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

