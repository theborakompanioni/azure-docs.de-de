---
title: "Schützen eines Windows-Clusters mithilfe von Windows-Sicherheit | Microsoft Docs"
description: "Es wird beschrieben, wie Sie die Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit in einem eigenständigen Cluster unter Windows per Windows-Sicherheit konfigurieren."
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
ms.date: 12/09/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 08024aedb0889c91909f12aa1e38c91b11d3090f
ms.openlocfilehash: 142b76f053adc273e2e071f169f8f647fbd1c241


---
# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit
Um den unbefugten Zugriff auf einen Service Fabric-Cluster zu verhindern, müssen Sie ihn absichern. Dies gilt besonders, wenn darin Workloads für die Produktion ausgeführt werden. In diesem Artikel wird beschrieben, wie Sie die Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit mithilfe der Windows-Sicherheit in der Datei *ClusterConfig.JSON* konfigurieren und bezieht sich auf den Schritt zum Konfigurieren der Sicherheit unter [Erstellen und Verwalten eines Windows Server-Clusters](service-fabric-cluster-creation-for-windows-server.md). Weitere Informationen zur Verwendung von Windows-Sicherheit durch Service Fabric finden Sie unter [Szenarios für die Clustersicherheit](service-fabric-cluster-security.md).

> [!NOTE]
> Sie sollten sich die Sicherheitsauswahl für die Knoten-zu-Knoten-Sicherheit sorgfältig überlegen, da eine Clusteraktualisierung von einer Sicherheitsauswahl zur anderen nicht möglich ist. Für eine Änderung der Sicherheitsauswahl ist eine vollständige Neuerstellung des Clusters erforderlich.
> 
> 

## <a name="configure-windows-security"></a>Konfigurieren der Windows-Sicherheit
Die Beispielkonfigurationsdatei *ClusterConfig.Windows.JSON*, die mit dem Paket [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) für den eigenständigen Cluster heruntergeladen wurde, enthält eine Vorlage zum Konfigurieren der Windows-Sicherheit.  Die Windows-Sicherheit wird im Abschnitt **Eigenschaften** konfiguriert:

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
| ClusterCredentialType |Die Windows-Sicherheit wird aktiviert, indem der Parameter **ClusterCredentialType** auf *Windows*festgelegt wird. |
| ServerCredentialType |Die Windows-Sicherheit für Clients wird aktiviert, indem der Parameter **ServerCredentialType** auf *Windows*festgelegt wird. Dies weist darauf hin, dass die Clients des Clusters und der Cluster selbst in einer Active Directory-Domäne ausgeführt werden. |
| WindowsIdentities |Enthält die Cluster und Clientidentitäten. |
| ClusterIdentity |Konfiguriert die Knoten-zu-Knoten-Sicherheit. Ein Computergruppenname |
| ClientIdentities |Konfiguriert die Client-zu-Knoten-Sicherheit. Ein Array mit Clientbenutzerkonten. |
| Identity |Die Clientidentität, ein Domänenbenutzer. |
| IsAdmin |„true“ gibt an, dass der Domänenbenutzer über Administratorclientzugriff verfügt, und „false“ gibt an, dass Benutzerclientzugriff besteht. |

[Knoten-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#node-to-node-security) wird mithilfe von **ClusterIdentity** konfiguriert. Um Vertrauensstellungen zwischen Knoten erstellen zu können, müssen die Knoten über das Vorhandensein des jeweils anderen Knotens informiert sein. Dies kann erreicht werden, indem Sie eine Domänengruppe erstellen, die alle Knoten im Cluster enthält. Dieser Gruppenname muss in **ClusterIdentity** angegeben werden. Weitere Informationen finden Sie unter [How to Create a Group in Active Directory (Erstellen einer Gruppe in Active Directory)](https://msdn.microsoft.com/en-us/library/aa545347(v=cs.70).aspx).

Die [Client-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#client-to-node-security) wird mit **ClientIdentities** konfiguriert. Um die Vertrauensstellung zwischen einem Client und dem Cluster herzustellen, müssen Sie den Cluster so konfigurieren, dass ermittelt werden kann, welche Clientidentitäten vertrauenswürdig sind. Dies ist auf zwei Arten möglich: Geben Sie die Domänengruppenbenutzer oder die Domänenknotenbenutzer an, die eine Verbindung herstellen können. Service Fabric unterstützt zwei unterschiedliche Zugriffsberechtigungstypen für Clients, die mit einem Service Fabric-Cluster verbunden sind: Administrator und Benutzer. Die Zugriffssteuerung ermöglicht es dem Clusteradministrator, den Zugriff auf bestimmte Arten von Clustervorgängen für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird.  Administratoren haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z. B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen.

Im folgenden Beispielabschnitt zur **Sicherheit** wird die Windows-Sicherheit konfiguriert und angegeben, dass die Computer in der Computergruppe *ServiceFabric\\ClusterNodes* Teil des Clusters sind und dass *CONTOSO\usera* über Administratorclientzugriff verfügt:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric\\ClusterNodes",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Nächste Schritte
Setzen Sie den Vorgang der Clustererstellung unter *Erstellen eines eigenständigen Clusters unter Windows* fort, nachdem Sie die Windows-Sicherheit in der Datei [ClusterConfig.JSON](service-fabric-cluster-creation-for-windows-server.md)konfiguriert haben.

Weitere Informationen zur Knoten-zu-Knoten-Sicherheit, Client-zu-Knoten-Sicherheit und rollenbasierten Zugriffssteuerung finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

Beispiele für die Herstellung der Verbindung mit PowerShell oder FabricClient finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md) .




<!--HONumber=Nov16_HO4-->


