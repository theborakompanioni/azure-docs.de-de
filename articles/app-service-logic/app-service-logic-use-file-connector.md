---
title: Verwenden des Dateisystem-Connectors mit lokalem Datengateway in Logic Apps | Microsoft Docs
description: "Logic Apps ermöglicht Ihnen mühelos das Herstellen einer Verbindung mit einem lokalen Dateisystem im Rahmen Ihres Workflows."
services: logic-apps
documentationcenter: dev-center-name
author: derek1ee
manager: erikre
ms.service: logic-apps
ms.devlang: wdl
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2016
ms.author: deli
translationtype: Human Translation
ms.sourcegitcommit: 9b2ae8a06cfa0cf686b7cae50d3b6903e536ee73
ms.openlocfilehash: fc81b6ca60af48ddfd8862d5f6620db537646ae7


---
# <a name="use-file-system-connector-with-on-premises-data-gateway"></a>Verwenden des Dateisystemconnectors mit lokalem Datengateway

Hybridcloudkonnektivität ist der Kern von Logic Apps. Das lokale Datengateway ermöglicht das Verwalten von Daten und den sicheren Zugriff auf Ressourcen, die für Logic Apps lokal sind. In diesem Artikel veranschaulichen wir das Herstellen einer Verbindung mit einem lokalen Dateisystem mithilfe eines einfachen Szenarios: Kopieren einer Datei, die in Dropbox hochgeladen wurde, in eine Dateifreigabe, und anschließendes Senden einer E-Mail.

## <a name="prerequisites"></a>Voraussetzungen
- Installieren und konfigurieren Sie das neueste [lokale Datengateway](https://www.microsoft.com/en-us/download/details.aspx?id=53127).

 Installieren Sie das neueste lokale Datengateway, Version 1.15.6150.1 oder höher, sofern noch nicht geschehen. Anweisungen finden Sie in [diesem Artikel](http://aka.ms/logicapps-gateway). Das Gateway muss auf einem lokalen Computer installiert werden, bevor Sie mit den verbleibenden Schritten fortfahren können.

## <a name="use-file-system-connector"></a>Verwenden eines Dateisystemconnectors

1. Lassen Sie uns einen Dropbox-Trigger des Typs „Wenn eine Datei erstellt wird“ erstellen. Werfen Sie anschließend einen Blick auf alle unterstützten Aktionen für Dateiconnectors, indem Sie einfach „Dateisystem“ in das Suchfeld eingeben.

 ![Suchen nach „Dateiconnector“](./media/app-service-logic-use-file-connector/search-file-connector.png)

2. Wählen Sie „Datei erstellen“, und erstellen Sie eine Verbindung für die Datei.
 - Wenn es noch keine Verbindung gibt, werden Sie aufgefordert, eine zu erstellen.
 - Aktivieren Sie das Kontrollkästchen „Connect via on-premises data gateway“ (Über lokales Datengateway verbinden). Anschließend werden weitere Felder angezeigt.
 - Geben Sie den Stammordner an. Dies kann ein lokaler Ordner auf dem Computer sein, auf dem das lokale Datengateway installiert ist, oder eine Netzwerkfreigabe, auf die der Computer Zugriff hat.
 - Geben Sie den Benutzernamen und das Kennwort für das Gateway ein.
 - Wählen Sie das Gateway aus, das Sie im vorherigen Schritt installiert haben.
    
 > [!NOTE]
 > „Stammordner“ ist der übergeordnete Hauptordner, der für relative Pfade für alle dateibezogenen Aktionen verwendet werden soll.

 ![Konfigurieren der Verbindung](./media/app-service-logic-use-file-connector/create-file.png)

3. Nachdem Sie alle Details angegeben haben, klicken Sie auf „Erstellen“. Logic Apps konfiguriert und testet die Verbindung, um sicherzustellen, dass sie ordnungsgemäß funktioniert. Wenn alles in Ordnung ist, werden Optionen für die Karte ausgewählt, die Sie zuvor ausgewählt haben, und der Dateiconnector ist nun betriebsbereit.

4. Kopieren Sie die in Dropbox hochgeladene Datei in den Stammordner der lokalen Dateifreigabe.

 ![Erstellen einer Dateiaktion](./media/app-service-logic-use-file-connector/create-file-filled.png)

5. Nachdem die Datei kopiert wurde, senden Sie eine E-Mail, um die entsprechenden Benutzer darüber zu informieren. Wie bei anderen Connectors steht die Ausgabe aus vorherigen Aktionen im Auswahlelement „Dynamischer Inhalt“ zur Verfügung.
 - Geben Sie die Empfänger, den Titel und Text der E-Mail an. Wählen Sie mithilfe des Auswahlelements „Dynamischer Inhalt“ die Ausgabe des Dateiconnectors aus, um die E-Mail mit mehr Informationen zu versehen.

 ![Aktion „E-Mail senden“](./media/app-service-logic-use-file-connector/send-email.png)

6. Speichern Sie Ihre Logik-App, und testen Sie sie, indem Sie eine Datei in Dropbox hochladen. Die Datei sollte in die lokale Dateifreigabe kopiert werden, und Sie sollten eine E-Mail-Benachrichtigung über den Vorgang erhalten.
 > [!TIP]
 > Lesen Sie, wie Sie [Logik-Apps überwachen](app-service-logic-monitor-your-logic-apps.md).

7. Alles fertig. Sie haben nun eine funktionsfähige Logik-App, die den Dateisystemconnector nutzt. Sie können nun beginnen, die anderen angebotenen Funktionen zu untersuchen:
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

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr zum [Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md). 
- Erstellen einer [lokalen Verbindung](app-service-logic-gateway-connection.md) mit Logic Apps.



<!--HONumber=Nov16_HO3-->


