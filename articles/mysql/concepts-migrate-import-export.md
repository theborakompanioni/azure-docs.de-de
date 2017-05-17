---
title: "Importieren und Exportieren in bzw. aus Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "Einführung in den Import und Export von Datenbanken in Azure-Datenbank für MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 582f83e3e80d855049abece42726748fcd113ab7
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="migrate-your-mysql-database-using-import-and-export"></a>Migrieren der MySQL-Datenbank durch Import und Export
In diesem Tutorial wird die gängigste Methode vorgestellt, um Daten in der Azure MySQL-Datenbank mit MySQL Workbench zu importieren und zu exportieren. 

## <a name="before-you-begin"></a>Voraussetzungen
Um diese Anleitung schrittweise auszuführen, benötigen Sie Folgendes:
- Ein Azure-Datenbank für MySQL-Server (siehe [Entwerfen der ersten Azure-MySQL-Datenbank](quickstart-create-mysql-server-database-using-azure-portal.md))
- MySQL Workbench ([Herunterladen von MySQL Workbench](https://dev.mysql.com/downloads/workbench/))

## <a name="use-common-tools"></a>Verwenden von gängigen Tools
Mithilfe von gängigen Tools wie MySQL Workbench, Toad oder Navicat können Sie eine Remoteverbindung zu Azure-Datenbank für MySQL herstellen und Daten importieren oder exportieren. Verwenden Sie diese Tools auf Ihrem Clientcomputer mit Internetverbindung, um eine Verbindung zu Azure-Datenbank für MySQL herzustellen. Um eine SSL-verschlüsselte Verbindung gemäß bewährter Sicherheitsmethoden zu verwenden, lesen Sie [Konfigurieren von SSL-Konnektivität in Azure-Datenbank für MySQL](concepts-ssl-connection-security.md). Bei der Migration auf Azure-Datenbank für MySQL müssen Sie Ihre Import- und Exportdateien nicht in einen bestimmten Cloudspeicherort verschieben. 

## <a name="create-a-database-on-the-target-azure-database-for-mysql-service"></a>Erstellen einer Datenbank im Azure-Datenbank für MySQL-Zieldienst
Sie müssen auf dem Azure-Datenbank für MySQL-Zielserver, auf den die Daten mit MySQL Workbench, Toad, Navicat oder einem beliebigen Drittanbietertool für MySQL migriert werden sollen, eine leere Datenbank erstellen. Die Datenbank kann denselben Namen aufweisen wie die Datenbank, die die Sicherungsdaten enthält, oder Sie können eine Datenbank mit einem anderen Namen erstellen.

![Azure-Datenbank für MySQL-Verbindungszeichenfolge](./media/concepts-migrate-import-export/p5.png)

![MySQL Workbench-Verbindungszeichenfolge](./media/concepts-migrate-import-export/p4.png)

## <a name="import-and-export-using-mysql-workbench"></a>Importieren und Exportieren mit MySQL Workbench
Es gibt zwei Möglichkeiten für den Export und Import von Daten in MySQL Workbench, die jeweils einem bestimmten Zweck dienen. 

## <a name="table-data-export-and-import-wizard-using-object-browser-context-menu"></a>Assistent zum Exportieren und Importieren von Tabellendaten über das Kontextmenü „Objektkatalog“
![Importieren und Exportieren mit MySQL Workbench über das Kontextmenü „Objektkatalog“](./media/concepts-migrate-import-export/p1.png)

Dieser Assistent unterstützt Import- und Exportvorgänge mit CSV- und JSON-Dateien, und bietet mehrere Konfigurationsoptionen (Trennzeichen, Spaltenauswahl Auswahl der Codierung und vieles mehr). Der Assistent kann auf lokal oder remote verbundenen MySQL-Servern ausgeführt werden. Die Importaktion enthält Tabellen-, Spalten- und Typzuordnungen. Der Assistent kann über das Kontextmenü „Objektkatalog“ aufgerufen werden, indem Sie mit der rechten Maustaste auf eine Tabelle klicken und entweder **Assistent zum Exportieren von Tabellendaten** oder **Assistent zum Importieren von Tabellendaten** auswählen. 

## <a name="table-data-export-wizard"></a>Assistent zum Exportieren von Tabellendaten
Im folgenden Beispiel wird die Tabelle als CSV-Datei exportiert. 
- Klicken Sie mit der rechten Maustaste auf die Tabelle der zu exportierenden Datenbank. 
- Wählen Sie **Assistent zum Exportieren von Tabellendaten** aus. Wählen Sie die zu exportierenden Spalten, das Zeilenoffset (sofern vorhanden) und die Anzahl (sofern vorhanden) aus. 
- Klicken Sie im Fenster „Auswählen der zu exportierenden Daten“ auf **Weiter**. Wählen Sie den Dateipfad, den Dateityp CSV oder JSON, Zeilentrennzeichen, eingeschlossene Zeichenfolgen und Feldtrennzeichen aus. 
- Wählen Sie im Fenster „Speicherort für die Ausgabedatei auswählen“ die Option **Weiter** aus und im Fenster „Daten exportieren“ ebenfalls „Weiter“.


## <a name="table-data-import-wizard"></a>Assistent zum Importieren von Tabellendaten
Im folgenden Beispiel wird die Tabelle als CSV-Datei importiert.
- Klicken Sie mit der rechten Maustaste auf die Tabelle der zu importierenden Datenbank. 
- Navigieren Sie zu der zu importierenden CSV-Datei und wählen Sie sie aus. Klicken Sie anschließend auf die Schaltfläche „Weiter“. 
- Wählen Sie die (neue oder vorhandene) Zieltabelle aus, aktivieren oder deaktivieren Sie das Kontrollkästchen „Tabelle vor dem Import verkürzen“, und klicken Sie auf die Schaltfläche „Weiter“.
- Wählen Sie die Codierung und die zu importierenden Spalten aus, und dann die Schaltfläche „Weiter“. 
- Wählen Sie im Fenster „Daten importieren“ die Option „Weiter“ aus. Die Daten werden daraufhin entsprechend importiert.

## <a name="sql-data-export-and-import-wizard-from-management-navigator"></a>Assistent zum Exportieren und Importieren von SQL-Daten über den Verwaltungsnavigator
Mit diesem Assistenten können Sie SQL-Daten, die mit MySQL Workbench oder dem Befehl „mysqldump“ generiert wurden, exportieren oder importieren. Rufen Sie diese Assistenten über den Bereich „Navigator“ auf, oder indem Sie im Hauptmenü „Server“ auswählen und dann entweder „Datenimport“ oder „Datenexport“. 

## <a name="data-export"></a>Datenexport
![Exportieren von Daten mit MySQL Workbench über den Verwaltungsnavigator](./media/concepts-migrate-import-export/p2.png)

Über diese Registerkarte können Sie Ihre MySQL-Daten exportieren. 
- Wählen Sie jedes zu exportierende Schema aus, wählen Sie optional bestimmte Schemaobjekte/-tabellen aus einzelnen Schemas aus, und generieren Sie den Export. Zu den Konfigurationsoptionen zählen das Exportieren in einen Projektordner oder als eigenständige SQL-Datei, optional das Sichern von gespeicherten Routinen und Ereignissen oder das Überspringen von Tabellendaten. 
- Verwenden Sie alternativ die Option **Resultset exportieren**, um ein bestimmtes Resultset im SQL-Editor in einem anderen Format (z.B. CSV, JSON, HTML, XML) zu exportieren. 
- Wählen Sie die zu exportierenden Datenbankobjekte aus, und konfigurieren Sie die entsprechenden Optionen. 
- Klicken Sie auf **Aktualisieren**, um die aktuellen Objekte zu laden. 
- Öffnen Sie optional die Registerkarte „Erweiterte Optionen“, mit der Sie den Exportvorgang verfeinern können. Fügen Sie beispielsweise Tabellensperren hinzu, verwenden Sie Replace- anstelle von Insert-Anweisungen sowie Anführungszeichner-Kennzeichner mit Backtick-Zeichen und vieles mehr. 
- Klicken Sie auf **Export starten**, um den Exportvorgang zu beginnen. 


## <a name="data-import"></a>Datenimport
![Importieren von Daten mit MySQL Workbench über den Verwaltungsnavigator](./media/concepts-migrate-import-export/p3.png)

Über diese Registerkarte können Sie die mit dem Datenexportvorgang exportierten Daten oder andere über den Befehl „mysqldump“ exportierten Daten importieren oder wiederherstellen. 
- Wählen Sie den Projektordner oder die eigenständige SQL-Datei aus, wählen Sie das Schema aus, in dem die Daten importiert werden sollen, oder wählen Sie „Neu“ aus, um ein neues Schema zu definieren. 
- Klicken Sie auf **Import starten**, um den Importvorgang zu starten. Der Import wird daraufhin entsprechend ausgeführt.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie mit den ersten Schritten mit diesem Datenbankdienst nicht vertraut sind, lesen Sie bitte folgende Themen:
-  [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](quickstart-create-mysql-server-database-using-azure-portal.md) 
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)

