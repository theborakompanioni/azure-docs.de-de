---
title: Übersicht über die Service Fabric-Programmiermodelle | Microsoft Docs
description: 'Service Fabric bietet zwei Frameworks zum Erstellen von Diensten: das Actors-Framework und das Services-Framework. Sie bieten unterschiedliche Kompromisse im Hinblick auf Einfachheit und Steuerung.'
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: vturecek

ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/18/2016
ms.author: seanmck

---
# Übersicht über die Service Fabric-Programmiermodelle
Service Fabric bietet verschiedene Methoden zum Schreiben und Verwalten von Diensten. Ein Dienst kann die Service Fabric-APIs verwenden, um alle Vorteile der Features und Anwendungsframeworks der Plattform zu nutzen. Bei einem Dienst kann es sich aber auch einfach um ein kompiliertes ausführbares Programm handeln, das in einer beliebigen Sprache geschrieben wurde und auf einem Service Fabric-Cluster gehostet wird.

## Ausführbare Gastanwendungsdatei
Bei einer ausführbaren Gastanwendungsdatei handelt es sich um eine beliebige ausführbare Datei, die in einer beliebigen Sprache geschrieben sein kann. Sie können also Ihre vorhandenen Anwendungen auf einem Service Fabric-Cluster hosten. Eine ausführbare Gastanwendungsdatei kann in einer Anwendung gepackt und neben weiteren Diensten gehostet werden. Service Fabric sorgt für die Orchestrierung und einfache Ausführung der ausführbaren Datei und stellt sicher, dass sie gemäß Dienstbeschreibung einsatzbereit bleibt. Da sich ausführbare Gastanwendungsdateien jedoch nicht direkt in Service Fabric-APIs integrieren lassen, profitieren sie nicht von allen Features, die die Plattform bietet. Beispiele: benutzerdefinierte Berichterstellung zu Integrität und Auslastung, Registrierung von Dienstendpunkten und zustandsbehaftete Berechnung.

Beginnen Sie damit, die erste [ausführbare Gastanwendungsdatei](service-fabric-deploy-existing-app.md) bereitzustellen.

## Reliable Services
Reliable Services ist ein schlankes Framework zum Schreiben von Diensten, die sich in die Service Fabric-Plattform integrieren lassen und von allen Features der Plattform profitieren. Reliable Services stellen einen minimalen Satz von APIs bereit, die der Service Fabric-Runtime ermöglichen, den Lebenszyklus Ihrer Dienste zu verwalten, und Ihren Diensten ermöglichen, mit der Runtime zu interagieren. Das Anwendungsframework ist sehr schlank, bietet vollständige Kontrolle über Entwurfs- und Implementierungsoptionen und kann zum Hosten jedes anderen Anwendungsframeworks wie ASP.NET MVC oder Web-API verwendet werden.

Reliable Services können zustandsfrei sein, ähnlich wie die meisten Dienstplattformen wie z.B. Webserver oder Workerrollen in Azure Cloud Services – hierbei werden alle Dienstinstanzen auf die gleiche Weise erstellt, und der Status wird in einer externen Lösung wie z.B. Azure DB oder Azure Table Storage gespeichert.

Reliable Services können auch zustandsbehaftet sein – ein exklusives Feature von Service Fabric, da der Status mithilfe von Reliable Collections direkt im Dienst gespeichert wird. Der Status wird durch Replikation als „hochverfügbar“ festgelegt und durch Partitionierung verteilt. Die Verwaltung erfolgt automatisch durch Service Fabric.

[Erfahren Sie mehr über Reliable Services](service-fabric-reliable-services-introduction.md) oder legen Sie los, [indem Sie Ihren ersten eigenen Reliable Service schreiben](service-fabric-reliable-services-quick-start.md).

## Reliable Actors
Das Reliable Actor-Anwendungsframework setzt auf Reliable Services auf und implementiert das „Virtual Actor“-Muster, basierend auf dem Entwurfsmuster für Akteure. Das Reliable Actor-Framework verwendet unabhängige Compute- und Statuseinheiten mit Singlethreadausführung, die als Akteure bezeichnet werden. Das Reliable Actor-Framework bietet integrierte Kommunikation für Akteure sowie voreingestellte Statuspersistenz und horizontal hochskalierbare Konfigurationen.

Da es sich bei Reliable Actors selbst um ein Anwendungsframework handelt, das auf Reliable Services aufsetzt, ist es vollständig in die Service Fabric-Plattform integriert und profitiert von allen Features, die die Plattform bietet.

## Nächste Schritte
[Erfahren Sie mehr über Reliable Actors](service-fabric-reliable-actors-introduction.md), oder legen Sie direkt los, indem Sie [Ihren ersten Reliable Actor-Dienst erstellen](service-fabric-reliable-actors-get-started.md).

<!---HONumber=AcomDC_0720_2016-->