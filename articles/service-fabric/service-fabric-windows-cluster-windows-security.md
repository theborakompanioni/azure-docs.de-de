<properties
   pageTitle="Schützen eines Clusters unter Windows per Windows-Sicherheit | Microsoft Azure"
   description="Es wird beschrieben, wie Sie die Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit in einem eigenständigen Cluster unter Windows per Windows-Sicherheit konfigurieren."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit

Um den unbefugten Zugriff auf einen Service Fabric-Cluster zu verhindern, müssen Sie ihn absichern. Dies gilt besonders, wenn darin Workloads für die Produktion ausgeführt werden. In diesem Artikel wird beschrieben, wie Sie die Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit mithilfe der Windows-Sicherheit in der Datei *ClusterConfig.JSON* konfigurieren. Er bezieht sich auf den Schritt zum Konfigurieren der Sicherheit unter [Erstellen eines eigenständigen Clusters unter Windows](service-fabric-cluster-creation-for-windows-server.md). Weitere Informationen dazu, wie Windows-Sicherheit von Service Fabric verwendet wird, finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

>[AZURE.NOTE]
Sie sollten sich die Sicherheitsauswahl für die Knoten-zu-Knoten-Sicherheit sorgfältig überlegen, da eine Clusteraktualisierung von einer Sicherheitsauswahl zur anderen nicht möglich ist. Für eine Änderung der Sicherheitsauswahl ist eine vollständige Neuerstellung des Clusters erforderlich.

## Konfigurieren der Windows-Sicherheit
Die Beispieldatei *ClusterConfig.Windows.JSON* für die Konfiguration, die mit dem Paket [Microsoft.Azure.ServiceFabric.WindowsServer.<Version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) für den eigenständigen Cluster heruntergeladen wurde, enthält eine Vorlage zum Konfigurieren der Windows-Sicherheit. Die Windows-Sicherheit wird im Abschnitt **Eigenschaften** konfiguriert:

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

|**Konfigurationseinstellung**|**Beschreibung**|
|-----------------------|--------------------------|
|ClusterCredentialType|Die Windows-Sicherheit wird aktiviert, indem der Parameter **ClusterCredentialType** auf *Windows* festgelegt wird.|
|ServerCredentialType|Die Windows-Sicherheit für Clients wird aktiviert, indem der Parameter **ServerCredentialType** auf *Windows* festgelegt wird. Dies weist darauf hin, dass die Clients des Clusters und der Cluster selbst in einer Active Directory-Domäne ausgeführt werden.|
|WindowsIdentities|Enthält die Cluster und Clientidentitäten.|
|ClusterIdentity|Konfiguriert die Knoten-zu-Knoten-Sicherheit. Eine kommagetrennte Liste mit gruppenverwalteten Dienstkonten oder Computernamen.|
|ClientIdentities|Konfiguriert die Client-zu-Knoten-Sicherheit. Ein Array mit Clientbenutzerkonten.|
|Identity|Die Clientidentität, ein Domänenbenutzer.|
|IsAdmin|„true“ gibt an, dass der Domänenbenutzer über Administratorclientzugriff verfügt, und „false“ gibt an, dass Benutzerclientzugriff besteht.|

[Knoten-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#node-to-node-security) wird konfiguriert, indem **ClusterIdentity** verwendet wird. Um Vertrauensstellungen zwischen Knoten erstellen zu können, müssen die Knoten über das Vorhandensein des jeweils anderen Knotens informiert sein. Dies kann auf zwei Arten erreicht werden: Geben Sie das gruppenverwaltete Dienstkonto an, das alle Knoten im Cluster enthält, oder geben Sie die Domänenknotenidentitäten aller Knoten im Cluster an. Wir empfehlen Ihnen dringend, den Ansatz der [gruppenverwalteten Dienstkonten](https://technet.microsoft.com/library/hh831782.aspx) zu verwenden. Dies gilt besonders für größere Cluster (mehr als zehn Knoten) oder für Cluster, die sich voraussichtlich vergrößern oder verkleinern. Bei diesem Ansatz können Knoten dem gruppenverwalteten Dienstkonto hinzugefügt oder daraus entfernt werden, ohne dass Änderungen am Clustermanifest erforderlich sind. Für diesen Ansatz ist keine Erstellung einer Domänengruppe erforderlich, für die Clusteradministratoren die Zugriffsrechte zum Hinzufügen und Entfernen von Mitgliedern gewährt wurden. Weitere Informationen finden Sie unter [Erste Schritte mit gruppenverwalteten Dienstkonten](http://technet.microsoft.com/library/jj128431.aspx).

Die [Client-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#client-to-node-security) wird mit **ClientIdentities** konfiguriert. Um die Vertrauensstellung zwischen einem Client und dem Cluster herzustellen, müssen Sie den Cluster so konfigurieren, dass ermittelt werden kann, welche Clientidentitäten vertrauenswürdig sind. Dies ist auf zwei Arten möglich: Geben Sie die Domänengruppenbenutzer oder die Domänenknotenbenutzer an, die eine Verbindung herstellen können. Service Fabric unterstützt zwei unterschiedliche Zugriffsberechtigungstypen für Clients, die mit einem Service Fabric-Cluster verbunden sind: Administrator und Benutzer. Die Zugriffssteuerung ermöglicht es dem Clusteradministrator, den Zugriff auf bestimmte Arten von Clustervorgängen für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird. Administratoren haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z. B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen.

Im folgenden Beispielabschnitt zur **Sicherheit** wird die Windows-Sicherheit konfiguriert und angegeben, dass die Computer unter *ServiceFabric/clusterA.contoso.com* Teil des Clusters sind und dass *CONTOSO\\usera* über Administratorclientzugriff verfügt:

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

## Nächste Schritte

Setzen Sie den Vorgang der Clustererstellung unter [Erstellen eines eigenständigen Clusters unter Windows](service-fabric-cluster-creation-for-windows-server.md) fort, nachdem Sie die Windows-Sicherheit in der Datei *ClusterConfig.JSON* konfiguriert haben.

Weitere Informationen zur Knoten-zu-Knoten-Sicherheit, Client-zu-Knoten-Sicherheit und rollenbasierten Zugriffssteuerung finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

Beispiele für die Herstellung der Verbindung mit PowerShell oder FabricClient finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md).

<!---HONumber=AcomDC_0831_2016-->