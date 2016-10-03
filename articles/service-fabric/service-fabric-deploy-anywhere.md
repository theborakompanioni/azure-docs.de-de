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
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# Erstellen von eigenständigen Service Fabric-Clustern unter Windows Server oder Linux
Azure Service Fabric ermöglicht die Erstellung von Service Fabric-Clustern auf VMs oder Computern mit Windows Server oder Linux. Dadurch lassen sich Service Fabric-Anwendungen in jeder lokalen oder cloudbasierten Umgebung bereitstellen und ausführen, in der sich miteinander verbundene Computer mit Windows Server oder Linux befinden.

**Hinweis**: Das Erstellen eines Clusters unter Azure sollte entweder mit einer Azure Resource Manager-Vorlage oder mit dem Azure-Portal durchgeführt werden. Weitere Informationen finden Sie unter [Erstellen eines Service Fabric-Clusters mit einer Azure Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md) oder [Erstellen eines Service Fabric-Clusters im Azure-Portal](service-fabric-cluster-creation-via-portal.md).

Service Fabric verfügt über ein Installationspaket, mit dem Sie diese eigenständigen Service Fabric-Cluster lokal erstellen können. Ein entscheidender Vorteil hierbei ist, dass es beim Erstellen einer Anwendung mit Service Fabric keine Anbieterabhängigkeit gibt, da Sie wählen, wo diese Anwendungen ausgeführt werden. So können Sie sich einen größeren Kundenstamm erschließen, da Kunden hinsichtlich der Umgebungen, in denen sie Ihre Anwendungen ausführen möchten, unter Umständen unterschiedliche Anforderungen haben. So haben Unternehmen im Gesundheitswesen und Finanzsektor ggf. andere Anforderungen als ein Autohersteller oder Reiseanbieter.

## Unterstützte Betriebssysteme
Sie können Cluster auf VMs oder Computer mit diesen Betriebssystemen erstellen:

* Windows Server 2012 R2
* Windows Server 2016 (sobald die allgemeine Verfügbarkeit angekündigt wurde)
* Linux (in Kürze)

Weitere Informationen zu Windows Server finden Sie unter [Erstellen und Verwalten eines Windows Server-Clusters](service-fabric-cluster-creation-for-windows-server.md).

## Erstellung und Konfiguration von Clustern
Service Fabric verfügt über ein Installationspaket, das Sie herunterladen können. Nach dem Herunterladen dieses Pakets müssen Sie eine JSON-Konfigurationsdatei bearbeiten, um die Einstellungen für Ihren Cluster anzugeben. Nach der Bearbeitung der Clustereinstellungen führen Sie ein Setupskript aus, mit dem der Cluster mit den Computern erstellt wird, die Sie in Ihren Clustereinstellungen angegeben haben. Sie können auch ein Skript ausführen, um einen Cluster für eine Gruppe von Computern zu entfernen.

## Gegenüberstellung von cloudbasierten und lokalen Bereitstellungen
Der Prozess zum lokalen Erstellen eines Service Fabric-Clusters ist vergleichbar mit der Erstellung eines Clusters in einer beliebigen Cloud mit einer Gruppe von VMs. Die ersten Schritte zum Bereitstellen der VMs sind abhängig von Ihrem Cloudanbieter bzw. von Ihrer lokalen Umgebung. Sobald Sie aber über eine Gruppe von VMs verfügen, die über ein Netzwerk verbunden sind, sind die Schritte zum Einrichten des Service Fabric-Pakets, Bearbeiten der Clustereinstellungen und Ausführen der Skripts zum Erstellen und Verwalten des Clusters identisch. Dadurch wird sichergestellt, dass Ihre Kompetenz und Erfahrung beim Betreiben und Verwalten von Service Fabric-Clustern bei Wahl einer neuen Hostingumgebung übertragbar sind.

## Vorteile der Erstellung eigenständiger Service Fabric-Cluster
* Aufgrund fehlender Anbieterabhängigkeit können Sie frei wählen, wo Sie Ihren Cluster erstellen.
* Einmal entwickelte Service Fabric-Anwendungen können mit minimalen oder sogar ganz ohne Änderungen in mehreren Hostingumgebungen ausgeführt werden.
* Kenntnisse der Erstellung von Service Fabric-Anwendungen lassen sich in andere Hostingumgebungen übertragen.
* Erfahrungen mit dem Betrieb und der Verwaltung von Service Fabric-Clustern lassen sich von einer Umgebung in eine andere Umgebung übertragen.
* Die Kundenreichweite wird nicht durch Einschränkungen bei der Hostingumgebung begrenzt.
* Die Möglichkeit, Dienste im Falle eines Ausfalls in einem Datencenter oder bei einem Cloudanbieter in eine andere Bereitstellungsumgebung zu verlagern, sorgt für ein zusätzliches Maß an Zuverlässigkeit und schützt vor weitreichenden Ausfällen.

## Vorteile von Service Fabric-Clustern in Azure gegenüber eigenständigen Service Fabric-Clustern, die lokal erstellt wurden
Die Verwendung von Service Fabric-Clustern unter Azure bietet Vorteile gegenüber der lokalen Option. Wenn Sie also hinsichtlich des Ausführungsorts Ihrer Cluster keine besondere Präferenz haben, empfiehlt sich die Ausführung unter Azure. Unter Azure profitieren Sie von der Integration anderer Azure-Features und -Dienste, die den Betrieb und die Verwaltung des Clusters vereinfachen und zuverlässiger machen.

* **Azure-Portal:** Erleichtert das Erstellen und Verwalten von Clustern.

* **Azure-Ressourcen-Manager:** Vereinfacht die Verwaltung aller vom Cluster genutzten Ressourcen als Einheit sowie die Abrechnung und sorgt für Kostentransparenz.
* **Service Fabric-Cluster als Azure-Ressource** Ein Service Fabric-Cluster ist eine ARM-Ressource, sodass Sie ihn genau wie andere ARM-Ressourcen in Azure modellieren können.
* **Integration in Azure-Infrastruktur** Service Fabric wird mit der zugrunde liegenden Azure-Infrastruktur für Betriebssystem, Netzwerk und andere Upgrades koordiniert, um die Verfügbarkeit und Zuverlässigkeit Ihrer Anwendungen zu verbessern.
* **Diagnose:** Integration von Azure-Diagnose und Log Analytics.
* **Automatische Skalierung:** Integrierte automatische Skalierungsfunktion für in Azure gehostete Cluster aufgrund von Virtual Machines-Skalierungsgruppen. In lokalen und anderen Cloudumgebungen müssen Sie eine eigene automatische Skalierungsfunktion entwickeln oder eine manuelle Skalierung mithilfe der APIs vornehmen, die Service Fabric für die Skalierung von Clustern verfügbar macht.

## Nächste Schritte
Erstellen eines Clusters auf virtuellen Windows- oder Windows Server-Computern: [Erstellen eines Service Fabric-Clusters für Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Erstellen eines Clusters auf virtuellen Linux- oder Linux-Computern: [Service Fabric on Linux](service-fabric-linux-overview.md) (Service Fabric unter Linux)

<!---HONumber=AcomDC_0921_2016-->