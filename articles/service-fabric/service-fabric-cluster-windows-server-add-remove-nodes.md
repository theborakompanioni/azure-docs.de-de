---
title: "Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster | Microsoft Docs"
description: "Enthält Informationen zum Hinzufügen oder Entfernen von Knoten für einen Azure Service Fabric-Cluster auf einem physischen oder virtuellen Computer mit Windows Server, der lokal oder in einer Cloud angeordnet sein kann."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/02/2017
ms.author: chackdan
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: fa59bae2b824e6b75e120ab2b61027746ee1ea78
ms.lasthandoff: 04/27/2017


---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server
Nachdem Sie Ihren [eigenständigen Service Fabric-Cluster auf Windows Server-Computern erstellt haben](service-fabric-cluster-creation-for-windows-server.md), können sich Ihre geschäftlichen Anforderungen ändern, sodass Sie Ihrem Cluster ggf. mehrere Knoten hinzufügen bzw. diese daraus entfernen müssen. Dieser Artikel enthält die ausführlichen Schritte, die hierfür erforderlich sind.

## <a name="add-nodes-to-your-cluster"></a>Hinzufügen von Knoten zum Cluster
1. Bereiten Sie den (virtuellen) Computer vor, den Sie dem Cluster hinzufügen möchten, indem Sie die Schritte im Abschnitt [Vorbereiten der Computer zur Erfüllung der Voraussetzungen für die Clusterbereitstellung](service-fabric-cluster-creation-for-windows-server.md) ausführen.
2. Planen Sie, welcher Fehlerdomäne und Upgradedomäne Sie diesen virtuellen bzw. physischen Computer hinzufügen werden.
3. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen bzw. physischen Computer her, den Sie dem Cluster hinzufügen möchten.
4. [Laden Sie das eigenständige Paket für Service Fabric für Windows Server auf den (virtuellen) Computer herunter](http://go.microsoft.com/fwlink/?LinkId=730690) (bzw. kopieren Sie es), und entzippen Sie das Paket.
5. Führen Sie PowerShell als Administrator aus, und navigieren Sie zum Speicherort des extrahierten Pakets.
6. Führen Sie das PowerShell-Skript *AddNode.ps1* mit den Parametern aus, die den hinzuzufügenden neuen Knoten beschreiben. Im folgenden Beispiel wird ein neuer Knoten mit dem Namen VM5, dem Typ NodeType0 und der IP-Adresse 182.17.34.52 für UD1 und fd:/dc1/r0 hinzugefügt. *ExistingClusterConnectionEndPoint* ist ein Verbindungsendpunkt für einen Knoten, der im Cluster bereits vorhanden ist. Für diesen Endpunkt können Sie die IP-Adresse von *jedem* Knoten im Cluster auswählen.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA

```
Sie können überprüfen, ob der neue Knoten hinzugefügt wurde, indem Sie das Cmdlet [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) ausführen.


## <a name="remove-nodes-from-your-cluster"></a>Entfernen von Knoten aus dem Cluster
Je nach ausgewählter Zuverlässigkeitsstufe des Clusters können Sie die ersten n (3/5/7/9) Knoten des primären Knotentyps nicht entfernen. Beachten Sie auch, dass die Ausführung des RemoveNode-Befehls in einem Entwicklungscluster nicht unterstützt wird.

1. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen bzw. physischen Computer her, den Sie aus dem Cluster entfernen möchten.
2. [Laden Sie das eigenständige Paket für Service Fabric für Windows Server herunter](http://go.microsoft.com/fwlink/?LinkId=730690) (bzw. kopieren Sie es), und entpacken Sie das Paket auf diesem virtuellen bzw. physischen Computer.
3. Führen Sie PowerShell als Administrator aus, und navigieren Sie zum Speicherort des extrahierten Pakets.
4. Führen Sie *RemoveNode.ps1* in PowerShell aus. Im folgenden Beispiel wird der aktuelle Knoten aus dem Cluster entfernt. *ExistingClientConnectionEndpoint* ist ein Clientverbindungsendpunkt für jeden Knoten, der im Cluster verbleibt. Wählen Sie die IP-Adresse und den Endpunktport von *jedem* **anderen Knoten** im Cluster aus. Dieser **andere Knoten** aktualisiert wiederum die Clusterkonfiguration für den entfernten Knoten. 

```

.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000

```

> [!NOTE]
> Einige Knoten werden möglicherweise aufgrund von Systemdienstabhängigkeiten nicht entfernt. Diese Knoten sind Primärknoten und können durch Abfragen des Clustermanifests mithilfe von `Get-ServiceFabricClusterManifest` und Suchen von mit `IsSeedNode=”true”` markierten Knoteneinträgen ermittelt werden. 
> 
> 

Ein Knoten kann auch nach dem Entfernen in Abfragen und SFX als ausgefallen angezeigt werden. Dies ist ein bekanntes Problem. Es wird in einer zukünftigen Version behoben. 


## <a name="remove-node-types-from-your-cluster"></a>Entfernen von Knotentypen aus dem Cluster
Das Entfernen eines Knotentyps erfordert besondere Vorsicht. Überprüfen Sie vor dem Entfernen eines Knotentyps gründlich, ob andere Knoten auf den Knotentyp verweisen.


## <a name="replace-primary-nodes-of-your-cluster"></a>Ersetzen primärer Knoten im Cluster
Das Ersetzen primärer Knoten muss für jeden Knoten einzeln ausgeführt werden. Das Entfernen und Hinzufügen in Batches ist nicht möglich.


## <a name="next-steps"></a>Nächste Schritte
* [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md)
* [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)
* [Erstellen eines eigenständigen Service Fabric-Clusters mit drei Knoten und Azure-VMs mit Windows Server](service-fabric-cluster-creation-with-windows-azure-vms.md)


