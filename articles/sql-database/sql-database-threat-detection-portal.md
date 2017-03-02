---
title: "Azure-Portal: Konfigurieren der Bedrohungserkennung – SQL-Datenbank | Microsoft Docs"
description: "Konfigurieren und Verwalten der Bedrohungserkennung über das Azure-Portal"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: v-romcal
ms.assetid: 
ms.service: sql-database
ms.custom: secure and protect
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 07/10/2016
ms.author: ronmat; ronitr
translationtype: Human Translation
ms.sourcegitcommit: 5d51a5ef3387b4c00079547b0f44ffe1f96bd77c
ms.openlocfilehash: 0222847321b82c9572e361cc5e578b779d859792
ms.lasthandoff: 02/17/2017


---
# <a name="configure-and-manage-azure-sql-database-threat-detection-in-the-azure-portal"></a>Konfigurieren und Verwalten der Bedrohungserkennung von Azure SQL-Datenbank im Azure-Portal

In diesem Thema wird erläutert, wie Sie die Bedrohungserkennung einrichten und anormale Datenbankaktivitäten untersuchen. Eine Übersicht über die Bedrohungserkennung von Azure SQL-Datenbank finden Sie in der [Übersicht über die Bedrohungserkennung](sql-database-threat-detection.md).

## <a name="set-up-threat-detection-for-your-database"></a>Einrichten der Bedrohungserkennung für Ihre Datenbank
1. Rufen Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com)auf.
2. Navigieren Sie zum Konfigurationsblatt der SQL-Datenbank, die Sie überwachen möchten. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung**.
   
    ![Navigationsbereich][1]
3. Aktivieren Sie die Überwachung auf dem Blatt für die Konfiguration der **Überwachung und Bedrohungserkennung******. Daraufhin werden die Einstellungen für die Bedrohungserkennung angezeigt.
   
    ![Navigationsbereich][2]
4. Aktivieren Sie **** die Bedrohungserkennung.
5. Konfigurieren Sie die Liste der E-Mail-Empfänger, die bei Erkennung anormaler Datenbankaktivitäten einen Sicherheitshinweis erhalten sollen.
6. Klicken Sie auf dem Blatt **Überwachung und Bedrohungserkennung** auf **Speichern**, um die neue oder aktualisierte Richtlinie für die Überwachung und Bedrohungserkennung zu speichern.
   
    ![Navigationsbereich][3]

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Untersuchen anormaler Datenbankaktivitäten bei Erkennung eines verdächtigen Ereignisses
1. Bei Erkennung anormaler Datenbankaktivitäten erhalten Sie eine E-Mail-Benachrichtigung. <br/>
   Die E-Mail enthält Informationen zum verdächtigen Sicherheitsereignis (wie etwa Art der anomalen Aktivitäten, Datenbankname, Servername und Zeit des Ereignisses). Darüber hinaus enthält sie Angaben zu möglichen Ursachen und empfohlenen Maßnahmen zur Untersuchung und Abwehr der potenziellen Bedrohung für die Datenbank.<br/>
   
    ![Navigationsbereich][4]
2. Klicken Sie in der E-Mail auf den Link für das Azure SQL-Überwachungsprotokoll, **** um das Azure-Portal aufzurufen und die relevanten Überwachungsdatensätze für die Zeit des verdächtigen Ereignisses anzuzeigen.
   
    ![Navigationsbereich][5]
3. Klicken Sie auf die Überwachungsdatensätze, um weitere Details zu den verdächtigen Datenbankaktivitäten (wie etwa SQL-Anweisung, Fehlerursache und Client-IP) anzuzeigen.
   
    ![Navigationsbereich][6]
4. Klicken Sie auf dem Blatt mit den Überwachungsdatensätzen auf **In Excel öffnen**, um eine vorkonfigurierte Excel-Vorlage zu öffnen, das Überwachungsprotokoll zu importieren und eine ausführlichere Analyse für die Zeit des verdächtigen Ereignisses durchzuführen.<br/>
   **Hinweis:** Ab Excel 2010 werden Power Query und die Einstellung **Schnelles Kombinieren** benötigt.
   
    ![Navigationsbereich][7]
5. So konfigurieren Sie die Einstellung **Schnelles Kombinieren**: Klicken Sie auf der Registerkarte **POWER QUERY** des Menübands auf **Optionen**, um das gleichnamige Dialogfeld anzuzeigen. Navigieren Sie zum Datenschutzabschnitt, und wählen Sie die zweite Option zum Ignorieren der Sicherheitsstufen und zur potenziellen Verbesserung der Leistung:
   
    ![Navigationsbereich][8]
6. Wenn Sie SQL-Überwachungsprotokolle laden möchten, vergewissern Sie sich, dass die Parameter auf der Einstellungsregisterkarte ordnungsgemäß festgelegt sind. Klicken Sie dann im Menüband auf „Daten“ und anschließend auf die Schaltfläche „Alle aktualisieren“.
   
    ![Navigationsbereich][9]
7. Die Ergebnisse werden auf dem Blatt mit den **SQL-Überwachungsprotokollen** angezeigt. Dadurch können Sie die erkannten anomalen Aktivitäten eingehender untersuchen und die Auswirkungen des Sicherheitsereignisses in Ihrer Anwendung begrenzen.

## <a name="next-steps"></a>Nächste Schritte

* Eine Übersicht über die Bedrohungserkennung von SQL-Datenbank finden Sie unter [Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection.md).
* Eine Übersicht über die Überwachung von SQL-Datenbank finden Sie unter [Datenbanküberwachung](sql-database-auditing.md).


<!--Image references-->
[1]: ./media/sql-database-threat-detection-get-started/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection-get-started/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection-get-started/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection-get-started/4_td_email.png
[5]: ./media/sql-database-threat-detection-get-started/5_td_audit_records.png
[6]: ./media/sql-database-threat-detection-get-started/6_td_audit_record_details.png
[7]: ./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png
[8]: ./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png
[9]: ./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png

