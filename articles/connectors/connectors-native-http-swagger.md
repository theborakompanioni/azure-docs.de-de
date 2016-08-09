<properties
	pageTitle="Hinzufügen der Aktion „HTTP + Swagger“ in Logik-Apps | Microsoft Azure"
	description="Übersicht über die Aktion „HTTP + Swagger“ und relevante Vorgänge"
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

# Erste Schritte mit der Aktion „HTTP + Swagger“

Mit der Aktion „HTTP + Swagger“ können Sie einen erstklassigen Connector für beliebige REST-Endpunkte über ein [Swagger-Dokument](https://swagger.io) erstellen.

- Erweitern Sie eine Logik-App, sodass sie beliebige REST-Endpunkte mit erstklassigen Entwurfsfunktionen aufruft.

Wenn Sie die Aktion „HTTP + Swagger“ in einer Logik-App verwenden möchten, müssen Sie zunächst eine [Logik-App erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## Verwenden von „HTTP + Swagger“ als Trigger oder Aktion

Der Trigger und die Aktion „HTTP + Swagger“ funktionieren wie die [HTTP-Aktion](connectors-native-http.md), bieten aber bessere Entwurfsfunktionen, da sie die Form der API und die Ausgaben im Designer anhand von [Swagger-Metadaten](https://swagger.io) anzeigen. Darüber hinaus können Sie „HTTP + Swagger“ als Trigger verwenden. Wenn Sie einen Abfragetrigger implementieren möchten, sollten Sie dem Abrufmuster folgen, das [in diesem Dokument beschrieben](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers) ist.
	
[Erfahren Sie mehr über Logik-App-Trigger und -Aktionen.](connectors-overview.md)

Im Folgenden finden Sie ein Beispiel der Verwendung des Vorgangs „HTTP + Swagger“ als Aktion in einem Workflow.

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.
1. Wählen Sie **Aktion hinzufügen** aus.
1. Geben Sie im Aktionssuchfeld die Zeichenfolge „Swagger“ ein, um die Aktion „HTTP + Swagger“ anzuzeigen.

	![Aktion „HTTP + Swagger“ auswählen](./media/connectors-native-http-swagger/using-action-1.png)

1. Geben Sie die URL für ein Swagger-Dokument ein.
	- Die URL muss ein HTTPS-Endpunkt und für CORS aktiviert sein, damit sie im Designer funktioniert. Wenn das Swagger-Dokument diese Anforderungen nicht erfüllt, können Sie [für CORS aktivierten Azure-Speicher](#hosting-swagger-from-storage) zum Speichern des Dokuments verwenden.
1. Klicken Sie auf „Weiter“, um das Swagger-Dokument zu lesen und zum Rendern zu nutzen.
1. Geben Sie alle Parameter an, die Sie ggf. für den HTTP-Aufruf benötigen.

	![Konfigurieren der HTTP-Aktion](./media/connectors-native-http-swagger/using-action-2.png)

1. Klicken Sie links oben auf der Symbolleiste auf „Speichern“. Dadurch wird Ihre Logik-App gespeichert und veröffentlicht (aktiviert).

### Hosten von Swagger über den Speicher

Möglicherweise möchten Sie ein Swagger-Dokument referenzieren, das nicht gehostet wird oder die Sicherheits- und Cross-Origin-Anforderungen nicht erfüllt, die für die Verwendung im Designer gelten. Um dieses Problem zu umgehen, können Sie das Swagger-Dokument in Azure Storage speichern und CORS aktivieren, um das Dokument zu referenzieren. Im Folgenden finden Sie die Schritte zum Erstellen, Konfigurieren und Speichern von Swagger in Azure Storage:

1. [Erstellen Sie ein Azure-Speicherkonto mit Blobspeicher](../storage/storage-create-storage-account.md), und legen Sie die Berechtigungen auf „Öffentlicher Zugriff“ fest.
1. Aktivieren Sie CORS für das Blob.
	- Sie können [dieses PowerShell-Skript](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) verwenden, um diese Einstellung automatisch zu konfigurieren.
1. Laden Sie die Swagger-Datei in das Azure-Blob hoch.
	- Hierzu können Sie das [Azure-Portal](https://portal.azure.com) oder ein Tool wie den [Azure-Speicher-Explorer](http://storageexplorer.com/) verwenden.
1. Verweisen Sie einen HTTPS-Link auf das Dokument im Azure-Blob (im Format `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`)

---

## Technische Details

Im Anschluss finden Sie Details zu den von diesem Connector unterstützten Triggern und Aktionen.

## „HTTP + Swagger“-Trigger

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. [Weitere Informationen zu Triggern finden Sie hier.](connectors-overview.md) Der Connector „HTTP + Swagger“ verfügt über einen Trigger.

|Trigger|Beschreibung|
|---|---|
|HTTP + Swagger|Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück.|

## „HTTP + Swagger“-Aktionen

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. [Weitere Informationen zu Aktionen finden Sie hier.](connectors-overview.md) Der Connector „HTTP + Swagger“ verfügt über eine mögliche Aktion.

|Aktion|Beschreibung|
|---|---|
|HTTP + Swagger|Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück.|

### Aktionsdetails

Der Connector „HTTP + Swagger“ weist eine mögliche Aktion auf. Im Anschluss finden Sie Informationen zu den einzelnen Aktionen und den erforderlichen und optionalen Eingabefeldern sowie entsprechende Ausgabedetails in Verbindung mit deren Verwendung.

#### HTTP + Swagger

Erstellen Sie eine ausgehende HTTP-Anforderung mit Unterstützung von Swagger-Metadaten. Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Methode*|method|Zu verwendendes HTTP-Verb|
|URI*|uri|URI für die HTTP-Anforderung|
|Header|headers|Ein JSON-Objekt für die einzubeziehenden Header|
|Body|body|Der HTTP-Anforderungstext|
|Authentifizierung|authentication|Authentifizierung für die Anforderung – [Einzelheiten finden Sie unter „HTTP“](./connectors-native-http.md#authentication)|
<br>

**Ausgabedetails**

HTTP-Antwort

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Header|Objekt|Antwortheader|
|Body|Objekt|Antwortobjekt|
|Statuscode|int|HTTP-Statuscode|

### HTTP-Antworten

Das Aufrufen verschiedener Aktionen löst unter Umständen bestimmte Antworten aus. Die folgende Tabelle enthält entsprechende Antworten und Beschreibungen:

|Name|Beschreibung|
|---|---|
|200|OK|
|202|Zulässig|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler ist aufgetreten|

---

## Nächste Schritte

Im Anschluss finden Sie Details zur den weiteren Schritten mit Logik-Apps und unserer Community.

## Erstellen einer Logik-App

Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

<!---HONumber=AcomDC_0727_2016-->