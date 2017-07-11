---
title: "Aktivieren von Event Hubs Capture über das Portal | Microsoft-Dokumentation"
description: "Aktivieren Sie die Event Hubs Capture-Feature über das Azure-Portal."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 1f3d373944b909db290f6cf2da7bf12a8a00e1c5
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---

<a id="enable-event-hubs-capture-using-the-azure-portal" class="xliff"></a>

# Aktivieren von Event Hubs Capture über das Azure-Portal

Sie können Capture über das [Azure-Portal](https://portal.azure.com) zum Zeitpunkt der Event Hub-Erstellung konfigurieren. Capture wird durch Klicken auf die Schaltfläche **Ein** auf dem Portalblatt **Event Hub erstellen** aktiviert. Konfigurieren Sie dann ein Speicherkonto und Container durch Klicken auf den Abschnitt **Container** des Blatts. Da Event Hubs Capture die Dienst-zu-Dienst-Authentifizierung mit Speicher verwendet, müssen Sie keine Speicherverbindungszeichenfolge angeben. Die Ressourcenauswahl wählt automatisch den Ressourcen-URI für Ihr Speicherkonto. Wenn Sie Azure Resource Manager verwenden, müssen Sie diesen URI explizit als Zeichenfolge angeben.

Das Standadzeitfenster beträgt fünf Minuten. Der Mindestwert ist 1, der Höchstwert 15. Für das **Größenfenster** gilt ein Bereich von 10 bis 500 MB.

![][1]

<a id="adding-capture-to-an-existing-event-hub" class="xliff"></a>

## Hinzufügen von Capture zu einem vorhandenen Event Hub

Capture kann auf vorhandenen Event Hubs-Instanzen konfiguriert werden, die sich in Event Hubs-Namespaces befinden. Das Feature ist für ältere **Messaging**-Namespaces oder Namespaces vom Typ **Gemischt** nicht verfügbar. Klicken Sie zum Aktivieren von Capture auf einem vorhandenen Event Hub oder zum Ändern der Capture-Einstellungen auf den Namespace, um das Blatt **Zusammenfassung** zu laden. Anschließend klicken Sie auf den Event Hub, den Sie aktivieren bzw. dessen Capture-Einstellung Sie ändern möchten. Klicken Sie abschließend auf den Abschnitt **Eigenschaften** des geöffneten Blatts, wie in der folgenden Abbildung dargestellt:

![][2]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

Sie können Event Hubs Capture auch über Azure Resource Manager-Vorlagen konfigurieren. Weitere Informationen finden Sie unter [Aktivieren von Capture über eine Azure Resource Manager-Vorlage](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).

