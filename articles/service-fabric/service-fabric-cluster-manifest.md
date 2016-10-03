<properties
   pageTitle="Konfigurieren eines eigenständigen Clusters | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie Sie einen eigenständigen oder privaten Service Fabric-Cluster konfigurieren."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="dkshir"/>


# Konfigurationseinstellungen für eigenständige Windows-Cluster

In diesem Artikel wird beschrieben, wie Sie einen eigenständigen Service Fabric-Cluster mithilfe der Datei _**ClusterConfig.JSON**_ konfigurieren. Diese Datei wird auf Ihren Arbeitscomputer heruntergeladen, wenn Sie [das eigenständige Paket für Service Fabric herunterladen](service-fabric-cluster-creation-for-windows-server.md#downloadpackage). In der Datei „ClusterConfig.JSON“ können Sie für Ihren Service Fabric-Cluster u.a. folgende Informationen festlegen: die Service Fabric-Knoten und ihre IP-Adressen, verschiedene Knotentypen im Cluster, die Sicherheitskonfigurationen sowie die Netzwerktopologie in Bezug auf Fehler-/Ugpradedomänen.

Im Folgenden beschreiben wir die verschiedenen Abschnitte dieser Datei.

## Allgemeine Clusterkonfigurationen
Hierzu gehören die allgemeinen clusterspezifischen Konfigurationen, wie im folgenden JSON-Codeausschnitt gezeigt.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",

Sie können für Ihren Service Fabric-Cluster einen beliebigen Anzeigenamen festlegen, indem sie ihn der Variablen **name** zuweisen. Den Wert für **clusterManifestVersion** können Sie gemäß Ihrem Setup ändern; er muss allerdings angepasst werden, bevor Sie Ihre Service Fabric-Konfiguration upgraden. Für **apiVersion** können Sie den Standardwert beibehalten.


<a id="clusternodes"></a>
## Knoten im Cluster
Sie können die Knoten in Ihrem Service Fabric-Cluster mithilfe des **nodes**-Abschnitts konfigurieren, wie im folgenden Codeausschnitt gezeigt.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Ein Service Fabric-Cluster benötigt mindestens 3 Knoten. Wenn es für Ihr Setup erforderlich ist, können Sie diesem Abschnitt weitere Knoten hinzufügen. In der folgenden Tabelle werden die Konfigurationseinstellungen für alle Knoten erläutert.

|**Knotenkonfiguration**|**Beschreibung**|
|-----------------------|--------------------------|
|nodeName|Sie können einen beliebigen Anzeigenamen für den Knoten festlegen.|
|iPAddress|Ermitteln Sie die IP-Adresse Ihres Knotens, indem Sie in einem Befehlsfenster `ipconfig` eingeben. Notieren Sie sich die IPv4-Adresse, und weisen Sie sie der Variablen **iPAddress** zu.|
|nodeTypeRef|Jedem Knoten kann ein anderer Knotentyp zugewiesen werden. Die [Knotentypen](#nodetypes) werden im folgenden Abschnitt definiert.|
|faultDomain|Mit Fehlerdomänen können Clusteradministratoren die physischen Knoten definieren, die unter Umständen gleichzeitig ausfallen können, weil gemeinsame physische Abhängigkeiten bestehen.|
|upgradeDomain|Upgradedomänen beschreiben Gruppen von Knoten, die zum Durchführen von Service Fabric-Upgrades zur gleichen Zeit heruntergefahren werden. Sie können selbst auswählen, welche Knoten den Upgradedomänen zugewiesen werden sollen, denn es bestehen hier keine physischen Einschränkungen.| 


## Eigenschaften des Clusters (**properties**)

Im Abschnitt **properties** der Datei „ClusterConfig.JSON“ wird der Cluster konfiguriert. Hierbei gehen Sie wie folgt vor:

### **diagnosticsStore**
Sie können Parameter konfigurieren, um bei Knoten- und Clusterausfällen die Diagnose und Fehlerbehandlung zu erleichtern. Dazu verwenden Sie den Abschnitt **diagnosticsStore**, wie im folgenden Codeausschnitt gezeigt.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**metadata** ist eine Beschreibung Ihres Clusters und kann gemäß Ihrem Setup festgelegt werden. Diese Variablen helfen Ihnen beim Sammeln von ETW-Ablaufprotokollen, Absturzabbildern und Leistungsindikatoren. Weitere Informationen über ETW-Ablaufprotokolle finden Sie unter [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) und [ETW-Ablaufverfolgung](https://msdn.microsoft.com/library/ms751538.aspx). Alle Protokolle einschließlich [Absturzabbilder](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) und [Leistungsindikatoren](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) können zum Ordner **connectionString** auf Ihrem Computer geleitet werden. Sie können auch **AzureStorage** zum Speichern von Diagnosedaten verwenden. Nachfolgend finden Sie einen Codeausschnitt als Beispiel.

	"diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### **security** 
Der Abschnitt **security** wird für einen sicheren eigenständigen Service Fabric-Cluster benötigt. Der folgende Codeausschnitt zeigt einen Teil dieses Abschnitts.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
		. . .
	}

**metadata** ist eine Beschreibung Ihres sicheren Clusters und kann gemäß Ihrem Setup festgelegt werden. **ClusterCredentialType** und **ServerCredentialType** bestimmen den Typ der Sicherheit, der im Cluster und in den Knoten implementiert wird. Sie können *X509* für eine auf Zertifikaten basierende Sicherheit oder *Windows* für eine auf Azure Active Directory basierende Sicherheit festlegen. Die übrigen Festlegungen im Abschnitt **security** hängen vom Typ der Sicherheit ab. Wie Sie die restlichen Teile des Abschnitts **security** ausfüllen, erfahren Sie in den Artikeln [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md) und [Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md).

### **reliabilityLevel**
**reliabilityLevel** definiert die Anzahl der Kopien von Systemdiensten, die auf den primären Knoten des Clusters ausgeführt werden können. Dadurch wird die Zuverlässigkeit dieser Dienste und folglich auch des Clusters erhöht. Sie können diese Variable auf *Bronze*, *Silver*, *Gold* oder *Platinum* festlegen, um je nach Einstellung 3, 5, 7 oder 9 Kopien dieser Dienste auszuführen. Unten finden Sie ein Beispiel hierzu.

	"reliabilityLevel": "Bronze",
	
Beachten Sie, dass auf einem primären Knoten nur jeweils eine einzige Kopie der Systemdienste ausgeführt wird. Daher benötigen Sie für die Zuverlässigkeitsstufe *Bronze* mindestens 3 primäre Knoten, für *Silber* mindestens 5, für *Gold* mindestens 7 und für *Platin* mindestens 9 Knoten.


<a id="nodetypes"></a>
### **nodeTypes**
Im Abschnitt **nodeTypes** werden die Typen der Knoten beschrieben, die in Ihrem Cluster vorhanden sind. Es muss mindestens ein Knotentyp für einen Cluster angegeben sein, wie im folgenden Codeausschnitt gezeigt.

	"nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpoint": "19001",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
			"startPort": "20001",
            "endPort": "20031"
        },
        "ephemeralPorts": {
            "startPort": "20032",
            "endPort": "20062"
        },
        "isPrimary": true
    }]

**name** ist der Anzeigename für diesen bestimmten Knotentyp. Um einen Knoten mit diesem Knotentyp zu erstellen, müssen Sie den Anzeigenamen für diesen Knotentyp der Variablen **nodeTypeRef** für diesen Knoten zuweisen, wie oben im Abschnitt [Knoten im Cluster](#clusternodes) beschrieben. Für jeden Knotentyp können Sie verschiedene Endpunkte definieren, die zum Herstellen einer Verbindung mit diesem Cluster verwendet werden. Für diese Verbindungsendpunkte können Sie jede beliebige Portnummer auswählen, sofern dadurch keine Konflikte mit anderen Endpunkten in diesem Cluster entstehen. Ein Cluster mit mehreren Knotentypen verfügt über einen primären Knotentyp, für den **isPrimary** auf *true* festgelegt ist. Für die übrigen Knoten ist **isPrimary** auf *false* festgelegt. Lesen Sie den Artikel [Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster](service-fabric-cluster-capacity.md), um sich ausführlicher über die für Ihre Clusterkapazität angemessenen Werte für **nodeTypes** und **reliabilityLevel** sowie über den Unterschied zwischen primären und nicht primären Knotentypen zu informieren.


### **fabricSettings**
In diesem Abschnitt können Sie die Stammverzeichnisse für die Service Fabric-Daten und -Protokolle festlegen. Sie können diese nur während der erstmaligen Clustererstellung anpassen. Nachfolgend finden Sie einen Codeausschnitt mit diesem Abschnitt als Beispiel.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\ProgramData\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\ProgramData\SF\Log"
    }]

Es wird empfohlen, ein Nicht-Betriebssystem-Laufwerk als „FabricDataRoot“ und „FabricLogRoot“ zu verwenden, da dies zur Verhinderung von Betriebssystemabstürzen mehr Zuverlässigkeit bietet. Beachten Sie: Wenn Sie nur das Stammverzeichnis für die Daten anpassen, wird das Stammverzeichnis für die Protokolle genau eine Ebene unterhalb des Stammverzeichnisses für die Daten angesiedelt.


## Nächste Schritte

Nachdem Sie die Datei „ClusterConfig.JSON“ vollständig entsprechend dem Setup für Ihren eigenständigen Cluster konfiguriert haben, können Sie den Cluster bereitstellen. Dazu befolgen Sie die Anweisungen im Artikel [Erstellen eines Azure Service Fabric-Clusters – lokal oder in der Cloud](service-fabric-cluster-creation-for-windows-server.md), und anschließend [visualisieren Sie den Cluster mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

<!---HONumber=AcomDC_0921_2016-->