---
title: Verwenden des Office 365-Video-Connectors in Ihren Logik-Apps | Microsoft Docs
description: Erste Schritte mit dem Office 365-Video-Connector in Ihren Microsoft Azure App Service-Logik-Apps
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 738e5aa7-2523-4116-8b65-211b9063852d
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: f0e3613d4a3fd5478787c0365eb7a0bcde886c81
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="get-started-with-the-office365-video-connector"></a>Erste Schritte mit dem Office 365-Video-Connector
Stellen Sie eine Verbindung mit Office 365 Video her, um Informationen über ein Office 365-Video oder eine Liste von Videos abzurufen und vieles mehr. Mit Office 365 Video können Sie folgende Aktionen ausführen:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Office 365 Video abgerufen werden. 
* Verwenden Sie Aktionen zum Überprüfen des Videoportalstatus, zum Abrufen einer Liste mit allen Videos in einem Kanal und vieles mehr. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Sie können z.B. mit dem Bing-Suche-Connector nach Office 365-Videos suchen und anschließend mithilfe des Office 365-Video-Connectors Informationen zu diesem Video abrufen. Wenn das Video Ihre Anforderungen erfüllt, können Sie es auf Facebook veröffentlichen. 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="create-a-connection-to-office365-video-connector"></a>Herstellen einer Verbindung mit dem Office 365-Video-Connector
Wenn Sie Ihren Logik-Apps diesen Connector hinzufügen, müssen Sie sich bei Ihrem Office 365-Video-Konto anmelden und den Logik-Apps das Herstellen einer Verbindung mit Ihrem Konto erlauben.

> [!INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]
> 
> 

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die Office 365 Video-Eigenschaften ein, z. B. den Mandantennamen oder die Kanal-ID. 


## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/office365videoconnector/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.
