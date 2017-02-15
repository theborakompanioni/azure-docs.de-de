---
title: "Verfügbarkeit von Service Fabric-Diensten | Microsoft Docs"
description: "Beschreibt Fehlererkennung, Failover und Wiederherstellung für Dienste"
services: service-fabric
documentationcenter: .net
author: appi101
manager: timlt
editor: 
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2016
ms.author: aprameyr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: af0880503cc720d86b81a6ec3d74197afe8de08d


---
# <a name="availability-of-service-fabric-services"></a>Verfügbarkeit der Service Fabric-Dienste
Azure Service Fabric-Dienste können zustandsbehaftet oder zustandslos sein. Dieser Artikel bietet eine Übersicht darüber, wie Service Fabric die Verfügbarkeit eines Diensts bei einem Ausfall aufrechterhält.

## <a name="availability-of-service-fabric-stateless-services"></a>Verfügbarkeit zustandsloser Service Fabric-Dienste
Eine zustandsloser Dienst ist ein Anwendungsdienst, der keinen [lokale persistenten Zustand](service-fabric-concepts-state.md)aufweist.

Um einen zustandslosen Dienst zu erstellen, muss die Anzahl der Instanzen des zustandslosen Diensts definiert werden, die im Cluster ausgeführt werden sollen. Hierbei handelt es sich um die Anzahl der Kopien der Anwendungslogik, die im Cluster instanziiert werden. Das Erhöhen der Anzahl der Instanzen ist die empfohlene Vorgehensweise für das Skalieren eines zustandslosen Diensts.

Wird in einer Instanz eines zustandslosen Diensts ein Fehler erkannt, wird auf einem anderen geeigneten Knoten im Cluster eine neue Instanz erstellt.

## <a name="availability-of-service-fabric-stateful-services"></a>Verfügbarkeit zustandsbehafteter Service Fabric-Dienste
Einem zustandsbehafteten Dienst ist ein Zustand zugeordnet. In Service Fabric ist ein zustandsbehafteter Dienst als eine Replikatgruppe modelliert. Jedes Replikat ist eine Instanz des Dienstcodes, die über eine Kopie des Status verfügt. Lese- und Schreibvorgänge erfolgen im selben Replikat (primäres Replikat). Änderungen des Zustands aufgrund von Schreibvorgängen werden in mehreren anderen Replikaten (aktive sekundäre Replikate) *repliziert* . Die Kombination aus primären und aktiven sekundären Replikaten ist die Replikatgruppe des Diensts.

Nur ein primäres Replikat kann Lese- und Schreibanforderungen verarbeiten, aber es können mehrere aktive sekundäre Replikate vorhanden sein. Die Anzahl der aktiven sekundären Replikate ist konfigurierbar. Eine höhere Anzahl von Replikaten kann eine höhere Anzahl von gleichzeitigen Software- und Hardwarefehlern tolerieren.

Bei einem Fehler (wenn das primäre Replikat ausfällt) macht Service Fabric eines der aktiven sekundären Replikate zum neuen primären Replikat. Dieses aktive sekundäre Replikat verfügt bereits über die aktualisierte Version des Zustands (durch *Replikation*) und kann die Verarbeitung von Lese- und Schreibvorgängen fortsetzen.

Dieses Konzept, bei dem ein Replikat ein primäres oder ein aktives sekundäres Replikat ist, wird Replikatrolle genannt.

### <a name="replica-roles"></a>Replikatrollen
Die Rolle eines Replikats wird zum Verwalten des Lebenszyklus des Zustands verwendet, der von diesem Replikat verwaltet wird. Ein Replikat mit der Rolle eines primären Replikats verarbeitet Leseanforderungen. Es verarbeitet auch Schreibanforderungen, indem es seinen Zustand aktualisiert und die Änderungen in den aktiven sekundären Replikaten in der Replikatgruppe repliziert. Die Rolle eines aktiven sekundären Replikats ist es, die vom primären Replikat replizierten Zustandsänderungen zu empfangen und den eigenen Zustand entsprechend zu aktualisieren.

> [!NOTE]
> In komplexen Programmiermodellen, wie z.B. im [Reliable Actors-Framework](service-fabric-reliable-actors-introduction.md), ist das Konzept der Replikatrolle von den Entwicklern weg abstrahiert.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Service Fabric-Konzepten finden Sie hier:

* [Scaling Service Fabric Applications (in englischer Sprache)](service-fabric-concepts-scalability.md)
* [Partitionieren von Service Fabric-Diensten](service-fabric-concepts-partitioning.md)
* [Definieren und Verwalten von Zuständen](service-fabric-concepts-state.md)




<!--HONumber=Nov16_HO3-->


