<properties
	pageTitle="Hinzufügen einer Verzögerung in Logic Apps | Microsoft Azure"
	description="Übersicht über die Aktionen „Verzögern“ und „Verzögern bis“ sowie über deren Verwendung in einer Azure-Logik-App."
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

# Erste Schritte mit den Aktionen „Verzögern“ und „Verzögern bis“

Die Aktionen „Verzögern“ und „Verzögern bis“ eignen sich beispielsweise für folgende Workflowszenarien:

- Versand einer E-Mail mit einem Statusupdate bis zum Erreichen eines Werktags verzögen
- Workflow verzögern, bis ein HTTP-Aufruf abgeschlossen werden konnte, und erst dann den Vorgang fortsetzen und das Ergebnis abrufen

Wenn Sie die Verzögerungsaktion in einer Logik-App verwenden möchten, müssen Sie zunächst eine [Logik-App erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## Verwenden der Verzögerungsaktionen

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](connectors-overview.md).

Im Anschluss sehen Sie eine Beispielsequenz für die Verwendung eines Verzögerungsschritts in einer Logik-App:

1. Klicken Sie nach dem Hinzufügen eines Triggers auf **Neuer Schritt**, um eine Aktion hinzuzufügen.
1. Suchen Sie nach „Verzögern“, um die Verzögerungsaktionen anzuzeigen. In diesem Beispiel wählen wir **Verzögern** aus.

	![Verzögern von Aktionen](./media/connectors-native-delay/using-action-1.png)

1. Geben Sie die gewünschten Aktionseigenschaften an, um die Verzögerung zu konfigurieren.

	![Verzögerungskonfiguration](./media/connectors-native-delay/using-action-2.png)

1. Klicken Sie auf „Speichern“, um die Logik-App zu veröffentlichen und zu aktivieren.

---

## Technische Details

Im Anschluss finden Sie Details zu den Aktionen „Verzögern“ und „Verzögern bis“.

### Aktionsdetails

Der Wiederholungstrigger verfügt über folgende konfigurierbare Eigenschaften:

#### Verzögern

Verzögert die Ausführung für ein bestimmtes Zeitintervall. Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Anzahl*|count|Die Anzahl von Zeiteinheiten für die Verzögerung.|
|Einheit*|unit|Die Zeiteinheit: `Second`, `Minute`, `Hour` oder `Day`.|
<br>

#### Verzögern bis

Verzögert die Ausführung bis zum angegebenen Zeitpunkt (Datum/Uhrzeit). Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Jahr*|timestamp|Das Jahr, bis zu dem verzögert werden soll. (GMT)|
|Monat*|timestamp|Der Monat, bis zu dem verzögert werden soll. (GMT)|
|Tag*|timestamp|Der Tag, bis zu dem verzögert werden soll. (GMT)|
<br>


## Nächste Schritte

Im Anschluss finden Sie Details zur den weiteren Schritten mit Logik-Apps und unserer Community.

## Erstellen einer Logik-App

Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

<!---HONumber=AcomDC_0727_2016-->