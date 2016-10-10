<properties
   pageTitle="Azure Service Fabric unter Linux | Microsoft Azure"
   description="Service Fabric-Cluster unterstützen Linux und Java, d.h. Sie können in Java bzw. C# geschriebene Anwendungen unter Linux bereitstellen und hosten."
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
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# Service Fabric unter Linux

Die Vorschauversion von Service Fabric unter Linux ermöglicht Ihnen, hoch verfügbare und skalierbare Anwendungen unter Linux (genauso wie unter Windows) zu erstellen, bereitzustellen und zu verwalten. Neben C# (.NET Core) stehen Service Fabric-Frameworks (Reliable Services und Reliable Actors) in Java unter Linux zur Verfügung. Sie können auch [ausführbare Gastdienste](service-fabric-deploy-existing-app.md) mit jeder beliebigen Sprache und jedem beliebigen Framework erstellen. Die Vorschau unterstützt darüber hinaus das Orchestrieren von Docker-Containern. Docker-Container können ausführbare Gastanwendungsdateien oder native Service Fabric-Dienste ausführen, die die Service Fabric-Frameworks nutzen.

Service Fabric unter Linux entspricht dem Konzept von Service Fabric unter Windows (mit Ausnahme von Betriebssystembesonderheiten und der Unterstützung von Programmiersprachen). Folglich können Sie einen Großteil unserer [vorhandenen Dokumentation](http://aka.ms/servicefabricdocs) nutzen, um sich mit der Technologie vertraut zu machen.

> [AZURE.VIDEO service-fabric-linux-preview]

## Unterstützte Betriebssysteme und Programmiersprachen

Die eingeschränkte Vorschauversion unterstützt das Erstellen von Entwicklungsclustern mit einem Computer sowie Clustern mit mehreren Computern in Azure, auf denen Ubuntu Server 16.04 ausgeführt wird. Die Vorschau unterstützt zusätzlich zu ausführbaren Gastanwendungsdateien und zur Orchestrierung von Docker-Containern die Reliable Actors-Frameworks und Frameworks für zustandslose Reliable Services in Java und C#.

>[AZURE.NOTE] Reliable Collections werden unter Linux noch nicht unterstützt. Eigenständige Cluster werden ebenfalls nicht unterstützt. In der Vorschau werden lediglich One-Box-Cluster und Azure Linux-Cluster mit mehreren Computern unterstützt.

## Unterstützte Tools

Die Vorschau unterstützt die Interaktion mit dem Cluster über die Azure-Befehlszeilenschnittstelle. Für Java-Entwickler erfolgt die Integration in Eclipse und Yeoman mit Eclipse. Die Integration wird unter Linux und OSX unterstützt. Die OSX-Integration basiert auf einem virtuellen Linux-Computer und erfolgt über Vagrant. Für C#-Entwickler wird die Integration in Yeoman zum Erstellen von Anwendungsvorlagen bereitgestellt.

## Nächste Schritte


1. Machen Sie sich mit den Programmierframeworks [Reliable Actors](service-fabric-reliable-actors-introduction.md) und [Reliable Services](service-fabric-reliable-services-introduction.md) vertraut.

2. [Vorbereiten Ihrer Entwicklungsumgebung unter Linux](service-fabric-get-started-linux.md)

3. [Prepare your development environment on OSX (Vorbereiten Ihrer Entwicklungsumgebung unter OSX)](service-fabric-get-started-mac.md)

4. [Create your first Service Fabric Java application on Linux](service-fabric-create-your-first-linux-application-with-java.md) (Erstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux)

<!---HONumber=AcomDC_0928_2016-->