---
title: 'So wird&quot;s gemacht: Verwenden des SFTP-Connectors in Ihren Logik-Apps | Microsoft Docs'
description: "Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine Verbindung mit der SFTP-API her, um Dateien zu senden und zu empfangen. Sie können verschiedene Vorgänge ausführen und beispielsweise Dateien erstellen, aktualisieren, abrufen oder löschen."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 31253d8daee1581167a96a20ba8ad529a04b3e92
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="get-started-with-the-sftp-connector"></a>Erste Schritte mit dem SFTP-Connector
Greifen Sie mithilfe des SFTP-Connectors auf ein SFTP-Konto zu, um Dateien zu senden und zu empfangen. Sie können verschiedene Vorgänge ausführen und beispielsweise Dateien erstellen, aktualisieren, abrufen oder löschen.  

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="connect-to-sftp"></a>Herstellen einer Verbindung mit SFTP
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.  

### <a name="create-a-connection-to-sftp"></a>Herstellen einer Verbindung mit SFTP
> [!INCLUDE [Steps to create a connection to SFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a>Verwenden eines SFTP-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

In diesem Beispiel erfahren Sie, wie mithilfe des Triggers **SFTP – Wenn eine Datei hinzugefügt oder geändert wird** ein Logik-App-Workflow initiiert wird, wenn einem SFTP-Server eine Datei hinzugefügt oder eine Datei auf einem SFTP-Server geändert wird. Darüber hinaus fügen Sie eine Bedingung hinzu, die den Inhalt der neuen oder geänderten Datei überprüft und entscheidet, dass die Datei extrahiert werden soll, wenn deren Inhalt darauf hindeutet, dass die Datei vor der Verwendung des Inhalts extrahiert werden muss. Schließlich fügen Sie eine Aktion hinzu, die den Inhalt einer Datei extrahiert und in einem Ordner auf dem SFTP-Server ablegt. 

In einem Unternehmensszenario können Sie mit diesem Trigger beispielsweise einen SFTP-Ordner auf neue Dateien überwachen, die jeweils Kundenaufträge darstellen.  Anschließend können Sie den Inhalt der Bestellung zur weiteren Verarbeitung und Speicherung in einer Bestelldatenbank mithilfe einer SFTP-Connectoraktion wie **Dateiinhalt abrufen** abrufen.

> [!INCLUDE [Steps to create an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a>Eine Bedingung hinzufügen
> [!INCLUDE [Steps to add a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a>Verwenden einer SFTP-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps to create an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/sftpconnector/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.
