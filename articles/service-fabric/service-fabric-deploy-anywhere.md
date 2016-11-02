<properties
   pageTitle="Erstellen von Azure Service Fabric-Clustern unter Windows Server und Linux| Microsoft Azure"
   description="Service Fabric-Cluster werden unter Windows Server und Linux ausgeführt, sodass Service Fabric-Anwendungen überall dort bereitgestellt und gehostet werden können, wo die Ausführung von Windows Server oder Linux möglich ist."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>


# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Erstellen von Service Fabric-Clustern unter Windows Server oder Linux

Azure Service Fabric ermöglicht die Erstellung von Service Fabric-Clustern auf VMs oder Computern mit Windows Server oder Linux. Dadurch lassen sich Service Fabric-Anwendungen in jeder lokalen oder cloudbasierten Umgebung oder in einer Microsoft Azure-Umgebung bereitstellen und ausführen, in der sich miteinander verbundene Computer mit Windows Server oder Linux befinden.

##<a name="create-service-fabric-clusters-on-azure"></a>Erstellen von Service Fabric-Clustern in Azure

Das Erstellen eines Clusters unter Azure wird entweder mit einer Resource Manager-Vorlage oder mit dem Azure-Portal durchgeführt. Weitere Informationen finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) oder [Erstellen eines Service Fabric-Clusters in Azure über das Azure-Portal](service-fabric-cluster-creation-via-portal.md).

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Unterstützte Betriebssysteme für Cluster in Azure

Sie können Cluster auf virtuellen Computern mit diesen Betriebssystemen erstellen:

* Windows Server 2012 R2
* Windows Server 2016 (sobald die allgemeine Verfügbarkeit angekündigt wurde)
* Linux Ubuntu 16.04 (in der öffentlichen Vorschau) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Erstellen lokaler oder cloudbasierter eigenständiger Service Fabric-Cluster

Service Fabric verfügt über ein Installationspaket, mit dem Sie eigenständige Service Fabric-Cluster lokal oder bei einem Cloudanbieter erstellen können.

Weitere Informationen zum Einrichten eigenständiger Service Fabric-Cluster unter Windows Server finden Sie unter [Erstellen und Verwalten eines Windows Server-Clusters](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs.-on-premises-deployments"></a>Gegenüberstellung von cloudbasierten und lokalen Bereitstellungen
Der Prozess zum lokalen Erstellen eines Service Fabric-Clusters ist vergleichbar mit der Erstellung eines Clusters in einer beliebigen Cloud mit einer Gruppe von VMs. Die ersten Schritte zum Bereitstellen der VMs sind abhängig von Ihrem Cloudanbieter bzw. von Ihrer lokalen Umgebung. Sobald Sie aber über eine Gruppe von VMs verfügen, die über ein Netzwerk verbunden sind, sind die Schritte zum Einrichten des Service Fabric-Pakets, Bearbeiten der Clustereinstellungen und Ausführen der Skripts zum Erstellen und Verwalten des Clusters identisch. Dadurch wird sichergestellt, dass Ihre Kompetenz und Erfahrung beim Betreiben und Verwalten von Service Fabric-Clustern bei Wahl einer neuen Hostingumgebung übertragbar sind.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Vorteile der Erstellung eigenständiger Service Fabric-Cluster
* Sie können einen beliebigen Cloudanbieter zum Hosten des Clusters auswählen.
* Einmal entwickelte Service Fabric-Anwendungen können mit minimalen oder sogar ganz ohne Änderungen in mehreren Hostingumgebungen ausgeführt werden.
* Kenntnisse der Erstellung von Service Fabric-Anwendungen lassen sich in andere Hostingumgebungen übertragen.
* Erfahrungen mit dem Betrieb und der Verwaltung von Service Fabric-Clustern lassen sich von einer Umgebung in eine andere Umgebung übertragen.
* Die Kundenreichweite wird nicht durch Einschränkungen bei der Hostingumgebung begrenzt.
* Die Möglichkeit, Dienste im Falle eines Ausfalls in einem Datencenter oder bei einem Cloudanbieter in eine andere Bereitstellungsumgebung zu verlagern, sorgt für ein zusätzliches Maß an Zuverlässigkeit und schützt vor weitreichenden Ausfällen.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Unterstützte Betriebssysteme für eigenständige Cluster
Sie können Cluster auf VMs oder Computer mit diesen Betriebssystemen erstellen:

* Windows Server 2012 R2
* Windows Server 2016 (sobald die allgemeine Verfügbarkeit angekündigt wurde)
* Linux (bald verfügbar)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Vorteile von Service Fabric-Clustern in Azure gegenüber eigenständigen Service Fabric-Clustern, die lokal erstellt wurden

Die Verwendung von Service Fabric-Clustern unter Azure bietet Vorteile gegenüber der lokalen Option. Wenn Sie also hinsichtlich des Ausführungsorts Ihrer Cluster keine besondere Präferenz haben, empfiehlt sich die Ausführung unter Azure. Unter Azure profitieren Sie von der Integration anderer Azure-Features und -Dienste, die den Betrieb und die Verwaltung des Clusters vereinfachen und zuverlässiger machen.

* **Azure-Portal:** Erleichtert das Erstellen und Verwalten von Clustern.

* **Azure-Ressourcen-Manager:** Vereinfacht die Verwaltung aller vom Cluster genutzten Ressourcen als Einheit sowie die Abrechnung und sorgt für Kostentransparenz.
* **Service Fabric-Cluster als Azure-Ressource** Ein Service Fabric-Cluster ist eine ARM-Ressource, sodass Sie ihn genau wie andere ARM-Ressourcen in Azure modellieren können.
* **Integration in Azure-Infrastruktur** Service Fabric wird mit der zugrunde liegenden Azure-Infrastruktur für Betriebssystem, Netzwerk und andere Upgrades koordiniert, um die Verfügbarkeit und Zuverlässigkeit Ihrer Anwendungen zu verbessern.  
* **Diagnose:** Integration von Azure-Diagnose und Log Analytics.
* **Automatische Skalierung:** Integrierte automatische Skalierungsfunktion für in Azure gehostete Cluster aufgrund von Virtual Machines-Skalierungsgruppen. In lokalen und anderen Cloudumgebungen müssen Sie eine eigene automatische Skalierungsfunktion entwickeln oder eine manuelle Skalierung mithilfe der APIs vornehmen, die Service Fabric für die Skalierung von Clustern verfügbar macht.

## <a name="next-steps"></a>Nächste Schritte
Erstellen eines Clusters auf virtuellen Windows- oder Windows Server-Computern: [Erstellen eines Service Fabric-Clusters für Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Erstellen eines Clusters auf virtuellen Linux- oder Linux-Computern: [Service Fabric on Linux](service-fabric-linux-overview.md)



<!--HONumber=Oct16_HO2-->


