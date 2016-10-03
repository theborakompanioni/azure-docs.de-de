<properties
   pageTitle="Azure Service Fabric unter Linux | Microsoft Azure"
   description="Service Fabric-Cluster unterstützen Linux und Java, d.h. Sie können in Java bzw. unter Linux geschriebene Anwendungen bereitstellen und hosten."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="SubramaR"/>

# Service Fabric unter Linux

Wie [in diesem Blogbeitrag](https://azure.microsoft.com/blog/service-fabric-on-linux-support-available-this-month/) angekündigt, wird die Vorschauversion am 26.09.2016 öffentlich zur Verfügung gestellt. Die Vorschauversion von Service Fabric unter Linux ermöglicht Ihnen, hoch verfügbare und skalierbare Anwendungen unter Linux (genauso wie unter Windows) zu erstellen, bereitzustellen und zu verwalten. Darüber hinaus stehen nun die Service Fabric-Frameworks für anspruchsvolle Aufgaben (Reliable Services und Reliable Actors) in Java unter Linux zur Verfügung.

> [AZURE.VIDEO service-fabric-linux-preview]

## Unterstützte Betriebssysteme und Programmiersprachen

Die eingeschränkte Vorschauversion unterstützt das Erstellen von Entwicklungsclustern mit einem Computer sowie Clustern mit mehreren Computern in Azure, auf denen Ubuntu Server 16.04 ausgeführt wird.

Sie können [ausführbare Gastdienste](service-fabric-deploy-existing-app.md) mit jeder beliebigen Sprache und jedem beliebigen Framework erstellen. Sie können zusätzlich zum Orchestrieren von Docker-Containern auch Java oder C# zum Erstellen von Diensten basierend auf den Reliable Services- und Reliable Actor-Frameworks verwenden.

>[AZURE.NOTE] Reliable Collections werden unter Linux noch nicht unterstützt.

Service Fabric unter Linux entspricht dem Konzept von Service Fabric unter Windows (mit Ausnahme von Betriebssystembesonderheiten und der Unterstützung von Programmiersprachen). Folglich können Sie einen Großteil unserer [vorhandenen Dokumentation](http://aka.ms/servicefabricdocs) nutzen, um sich mit der Technologie vertraut zu machen.

## Nächste Schritte

Machen Sie sich mit den Programmierframeworks [Reliable Actors](service-fabric-reliable-actors-introduction.md) und [Reliable Services](service-fabric-reliable-services-introduction.md) vertraut.

<!---HONumber=AcomDC_0921_2016-->