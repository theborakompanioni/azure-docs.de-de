---
title: "Konfigurieren eines eigenständigen Clusters | Microsoft Docs"
description: "Dieser Artikel beschreibt, wie Sie einen eigenständigen oder privaten Service Fabric-Cluster konfigurieren."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 4fb6ef56d694aff967840ab26b75b66a2e799cc1
ms.openlocfilehash: 977de9160be63a91b5926daa45528e5ee205e448


---
# <a name="configuration-settings-for-standalone-windows-cluster"></a>Konfigurationseinstellungen für eigenständige Windows-Cluster
In diesem Artikel wird beschrieben, wie Sie einen eigenständigen Service Fabric-Cluster mithilfe der Datei ***ClusterConfig.JSON*** konfigurieren. Sie können diese Datei zum Angeben von Informationen für Ihren eigenständigen Cluster verwenden, z.B. die Service Fabric-Knoten und ihre IP-Adressen, verschiedene Knotentypen im Cluster, die Sicherheitskonfigurationen sowie die Netzwerktopologie in Bezug auf Fehler-/Upgradedomänen.

Wenn Sie [das eigenständige Service Fabric-Paket herunterladen](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), werden einige Beispiele der Datei „ClusterConfig.JSON“ auf Ihren Arbeitscomputer heruntergeladen. Die Beispiele mit *DevCluster* im Namen dienen Ihnen als Hilfe beim Erstellen eines Clusters mit allen drei Knoten auf demselben Computer, z.B. logischen Knoten. Davon muss mindestens ein Knoten als primärer Knoten gekennzeichnet sein. Dieser Cluster ist gut für eine Entwicklungs- oder Testumgebung geeignet und wird nicht als Produktionscluster unterstützt. Die Beispiele mit *MultiMachine* im Namen dienen Ihnen als Hilfe beim Erstellen eines Clusters in Produktionsqualität, bei dem jeder Knoten auf einem separaten Computer angeordnet ist. Die Anzahl von primären Knoten für diesen Cluster basiert jeweils auf der [Zuverlässigkeitsstufe](#reliability).

1. Mit *ClusterConfig.Unsecure.DevCluster.JSON* und *ClusterConfig.Unsecure.MultiMachine.JSON* wird veranschaulicht, wie Sie einen ungeschützten Test- bzw. Produktionscluster erstellen. 
2. Mit *ClusterConfig.Windows.DevCluster.JSON* und *ClusterConfig.Windows.MultiMachine.JSON* wird veranschaulicht, wie Sie einen Test- oder Produktionscluster erstellen, der per [Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md) geschützt ist.
3. Mit *ClusterConfig.X509.DevCluster.JSON* und *ClusterConfig.X509.MultiMachine.JSON* wird veranschaulicht, wie Sie einen Test- oder Produktionscluster erstellen, der per [Sicherheit auf Basis des X509-Zertifikats](service-fabric-windows-cluster-x509-security.md) geschützt ist. 

Als Nächstes werden die verschiedenen Abschnitte der Datei ***ClusterConfig.JSON*** beschrieben.

## <a name="general-cluster-configurations"></a>Allgemeine Clusterkonfigurationen
Hierzu gehören die allgemeinen clusterspezifischen Konfigurationen, wie im folgenden JSON-Codeausschnitt gezeigt.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",

Sie können für Ihren Service Fabric-Cluster einen beliebigen Anzeigenamen festlegen, indem sie ihn der Variablen **name** zuweisen. **clusterConfigurationVersion** ist die Versionsnummer des Clusters. Sie sollten sie jedes Mal erhöhen, wenn Sie für den Service Fabric-Cluster ein Upgrade durchführen. Behalten Sie für die **apiVersion** aber den Standardwert bei.

<a id="clusternodes"></a>

## <a name="nodes-on-the-cluster"></a>Knoten im Cluster
Sie können die Knoten in Ihrem Service Fabric-Cluster mithilfe des **nodes** -Abschnitts konfigurieren, wie im folgenden Codeausschnitt gezeigt.

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

Ein Service Fabric-Cluster muss mindestens drei Knoten enthalten. Wenn es für Ihr Setup erforderlich ist, können Sie diesem Abschnitt weitere Knoten hinzufügen. In der folgenden Tabelle werden die Konfigurationseinstellungen für alle Knoten erläutert.

| **Knotenkonfiguration** | **Beschreibung** |
| --- | --- |
| nodeName |Sie können einen beliebigen Anzeigenamen für den Knoten festlegen. |
| iPAddress |Ermitteln Sie die IP-Adresse Ihres Knotens, indem Sie in einem Befehlsfenster `ipconfig`eingeben. Notieren Sie sich die IPv4-Adresse, und weisen Sie sie der Variablen **iPAddress** zu. |
| nodeTypeRef |Jedem Knoten kann ein anderer Knotentyp zugewiesen werden. Die [Knotentypen](#nodetypes) werden im folgenden Abschnitt definiert. |
| faultDomain |Mit Fehlerdomänen können Clusteradministratoren die physischen Knoten definieren, die unter Umständen gleichzeitig ausfallen können, weil gemeinsame physische Abhängigkeiten bestehen. |
| upgradeDomain |Upgradedomänen beschreiben Gruppen von Knoten, die zum Durchführen von Service Fabric-Upgrades zur gleichen Zeit heruntergefahren werden. Sie können selbst auswählen, welche Knoten den Upgradedomänen zugewiesen werden sollen, denn es bestehen hier keine physischen Einschränkungen. |

## <a name="cluster-properties"></a>Clustereigenschaften
Im Abschnitt **properties** der Datei „ClusterConfig.JSON“ wird der Cluster konfiguriert wie folgt.

<a id="reliability"></a>

### <a name="reliability"></a>Zuverlässigkeit
Im Abschnitt **reliabilityLevel** wird die Anzahl von Systemdienstkopien definiert, die auf den primären Knoten des Clusters ausgeführt werden können. Dadurch wird die Zuverlässigkeit dieser Dienste und folglich auch des Clusters erhöht. Sie können diese Variable auf *Bronze*, *Silver*, *Gold* oder *Platinum* festlegen, um 3, 5, 7 bzw. 9 Kopien dieser Dienste auszuführen. Unten finden Sie ein Beispiel hierzu.

    "reliabilityLevel": "Bronze",

Beachten Sie, dass auf einem primären Knoten nur jeweils eine einzige Kopie der Systemdienste ausgeführt wird. Daher benötigen Sie für die Zuverlässigkeitsstufe *Bronze* mindestens 3 primäre Knoten, für *Silver* mindestens 5, für *Gold* mindestens 7 und für *Platinum* mindestens 9 Knoten.

### <a name="diagnostics"></a>Diagnose
Im Abschnitt **diagnosticsStore** können Sie Parameter konfigurieren, um bei Knoten- und Clusterausfällen die Diagnose und Fehlerbehandlung zu ermöglichen. Dies wird im folgenden Codeausschnitt veranschaulicht. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**metadata** ist eine Beschreibung Ihres Clusters und kann gemäß Ihrem Setup festgelegt werden. Diese Variablen helfen Ihnen beim Sammeln von ETW-Ablaufprotokollen, Absturzabbildern und Leistungsindikatoren. Weitere Informationen über ETW-Ablaufprotokolle finden Sie unter [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) und [ETW-Ablaufverfolgung](https://msdn.microsoft.com/library/ms751538.aspx). Alle Protokolle einschließlich [Absturzabbildern](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) und [Leistungsindikatoren](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) können zum Ordner **connectionString** auf Ihrem Computer geleitet werden. Sie können auch *AzureStorage* zum Speichern von Diagnosedaten verwenden. Nachfolgend finden Sie einen Codeausschnitt als Beispiel.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Sicherheit
Der Abschnitt **security** wird für einen sicheren eigenständigen Service Fabric-Cluster benötigt. Der folgende Codeausschnitt zeigt einen Teil dieses Abschnitts.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

**metadata** ist eine Beschreibung Ihres sicheren Clusters und kann gemäß Ihrem Setup festgelegt werden. **ClusterCredentialType** und **ServerCredentialType** bestimmen den Typ der Sicherheit, der im Cluster und in den Knoten implementiert wird. Sie können *X509* für eine auf Zertifikaten basierende Sicherheit oder *Windows* für eine auf Azure Active Directory basierende Sicherheit festlegen. Die übrigen Festlegungen im Abschnitt **security** hängen vom Typ der Sicherheit ab. Wie Sie die restlichen Teile des Abschnitts [security](service-fabric-windows-cluster-x509-security.md) ausfüllen, erfahren Sie in den Artikeln [Zertifikatbasierte Sicherheit in eigenständigen Windows-Clustern](service-fabric-windows-cluster-windows-security.md) und **Windows-Sicherheit in eigenständigen Windows-Clustern**.

<a id="nodetypes"></a>

### <a name="node-types"></a>Knotentypen
Im Abschnitt **nodeTypes** werden die Typen der Knoten beschrieben, die in Ihrem Cluster vorhanden sind. Es muss mindestens ein Knotentyp für einen Cluster angegeben sein, wie im folgenden Codeausschnitt gezeigt. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

**name** ist der Anzeigename für diesen bestimmten Knotentyp. Weisen Sie zum Erstellen eines Knotens dieses Typs seinen Anzeigenamen der Variablen **nodeTypeRef** für den Knoten zu, wie [weiter oben](#clusternodes) beschrieben. Definieren Sie für jeden Knotentyp die zu verwendenden Verbindungsendpunkte. Für diese Verbindungsendpunkte können Sie jede beliebige Portnummer auswählen, sofern dadurch keine Konflikte mit anderen Endpunkten in diesem Cluster entstehen. In einem Cluster mit mehreren Knoten gibt es mindestens einen primären Knoten (**isPrimary** ist *true*). Dies richtet sich nach der Zuverlässigkeitsstufe ([**reliabilityLevel**](#reliability)). Informationen zu den Werten **nodeTypes** und **reliabilityLevel** und zur Unterscheidung von primären Knoten und anderen Knotentypen finden Sie unter [Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster](service-fabric-cluster-capacity.md). 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Endpunkte zum Konfigurieren der Knotentypen
* *clientConnectionEndpointPort* ist der Port, der vom Client zum Herstellen der Verbindung mit dem Cluster verwendet wird, wenn die Client-APIs genutzt werden. 
* *clusterConnectionEndpointPort* ist der Port, über den die Knoten miteinander kommunizieren.
* *leaseDriverEndpointPort* ist der Port, über den vom Clusterleasetreiber ermittelt wird, ob die Knoten noch aktiv sind. 
* *serviceConnectionEndpointPort* ist der Port, der von den auf einem Knoten bereitgestellten Anwendungen und Diensten verwendet wird, um mit dem Service Fabric-Client auf dem jeweiligen Knoten zu kommunizieren.
* *httpGatewayEndpointPort* ist der Port, der vom Service Fabric Explorer zum Herstellen der Verbindung mit dem Cluster verwendet wird.
* *ephemeralPorts* setzen die [vom Betriebssystem verwendeten dynamischen Ports](https://support.microsoft.com/kb/929851) außer Kraft. Service Fabric nutzt einen Teil davon als Anwendungsports, und die restlichen Ports sind für das Betriebssystem verfügbar. Außerdem wird dieser Bereich dem vorhandenen Bereich des Betriebssystems zugeordnet, und Sie können die in den JSON-Beispieldateien angegebenen Bereiche für alle Zwecke verwenden. Sie müssen sicherstellen, dass der Unterschied zwischen den Start- und Endports mindestens 255 beträgt. Wenn dieser Unterschied zu gering ist, kann es zu Konflikten kommen, da dieser Bereich für das Betriebssystem freigegeben ist. Zeigen Sie den konfigurierten dynamischen Portbereich an, indem Sie `netsh int ipv4 show dynamicport tcp` ausführen.
* *applicationPorts* sind die Ports, die von den Service Fabric-Anwendungen verwendet werden. Dies sollte eine Teilmenge von *ephemeralPorts* und ausreichend sein, um die Endpunktanforderung Ihrer Anwendungen abzudecken. Service Fabric greift hierauf zurück, wenn neue Ports benötigt werden, und öffnet die Firewall für diese Ports. 
* *reverseProxyEndpointPort* ist ein optionaler Reverseproxy-Endpunkt. Weitere Informationen finden Sie unter [Service Fabric-Reverseproxy](service-fabric-reverseproxy.md). 

### <a name="other-settings"></a>Andere Einstellungen
Im Abschnitt **fabricSettings** können Sie die Stammverzeichnisse für die Service Fabric-Daten und -Protokolle festlegen. Sie können diese nur während der erstmaligen Clustererstellung anpassen. Nachfolgend finden Sie einen Codeausschnitt mit diesem Abschnitt als Beispiel.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Es wird empfohlen, ein Nicht-Betriebssystem-Laufwerk als „FabricDataRoot“ und „FabricLogRoot“ zu verwenden, da dies zur Verhinderung von Betriebssystemabstürzen mehr Zuverlässigkeit bietet. Beachten Sie: Wenn Sie nur das Stammverzeichnis für die Daten anpassen, wird das Stammverzeichnis für die Protokolle genau eine Ebene unterhalb des Stammverzeichnisses für die Daten angesiedelt.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die Datei „ClusterConfig.JSON“ vollständig entsprechend dem Setup für Ihren eigenständigen Cluster konfiguriert haben, können Sie den Cluster bereitstellen. Dazu befolgen Sie die Anweisungen im Artikel [Erstellen eines eigenständigen Service Fabric-Clusters](service-fabric-cluster-creation-for-windows-server.md), und anschließend [visualisieren Sie den Cluster mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).




<!--HONumber=Dec16_HO2-->


