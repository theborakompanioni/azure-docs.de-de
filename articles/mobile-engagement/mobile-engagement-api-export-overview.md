---
title: Übersicht über die Mobile Engagement-Export-API
description: Erfahren Sie die Grundlagen zum Exportieren der Rohdaten, die von den Geräten Ihrer Benutzer generiert werden. So können Sie diese Daten in Ihren eigenen Tools nutzen.
services: mobile-engagement
documentationcenter: mobile
author: kpiteira
manager: erikre
editor: ''

ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 04/26/2016
ms.author: kpiteira

---
# Übersicht über die Mobile Engagement-Export-API
## Einführung
In diesem Dokument erfahren Sie die Grundlagen zum Exportieren der Rohdaten, die von den Geräten Ihrer Benutzer generiert werden. So können Sie diese Daten in Ihren eigenen Tools nutzen.

## Voraussetzungen
Für den Export der Rohdaten aus Mobile Engagement ist Folgendes erforderlich:

* Eingerichtete API-Authentifizierung, um die APIs verwenden zu können (Informationen hierzu finden Sie unter [Authentifizierung – manuelles Setup](mobile-engagement-api-authentication-manual.md))
* Verwendung der REST-APIs oder des [.NET-SDK](mobile-engagement-dotnet-sdk-service-api.md)
* Azure Storage-Konto

> [!NOTE]
> Zumindest während der Entwicklungsphase empfehlen wir auch den hervorragenden [Microsoft Azure Storage Explorer](http://storageexplorer.com/), da er eine benutzerfreundliche Oberfläche für die Interaktion mit Azure Storage bietet.
> 
> 

## Was kann exportiert werden?
Mit Mobile Engagement können Benutzer viele Arten von Daten sammeln. Daher gibt es mehrere Exporttypen, die für diese verschiedenen Datentypen geeignet sind. Die beiden wichtigsten Exporttypen sind:

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

## Wie funktioniert Application Insights?
Exporte sind Tasks mit langer Ausführungsdauer, die möglicherweise große Datenmengen erzeugen. Aus diesem Grund können sie nicht aufgerufen werden, um sofort eine Datei zum Download zurückzugeben. Zum Exportieren von Daten aus Mobile Engagement müssen Sie per API einen **Exportauftrag** erstellen, in dem Sie in der Regel Folgendes angeben:

* Exporttyp (Momentaufnahme oder Verlauf)
* Datentyp
* **Azure Storage-Container** (einschließlich einer gültigen SAS mit Schreibzugriff), in den das Ergebnis des Exports geschrieben wird

Beachten Sie, dass es möglicherweise einige Minuten dauert, bis der Auftrag gestartet wird, und dass er von ein paar Sekunden bei sehr kleinen Apps bis hin zu mehreren Stunden bei Apps mit vielen Benutzern oder umfangreicher Aktivität ausgeführt werden kann.

Sobald der Auftrag erstellt wurde, kann sein Status überprüft werden, um festzustellen, ob er noch ausgeführt wird oder ob er abgeschlossen wurde.

Sobald der Auftrag ERFOLGREICH abgeschlossen ist, steht die resultierende Datendatei im bereitgestellten Speichercontainer zur Verfügung.

<!---HONumber=AcomDC_0504_2016-->