---
title: "Azure Service Fabric Reliable State Manager und Reliable Collection – ausführliche Informationen | Microsoft-Dokumentation"
description: "Ausführliche Informationen über Konzepte für zuverlässige Sammlungen und über deren Design in Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.translationtype: Human Translation
ms.sourcegitcommit: de674af369080ad7eb608608685e293f2326c8e6
ms.openlocfilehash: d607449a16e886337ab1bd96213fbb4231124353
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017


---

# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a>Ausführliche Informationen über Service Fabric Reliable State Manager und Reliable Collection
In diesem Dokument werden ausführliche Informationen über den Reliable State Manager und über Reliable Collections gegeben. Es wird beschrieben, wie die wichtigsten Komponenten im Hintergrund funktionieren.

> [!NOTE]
> Dieses Dokument ist in Bearbeitung. Sie können uns in Kommentaren zu diesem Artikel gerne mitteilen, über welche Themen Sie noch mehr erfahren möchten.
>

##  <a name="local-persistence-model-log-and-checkpoint"></a>Lokales Persistenzmodell: Protokoll und Prüfpunkt
Reliable State Manager und Reliable Collections basieren auf einem Persistenzmodell, das als Protokoll und Prüfpunkt bezeichnet wird.
Bei diesem Modell wird jede Zustandsänderung zunächst auf dem Datenträger protokolliert und dann im Arbeitsspeicher angewendet.
Der vollständige Zustand selbst wird nur gelegentlich persistent gespeichert (auch als Prüfpunkt bezeichnet).
Der Vorteil ist: Deltas werden zur Leistungsverbesserung in sequenzielle Nur-Anfügen-Schreibvorgänge auf dem Datenträger umgewandelt.

Zum besseren Verständnis des Protokoll- und Prüfpunktmodells sehen wir uns zunächst das Szenario des Endlosdatenträgers an.
Reliable State Manager protokolliert jeden Vorgang, bevor dieser repliziert wird.
Die Protokollierung ermöglicht es Reliable Collections, den Vorgang nur im Arbeitsspeicher anzuwenden.
Da Protokolle persistent sind, verfügt der Reliable State Manager selbst dann, wenn das Replikat aufgrund eines Fehlers neu gestartet werden muss, über ausreichend Informationen in seinem Protokoll, um alle im Replikat verloren gegangenen Vorgänge zu wiederholen.
Da es sich um einen Endlosdatenträger handelt, müssen Protokolleinträge nie entfernt werden. Reliable Collections verwalten lediglich den Zustand im Speicher.

Sehen wir uns jetzt das Szenario mit begrenzten Datenträgern an.
Da sich Protokolldatensätze akkumulieren, ist der gesamte Speicherplatz auf dem Datenträger von Reliable State Manager irgendwann belegt.
Bevor dies geschieht, muss Reliable State Manager sein Protokoll kürzen, um Platz für neuere Datensätze zu schaffen.
Der Reliable State Manager fordert die Reliable Collections auf, ihren Zustand im Arbeitsspeicher auf dem Datenträger zu prüfen.
An diesem Punkt würde Reliable Collections seinen Zustand im Arbeitsspeicher beibehalten.
Sobald die zuverlässigen Auflistungen ihre Prüfpunkte abgeschlossen haben, kann der zuverlässige Zustands-Manager das Protokoll kürzen, um Speicherplatz freizugeben.
Wenn das Replikat neu gestartet werden muss, stellen zuverlässige Auflistungen den Zustand ihres Prüfpunkts wieder her. Zudem stellt der Reliable State Manager alle seit dem letzten Prüfpunkt vorgenommenen Zustandsänderungen wieder her.

Ein weiterer Vorteil der Verwendung von Prüfpunkten ist die Verbesserung der Wiederherstellungsleistung in den üblichen Szenarios. Das Protokoll enthält alle Vorgänge, die seit dem letzten Prüfpunkt aufgetreten sind.
Daher kann es vorkommen, dass mehrere Versionen eines Elements wie z.B. mehrere Werte für eine gegebene Reliable Dictionary-Zeile enthalten sind.
Reliable Collection dagegen prüft nur die aktuellste Version der einzelnen Schlüsselwerte.

## <a name="next-steps"></a>Nächste Schritte
* [Transaktionen und Sperren](service-fabric-reliable-services-reliable-collections-transactions-locks.md)


