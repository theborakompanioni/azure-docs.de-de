---
title: "Azure Event Grid – Begriffe"
description: "In diesem Artikel werden Azure Event Grid und die zugehörigen Begriffe beschrieben. Zudem werden verschiedene Schlüsselkomponenten von Event Grid definiert."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/10/2017
ms.author: babanisa
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: 83b9b2c7bb4134e1d9bdf857449bfb85884333d0
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---

# <a name="concepts-in-azure-event-grid"></a>Begriffe in Azure Event Grid

Zu den zentralen Begriffen in Azure Event Grid zählen Folgende:

## <a name="events"></a>Ereignisse

Ein Ereignis ist die kleinste Informationsmenge, die einen Vorgang im System umfassend beschreibt.  Jedes Ereignis enthält allgemeine Informationen wie Quelle des Ereignisses, Zeitpunkt, an dem das Ereignis aufgetreten ist, und den eindeutigen Bezeichner.  Jedes Ereignis enthält auch spezielle Informationen, die nur für das jeweilige Ereignis relevant sind. Beispielsweise enthält ein Ereignis zu einer neuen Datei, die in Azure Storage erstellt wird, Details über die Datei, z.B. den Wert von „lastTimeModified“. Alternativ enthält ein Ereignis zum Neustart eines virtuellen Computers den Namen des virtuellen Computers und den Grund für den Neustart.

## <a name="event-sourcespublishers"></a>Ereignisquellen/-herausgeber

Eine Ereignisquelle ist die Quelle, in der das Ereignis auftritt. Azure Storage ist z.B. die Ereignisquelle für durch Blobs erstellte Ereignisse. Das Azure VM-Fabric ist die Ereignisquelle für Ereignisse zu virtuellen Computern. Ereignisquellen sind für das Veröffentlichen von Ereignissen in Event Grid zuständig.

## <a name="topics"></a>Themen

Herausgeber kategorisieren Ereignisse in Themen. Dieses Thema enthält einen Endpunkt, an den der Herausgeber Ereignisse sendet. Um auf bestimmte Arten von Ereignissen zu reagieren, legen Abonnenten fest, welche Themen sie abonnieren. Die Themen enthalten auch ein Ereignisschema, mit der Abonnenten feststellen können, wie die Ereignisse entsprechend genutzt werden.

Systemthemen sind integrierte Themen, die von Azure-Diensten bereitgestellt werden. Benutzerdefinierte Themen sind Anwendungs- und Drittanbieterthemen.

## <a name="event-subscriptions"></a>Ereignisabonnements

Ein Abonnement gibt in Event Grid an, welche Ereignisse zu einem Thema ein Abonnent erhalten möchte.  Ein Abonnement enthält auch Informationen darüber, wie Ereignisse an den Abonnenten übermittelt werden sollen.

## <a name="event-handlers"></a>Ereignishandler

In Bezug auf Event Grid ist ein Ereignishandler das Ziel, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses weitere Maßnahmen.  Event Grid unterstützt mehrere Abonnententypen. Je nach Typ des Abonnenten führt Event Grid unterschiedliche Methoden durch, um die Übermittlung des Ereignisses zu gewährleisten.  Bei HTTP-Webhookereignishandlern wird das Ereignis solange wiederholt, bis der Handler einen Statuscode von `200 – OK` zurückgibt. Bei Azure Storage Queue werden die Ereignisse solange wiederholt, bis der Warteschlangendienst den Nachrichtenpush in der Warteschlange verarbeiten kann.

## <a name="filters"></a>Filter

Wenn Sie ein Thema abonnieren, können Sie die Ereignisse filtern, die an den Endpunkt gesendet werden. Sie können nach Ereignistyp oder Betreffmuster filtern. Weitere Informationen finden Sie unter [Event Grid – Abonnementschema](subscription-creation-schema.md).

## <a name="security"></a>Sicherheit

Event Grid ermöglicht ein sicheres Abonnieren und Veröffentlichen von Themen. Für das Abonnieren sind entsprechende Berechtigungen für die Ressource oder das Thema erforderlich. Für das Veröffentlichen ist ein SAS-Token oder eine Schlüsselauthentifizierung für das Thema erforderlich. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).

## <a name="failed-delivery"></a>Fehlerhafte Übermittlung

Wenn Event Grid nicht bestätigen kann, dass ein Ereignis vom Endpunkt des Abonnenten eingegangen ist, wird das Ereignis erneut übermittelt. Weitere Informationen finden Sie unter [Event Grid – Nachrichtenübermittlung und -wiederholung](delivery-and-retry.md).

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
