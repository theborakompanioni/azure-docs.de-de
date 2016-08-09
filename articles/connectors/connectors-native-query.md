<properties
	pageTitle="Hinzufügen der Abfrageaktion in Logik-Apps | Microsoft Azure"
	description="Übersicht über die Abfrageaktion zum Ausführen von Aktionen wie „Array filtern“."
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
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# Erste Schritte mit der Abfrageaktion

Mit der Abfrageaktion können Sie Batches und Arrays zum Ausführen von Workflows nutzen:

- Erstellen Sie eine Aufgabe für alle Datensätze mit hoher Priorität aus einer Datenbank.
- Speichern Sie alle PDF-Anlagen für E-Mails in einem Azure-Blob.

Wenn Sie die Abfrageaktion in einer Logik-App verwenden möchten, müssen Sie zunächst eine [Logik-App erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## Verwenden der Abfrageaktion
	
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. [Weitere Informationen zu Aktionen finden Sie hier.](connectors-overview.md) Für die Abfrageaktion ist derzeit ein Vorgang im Designer verfügbar: „Array filtern“. Dadurch können Sie ein Array abfragen und einen Satz gefilterter Ergebnisse zurückgeben. Im Folgenden wird erläutert, wie Sie die Aktion in einer Logik-App hinzufügen können:

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.
1. Wählen Sie **Aktion hinzufügen** aus.
1. Geben Sie in das Aktionssuchfeld „Filter“ ein, um die Aktion **Array filtern** anzuzeigen.

	![Auswählen der Abfrageaktion](./media/connectors-native-query/using-action-1.png)

1. Wählen Sie ein zu filterndes Array aus (der folgende Screenshot zeigt die Ergebnisse einer Twitter-Suche).
1. Erstellen Sie eine Bedingung zur Auswertung der einzelnen Elemente (der folgende Screenshot zeigt, wie Tweets von Benutzern mit mehr als 100 Followern gefiltert werden).

	![Abschließen der Abfrageaktion](./media/connectors-native-query/using-action-2.png)

1. Die Aktion gibt ein neues Array aus, das nur Ergebnisse enthält, die die Filteranforderungen erfüllen.
1. Klicken Sie links oben auf der Symbolleiste auf „Speichern“. Dadurch wird Ihre Logik-App gespeichert und veröffentlicht (aktiviert).

---

## Technische Details

Im Folgenden finden Sie Details zu den von diesem Connector unterstützten Aktionen.

## Abfrageaktionen

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. [Weitere Informationen zu Aktionen finden Sie hier.](connectors-overview.md) Der Connector verfügt über eine mögliche Aktion.

|Aktion|Beschreibung|
|---|---|
|Array filtern|Auswerten einer Bedingung für jedes Element in einem Array und Zurückgeben der Ergebnisse|

### Aktionsdetails

Für die Abfrageaktion steht eine mögliche Aktion zur Verfügung. Im Anschluss finden Sie Informationen zu den einzelnen Aktionen und den erforderlichen und optionalen Eingabefeldern sowie entsprechende Ausgabedetails in Verbindung mit deren Verwendung.

#### Array filtern
Führt eine ausgehende HTTP-Anforderung durch. Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Von*|from|Das zu filternde Array|
|Bedingung*|Hierbei gilt:|Die für jedes Element auszuwertende Bedingung|
<br>

**Ausgabedetails**

HTTP-Antwort

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Array gefiltert|array|Array, das ein Objekt für jedes gefilterte Ergebnis enthält|

---

## Nächste Schritte

Im Anschluss finden Sie Details zur den weiteren Schritten mit Logik-Apps und unserer Community.

## Erstellen einer Logik-App

Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

<!---HONumber=AcomDC_0727_2016-->