---
title: SMTP | Microsoft Docs
description: Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine SMTP-Verbindung her, um E-Mails zu senden.
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: app-service-logic
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7e70351bda39d58b653273a5c3a795a1c6048c2c


---
# <a name="get-started-with-the-smtp-connector"></a>Erste Schritte mit dem SMTP-Connector
Stellen Sie eine SMTP-Verbindung her, um E-Mails zu senden.

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## <a name="connect-to-smtp"></a>Herstellen einer SMTP-Verbindung
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine SMTP-Verbindung herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen an, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Im Falle von SMTP benötigen Sie beispielsweise die Anmeldeinformationen für Ihren Verbindungsnamen, die SMTP-Serveradresse und die Benutzeranmeldeinformationen, um die SMTP-Verbindung zu erstellen. [Weitere Informationen zu Verbindungen]()  

### <a name="create-a-connection-to-smtp"></a>Herstellen einer Verbindung mit SMTP
> [!INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a>Verwenden eines SMTP-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Da SMTP über keinen eigenen Trigger verfügt, verwenden wir in diesem Beispiel den Trigger **Salesforce – Wenn ein Objekt erstellt wird**. Dieser Trigger wird aktiviert, wenn in Salesforce ein neues Objekt erstellt wird. In unserem Beispiel legen wir den Trigger so fest, dass über den SMTP-Connector bei jeder Erstellung eines Leads in Salesforce eine Aktion vom Typ *E-Mail senden* mit einer entsprechenden Benachrichtigung ausgelöst wird.

1. Geben Sie im Suchfeld des Designers für Logik-Apps die Zeichenfolge *Salesforce* ein, und wählen Sie anschließend den Trigger **Salesforce - Wenn ein Objekt erstellt wird** aus.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. Das Steuerelement **Wenn ein Objekt erstellt wird** wird angezeigt.
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. Wählen Sie den **Objekttyp** und anschließend in der Objektliste die Option *Lead* aus. In diesem Schritt geben Sie an, dass Sie einen Trigger erstellen, der Ihre Logik-App benachrichtigt, wenn in Salesforce ein neuer Lead erstellt wird.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. Der Trigger wurde erstellt.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Verwenden einer SMTP-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Führen Sie nach dem Hinzufügen des Triggers die folgenden Schritte aus, um eine SMTP-Aktion hinzuzufügen, die ausgelöst wird, wenn in Salesforce ein neuer Lead erstellt wird.

1. Wählen Sie **+ Neuer Schritt** aus, um die Aktion hinzuzufügen, die ausgeführt werden soll, wenn ein neuer Lead erstellt wird.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. Wählen Sie **Aktion hinzufügen**aus. Daraufhin öffnet sich das Suchfeld, mit dem Sie nach der gewünschten Aktion suchen können.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. Geben Sie *smtp* ein, um nach SMTP-bezogenen Aktionen zu suchen.  
4. Wählen Sie **SMTP – E-Mail senden** als Aktion aus, die ausgeführt werden soll, wenn der neue Lead erstellt wird. Die Aktionskontrollblock wird geöffnet. Im Designerblock muss die SMTP-Verbindung eingerichtet werden, sofern Sie diesen Schritt noch nicht ausgeführt haben.  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. Geben Sie im Block **SMTP – E-Mail senden** die gewünschten E-Mail-Informationen ein.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. Speichern Sie Ihre Arbeit, um den Workflow zu aktivieren.  

## <a name="technical-details"></a>Technische Details
Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## <a name="smtp-triggers"></a>SMTP-Trigger
SMTP verfügt über keine Trigger. 

## <a name="smtp-actions"></a>SMTP-Aktionen
Für SMTP steht die folgende Aktion zur Verfügung:

| Aktion | Beschreibung |
| --- | --- |
| [Senden von E-Mails](connectors-create-api-smtp.md#send-email) |Dieser Vorgang sendet eine E-Mail an einen oder mehrere Empfänger. |

### <a name="action-details"></a>Aktionsdetails
Im Anschluss finden Sie ausführliche Informationen zu den Aktionen für diesen Connector sowie die jeweiligen Antworten:

### <a name="send-email"></a>E-Mail senden
Dieser Vorgang sendet eine E-Mail an einen oder mehrere Empfänger. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| To
 |To
 |Geben Sie E-Mail-Adressen an, und trennen Sie sie jeweils durch ein Semikolon. (Beispiel: recipient1@domain.com;recipient2@domain.com) |
| CC |cc |Geben Sie E-Mail-Adressen an, und trennen Sie sie jeweils durch ein Semikolon. (Beispiel: recipient1@domain.com;recipient2@domain.com) |
| Subject (Antragsteller) |Subject (Antragsteller) |E-Mail-Betreff |
| Body |body |E-Mail-Text |
| Aus |Aus |E-Mail-Adresse des Absenders (Beispiel: sender@domain.com) |
| IsHtml |Is HTML (Ist HTML) |E-Mail im HTML-Format senden (True/False) |
| Bcc |bcc |Geben Sie E-Mail-Adressen an, und trennen Sie sie jeweils durch ein Semikolon. (Beispiel: recipient1@domain.com;recipient2@domain.com) |
| Priorität |Priorität |Wichtigkeit der E-Mail (hoch, normal oder gering) |
| ContentData |Attachments Content Data (Anlageninhaltsdaten) |Inhaltsdaten (base64-codiert für Streams, unverändert für Zeichenfolgen) |
| ContentType |Attachments Content Type (Anlageninhaltstyp) |Content-Typ |
| ContentTransferEncoding |Attachments Content Transfer Encoding (Codierung für die Anlageninhaltsübertragung) |Codierung für die Inhaltsübertragung (base64 oder keine) |
| FileName |Attachments File Name (Anlagendateiname) |Dateiname |
| ContentId |Attachments Content ID (Anlageninhalts-ID) |Inhalts-ID |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

## <a name="http-responses"></a>HTTP-Antworten
Von den oben angegebenen Aktionen und Triggern können folgende HTTP-Statuscodes zurückgegeben werden: 

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler. |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)




<!--HONumber=Nov16_HO3-->


