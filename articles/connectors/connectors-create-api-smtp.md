<properties
pageTitle="SMTP | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine SMTP-Verbindung her, um E-Mails zu senden."
services="logic-apps"	
documentationCenter=".net,nodejs,java" 	
authors="msftman"	
manager="erikre"	
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem SMTP-Connector

Stellen Sie eine SMTP-Verbindung her, um E-Mails zu senden.

Wenn Sie einen [Connector](./apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Herstellen einer SMTP-Verbindung

Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine [Verbindung](./connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine SMTP-Verbindung herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen an, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Im Falle von SMTP benötigen Sie beispielsweise die Anmeldeinformationen für Ihren Verbindungsnamen, die SMTP-Serveradresse und die Benutzeranmeldeinformationen, um die SMTP-Verbindung zu erstellen. [Weitere Informationen zu Verbindungen]()

### Herstellen einer Verbindung mit SMTP

>[AZURE.INCLUDE [Schritte zum Herstellen einer SMTP-Verbindung](../../includes/connectors-create-api-smtp.md)]

## Verwenden eines SMTP-Triggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Da SMTP über keinen eigenen Trigger verfügt, verwenden wir in diesem Beispiel den Trigger **Salesforce – Wenn ein Objekt erstellt wird**. Dieser Trigger wird aktiviert, wenn in Salesforce ein neues Objekt erstellt wird. In unserem Beispiel legen wir den Trigger so fest, dass über den SMTP-Connector bei jeder Erstellung eines Leads in Salesforce eine Aktion vom Typ *E-Mail senden* mit einer entsprechenden Benachrichtigung ausgelöst wird.

1. Geben Sie im Suchfeld des Logik-App-Designers die Zeichenfolge *salesforce* ein, und wählen Sie anschließend den Trigger **Salesforce – Wenn ein Objekt erstellt wird** aus. ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)

2. Das Steuerelement **Wenn ein Objekt erstellt wird** wird angezeigt. ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)

3. Wählen Sie den **Objekttyp** und anschließend in der Objektliste die Option *Lead* aus. In diesem Schritt geben Sie an, dass Sie einen Trigger erstellen, der Ihre Logik-App benachrichtigt, wenn in Salesforce ein neuer Lead erstellt wird. ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)

4. Der Trigger wurde erstellt. ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)

## Verwenden einer SMTP-Aktion

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Führen Sie nach dem Hinzufügen des Triggers die folgenden Schritte aus, um eine SMTP-Aktion hinzuzufügen, die ausgelöst wird, wenn in Salesforce ein neuer Lead erstellt wird.

1. Wählen Sie **+ Neuer Schritt** aus, um die Aktion hinzuzufügen, die ausgeführt werden soll, wenn ein neuer Lead erstellt wird. ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)

2. Wählen Sie **Aktion hinzufügen** aus. Daraufhin öffnet sich das Suchfeld, in dem Sie nach der gewünschten Aktion suchen können. ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)

3. Geben Sie *smtp* ein, um nach SMTP-bezogenen Aktionen zu suchen.

4. Wählen Sie **SMTP – E-Mail senden** als Aktion aus, die ausgeführt werden soll, wenn der neue Lead erstellt wird. Die Aktionskontrollblock wird geöffnet. Im Designerblock muss die SMTP-Verbindung eingerichtet werden, sofern Sie diesen Schritt noch nicht ausgeführt haben. ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)

5. Geben Sie im Block **SMTP – E-Mail senden** die gewünschten E-Mail-Informationen ein. ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)

6. Speichern Sie Ihre Arbeit, um den Workflow zu aktivieren.

## Technische Details

Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## SMTP-Trigger

SMTP verfügt über keine Trigger.

## SMTP-Aktionen

Für SMTP steht die folgende Aktion zur Verfügung:


|Aktion|Beschreibung|
|--- | ---|
|[E-Mail senden](connectors-create-api-smtp.md#send-email)|Dieser Vorgang sendet eine E-Mail an einen oder mehrere Empfänger.|

### Aktionsdetails

Im Anschluss finden Sie ausführliche Informationen zu den Aktionen für diesen Connector sowie die jeweiligen Antworten:


### E-Mail senden
Dieser Vorgang sendet eine E-Mail an einen oder mehrere Empfänger.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|To  
|To  
|Geben Sie E-Mail-Adressen an, und trennen Sie sie jeweils durch ein Semikolon. (Beispiel: recipient1@domain.com;recipient2@domain.com)|
|CC|cc|Geben Sie E-Mail-Adressen an, und trennen Sie sie jeweils durch ein Semikolon. (Beispiel: recipient1@domain.com;recipient2@domain.com)|
|Betreff|Betreff|E-Mail-Betreff|
|Body|Body|E-Mail-Text|
|Aus|Aus|E-Mail-Adresse des Absenders (Beispiel: sender@domain.com)|
|IsHtml|Is HTML (Ist HTML)|E-Mail im HTML-Format senden (True/False)|
|Bcc|bcc|Geben Sie E-Mail-Adressen an, und trennen Sie sie jeweils durch ein Semikolon. (Beispiel: recipient1@domain.com;recipient2@domain.com)|
|Priorität|Priorität|Wichtigkeit der E-Mail (hoch, normal oder gering)|
|ContentData|Attachments Content Data (Anlageninhaltsdaten)|Inhaltsdaten (base64-codiert für Streams, unverändert für Zeichenfolgen)|
|ContentType|Attachments Content Type (Anlageninhaltstyp)|Content-Typ|
|ContentTransferEncoding|Attachments Content Transfer Encoding (Codierung für die Anlageninhaltsübertragung)|Codierung für die Inhaltsübertragung (base64 oder keine)|
|FileName|Attachments File Name (Anlagendateiname)|Dateiname|
|ContentId|Attachments Content ID (Anlageninhalts-ID)|Inhalts-ID|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.


## HTTP-Antworten

Von den oben angegebenen Aktionen und Triggern können folgende HTTP-Statuscodes zurückgegeben werden:

|Name|Beschreibung|
|---|---|
|200|OK|
|202|Zulässig|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler.|
|die Standardeinstellung|Fehler beim Vorgang.|

## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0803_2016-->