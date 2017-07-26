---
title: Verwenden des SharePoint Server-Connectors in Ihren Logik-Apps | Microsoft-Dokumentation
description: Erste Schritte mit dem SharePoint Server-Connector in Ihren Logik-Apps
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 0f3274816e279a1aa57febaa2f8294914900799a
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="get-started-with-the-sharepoint-connector"></a>Erste Schritte mit dem SharePoint-Connector
Der SharePoint-Connector bietet eine Methode zum Arbeiten mit Listen in SharePoint.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="create-a-connection-to-sharepoint"></a>Herstellen einer Verbindung mit SharePoint
Erstellen Sie zum Verwenden des SharePoint-Connectors zunächst eine **Verbindung** , und geben Sie anschließend die Details für die folgenden Eigenschaften an: 

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Bereitstellen von SharePoint-Anmeldeinformationen |

Um eine Verbindung mit **SharePoint** herzustellen, geben Sie Ihre Identität (Benutzername und Kennwort, Smartcard-Anmeldeinformationen usw.) für SharePoint ein. Nach der Authentifizierung können Sie den SharePoint-Connector in Ihrer Logik-App verwenden. 

Führen Sie im Designer der Logik-App die folgenden Schritte durch, um sich bei SharePoint anzumelden und die **Verbindung** zu erstellen, die Sie in Ihrer Logik-App verwenden können:

1. Geben Sie in das Suchfeld „SharePoint“ ein, und warten Sie, bis die Suche alle Einträge mit „SharePoint“ im Namen zurückgibt:    
   ![SharePoint konfigurieren][1]  
2. Wählen Sie **SharePoint – Wenn eine Datei erstellt wird** aus.   
3. Wählen Sie **Bei SharePoint anmelden** aus:   
   ![SharePoint konfigurieren][2]    
4. Geben Sie Ihre SharePoint-Anmeldeinformationen ein, um sich bei SharePoint zu authentifizieren.   
   ![SharePoint konfigurieren][3]     
5. Nach Abschluss der Authentifizierung werden Sie zu Ihrer Logik-App umgeleitet, damit Sie diese vervollständigen können. Dazu konfigurieren Sie das SharePoint-Dialogfeld **Wenn eine Datei erstellt wird**.          
   ![SharePoint konfigurieren][4]  
6. Sie können dann weitere Trigger und Aktionen hinzufügen, die Sie benötigen, um Ihre Logik-App abzuschließen.   
7. Wählen Sie auf der oberen Menüleiste die Option **Speichern** aus, um Ihre Arbeit zu speichern.  

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/sharepoint/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png

