---
title: Containergruppen von Azure Container Instances
description: Funktionsweise von Containergruppen in Azure Container Instances
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: f9003c65d1818952c6a019f81080d595791f63bf
ms.openlocfilehash: 5b2abb76fc2bfcf719292db1fdafc8b353d13a3c
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---

# <a name="container-groups-in-azure-container-instances"></a>Containergruppen in Azure Container Instances

Die oberste Ressource in Azure Container Instances ist eine Containergruppe. In diesem Artikel wird beschrieben, was Containergruppen sind und welche Arten von Szenarien sie aktivieren.

## <a name="how-a-container-group-works"></a>Funktionsweise einer Containergruppe

Eine Containergruppe ist eine Sammlung von Containern, die auf demselben Hostcomputer geplant werden und für die Lebenszyklus, lokales Netzwerk und Speichervolumen gemeinsam genutzt werden. Dies ähnelt dem *Pod*-Konzept in [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) und [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).

Das folgende Diagramm zeigt ein Beispiel für eine Containergruppe, die mehrere Container enthält.

![Beispiel zu Containergruppen][container-groups-example]

Beachten Sie Folgendes:

- Die Gruppe wird auf einem einzelnen Hostcomputer geplant.
- Die Gruppe macht eine einzelne öffentliche IP-Adresse mit einem Port verfügbar.
- Die Gruppe besteht aus zwei Containern. Ein Container lauscht über Port 80 und der andere über Port 5000.
- Die Gruppe enthält zwei Azure-Dateifreigaben als Volumebereitstellungen, und jeder Container stellt eine der Freigaben lokal bereit.

### <a name="networking"></a>Netzwerk

Containergruppen nutzen eine IP-Adresse und einen Portnamespace dieser IP-Adresse gemeinsam. Sie müssen den Port unter der IP-Adresse und für den Container verfügbar machen, um es externen Clients zu ermöglichen, einen Container in der Gruppe zu erreichen. Da Container in der Gruppe einen Portnamespace gemeinsam nutzen, wird die Portzuordnung nicht unterstützt. Container in einer Gruppe können einander per localhost auf den Ports erreichen, die sie verfügbar gemacht haben. Dies gilt auch, wenn diese Port für die IP-Adresse der Gruppe nicht extern verfügbar gemacht werden.

### <a name="storage"></a>Speicher

Sie können externe Volumes für die Bereitstellung in einer Containergruppe angeben. Sie können diese Volumes bestimmten Pfaden in den einzelnen Containern einer Gruppe zuordnen.

## <a name="common-scenarios"></a>Gängige Szenarien

Gruppen mit mehreren Containern sind nützlich, wenn Sie eine einzelne funktionale Aufgabe in eine geringe Anzahl von Containerimages unterteilen möchten, die von unterschiedlichen Teams bereitgestellt werden können und über separate Ressourcenanforderungen verfügen. Beispiel für Verwendung:

- Ein Anwendungscontainer und ein Protokollierungscontainer. Mit dem Protokollierungscontainer werden die Protokolle und Metriken gesammelt, die von der Hauptanwendung ausgegeben werden, und in den Langzeitspeicher geschrieben.
- Eine Anwendung und ein Überwachungscontainer. Der Überwachungscontainer sendet regelmäßig eine Anforderung an die Anwendung, um sicherzustellen, dass sie ausgeführt wird und richtig reagiert. Andernfalls wird eine Warnung ausgelöst.
- Ein Container, über den eine Webanwendung bereitgestellt wird, und ein Container, der den aktuellen Inhalt aus der Quellcodeverwaltung abruft.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das [Bereitstellen einer Gruppe mit mehreren Containern](container-instances-multi-container-group.md) per Azure Resource Manager-Vorlage.

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png
