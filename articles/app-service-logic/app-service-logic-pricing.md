<properties 
	pageTitle="Preismodell für Logik-Apps | Microsoft Azure" 
	description="Enthält Details zu den Preisen von Logik-Apps." 
	authors="kevinlam1" 
	manager="dwrede" 
	editor="" 
	services="app-service\logic" 
	documentationCenter=""/>

<tags
	ms.service="app-service-logic"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article" 
	ms.date="07/25/2016"
	ms.author="klam"/>

# Preismodell für Logik-Apps

Mit Logik-Apps können Sie einen Integrationsworkflow in der Cloud skalieren und ausführen. Unten sind die Details zum Abrechnungsplan und den Tarifen für Azure Logic Apps angegeben.

## Nutzungspreise

Für neu erstellte Logik-Apps wird ein Nutzungsplan verwendet. Beim Preismodell, das auf der Nutzung von Logik-Apps basiert, ist die Bezahlung nutzungsabhängig. Logik-Apps werden bei Verwendung eines Nutzungsplans nicht gedrosselt. Alle Aktionsausführungen, die bei der Ausführung einer Logik-App-Instanz verwendet werden, werden gemessen.

### Was sind Aktionsausführungen?

Jeder Schritt einer Logik-App-Definition ist eine Aktion. Beispiele hierfür sind Trigger, Ablaufsteuerungsschritte wie Bedingungen, Bereiche, For-Each-Schleifen und Do-Until-Schleifen, Aufrufe von Connectors und Aufrufe von nativen Aktionen. Trigger sind einfach spezielle Aktionen, die für das Instanziieren einer neuen Instanz einer Logik-App beim Empfang eines bestimmten Ereignisses ausgelegt sind. Es gibt eine Reihe verschiedener Verhalten für Trigger, die sich darauf auswirken können, wie die Logik-App gemessen wird.

-	**Abfragetrigger**: Mit diesem Trigger wird ein Endpunkt fortlaufend abgefragt, bis er eine Nachricht empfängt, die die Kriterien für die Erstellung einer neuen Instanz einer Logik-App erfüllt. Das Abrufintervall kann mit dem Designer für Logik-Apps im Trigger konfiguriert werden. Jede Abfrageanforderung wird als Aktionsausführung gezählt. Dies gilt auch, wenn keine neue Instanz einer Logik-App erstellt wird.

-	**Webhooktrigger**: Dieser Trigger wartet darauf, dass von einem Client eine Anforderung an einen bestimmten Endpunkt gesendet wird. Jede Anforderung, die an den Webhookendpunkt gesendet wird, wird als eine Aktionsausführung gezählt. Sowohl beim Anforderungstrigger als auch beim HTTP-Webhooktrigger handelt es sich um Webhooktrigger.

-	**Wiederholungstrigger**: Dieser Trigger erstellt eine neue Instanz der Logik-App basierend auf dem Wiederholungsintervall, das im Trigger konfiguriert ist. Beispielsweise kann ein Wiederholungstrigger so konfiguriert werden, dass er alle drei Tage oder auch jede Minute ausgeführt wird.

Triggerausführungen werden auf dem Ressourcenblatt „Logik-Apps“ unter dem „Triggerverlauf“ angezeigt.

Alle Aktionen, die ausgeführt wurden – ob erfolgreich oder nicht – werden als Aktionsausführung gemessen. Aktionen, die aufgrund einer nicht erfüllten Bedingung übersprungen wurden, oder Aktionen, die aufgrund einer Beendigung der Logik-App vor dem Abschluss nicht ausgeführt wurden, werden nicht als Aktionsausführungen gezählt.

Innerhalb von Schleifen ausgeführte Aktionen werden pro Iteration der Schleife gezählt. Eine einzelne Aktion in einer For-Each-Schleife, die eine Liste mit zehn Einträgen durchläuft, wird wie folgt gezählt: Anzahl von Einträgen in der Liste (10) multipliziert mit der Anzahl von Aktionen in der Schleife (1) plus eine Aktion für die Initiierung der Schleife. Für dieses Beispiel ergeben sich also (10 * 1) + 1 = 11 Aktionsausführungen.

Für Logik-Apps, die deaktiviert sind, können keine neuen Instanzen instanziiert werden. Daher werden sie für die Dauer der Deaktivierung auch nicht berechnet. Beachten Sie, dass es nach dem Deaktivieren einer Logik-App etwas dauern kann, bis die Instanzen stillgelegt werden und vollständig deaktiviert sind.

## App Service-Pläne

App Service-Pläne sind für das Erstellen einer Logik-App nicht mehr erforderlich. Bereits mit App Service-Plan erstellte Logik-Apps verhalten sich wie bisher. Je nach dem gewählten Plan werden sie nach dem Überschreiten einer bestimmten Anzahl von täglichen Ausführungen gedrosselt und nicht per Messung der Aktionsausführungen berechnet.

App Service-Pläne und ihre täglich zulässigen Aktionsausführungen:

| |Free/Shared/Basic|Standard|Premium|
|---|---|---|---|
|Aktionsausführungen pro Tag| 200|10\.000|50\.000|

Um eine Logik-App mit zugeordnetem App Service-Plan in ein Verbrauchsmodell zu ändern, entfernen Sie den Verweis auf den App Service-Plan in der Logik-App-Definition. Dies ist durch einen einfachen Aufruf eines PowerShell-Cmdlets möglich:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## Preise

Eine Preisübersicht finden Sie unter [Logik-Apps Preise](https://azure.microsoft.com/pricing/details/logic-apps/).

## Nächste Schritte

- [Übersicht über Logik-Apps][whatis]
- [Erstellen Ihrer ersten Logik-App][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

<!---HONumber=AcomDC_0727_2016-->