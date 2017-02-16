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
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: 1db7b4bcfa4b7c474a4b0eb4ef469a6cb1fe54a0


---
# <a name="availability-of-service-fabric-services"></a>Verfügbarkeit der Service Fabric-Dienste
Dieser Artikel bietet eine Übersicht darüber, wie Service Fabric die Verfügbarkeit eines Diensts aufrechterhält.

## <a name="availability-of-service-fabric-stateless-services"></a>Verfügbarkeit zustandsloser Service Fabric-Dienste
Azure Service Fabric-Dienste können zustandsbehaftet oder zustandslos sein. Eine zustandsloser Dienst ist ein Anwendungsdienst, der keinen [lokale persistenten Zustand](service-fabric-concepts-state.md)aufweist.

Für das Erstellen eines zustandslosen Diensts muss eine Anzahl von Instanzen definiert werden. Dieser Wert legt die Anzahl von Instanzen für die Anwendungslogik des statusfreien Diensts fest, die im Cluster ausgeführt werden sollen. Das Erhöhen der Anzahl der Instanzen ist die empfohlene Vorgehensweise für das Skalieren zustandsloser Dienste.

Wird in einer Instanz eines zustandslosen Diensts ein Fehler erkannt, wird auf einem anderen geeigneten Knoten im Cluster eine neue Instanz erstellt.

## <a name="availability-of-service-fabric-stateful-services"></a>Verfügbarkeit zustandsbehafteter Service Fabric-Dienste
Einem zustandsbehafteten Dienst ist ein Zustand zugeordnet. In Service Fabric ist ein zustandsbehafteter Dienst als eine Replikatgruppe modelliert. Jedes Replikat ist eine Instanz des Dienstcodes, die über eine Kopie des Status verfügt. Lese- und Schreibvorgänge erfolgen im selben Replikat (primäres Replikat). Änderungen des Zustands aufgrund von Schreibvorgängen werden in mehreren anderen Replikaten (aktive sekundäre Replikate) *repliziert*. Die Kombination aus primärem Replikat und aktiven sekundären Replikaten ist die Replikatgruppe des Diensts.

Nur ein primäres Replikat kann Lese- und Schreibanforderungen verarbeiten, aber es können mehrere aktive sekundäre Replikate vorhanden sein. Die Anzahl der aktiven sekundären Replikate ist konfigurierbar. Eine höhere Anzahl von Replikaten kann eine höhere Anzahl von gleichzeitigen Software- und Hardwarefehlern tolerieren.

Wenn das primäre Replikat ausfällt, macht Service Fabric eines der aktiven sekundären Replikate zum neuen primären Replikat. Dieses aktive sekundäre Replikat verfügt bereits über die aktualisierte Version des Zustands (durch *Replikation*) und kann die Verarbeitung von Lese- und Schreibvorgängen fortsetzen.

Dieses Konzept, bei dem ein Replikat ein primäres oder ein aktives sekundäres Replikat ist, wird Replikatrolle genannt.

### <a name="replica-roles"></a>Replikatrollen
Die Rolle eines Replikats wird zum Verwalten des Lebenszyklus des Zustands verwendet, der von diesem Replikat verwaltet wird. Ein Replikat mit der Rolle eines primären Replikats verarbeitet Leseanforderungen. Das primäre Replikat verarbeitet auch alle Schreibanforderungen, indem es seinen Status aktualisiert und die Änderungen repliziert. Diese Änderungen werden auf den aktiven sekundären Replikaten in der Replikatgruppe übernommen. Die Aufgabe eines aktiven sekundären Replikats ist es, die vom primären Replikat replizierten Zustandsänderungen zu empfangen und den eigenen Zustand entsprechend zu aktualisieren.

> [!NOTE]
> In komplexen Programmiermodellen, wie z.B. im [Reliable Actors-Framework](service-fabric-reliable-actors-introduction.md) und bei [Reliable Services](service-fabric-reliable-services-introduction.md), ist das Konzept der Replikatrolle von den Entwicklern weg abstrahiert. Bei Akteuren sind Rollen unnötig, während sie in Diensten bei Bedarf sichtbar sind, aber stark vereinfacht.
>
>

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Service Fabric-Konzepten finden Sie in den folgenden Artikeln:

* [Scaling Service Fabric Applications (in englischer Sprache)](service-fabric-concepts-scalability.md)
* [Partitionieren von Service Fabric-Diensten](service-fabric-concepts-partitioning.md)
* [Definieren und Verwalten von Zuständen](service-fabric-concepts-state.md)
* [Reliable Services](service-fabric-reliable-services-introduction.md)



<!--HONumber=Jan17_HO1-->


