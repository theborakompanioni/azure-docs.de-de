---
title: SMTP-Connector in Azure Logic Apps | Microsoft-Dokumentation
description: Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine SMTP-Verbindung her, um E-Mails zu senden.
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 1cf96bbf8bd215d7ddb3c99860a5cb4e668be3c2
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="get-started-with-the-smtp-connector"></a>Erste Schritte mit dem SMTP-Connector
Stellen Sie eine SMTP-Verbindung her, um E-Mails zu senden.

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="connect-to-smtp"></a>Herstellen einer SMTP-Verbindung
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine SMTP-Verbindung herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Im Fall von SMTP geben Sie beispielsweise die Anmeldeinformationen für Ihren Verbindungsnamen, die SMTP-Serveradresse und die Benutzeranmeldeinformationen ein, um die SMTP-Verbindung zu erstellen.  

### <a name="create-a-connection-to-smtp"></a>Herstellen einer Verbindung mit SMTP
> [!INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a>Verwenden eines SMTP-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

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
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

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

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/smtpconnector/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.
