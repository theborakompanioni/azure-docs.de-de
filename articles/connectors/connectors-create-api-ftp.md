---
title: Verwenden des FTP-Connectors in Logik-Apps | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine Verbindung mit einem FTP-Server her, um Dateien zu verwalten. Sie können verschiedene Aktionen ausführen und beispielsweise Dateien hochladen, aktualisieren, abrufen und vom FTP-Server löschen."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d83c55fe-eb59-4b7b-a5ec-afac5c772616
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/22/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 61bfbedfd4f1e84b6976099323a32f3a720634c0
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="get-started-with-the-ftp-connector"></a>Erste Schritte mit dem FTP-Connector
Mit dem FTP-Connector können Sie Dateien auf einem FTP-Server überwachen, verwalten und erstellen. 

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="connect-to-ftp"></a>Herstellen einer FTP-Verbindung
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.  

### <a name="create-a-connection-to-ftp"></a>Herstellen einer FTP-Verbindung
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>Verwenden eines FTP-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!IMPORTANT]
> Für den FTP-Connector wird ein FTP-Server benötigt, auf den über das Internet zugegriffen werden kann, und der mit dem passiven Betriebsmodus konfiguriert ist. Der FTP-Connector ist zudem **nicht mit implizitem FTPS (FTP über SSL) kompatibel**. Er unterstützt nur explizites FTPS (FTP über SSL).  
> 
> 

In diesem Beispiel erfahren Sie, wie Sie mithilfe des Triggers **FTP – When a file is added or modified** (FTP – wenn eine Datei hinzugefügt oder geändert wird) einen Logik-App-Workflow auslösen, wenn einem FTP-Server eine Datei hinzugefügt oder eine Datei auf einem FTP-Server geändert wird. In einem Unternehmensszenario können Sie mit diesem Trigger beispielsweise einen FTP-Ordner auf neue Dateien überwachen, die jeweils Bestellungen von Kunden darstellen.  Anschließend können Sie den Inhalt der Bestellung zur weiteren Verarbeitung und Speicherung in Ihrer Bestelldatenbank mithilfe einer FTP-Connectoraktion wie **Get file content** (Dateiinhalt abrufen) abrufen.

1. Geben Sie im Suchfeld des Designers für Logik-Apps die Zeichenfolge *ftp* ein, und wählen Sie anschließend den Trigger **FTP – When a file is added or modified** (FTP – Wenn eine Datei hinzugefügt oder geändert wird) aus.   
   ![FTP-Trigger – Abbildung 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   Das Steuerelement **Wenn eine Datei hinzugefügt oder geändert wird** wird geöffnet.  
   ![FTP-Trigger – Abbildung 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Wählen Sie rechts im Steuerelement die drei Punkte (**...**) aus. Die Ordnerauswahl wird geöffnet.  
   ![FTP-Trigger – Abbildung 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Wählen Sie den Pfeil nach rechts (**>**) aus, und navigieren Sie zu dem Ordner, den Sie auf neue oder geänderte Dateien überwachen möchten. Wählen Sie den Ordner aus. Der Ordner wird nun im Steuerelement **Folder** (Ordner) angezeigt.  
   ![FTP-Trigger – Abbildung 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Ihre Logik-App ist jetzt mit einem Trigger konfiguriert, die eine Ausführung der anderen Trigger und Aktionen im Workflow startet, wenn in dem angegebenen FTP-Ordner eine Datei geändert oder erstellt wird. 

> [!NOTE]
> Eine Logik-App muss mindestens einen Trigger und eine Aktion enthalten. Führen Sie die Schritte im nächsten Abschnitt aus, um eine Aktion hinzufügen.  
> 
> 

## <a name="use-a-ftp-action"></a>Verwenden einer FTP-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Nachdem Sie bereits einen Trigger hinzugefügt haben, gehen Sie wie folgt vor, um eine Aktion hinzuzufügen, die den Inhalt der neuen oder geänderten Datei abruft, die der Trigger ermittelt hat.    

1. Wählen Sie **+ New step** (+ Neuer Schritt) aus, um die Aktion zum Abrufen des Inhalts der Datei auf dem FTP-Server hinzuzufügen.  
2. Wählen Sie den Link **Aktion hinzufügen** aus.  
   ![FTP-Aktion – Abbildung 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Geben Sie *FTP* ein, um nach allen FTP-bezogenen Aktionen zu suchen.
4. Wählen Sie als Aktion, die ausgeführt werden soll, wenn in dem FTP-Ordner eine neue oder geänderte Datei gefunden wird, die Option **FTP – Get file content** (FTP – Dateiinhalt abrufen) aus.      
   ![FTP-Aktion – Abbildung 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
   Das Steuerelement **Get file content** (Dateiinhalt abrufen) wird geöffnet. **Hinweis**: Sie werden aufgefordert, den Zugriff Ihrer Logik-App auf Ihr FTP-Serverkonto zu autorisieren, sofern Sie diesen Schritt noch nicht ausgeführt haben.  
   ![FTP-Aktion – Abbildung 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Wählen Sie das Steuerelement **Datei** (leerer Bereich unterhalb von **FILE*** [DATEI]) aus. Hier können Sie die unterschiedlichen Eigenschaften der neuen oder geänderten Datei verwenden, die auf dem FTP-Server gefunden wurde.  
6. Wählen Sie die Option **File content** (Dateiinhalt) aus.  
   ![FTP-Aktion – Abbildung 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. Das Steuerelement wird aktualisiert und gibt an, dass die Aktion **FTP – Get file content** (FTP – Dateiinhalt abrufen) den *Dateiinhalt* der neuen oder geänderten Datei auf dem FTP-Server abruft.      
   ![FTP-Aktion – Abbildung 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Speichern Sie Ihre Arbeit, und fügen Sie dem FTP-Ordner eine Datei hinzu, um den Workflow zu testen.    

Die Logik-App ist nun mit einem Trigger konfiguriert, der einen Ordner auf einem FTP-Server überwacht und den Workflow initiiert, wenn auf dem FTP-Server eine neue oder geänderte Datei gefunden wird. 

Des Weiteren ist die Logik-App mit einer Aktion konfiguriert, die den Inhalt der neuen oder geänderten Datei abruft.

Sie können nun eine weitere Aktion hinzufügen – beispielsweise die Aktion [SQL Server – insert row](connectors-create-api-sqlazure.md) (SQL Server – Zeile einfügen), um den Inhalt der neuen oder geänderten Datei in eine SQL-Datenbanktabelle einzufügen.  

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/ftpconnector/) an. 

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)


