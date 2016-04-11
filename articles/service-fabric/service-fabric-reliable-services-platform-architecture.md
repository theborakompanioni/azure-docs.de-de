<properties
   pageTitle="Reliable Services-Architektur | Microsoft Azure"
   description="Übersicht über die Reliable Services-Architektur für zustandsbehaftete und zustandslose Dienste"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# Architektur für zustandsbehaftete und zustandslose Reliable Services

Reliable Services von Azure Service Fabric können zustandsbehaftet oder zustandslos sein. Jeder Diensttyp wird im Rahmen einer bestimmten Architektur ausgeführt. Diese Architekturen werden in diesem Artikel beschrieben. Weitere Informationen zu den Unterschieden zwischen zustandsbehafteten und zustandslosen Diensten finden Sie in der [Übersicht über Reliable Services](service-fabric-reliable-services-introduction.md).

## Zustandsbehaftete Reliable Services

### Architektur eines zustandsbehafteten Diensts
![Architekturdiagramm eines zustandsbehafteten Diensts](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### Zustandsbehafteter Reliable Service

Ein zustandsbehafteter Reliable Service kann von der Klasse StatefulService oder StatefulServiceBase abgeleitet werden. Beide Basisklassen werden von Service Fabric bereitgestellt. Sie bieten verschiedene Unterstützungs- und Abstraktionsstufen, damit der zustandsbehaftete Dienst eine Verbindung mit Service Fabric herstellen und als Dienst innerhalb des Service Fabric-Cluster agieren kann.

„StatefulService“ wird von „StatefulServiceBase“ abgeleitet. „StatefulServiceBase“ bietet Diensten mehr Flexibilität, erfordert jedoch mehr Kenntnisse über Service Fabric. Weitere Informationen zum Schreiben von Diensten mit den Klassen „StatefulService“ und „StatefulServiceBase“ finden Sie in der [Übersicht über Reliable Services](service-fabric-reliable-services-introduction.md) und im Artikel über die [erweiterte Verwendung von Reliable Services](service-fabric-reliable-services-advanced-usage.md).

Beide Basisklassen verwalten die Lebensdauer und die Rolle der Dienstimplementierung. Die Dienstimplementierung kann die virtuellen Methoden beider Basisklassen überschreiben, wenn sie an diesen Punkten in ihrem Lebenszyklus Aufgaben auszuführen hat oder ein Objekt für den Kommunikationslistener erstellt werden soll. Eine Dienstimplementierung kann ihr eigenes Kommunikationslistener-Objekt implementieren und „ICommunicationListener“ verfügbar machen. In der vorangegangenen Abbildung wird der Kommunikationslistener jedoch von Service Fabric implementiert, da die Dienstimplementierung einen von Service Fabric implementierten Kommunikationslistener verwendet.

Ein zustandsbehafteter Reliable Service verwendet den zuverlässigen Zustands-Manager, um von zuverlässigen Auflistungen zu profitieren. Zuverlässige Auflistungen sind lokale, hochverfügbare Datenstrukturen für den Dienst. Sie sind also unabhängig von Dienstfailovern immer verfügbar. Jeder Typ einer zuverlässigen Auflistung wird von einem zuverlässigen Zustandsanbieter implementiert. Weitere Informationen zu zuverlässigen Auflistungen finden Sie in der [Übersicht über zuverlässige Auflistungen](service-fabric-reliable-services-reliable-collections.md).

### Zuverlässiger Zustands-Manager und zuverlässige Zustandsanbieter

Der zuverlässige Zustands-Manager ist das Objekt für die Verwaltung zuverlässiger Zustandsanbieter. Es umfasst Funktionen zum Erstellen, Löschen, Auflisten und Sicherstellen der Persistenz und hohen Verfügbarkeit zuverlässiger Zustandsanbieter. Eine Instanz eines zuverlässigen Zustandsanbieters repräsentiert eine persistente und hochverfügbare Datenstruktur (beispielsweise ein Wörterbuch oder eine Warteschlange).

Jeder zuverlässige Zustandsanbieter macht eine Schnittstelle verfügbar, über die der zustandsbehaftete Dienst mit dem zuverlässigen Zustandsanbieter interagiert. „IReliableDictionary“ wird beispielsweise verwendet, um eine Verbindung mit dem zuverlässigen Wörterbuch herzustellen, während mit „IReliableQueue“ eine Verbindung mit der zuverlässigen Warteschlange hergestellt wird. Alle zuverlässigen Zustandsanbieter implementieren die IReliableState-Schnittstelle.

Der zuverlässige Zustands-Manager verfügt über eine Schnittstelle namens „IReliableStateManager“, die den Zugriff über einen zustandsbehafteten Dienst ermöglicht. Schnittstellen zu zuverlässigen Zustandsanbietern werden über IReliableStateManager zurückgegeben.

Der zuverlässige Zustands-Manager basiert auf einer dynamischen Plug-In-Architektur, sodass neue Typen zuverlässiger Auflistungen dynamisch integriert werden können.

Das zuverlässige Wörterbuch und die zuverlässige Warteschlange basieren auf der Implementierung eines leistungsfähigen, versionierten, differenziellen Speichers.

### Transaktionsreplikator

Der Transaktionsreplikator stellt sicher, dass der Zustand des Diensts (also der Zustand innerhalb des zuverlässigen Zustands-Managers und der zuverlässigen Auflistungen) in allen Replikaten, die den Dienst ausführen, konsistent ist. Er stellt außerdem sicher, dass der Zustand im Protokoll persistent ist. Der zuverlässige Zustands-Manager wird über einen privaten Mechanismus mit dem Transaktionsreplikator verbunden.

Der Transaktionsreplikator kommuniziert den Zustand mithilfe eines Netzwerkprotokolls an andere Replikate der Dienstinstanz, sodass alle Replikate über aktuelle Zustandsinformationen verfügen.

Der Transaktionsreplikator verwendet ein Protokoll, um Zustandsinformationen auch dann beizubehalten, wenn ein Prozess unterbrochen wird oder ein Knoten ausfällt. Die Schnittstelle zum Protokoll wird über einen privaten Mechanismus hergestellt.

### Protokoll

Die Protokollierungskomponente bietet einen leistungsfähigen persistenten Speicher, der für Schreibvorgänge auf herkömmliche rotierende Festplatten oder auf SSDs optimiert werden kann. Das Protokolldesign sieht vor, dass sich der persistente Speicher (also die Festplatten) lokal auf den Knoten befindet, die den zustandsbehafteten Dienst ausführen. Im Gegensatz zu einem persistenten, sich nicht lokal auf den Knoten befindlichen Remotespeicher ermöglicht dies geringere Latenzen und einen höheren Durchsatz.

Die Protokollierungskomponente verwendet mehrere Protokolldateien. Eine ist knotenübergreifend freigegeben und wird von allen Replikaten verwendet, da sie die niedrigste Latenz und den höchsten Durchsatz zum Speichern von Daten bieten kann. Standardmäßig befindet sich das freigegebene Protokoll im Service Fabric-Knotenarbeitsverzeichnis. Es kann aber auch so konfiguriert werden, dass es an einem anderen Ort, idealerweise auf einem extra dafür reservierten Datenträger gespeichert wird. Jedes Replikat für den Dienst verfügt ebenfalls über eine dedizierte Protokolldatei. Das dedizierte Protokoll wird im Arbeitsverzeichnis des Diensts gespeichert. Es gibt keinen Mechanismus zum Konfigurieren eines anderen Speicherorts für das dedizierte Protokoll.

Das freigegebene Protokoll fungiert als Übergangsbereich für die Zustandsinformationen des Replikats. Die dedizierte Protokolldatei hingegen ist das endgültige Ziel, an dem diese Informationen persistent gespeichert werden. Die Zustandsinformationen werden in diesem Fall zunächst in die freigegebene Protokolldatei geschrieben und anschließend im Hintergrund zeitverzögert in die dedizierte Protokolldatei verschoben. Auf diese Weise erfolgen Schreibvorgänge in das freigegebene Protokoll mit der geringstmöglichen Latenz und dem höchstmöglichen Durchsatz, was den Dienst insgesamt beschleunigt.

Lese- und Schreibvorgänge zum freigegebenen Protokoll erfolgen über direkte E/A an einen vorab zugeordneten Speicherplatz auf dem Datenträger für die freigegebene Protokolldatei. Um eine optimale Nutzung des Speicherplatzes auf dem Laufwerk mit dedizierten Protokollen zu ermöglichen, wird die dedizierte Protokolldatei als NTFS-Datei mit geringer Datendichte erstellt. Beachten Sie, dass dadurch übermäßig viel Festplattenspeicherplatz bereitgestellt werden kann, und dass das Betriebssystem möglicherweise für die dedizierten Protokolldateien die Verwendung von deutlich mehr Festplattenspeicher meldet, als tatsächlich der Fall ist.

Abgesehen von einer minimalen Benutzermodusschnittstelle wird das Protokoll als Kernelmodustreiber geschrieben. Durch die Ausführung als Kernelmodustreiber kann das Protokoll allen Diensten, die es verwenden, die höchste Leistung bereitstellen.

Weitere Informationen zum Konfigurieren des Protokolls finden Sie unter [Konfigurieren zustandsbehafteter Reliable Services](service-fabric-reliable-services-configuration.md).

## Zustandsloser Reliable Service

### Architektur eines zustandslosen Diensts
![Architekturdiagramm eines zustandslosen Diensts](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### Zustandsloser Reliable Service

Zustandslose Dienstimplementierungen werden von der Klasse „StatelessService“ oder „StatelessServiceBase“ abgeleitet. Die Klasse „StatelessServiceBase“ ist flexibler als „StatelessService“. Beide Basisklassen verwalten die Lebensdauer und Rolle eines Diensts.

Die Dienstimplementierung kann die virtuellen Methoden beider Basisklassen überschreiben, wenn der Dienst an diesen Punkten in seinem Lebenszyklus Aufgaben auszuführen hat oder ein Objekt für den Kommunikationslistener erstellt werden soll. Der Dienst kann sein eigenes Kommunikationslistener-Objekt implementieren und „ICommunicationListener“ verfügbar machen. In der vorangegangenen Abbildung wird der Kommunikationslistener jedoch von Service Fabric implementiert, da diese Dienstimplementierung einen von Service Fabric implementierten Kommunikationslistener verwendet.

Weitere Informationen zum Schreiben von Diensten mit den Klassen „StatelessService“ und „StatelessServiceBase“ finden Sie in der [Übersicht über Reliable Services](service-fabric-reliable-services-introduction.md) und im Artikel zur [erweiterten Verwendung von Reliable Services](service-fabric-reliable-services-advanced-usage.md).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte

Weitere Informationen zu Service Fabric finden Sie unter:

[Übersicht über Reliable Services](service-fabric-reliable-services-introduction.md)

[Schnellstart](service-fabric-reliable-services-quick-start.md)

[Übersicht über zuverlässige Auflistungen](service-fabric-reliable-services-reliable-collections.md)

[Erweiterte Verwendung von Reliable Services](service-fabric-reliable-services-advanced-usage.md)

[Konfigurieren von Reliable Services](service-fabric-reliable-services-configuration.md)

<!---HONumber=AcomDC_0330_2016-->