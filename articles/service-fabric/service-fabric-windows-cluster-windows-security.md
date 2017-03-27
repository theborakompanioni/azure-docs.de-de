---
title: "Schützen eines Windows-Clusters mithilfe von Windows-Sicherheit | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie die Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit in einem eigenständigen Cluster unter Windows mit Windows-Sicherheit konfigurieren."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/17/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 6294a6fac8748f70c807df76feb425cb627bf4c3
ms.lasthandoff: 03/15/2017


---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Schützen eines eigenständigen Windows-Clusters mit Windows-Sicherheit
Um nicht autorisierten Zugriff auf einen Service Fabric-Cluster zu verhindern, müssen Sie den Cluster schützen. Sicherheit ist besonders wichtig, wenn der Cluster Produktionsworkloads ausführt. In diesem Artikel wird beschrieben, wie Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit mit Windows-Sicherheit in der *ClusterConfig.JSON*-Datei konfiguriert wird.  Der Prozess entspricht dem Sicherheitskonfigurationsschritt von [Erstellen eines eigenständigen Clusters unter Windows Server](service-fabric-cluster-creation-for-windows-server.md). Weitere Informationen zur Verwendung von Windows-Sicherheit durch Service Fabric finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

> [!NOTE]
> Sie sollten sich die Auswahl für die Knoten-zu-Knoten-Sicherheit sorgfältig überlegen, da eine Clusteraktualisierung von einer Sicherheitsauswahl zur anderen nicht möglich ist. Um die Sicherheitsauswahl zu ändern, müssen Sie den vollständigen Cluster neu erstellen.
>
>

## <a name="configure-windows-security"></a>Konfigurieren der Windows-Sicherheit  
Die Beispielkonfigurationsdatei *ClusterConfig.Windows.JSON*, die mit dem Paket [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) für den eigenständigen Cluster heruntergeladen wurde, enthält eine Vorlage zum Konfigurieren der Windows-Sicherheit. Die Windows-Sicherheit wird im Abschnitt **Eigenschaften** konfiguriert:

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
                "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

| **Konfigurationseinstellung** | **Beschreibung** |
| --- | --- |
| ClusterCredentialType |Legen Sie den Wert *Windows* fest, um Windows-Sicherheit zu aktivieren. |
| ServerCredentialType |Legen Sie den Wert *Windows* fest, um Windows-Sicherheit für Clients zu aktivieren.<br /><br />Dies weist darauf hin, dass die Clients des Clusters und der Cluster selbst in einer Active Directory-Domäne ausgeführt werden. |  
| WindowsIdentities |Enthält die Cluster und Clientidentitäten. |  
| ClusterIdentity |Verwenden Sie einen Computergruppennamen – „Domäne\Computergruppe“ – um Knoten-zu-Knoten-Sicherheit zu konfigurieren. |  
| ClientIdentities |Konfiguriert die Client-zu-Knoten-Sicherheit. Ein Array mit Clientbenutzerkonten. |  
| Identity |Fügen Sie den Domänenbenutzer – „Domäne\Benutzername“ – für die Identität des Clients hinzu. |  
| IsAdmin |Legen Sie den Wert „true“ fest, um anzugeben, dass der Domänenbenutzer über Administratorclientzugriff verfügt, oder „false“ für Benutzerclientzugriff. |  

[Knoten-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#node-to-node-security) wird mithilfe von **ClusterIdentity** konfiguriert. Um Vertrauensstellungen zwischen Knoten erstellen zu können, müssen die Knoten über das Vorhandensein des jeweils anderen Knotens informiert sein. Damit diese Information vorhanden ist, erstellen Sie eine Domänengruppe, die alle Knoten im Cluster enthält. Dieser Gruppenname muss in **ClusterIdentity** angegeben werden. Weitere Informationen finden Sie unter [How to Create a Group in Active Directory (Erstellen einer Gruppe in Active Directory)](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).  

[Client-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#client-to-node-security) wird mit **ClientIdentities** konfiguriert. Um die Vertrauensstellung zwischen einem Client und dem Cluster herzustellen, müssen Sie den Cluster so konfigurieren, dass ermittelt werden kann, welche Clientidentitäten vertrauenswürdig sind. Sie können Vertrauensstellungen auf zwei verschiedene Arten erstellen:

- Geben Sie die Domänengruppenbenutzer an, die eine Verbindung herstellen können.
- Geben Sie die Domänenknotenbenutzer an, die eine Verbindung herstellen können.

Service Fabric unterstützt zwei unterschiedliche Zugriffsberechtigungstypen für Clients, die mit einem Service Fabric-Cluster verbunden sind: Administrator und Benutzer. Die Zugriffssteuerung ermöglicht dem Clusteradministrator, den Zugriff auf bestimmte Arten von Clustervorgängen für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird.  Administratoren haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z. B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen.  

Im folgenden Beispielabschnitt zur **Sicherheit** wird die Windows-Sicherheit konfiguriert und angegeben, dass die Computer in *ServiceFabric/clusterA.contoso.com* Teil des Clusters sind, und dass *CONTOSO\usera* über Administratorclientzugriff verfügt:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric sollte nicht auf einem Domänencontroller bereitgestellt werden. Stellen Sie sicher, dass „ClusterConfig.json“ nicht die IP-Adresse des Domänencontrollers einschließt, wenn Sie eine Computergruppe oder ein gruppenverwaltetes Dienstkonto (group Managed Service Account, gMSA) verwenden.
>
>

## <a name="next-steps"></a>Nächste Schritte
Setzen Sie den Vorgang der Clustererstellung unter *Erstellen eines eigenständigen Clusters unter Windows* fort, nachdem Sie die Windows-Sicherheit in der Datei [ClusterConfig.JSON](service-fabric-cluster-creation-for-windows-server.md)konfiguriert haben.

Weitere Informationen zur Knoten-zu-Knoten-Sicherheit, Client-zu-Knoten-Sicherheit und rollenbasierten Zugriffssteuerung finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

Beispiele für die Herstellung der Verbindung mit PowerShell oder FabricClient finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md) .

