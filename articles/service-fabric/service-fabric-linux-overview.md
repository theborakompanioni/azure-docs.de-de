---
title: Azure Service Fabric unter Linux | Microsoft Docs
description: "Service Fabric-Cluster unterstützen Linux und Java, d.h. Sie können in Java bzw. C# geschriebene Anwendungen unter Linux bereitstellen und hosten."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 459afade-145d-4ee6-b72b-ddf380ccd1bf
ms.service: service-fabric
ms.devlang: Java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: SubramaR
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: dddc9f698d9776999d406117b46285a0f90d9620
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="service-fabric-on-linux"></a>Service Fabric unter Linux
Die Vorschauversion von Service Fabric unter Linux ermöglicht Ihnen, hoch verfügbare und skalierbare Anwendungen unter Linux (genauso wie unter Windows) zu erstellen, bereitzustellen und zu verwalten. Neben C# (.NET Core) stehen Service Fabric-Frameworks (Reliable Services und Reliable Actors) in Java unter Linux zur Verfügung.  Sie können auch [ausführbare Gastdienste](service-fabric-deploy-existing-app.md) mit jeder beliebigen Sprache und jedem beliebigen Framework erstellen. Die Vorschau unterstützt darüber hinaus das Orchestrieren von Docker-Containern. Docker-Container können ausführbare Gastanwendungsdateien oder native Service Fabric-Dienste ausführen, die die Service Fabric-Frameworks nutzen.

Service Fabric unter Linux entspricht dem Konzept von Service Fabric unter Windows (mit Ausnahme von Betriebssystembesonderheiten und der Unterstützung von Programmiersprachen). Folglich können Sie einen Großteil unserer [vorhandenen Dokumentation](http://aka.ms/servicefabricdocs) nutzen, um sich mit der Technologie vertraut zu machen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a>Unterstützte Betriebssysteme und Programmiersprachen
Die eingeschränkte Vorschauversion unterstützt das Erstellen von Entwicklungsclustern mit einem Computer sowie Clustern mit mehreren Computern in Azure, auf denen Ubuntu Server 16.04 ausgeführt wird. Die Vorschau unterstützt zusätzlich zu ausführbaren Gastanwendungsdateien und zur Orchestrierung von Docker-Containern die Reliable Actors-Frameworks und Frameworks für zustandslose Reliable Services in Java und C#.  

> [!NOTE]
> Reliable Collections werden unter Linux noch nicht unterstützt. Eigenständige Cluster werden ebenfalls nicht unterstützt. In der Vorschau werden lediglich One-Box-Cluster und Azure Linux-Cluster mit mehreren Computern unterstützt.
>
>


## <a name="supported-tooling"></a>Unterstützte Tools
Die Vorschau unterstützt die Interaktion mit dem Cluster über die Service Fabric CLI. Für Java-Entwickler erfolgt die Integration in Eclipse und Yeoman mit Eclipse. Die Integration wird unter Linux und OSX unterstützt. Die OSX-Integration basiert auf einem virtuellen Linux-Computer und erfolgt über Vagrant. Für C#-Entwickler wird die Integration in Yeoman zum Erstellen von Anwendungsvorlagen bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit den Programmierframeworks [Reliable Actors](service-fabric-reliable-actors-introduction.md) und [Reliable Services](service-fabric-reliable-services-introduction.md) vertraut.
* [Vorbereiten Ihrer Entwicklungsumgebung unter Linux](service-fabric-get-started-linux.md)
* [Prepare your development environment on OSX (Vorbereiten Ihrer Entwicklungsumgebung unter OSX)](service-fabric-get-started-mac.md)
* [Create your first Service Fabric Java application on Linux](service-fabric-create-your-first-linux-application-with-java.md)
* [Einrichten von Continuous Integration und Continuous Deployment für Service Fabric mit Jenkins und GitHub](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [Unterschiede zwischen Service Fabric unter Linux und Windows](service-fabric-linux-windows-differences.md)

