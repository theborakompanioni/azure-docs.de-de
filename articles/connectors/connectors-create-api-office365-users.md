---
title: "Hinzufügen des Office 365-Benutzer-Connectors in Logic Apps | Microsoft Docs"
description: "Übersicht über den Office 365-Benutzer-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2146481-9105-4f56-b4c2-7ae340cb922f
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: 8f544b781bbf11214183bc0f46215bb9e16c9f55
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="get-started-with-the-office-365-users-connector"></a>Erste Schritte mit dem Office 365-Benutzer-Connector
Stellen Sie eine Verbindung mit der Office 365-Benutzer-API her, um Profile abzurufen, Benutzer zu suchen und vieles mehr. Mit der Office 365-Benutzer-API können Sie folgende Aktionen ausführen:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus der Office 365-Benutzer-API abgerufen werden. 
* Verwenden von Aktionen zum Abrufen von direkt unterstellten Mitarbeitern oder Benutzerprofilen von Vorgesetzten und vieles mehr. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Rufen Sie beispielsweise die direkt unterstellten Mitarbeiter einer Person ab, und aktualisieren Sie mit diesen Informationen eine Azure SQL-Datenbank. 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="create-a-connection-to-office-365-users"></a>Herstellen einer Verbindung mit Office 365-Benutzern
Wenn Sie Ihren Logik-Apps diesen Connector hinzufügen, müssen Sie sich bei Ihrem Office 365-Benutzer-Konto anmelden und den Logik-Apps das Herstellen einer Verbindung mit Ihrem Konto erlauben.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die Eigenschaften für die Office 365-Benutzer-API ein, z. B. die Benutzer-ID. In der **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

## <a name="view-the-swagger"></a>Anzeigen von Swagger

Weitere Informationen finden Sie unter [Details zu Swagger](/connectors/officeusers/).

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.
