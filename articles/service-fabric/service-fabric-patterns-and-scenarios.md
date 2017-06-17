---
title: "Muster und Szenarien für Azure Service Fabric | Microsoft-Dokumentation"
description: "Informieren Sie sich über empfohlene Vorgehensweisen und bewährte, wiederverwendbare Muster zum Entwerfen, Entwickeln und Betreiben Ihrer Microservices unter Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: d5aa75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: df7c127f4ab140cb26c82e723964f1d99f03131a
ms.contentlocale: de-de
ms.lasthandoff: 03/09/2017


---
# <a name="service-fabric-patterns-and-scenarios"></a>Muster und Szenarien für Service Fabric
Benutzer, die umfangreiche Microservices mit Azure Service Fabric erstellen möchten, erhalten hier Informationen von den Experten, die diese PaaS (Platform-as-a-Service) konzipiert und erstellt haben. Beginnen Sie mit der richtigen Architektur, und erfahren Sie, wie Sie Ressourcen für Ihre Anwendung optimieren. Der Kurs [Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) (Service Fabric-Muster und -Methoden) gibt Antworten auf häufig gestellte Fragen von echten Kunden zu Service Fabric-Szenarien und Anwendungsbereichen.
 
Hier erfahren Sie, wie Sie Ihre Microservices unter Service Fabric mithilfe empfohlener Vorgehensweisen und bewährter, wiederverwendbarer Muster entwerfen, entwickeln und betreiben. Verschaffen Sie sich einen Überblick über Service Fabric, und beschäftigen Sie sich ausführlicher mit Themen wie Clusteroptimierung und -sicherheit, Migrieren von Legacy-Apps, IoT mit Skalierung, Hosten von Spiel-Engines und vielem mehr. Sehen Sie sich Continuous Delivery für verschiedene Workloads an, und informieren Sie sich ausführlich über Linux-Unterstützung und Container. 

## <a name="introduction"></a>Einführung
Beschäftigen Sie sich mit bewährten Methoden sowie mit den Vorteilen von PaaS (Platform-as-a-Service) gegenüber IaaS (Infrastructure-as-a-Service). Informieren Sie sich im Detail über die Einhaltung bewährter Anwendungsdesignprinzipien.

<table><tr><th>Video</th><th>PowerPoint-Folienstapel</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Einführung in Service Fabric</a></td></tr>
</table>

## <a name="cluster-planning-and-management"></a>Clusterplanung und -verwaltung
Dieser Beitrag zu Azure Service Fabric enthält Informationen zu Kapazitätsplanung, Clusteroptimierung und Clustersicherheit.

<table><tr><th>Video</th><th>PowerPoint-Folienstapel</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Clusterplanung und -verwaltung</a></td></tr>
</table>

## <a name="hyper-scale-web"></a>Web mit Hyperskalierung
Informieren Sie sich über Konzepte rund um das Web mit Hyperskalierung wie Verfügbarkeit und Zuverlässigkeit, Hyperskalierung und Zustandsverwaltung.

<table><tr><th>Video</th><th>PowerPoint-Folienstapel</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Web mit Hyperskalierung</a></td></tr>
</table>

## <a name="iot"></a>IoT
Machen Sie sich mit dem Internet der Dinge (Internet of Things, IoT) im Kontext von Azure Service Fabric vertraut – einschließlich Azure IoT-Pipeline, Mehrinstanzenfähigkeit und IoT mit Skalierung.

<table><tr><th>Video</th><th>PowerPoint-Folienstapel</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></td></tr>
</table>

## <a name="gaming"></a>Spiele
Beschäftigen Sie sich mit rundenbasierten und interaktiven Spielen sowie mit dem Hosten vorhandener Spiel-Engines.

<table><tr><th>Video</th><th>PowerPoint-Folienstapel</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Spiele</a></td></tr>
</table>

## <a name="continuous-delivery"></a>Continuous Delivery
Machen Sie sich mit Konzepten wie Continuous Integration/Continuous Delivery mit Visual Studio Team Services, dem Workflow zum Erstellen/Packen/Veröffentlichen, der Einrichtung mehrerer Umgebungen und dem Packen/Freigeben von Diensten vertraut.

<table><tr><th>Video</th><th>PowerPoint-Folienstapel</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Continuous delivery</a></td></tr>
</table>

## <a name="migration"></a>Migration
Informieren Sie sich über das Migrieren aus einem Clouddienst sowie über das Migrieren von Legacy-Apps.

<table><tr><th>Video</th><th>PowerPoint-Folienstapel</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Migration</a></td></tr>
</table>

## <a name="containers-and-linux-support"></a>Container und Linux-Unterstützung
Beschäftigen Sie sich mit Gründen für die Verwendung von Containern. Informieren Sie sich über die Vorschau für Windows-Container, Linux-Unterstützung und Linux-Containerorchestrierung. Außerdem erfahren Sie hier, wie Sie .NET Core-Apps zu Linux migrieren.

<table><tr><th>Video</th><th>PowerPoint-Folienstapel</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Container- und Linux-Unterstützung</a></td></tr>
</table>

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun mit Mustern und Szenarien für Service Fabric vertraut sind, können Sie sich mit folgenden Themen beschäftigen: [Erstellen und Verwalten von Clustern](service-fabric-deploy-anywhere.md), [Migrieren von Cloud Services-Apps zu Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [Einrichten von Continuous Delivery](service-fabric-set-up-continuous-integration.md) und [Bereitstellen von Containern](service-fabric-containers-overview.md).

