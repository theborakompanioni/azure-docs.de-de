---
title: Herstellen einer Verbindung mit lokalen Dateisystemen in Azure Logic Apps | Microsoft-Dokumentation
description: "Herstellen einer Verbindung mit lokalen Dateisystemen in Ihrem Logik-App-Workflow über das lokale Datengateway und den Dateisystem-Connector"
keywords: Dateisysteme
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2017
ms.author: deli; LADocs
translationtype: Human Translation
ms.sourcegitcommit: 26d460a699e31f6c19e3b282fa589ed07ce4a068
ms.openlocfilehash: 9eab48d40c3d9db896724d3bdb574d89fdab2796
ms.lasthandoff: 04/04/2017

---

# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a>Herstellen einer Verbindung mit lokalen Dateisystemen in Logik-Apps mit dem Dateisystem-Connector

Hybridcloudkonnektivität ist für Logik-Apps von zentraler Bedeutung. Daher können Sie zum Verwalten von Daten und für den sicheren Zugriff auf lokale Ressourcen für Ihre Logik-Apps das lokale Datengateway verwenden. In diesem Artikel wird gezeigt, wie Sie eine Verbindung mit einem lokalen Dateisystem mithilfe eines einfachen Szenarios herstellen: Kopieren einer Datei, die in Dropbox in eine Dateifreigabe hochgeladen wird, und anschließendes Senden einer E-Mail.

## <a name="prerequisites"></a>Voraussetzungen

- Installieren und konfigurieren Sie das neueste [lokale Datengateway](https://www.microsoft.com/download/details.aspx?id=53127).
- Installieren Sie das neueste lokale Datengateway, Version 1.15.6150.1 oder höher. In [Herstellen einer Verbindung mit dem lokalen Datengateway für Logic Apps](http://aka.ms/logicapps-gateway) sind die nötigen Schritte aufgeführt. Das Gateway muss auf einem lokalen Computer installiert werden, bevor Sie mit den verbleibenden Schritten fortfahren können.

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a>Hinzufügen eines Triggers und von Aktionen für die Verbindung mit dem Dateisystem

1. Erstellen Sie eine Logik-App, und fügen Sie den folgenden Dropbox-Trigger hinzu: **When a file is created** (Beim Erstellen einer Datei). 
2. Wählen Sie unter dem Trigger die Optionen **Nächster Schritt** > **Aktion hinzufügen** aus. 
3. Geben Sie im Suchfeld `file system` ein, damit Sie alle unterstützten Aktionen für den Dateisystem-Connector anzeigen können.

   ![Suchen nach „Dateiconnector“](media/logic-apps-using-file-connector/search-file-connector.png)

2. Wählen Sie die Aktion **Datei erstellen** aus, und erstellen Sie eine Verbindung mit dem Dateisystem.

   Wenn noch keine Verbindung vorhanden ist, werden Sie aufgefordert, eine herzustellen.

   1. Wählen Sie **Verbinden über lokales Datengateway** aus. Es werden weitere Eigenschaften angezeigt.
   2. Wählen Sie den Stammordner für das Dateisystem aus.
      
       > [!NOTE]
       > Der Stammordner ist der übergeordnete Hauptordner, der bei allen dateibezogenen Aktionen für relative Pfade verwendet wird. Sie können einen lokalen Ordner auf dem Computer angeben, auf dem das lokale Datengateway installiert ist, oder eine Netzwerkfreigabe, auf die der Computer Zugriff hat.

   3. Geben Sie den Benutzernamen und das Kennwort für das Gateway ein.
   4. Wählen Sie das Gateway aus, das Sie zuvor installiert haben.

       ![Konfigurieren der Verbindung](media/logic-apps-using-file-connector/create-file.png)

3. Wählen Sie **Erstellen** aus, nachdem Sie alle Details angegeben haben. 

   Die Verbindung wird in Logic Apps konfiguriert und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert. 
   Wenn die Verbindung ordnungsgemäß eingerichtet ist, werden Optionen für die zuvor ausgewählte Aktion angezeigt. 
   Der Dateisystem-Connector kann jetzt verwendet werden.

4. Geben Sie an, dass Sie Dateien aus Dropbox in den Stammordner für die lokale Dateifreigabe kopieren möchten.

   ![Erstellen einer Dateiaktion](media/logic-apps-using-file-connector/create-file-filled.png)

5. Nachdem die Datei in Ihrer Logik-App kopiert wurde, fügen Sie eine Outlook-Aktion hinzu, über die eine E-Mail gesendet wird, sodass die entsprechenden Benutzer über die neue Datei informiert werden. Geben Sie die Empfänger, den Titel und Text der E-Mail ein. 

   In der Auswahl für dynamische Inhalte können Sie Datenausgaben im Datei-Connector auswählen und so der E-Mail weitere Details hinzufügen.

   ![Aktion „E-Mail senden“](media/logic-apps-using-file-connector/send-email.png)

6. Speichern Sie Ihre Logik-App. Testen Sie die App durch Hochladen einer Datei in Dropbox. Die Datei sollte in die lokale Dateifreigabe kopiert werden, und Sie sollten eine E-Mail zu diesem Vorgang erhalten.

   > [!TIP] 
   > Erfahren Sie, wie Sie [Logik-Apps überwachen](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Herzlichen Glückwunsch, Sie verfügen nun über eine funktionsfähige Logik-App, über die eine Verbindung mit dem lokalen Dateisystem hergestellt werden kann. Sie können nun weitere Funktionen des Connectors ausprobieren, beispielsweise:

- Datei erstellen
- Dateien im Ordner aufführen
- Datei anfügen
- Datei löschen
- Dateiinhalte abrufen
- Dateiinhalt anhand des Pfads abrufen
- Dateimetadaten abrufen
- Dateimetadaten anhand des Pfads abrufen
- Dateien im Stammordner aufführen
- Datei aktualisieren

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Im [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) können Sie Fragen stellen, Fragen beantworten und sich über die Aktivitäten anderer Azure Logic Apps-Benutzer informieren.

Zur Optimierung von Azure Logic Apps und Connectors können Sie auf der [Benutzerfeedbackwebsite für Azure Logic Apps](http://aka.ms/logicapps-wish) über Ideen abstimmen oder selbst Ideen einreichen.

## <a name="next-steps"></a>Nächste Schritte

- [Herstellen einer Verbindung mit lokalen Daten für Logik-Apps](../logic-apps/logic-apps-gateway-connection.md)
- Informationen zu [Enterprise Integration](../logic-apps/logic-apps-enterprise-integration-overview.md)

