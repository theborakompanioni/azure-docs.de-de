---
title: "Verfügbarkeit von Service Fabric-Diensten | Microsoft Docs"
description: "Beschreibt Fehlererkennung, Failover und Wiederherstellung für Dienste"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: b0d4615a9b8ab566f69b27e4879b6e2d597b4990
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---

# <a name="availability-of-service-fabric-services"></a>Verfügbarkeit der Service Fabric-Dienste
Dieser Artikel bietet eine Übersicht darüber, wie Service Fabric die Verfügbarkeit eines Diensts aufrechterhält.

## <a name="availability-of-service-fabric-stateless-services"></a>Verfügbarkeit zustandsloser Service Fabric-Dienste
Azure Service Fabric-Dienste können zustandsbehaftet oder zustandslos sein. Ein zustandsloser Dienst ist ein Anwendungsdienst, der keinen [lokalen Zustand](service-fabric-concepts-state.md) aufweist, welcher hoch verfügbar oder zuverlässig sein muss.

Für das Erstellen eines zustandslosen Diensts muss eine `InstanceCount` definiert werden. Dieser Wert legt die Anzahl von Instanzen für die Anwendungslogik des statusfreien Diensts fest, die im Cluster ausgeführt werden sollen. Das Erhöhen der Anzahl der Instanzen ist die empfohlene Vorgehensweise für das Skalieren zustandsloser Dienste.

Wird in einer Instanz eines zustandslosen benannten Diensts ein Fehler erkannt, wird auf einem anderen geeigneten Knoten im Cluster eine neue Instanz erstellt. Eine zustandslosen Dienstinstanz kann z.B. auf Knoten1 ausfallen und auf Knoten5 neu erstellt werden.

## <a name="availability-of-service-fabric-stateful-services"></a>Verfügbarkeit zustandsbehafteter Service Fabric-Dienste
Einem zustandsbehafteten Dienst ist ein Zustand zugeordnet. In Service Fabric ist ein zustandsbehafteter Dienst als eine Replikatgruppe modelliert. Jedes Replikat ist eine ausgeführte Instanz des Dienstcodes, die über eine Kopie des Zustands des jeweiligen Diensts verfügt. Lese- und Schreibvorgänge erfolgen im selben Replikat (primäres Replikat). Änderungen des Zustands aufgrund von Schreibvorgängen werden in mehrere andere Replikate (sog. aktive sekundäre Replikate) *repliziert*. 

Es kann nur ein primäres Replikat geben, aber es können mehrere aktive sekundäre Replikate vorhanden sein. Die Anzahl der aktiven sekundären Replikate ist konfigurierbar. Eine höhere Anzahl von Replikaten kann eine höhere Anzahl von gleichzeitigen Software- und Hardwarefehlern tolerieren.

Wenn das primäre Replikat ausfällt, macht Service Fabric eines der aktiven sekundären Replikate zum neuen primären Replikat. Dieses aktive sekundäre Replikat verfügt bereits über die aktualisierte Version des Zustands (durch *Replikation*) und kann die Verarbeitung von Lese- und Schreibvorgängen fortsetzen.

Dieses Konzept, bei dem ein Replikat ein primäres oder ein aktives sekundäres Replikat ist, wird Replikatrolle genannt.

### <a name="replica-roles"></a>Replikatrollen
Die Rolle eines Replikats wird zum Verwalten des Lebenszyklus des Zustands verwendet, der von diesem Replikat verwaltet wird. Ein Replikat mit der Rolle eines primären Replikats verarbeitet Leseanforderungen. Das primäre Replikat verarbeitet auch alle Schreibanforderungen, indem es seinen Status aktualisiert und die Änderungen repliziert. Diese Änderungen werden auf den aktiven sekundären Replikaten in der Replikatgruppe übernommen. Die Aufgabe eines aktiven sekundären Replikats ist es, die vom primären Replikat replizierten Zustandsänderungen zu empfangen und den eigenen Zustand entsprechend zu aktualisieren.

> [!NOTE]
> In komplexen Programmiermodellen, wie z.B. [Reliable Actors](service-fabric-reliable-actors-introduction.md) und [Reliable Services](service-fabric-reliable-services-introduction.md), ist das Konzept der Replikatrolle vor den Entwicklern verborgen. Bei „Reliable Actors“ sind Rollen unnötig, während sie in „Reliable Services“ in den meisten Szenarien stark vereinfacht sind.
>

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Service Fabric-Konzepten finden Sie in den folgenden Artikeln:

- [Skalieren von Service Fabric-Diensten](service-fabric-concepts-scalability.md)
- [Partitionieren von Service Fabric-Diensten](service-fabric-concepts-partitioning.md)
- [Definieren und Verwalten von Zuständen](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)

