---
title: "Übersicht über die Service Fabric-Programmiermodelle | Microsoft Docs"
description: 'Service Fabric bietet zwei Frameworks zum Erstellen von Diensten: das Actors-Framework und das Services-Framework. Sie bieten unterschiedliche Kompromisse im Hinblick auf Einfachheit und Steuerung.'
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: vturecek
ms.assetid: 974b2614-014e-4587-a947-28fcef28b382
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: vturecek
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 102ede8d2aafaf485a5212faad47de6781d84578
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="service-fabric-programming-model-overview"></a>Übersicht über die Service Fabric-Programmiermodelle
Service Fabric bietet verschiedene Methoden zum Schreiben und Verwalten von Diensten. Dienste können die Service Fabric-APIs verwenden, um Features und Anwendungsframeworks der Plattform umfassend zu nutzen. Ein Dienst kann auch ein kompiliertes ausführbares Programm sein, das in einer beliebigen Sprache oder in einem beliebigen Code geschrieben wurde und in einem Container ausgeführt wird, der einfach in einem Service Fabric-Cluster gehostet wird.

### <a name="guest-executables"></a>Ausführbare Gastanwendungsdateien
Bei einer [ausführbaren Gastanwendungsdatei](service-fabric-deploy-existing-app.md) handelt es sich um eine vorhandene, beliebig ausführbare Datei (die in einer beliebigen Sprache geschrieben sein kann), die neben anderen Diensten auf einem Service Fabric-Cluster gehostet wird. Ausführbare Gastanwendungsdateien lassen sich nicht direkt in Service Fabric-APIs integrieren. Sie profitieren jedoch weiterhin von den angebotenen Features der Plattform. Hierzu zählen beispielsweise Integritäts- und Auslastungsberichte und die Auffindbarkeit von Diensten durch Aufrufen von REST-APIs. Außerdem wird der Anwendungslebenszyklus vollständig unterstützt.

Beginnen Sie damit, die erste [ausführbare Gastanwendungsdatei](service-fabric-deploy-existing-app.md)bereitzustellen.

### <a name="containers"></a>Container
Standardmäßig werden Dienste von Service Fabric als Prozesse bereitgestellt und aktiviert. Service Fabric kann Dienste auch in [Containern](service-fabric-containers-overview.md) bereitstellen. Service Fabric unterstützt die Bereitstellung von Linux-Containern sowie von Windows-Containern unter Windows Server 2016. Sie können vorhandene Anwendungen, zustandslose Dienste oder zustandsbehaftete Dienste in Containern bereitstellen, und Sie können Dienste in Prozessen und Diensten in Containern in derselben Anwendung mischen.

## <a name="reliable-services"></a>Reliable Services
Reliable Services ist ein schlankes Framework zum Schreiben von Diensten, die sich in die Service Fabric-Plattform integrieren lassen und von allen Features der Plattform profitieren. Reliable Services stellen einen minimalen Satz von APIs bereit, die der Service Fabric-Runtime ermöglichen, den Lebenszyklus Ihrer Dienste zu verwalten, und Ihren Diensten ermöglichen, mit der Runtime zu interagieren. Das Anwendungsframework ist sehr schlank, bietet vollständige Kontrolle über Entwurfs- und Implementierungsoptionen und kann zum Hosten jedes anderen Anwendungsframeworks (beispielsweise ASP.NET Core) verwendet werden.

Reliable Services können ähnlich wie die meisten Dienstplattformen zustandslos sein. Ein Beispiel wären Webserver, bei denen jede Instanz des Diensts gleichartig erstellt und der Zustand in einer externen Lösung (beispielsweise Azure DB oder Azure Table Storage) gespeichert wird.

Reliable Services können auch zustandsbehaftet sein – ein exklusives Feature von Service Fabric, da der Status mithilfe von Reliable Collections direkt im Dienst gespeichert wird. Der Status wird durch Replikation als „hochverfügbar“ festgelegt und durch Partitionierung verteilt. Die Verwaltung erfolgt automatisch durch Service Fabric.

[Erfahren Sie mehr über Reliable Services](service-fabric-reliable-services-introduction.md) oder legen Sie los, indem Sie [Ihren ersten eigenen Reliable Service schreiben](service-fabric-reliable-services-quick-start.md).

## <a name="reliable-actors"></a>Reliable Actors
Das Reliable Actor-Anwendungsframework setzt auf Reliable Services auf und implementiert das „Virtual Actor“-Muster, basierend auf dem Entwurfsmuster für Akteure. Das Reliable Actor-Framework verwendet unabhängige Compute- und Statuseinheiten mit Singlethreadausführung, die als Akteure bezeichnet werden. Das Reliable Actor-Framework bietet integrierte Kommunikation für Akteure sowie voreingestellte Statuspersistenz und horizontal hochskalierbare Konfigurationen.

Da es sich bei Reliable Actors selbst um ein Anwendungsframework handelt, das auf Reliable Services aufsetzt, ist es vollständig in die Service Fabric-Plattform integriert und profitiert von allen Features, die die Plattform bietet.

### <a name="aspnet-core"></a>ASP.NET Core
Service Fabric arbeitet zur Erstellung von Web- und API-Anwendungen mit [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) zusammen. 

## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr über Reliable Actors](service-fabric-reliable-actors-introduction.md), oder [schreiben Sie Ihren ersten Reliable Actor-Dienst](service-fabric-reliable-actors-get-started.md).
[Erfahren Sie mehr über das Ausführen Ihrer Dienste unter Windows oder Linux in Containern.](service-fabric-deploy-container.md)
[Erstellen Sie einen Front-End-Dienst mithilfe von ASP.NET Core.](service-fabric-add-a-web-frontend.md)



