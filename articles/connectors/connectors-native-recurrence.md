<properties
	pageTitle="Hinzufügen des Wiederholungstriggers in Logik-Apps | Microsoft Azure"
	description="Übersicht über den Wiederholungstrigger und dessen Verwendung in einer Azure-Logik-App."
	services=""
	documentationCenter=""
	authors="jeffhollan"
	manager="erikre"
	editor=""
	tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# Erste Schritte mit dem Wiederholungstrigger

Mit dem Wiederholungstrigger können Sie leistungsstarke Workflows in der Cloud erstellen.

Dazu zählen z. B.:

- Planen eines Workflows zur täglichen Ausführung einer gespeicherten SQL-Prozedur.
- Senden einer E-Mail mit einer Zusammenfassung aller Tweets der letzten Woche zu einem bestimmten Hashtag.

Wenn Sie den Wiederholungstrigger in einer Logik-App verwenden möchten, müssen Sie zunächst eine [Logik-App erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Verwenden eines Wiederholungstriggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md).

Hier sehen Sie eine Beispielsequenz für die Einrichtung eines Wiederholungstriggers in einer Logik-App:

1. Fügen Sie den Trigger **Serie** als ersten Schritt in einer Logik-App hinzu.
2. Füllen Sie die Parameter für das Wiederholungsintervall aus.

Die Logik-App startet nun nach jedem Zeitintervall eine Ausführung.

![HTTP-Trigger](./media/connectors-native-recurrence/using-trigger.png)

## Triggerdetails

Der Wiederholungstrigger verfügt über die folgenden Eigenschaften, die Sie konfigurieren können.

Er startet nach Ablauf einer festgelegten Zeitspanne eine Logik-App. Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Häufigkeit*|frequency|Die Zeiteinheit: `Second`, `Minute`, `Hour`, `Day` oder `Year`.|
|Intervall*|interval|Das Intervall mit der angegebenen Häufigkeit für die Wiederholung.|
|Zeitzone|timeZone|Bei Angabe einer Startzeit ohne UTC-Abweichung wird die angegebene Zeitzone verwendet.|
|Startzeit|startTime|Die Startzeit im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).|
<br>


## Nächste Schritte

Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

<!---HONumber=AcomDC_0810_2016-->