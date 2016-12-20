---
title: "Übersicht über die Mobile Engagement-Export-API"
description: "Erfahren Sie die Grundlagen zum Exportieren der Rohdaten, die von den Geräten Ihrer Benutzer generiert werden. So können Sie diese Daten in Ihren eigenen Tools nutzen."
services: mobile-engagement
documentationcenter: mobile
author: kpiteira
manager: erikre
editor: 
ms.assetid: 9380d47b-d7fa-4d4c-888f-97e6482196bb
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 04/26/2016
ms.author: kapiteir
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9c17a1cb33616ad297d0fad9b8ea674de75bd8fb


---
# <a name="mobile-engagement-export-api-overview"></a>Übersicht über die Mobile Engagement-Export-API
## <a name="introduction"></a>Einführung
In diesem Dokument erfahren Sie die Grundlagen zum Exportieren der Rohdaten, die von den Geräten Ihrer Benutzer generiert werden. So können Sie diese Daten in Ihren eigenen Tools nutzen.

## <a name="pre-requisites"></a>Voraussetzungen
Für den Export der Rohdaten aus Mobile Engagement ist Folgendes erforderlich:

* Eingerichtete API-Authentifizierung, um die APIs verwenden zu können (Informationen hierzu finden Sie unter [Authentifizierung – manuelles Setup](mobile-engagement-api-authentication-manual.md))
* Verwendung der REST-APIs oder des [.NET-SDK](mobile-engagement-dotnet-sdk-service-api.md)
* Azure Storage-Konto

> [!NOTE]
> Zumindest während der Entwicklungsphase empfehlen wir auch den hervorragenden [Microsoft Azure Storage Explorer](http://storageexplorer.com/), da er eine benutzerfreundliche Oberfläche für die Interaktion mit Azure Storage bietet.
> 
> 

## <a name="what-can-be-exported"></a>Was kann exportiert werden?
Mit Mobile Engagement können Benutzer viele Arten von Daten sammeln. Daher gibt es mehrere Exporttypen, die für diese verschiedenen Datentypen geeignet sind.
Die beiden wichtigsten Exporttypen sind:

* Momentaufnahme: Wird in der Regel zum Exportieren von Daten verwendet, die einen Zustand darstellen und für die Mobile Engagement keinen Verlauf besitzt. Dazu gehören z.B. Tags (App-Info), Token oder Feedbacks zu Pushkampagnen. Daher beziehen sich diese Arten von Export nicht auf ein Datum.
* Verlaufsdaten: Dieser Exporttyp wird für Daten verwendet, die sich über einen Zeitraum ansammeln, z.B. Ereignisse oder Aktivitäten.

In der folgenden Tabelle werden alle möglichen Exporte ausführlich beschrieben:

| Exporttyp | Datentyp | Beschreibung |
| --- | --- | --- |
| Momentaufnahme |Push |Generiert einen Export von Feedbacks zu Pushkampagnen pro Geräte-ID/Benutzer-ID |
| Momentaufnahme |Tag |Generiert einen Export der den einzelnen Geräten zugeordneten Tags (App-Info) |
| Momentaufnahme |Gerät |Generiert einen Export eines Großteils der Daten über Geräte, z.B. der technischen Informationen (Modell, Gebietsschema, Zeitzone usw.), der Tags, des Zeitpunkts der ersten Anzeige usw. |
| Momentaufnahme |Tokenverschlüsselung |Generiert einen Export aller gültigen Token |
| Verlaufsdaten |Aktivität |Generiert einen Export aller Aktivitäten für die einzelnen Geräte während eines bestimmten Zeitraums |
| Verlaufsdaten |Ereignis |Generiert einen Export aller Aktivitäten für die einzelnen Geräte während eines bestimmten Zeitraums |
| Verlaufsdaten |Job |Generiert einen Export aller Aufträge für die einzelnen Geräte während eines bestimmten Zeitraums |
| Verlaufsdaten |Fehler |Generiert einen Export aller Fehler für die einzelnen Geräte während eines bestimmten Zeitraums |

## <a name="how-does-it-work"></a>Wie funktioniert Application Insights?
Exporte sind Tasks mit langer Ausführungsdauer, die möglicherweise große Datenmengen erzeugen. Aus diesem Grund können sie nicht aufgerufen werden, um sofort eine Datei zum Download zurückzugeben.
Zum Exportieren von Daten aus Mobile Engagement müssen Sie per API einen **Exportauftrag** erstellen, in dem Sie in der Regel Folgendes angeben:

* Exporttyp (Momentaufnahme oder Verlauf)
* Datentyp
* **Azure Storage-Container** (einschließlich einer gültigen SAS mit Schreibzugriff), in den das Ergebnis des Exports geschrieben wird

Beachten Sie, dass es möglicherweise einige Minuten dauert, bis der Auftrag gestartet wird, und dass er von ein paar Sekunden bei sehr kleinen Apps bis hin zu mehreren Stunden bei Apps mit vielen Benutzern oder umfangreicher Aktivität ausgeführt werden kann.

Sobald der Auftrag erstellt wurde, kann sein Status überprüft werden, um festzustellen, ob er noch ausgeführt wird oder ob er abgeschlossen wurde.

Sobald der Auftrag ERFOLGREICH abgeschlossen ist, steht die resultierende Datendatei im bereitgestellten Speichercontainer zur Verfügung.




<!--HONumber=Nov16_HO3-->


