<properties
 pageTitle="Größen für Clouddienste"
 description="Führt die verschiedenen VM-Größen für Web-und Workerrollen von Azure-Clouddiensten auf."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="05/25/2016"
 ms.author="adegeo"/>

# Größen für Clouddienste

In diesem Thema werden die verfügbaren Größen und Optionen für Cloud Service-Rolleninstanzen (Web- und Workerrollen) beschrieben. Darüber hinaus werden Überlegungen zur Bereitstellung angestellt, die Sie berücksichtigen sollten, wenn Sie eine Verwendung dieser Ressourcen planen.

Cloud Services ist eine der verschiedenen Arten von Computeressourcen, die von Azure angeboten werden. Klicken Sie [hier](cloud-services-choose-me.md), um weitere Informationen zu Cloud Services zu erhalten.

> [AZURE.NOTE]Zugehörige Einschränkungen von Azure finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

## Größen für Web- und Workerrolleninstanzen

Die folgenden Überlegungen können Ihnen bei der Entscheidung über die Größe behilflich sein:

* VM-Instanzen der D-Serie dienen zum Ausführen von Anwendungen, die eine höhere Rechenleistung und eine höhere temporäre Datenträgerleistung erfordern. VMs der D-Serie bieten schnellere Prozessoren, ein höheres Verhältnis von Speicher zu Kern und ein SSD (Solid State Drive) für den temporären Datenträger. Einzelheiten finden Sie in der Ankündigung im Azure-Blog unter [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) (Neue VM-Größen der D-Serie, in englischer Sprache).  

* Die Dv2-Serie, eine Nachfolgerin der ursprünglichen D-Serie, hat eine leistungsfähigere CPU. Die CPU der Dv2-Serie ist ca. 35 % schneller als die CPU der D-Serie. Sie basiert auf der neuesten Generation des 2,4-GHz-Intel Xeon® E5-2673 v3-Prozessors (Haswell) und kann mit der Intel Turbo Boost Technology 2.0 bis auf 3,1 GHz erhöht werden. Der Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.

* Web- und Workerrollen erfordern aufgrund der Systemanforderungen mehr temporären Speicherplatz als Azure Virtual Machines. Die Systemdateien reservieren 4 GB Speicherplatz für die Windows-Auslagerungsdatei und 2 GB Speicherplatz für die Windows-Dumpdatei.

* Der Betriebssystem-Datenträger enthält das Windows-Gastbetriebssystem, den Ordner "Programme" (einschließlich mithilfe von Startaufgaben durchgeführter Installationen, es sei denn, Sie geben Sie einen anderen Datenträger an), Registrierungsänderungen, den Ordner "System32" und .NET Framework.

* Der **temporäre Speicherdatenträger** enthält Azure-Protokolle und Konfigurationsdateien, Azure Diagnose (enthält IIS-Protokolle) und die von Ihnen definierten lokalen Speicherressourcen.

* Der **Anwendungsdatenträger** ist der Ort, an dem Ihre CSPKG-Datei extrahiert wird. Er enthält u.a. Ihre Website, Binärdateien, den Rollenhostprozess, Startaufgaben und die Datei „web.config“.

* Die VM-Größen A8/A10 und A9/A11 haben die gleiche Kapazität. Die VM-Instanzen A8 und A9 umfassen einen zusätzlichen Netzwerkadapter, der mit einem RDMA (Remote Direct Memory Access)-Netzwerk für die schnelle Kommunikation zwischen virtuellen Computern verbunden ist. Die A8- und A9-Instanzen wurden für High Performance Computing-Anwendungen entwickelt, die während der Ausführung eine konstante Kommunikation mit niedriger Latenz zwischen den Knoten erfordern, z. B. MPI (Message Passing Interface)-Anwendungen. Die VM-Instanzen A10 und A11 enthalten keinen zusätzlichen Netzwerkadapter. Die A10- und A11-Instanzen sind für High Performance Computing-Anwendungen vorgesehen, die keine konstante Kommunikation mit niedriger Latenz zwischen den Knoten erfordern, auch bekannt als parametrische oder hochgradig parallele Anwendungen.

    >[AZURE.NOTE] Wenn Sie die Größen A8 bis A11 in Betracht ziehen, lesen Sie bitte [diese](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) Informationen.

>[AZURE.NOTE] Alle Computergrößen stellen einen **Anwendungsdatenträger** zur Verfügung, der alle Dateien aus Ihrem Clouddienstpaket speichert. Die ungefähre Größe des Datenträgers beträgt 1,5 GB.

Überprüfen Sie die [Preise](https://azure.microsoft.com/pricing/details/cloud-services/) der einzelnen Clouddienstgrößen.

## Allgemeiner Zweck

Für Websites, kleine bis mittlere Datenbanken und andere herkömmliche Anwendungen

>[AZURE.NOTE] Die Speicherkapazität wird unter Verwendung von 1024^3 Bytes als Maßeinheit für GB dargestellt. Dies wird manchmal als Gibibyte oder Binärdefinition (bzw. Definition zur Basis 2) bezeichnet. Denken Sie beim Vergleichen von Größen, die andere Basissysteme verwenden, jedoch daran, dass eine als Binärwert angegebene Größe möglicherweise kleiner als ein Dezimalwert zur Basis 10 erscheint (z. B. 1 GB), das Binärsystem jedoch mehr Kapazität als das Dezimalsystem bietet, da 1024^3 größer als 1000^3 ist.

| Größe (ID) | Kerne | RAM | Nettobandbreite | Gesamtgröße des Datenträgers: |
| --------------- | :-------: | ------: | :-----------: | -------: |
| Sehr klein | 1 | 0,75 GB | Niedrig | 19 GB |
| Klein | 1 | 1,75 GB | Moderat | 224 GB |
| Mittel | 2 | 3,5 GB | Moderat | 489 GB |
| Groß | 4 | 7 GB | Hoch | 999 GB |
| Extragroß | 8 | 14 GB | Hoch | 2\.039 GB |

>[AZURE.NOTE] **ExtraSmall** bis **ExtraLarge** können auch jeweils **A0-A4** genannt werden.

## Arbeitsspeicherintensiv

Für große Datenbanken, SharePoint-Serverfarmen und Anwendungen mit hohem Durchsatz.

| Größe (ID) | Kerne | RAM | Nettobandbreite | Gesamtgröße des Datenträgers: |
| --------------- | :-------: | ------: | :-----------: | ------:  |
| A5 | 2 | 14 GB | Moderat | 489 GB |
| A6 | 4 | 28 GB | Hoch | 999 GB |
| A7 | 8 | 56 GB | Hoch | 2\.039 GB |

## Netzwerkoptimierung mit InfiniBand-Unterstützung

Verfügbar in ausgewählten Rechenzentren. Virtuelle A8- und A9-Computer sind mit [Intel® Xeon® E5-Prozessoren](http://www.intel.com/content/www/us/en/processors/xeon/xeon-processor-e5-family.html) ausgestattet. Fügt ein **InfiniBand**-Netzwerk mit 32 GBit/s und RDMA-Technologie (Remote Direct Memory Access) hinzu. Ideal geeignet für MPI-Anwendungen (Message Passing Interface), Hochleistungscluster, Modellierung und Simulationen, Videocodierung und andere rechen- oder netzwerkintensive Szenarios.

| Größe (ID) | Kerne | RAM | Nettobandbreite | Gesamtgröße des Datenträgers: |
| --------------- | :-------: | ------: | :-----------: | ------: |
| A8 | 8 | 56 GB | Hoch | 382 GB |
| A9 | 16 | 112 GB | Sehr hoch | 382 GB |

## Rechenintensiv

Verfügbar in ausgewählten Rechenzentren. Virtuelle A10- und A11-Computer sind mit [Intel® Xeon® E5-Prozessoren](http://www.intel.com/content/www/us/en/processors/xeon/xeon-processor-e5-family.html) ausgestattet. Für hochleistungsfähige Cluster, Modellierung und Simulationen, Videocodierung sowie andere rechen- oder netzwerkintensive Szenarios. Mit der Konfiguration der A8- und A9-Instanzen vergleichbar, jedoch ohne InfiniBand-Netzwerk und RDMA-Technologie.

| Größe (ID) | Kerne | RAM | Nettobandbreite | Gesamtgröße des Datenträgers: |
| --------------- | :-------: | ------: | :-----------: | ------: |
| A10 | 8 | 56 GB | Hoch | 382 GB |
| A11 | 16 | 112 GB | Sehr hoch | 382 GB |

## D-Serie: Optimierte Computeleistung

Virtuelle Computer der D-Serie verfügen über SSD-Festplatten (Solid State Drives) und schnellere Prozessoren als die A-Serie (60 % schneller) und sind auch für die Web- und Workerrollen in Azure Cloud Services verfügbar. Die Serie eignet sich ideal für Anwendungen, die schnellere CPUs, eine bessere Leistung des lokalen Laufwerks oder mehr Arbeitsspeicher benötigen.

## Allgemeiner Zweck (D)

Für Websites, kleine bis mittlere Datenbanken und andere herkömmliche Anwendungen

| Größe (ID) | Kerne | RAM | Nettobandbreite | Gesamtgröße des Datenträgers: |
| --------------- | :-------: | ------: | :-----------: | ------: |
| Standard\_D1 | 1 | 3,5 GB | Moderat | 50 GB |
| Standard\_D2 | 2 | 7 GB | Hoch | 100 GB |
| Standard\_D3 | 4 | 14 GB | Hoch | 200 GB |
| Standard\_D4 | 8 | 28 GB | Hoch | 400 GB |

## Arbeitsspeicherintensiv (D)

Für große Datenbanken, SharePoint-Serverfarmen und Anwendungen mit hohem Durchsatz.

| Größe (ID) | Kerne | RAM | Nettobandbreite | Gesamtgröße des Datenträgers: |
| --------------- | :-------: | ------: | :-----------: | ------: |
| Standard\_D11 | 2 | 14 GB | Hoch | 100 GB |
| Standard\_D12 | 4 | 28 GB | Hoch | 200 GB |
| Standard\_D13 | 8 | 56 GB | Hoch | 400 GB |
| Standard\_D14 | 16 | 112 GB | Sehr hoch | 800 GB |

## Dv2-Serie: Optimierte Computeleistung

Bei Instanzen der Dv2-Serie handelt es sich um die nächste Generation von Instanzen der D-Serie, die als virtuelle Computer oder Cloud Services genutzt werden können. Instanzen der Dv2-Serie bieten leistungsfähigere CPUs, die durchschnittlich ca. 35 % schneller sind als die CPUs der Instanzen der D-Serie. Die Arbeitsspeicher- und Datenträgerkonfiguration entspricht der Konfiguration der D-Serie. Instanzen der Dv2-Serie basieren auf der jüngsten Generation von 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell)-Prozessoren und können mit Intel Turbo Boost Technology 2.0 bis zu 3,1 GHz erreichen. Die Dv2- und die D-Serie eignen sich ideal für Anwendungen, die schnellere CPUs, ein besseres lokales Laufwerk oder mehr Arbeitsspeicher benötigen. Diese Instanzen bieten eine leistungsfähige Kombination für eine Vielzahl von Anwendungen auf Unternehmensniveau.

## Allgemeiner Zweck (Dv2)

Für Websites, kleine bis mittlere Datenbanken und andere herkömmliche Anwendungen

| Größe (ID) | Kerne | RAM | Nettobandbreite | Gesamtgröße des Datenträgers: |
| --------------- | :-------: | ------: | :-----------: | ------: |
| Standard\_D1\_v2 | 1 | 3,5 GB | Moderat | 50 GB |
| Standard\_D2\_v2 | 2 | 7 GB | Hoch | 100 GB |
| Standard\_D3\_v2 | 4 | 14 GB | Hoch | 200 GB |
| Standard\_D4\_v2 | 8 | 28 GB | Hoch | 400 GB |
| Standard\_D5\_v2 | 16 | 56 GB | Sehr hoch | 800 GB |

## Arbeitsspeicherintensiv (Dv2)

Für große Datenbanken, SharePoint-Serverfarmen und Anwendungen mit hohem Durchsatz

| Größe (ID) | Kerne | RAM | Nettobandbreite | Gesamtgröße des Datenträgers: |
| --------------- | :-------: | ------: | :-----------: | -------: |
| Standard\_D11\_v2 | 2 | 14 GB | Hoch | 100 GB |
| Standard\_D12\_v2 | 4 | 28 GB | Hoch | 200 GB |
| Standard\_D13\_v2 | 8 | 56 GB | Hoch | 400 GB |
| Standard\_D14\_v2 | 16 | 112 GB | Sehr hoch | 800 GB |

## Konfigurieren von Größen für Cloud Services

Sie können die Größe des virtuellen Computers einer Rolleninstanz als Teil des Dienstmodells angeben, wie durch die [Dienstdefinitionsdatei](cloud-services-model-and-package.md#csdef) beschrieben. Die Größe einer Rolle bestimmt die Anzahl der CPU-Kerne, die Speicherkapazität und die lokale Dateisystemgröße, die einer aktiven Instanz zugeordnet werden. Wählen Sie die Rollengröße basierend auf der Ressourcenanforderung Ihrer Anwendung.

Dies ist ein Beispiel, bei dem die Rollengröße [Standard\_D2](#general-purpose-d) für eine Webrolleninstanz festgelegt wird:

```xml
<WebRole name="WebRole1" vmsize="<mark>Standard_D2</mark>">
...
</WebRole>
```

<!---HONumber=AcomDC_0615_2016-->