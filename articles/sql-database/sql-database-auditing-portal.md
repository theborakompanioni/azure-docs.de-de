---
title: "Azure-Portal: Verwalten der SQL-Datenbanküberwachung | Microsoft-Dokumentation"
description: "Konfigurieren der Azure SQL-Datenbanküberwachung im Azure-Portal zum Nachverfolgen von Datenbankereignissen und Protokollieren der Ereignisse in einem Überwachungsprotokoll in Ihrem Azure Storage-Konto."
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: secure and protect
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/25/2017
ms.author: ronitr;giladm
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: d1350081aa4f70660802c61a77250496e1e7fc2a
ms.lasthandoff: 03/10/2017


---

# <a name="configure-and-manage-sql-database-auditing-in-the-azure-portal"></a>Konfigurieren und Verwalten der SQL-Datenbanküberwachung im Azure-Portal

In diesem Abschnitt wird beschrieben, wie die Überwachung im Azure-Portal konfiguriert und verwaltet wird. Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe von PowerShell finden Sie unter [Configure auditing with PowerShell](sql-database-auditing-powershell.md) (Konfigurieren der Überwachung mithilfe von PowerShell). 

Eine Übersicht über die Überwachung finden Sie unter [SQL-Datenbanküberwachung](sql-database-auditing.md).

## <a name="configure-blob-auditing"></a>Konfigurieren der Blobüberwachung

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) unter https://portal.azure.com.
2. Navigieren Sie zum Blatt mit den Einstellungen der SQL-Datenbank-/SQL Server-Instanz, die Sie überwachen möchten. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung**.

    ![Navigationsbereich](./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png)
3. Auf dem Blatt für die Konfiguration der Datenbanküberwachung können Sie das Kontrollkästchen **Einstellungen vom Server erben** aktivieren, um anzugeben, dass diese Datenbank gemäß den Einstellungen des zugehörigen Servers überwacht wird. Wenn Sie diese Option aktivieren, wird der Link **Server-Überwachungseinstellungen anzeigen** angezeigt, den Sie verwenden können, um die Überwachungseinstellungen des Servers für diesen Kontext anzuzeigen oder zu ändern.

    ![Navigationsbereich][2]
4. Wenn Sie die Blobüberwachung (zusätzlich zur oder anstelle der Überwachung auf Serverebene) auf Datenbankebene aktivieren möchten, **deaktivieren** Sie die Option **Überwachungseinstellungen vom Server erben**, **aktivieren** Sie die Überwachung, und wählen Sie den Überwachungstyp **Blob** aus.

    ![Navigationsbereich][3]
5. Wählen Sie **Speicherdetails**, um das Blatt „Speicherung von Überwachungsprotokollen“ zu öffnen. Wählen Sie das Azure-Speicherkonto aus, in dem die Protokolle gespeichert werden sollen, sowie die Aufbewahrungsdauer, nach der die alten Protokolle gelöscht werden. Klicken Sie anschließend unten auf **OK**. **Tipp:** Verwenden Sie für alle überwachten Datenbanken dasselbe Speicherkonto, um die Berichtvorlagen für die Überwachung optimal einzusetzen.

    <a id="storage-screenshot"></a>
    ![Navigationsbereich][4]
6. Wenn Sie die überwachten Ereignisse anpassen möchten, erreichen Sie dies über PowerShell oder REST-API.
7. Nachdem Sie Ihre Überwachungseinstellungen konfiguriert haben, können Sie das neue Feature Bedrohungserkennung (Vorschau) aktivieren und die E-Mail-Konten konfigurieren, an die Sicherheitswarnungen gesendet werden sollen. Mit der Bedrohungserkennung können Sie proaktive Warnungen bei anomalen Datenbankaktivitäten erhalten, die auf mögliche Sicherheitsbedrohungen hinweisen können. Weitere Informationen finden Sie unter [Bedrohungserkennung](sql-database-threat-detection.md).
8. Klicken Sie auf **Speichern**.


## <a name="table-auditing"></a>Tabellenüberwachung

> [!IMPORTANT]
> Überprüfen Sie vor der Einrichtung der **Tabellenüberwachung**, ob Sie einen [„kompatiblen Client“](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) verwenden. Falls Sie strikte Firewalleinstellungen haben, beachten Sie außerdem, dass der [IP-Endpunkt Ihrer Datenbank geändert wird](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md), wenn Sie die Tabellenüberwachung aktivieren.
>

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) unter https://portal.azure.com.
2. Navigieren Sie zum Blatt mit den Einstellungen der SQL-Datenbank-/SQL Server-Instanz, die Sie überwachen möchten. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung** (*[siehe Screenshot im Abschnitt „Blobüberwachung“](#auditing-screenshot)*).
3. Auf dem Blatt für die Konfiguration der Datenbanküberwachung können Sie das Kontrollkästchen **Einstellungen vom Server erben** aktivieren, um anzugeben, dass diese Datenbank gemäß den Einstellungen des zugehörigen Servers überwacht wird. Wenn Sie diese Option aktivieren, wird der Link **Server-Überwachungseinstellungen anzeigen** angezeigt, den Sie verwenden können, um die Überwachungseinstellungen des Servers für diesen Kontext anzuzeigen oder zu ändern.

    ![Navigationsbereich][2]
4. Wenn die Überwachungseinstellungen nicht vom Server geerbt werden sollen, **deaktivieren** Sie die Option **Überwachungseinstellungen vom Server erben**, **aktivieren** Sie die Überwachung, und wählen Sie den Überwachungstyp **Tabelle** aus.

    ![Navigationsbereich][3-tbl]
5. Wählen Sie **Speicherdetails**, um das Blatt „Speicherung von Überwachungsprotokollen“ zu öffnen. Wählen Sie das Azure-Speicherkonto aus, in dem die Protokolle gespeichert werden sollen, sowie die Aufbewahrungsdauer, nach der die alten Protokolle gelöscht werden. **Tipp:** Verwenden Sie für alle überwachten Datenbanken dasselbe Speicherkonto, um die Berichtvorlagen für die Überwachung optimal einzusetzen (*[siehe Screenshot im Abschnitt „Blobüberwachung“](#storage-screenshot)*).
6. Klicken Sie auf **Überwachte Ereignisse** , um anzupassen, welche Ereignisse überwacht werden sollen. Klicken Sie auf dem Blatt „Protokollierung nach Ereignis“ auf **Erfolg** und **Fehler**, um alle Ereignisse zu protokollieren oder einzelne Ereigniskategorien auszuwählen.

    ![Navigationsbereich][5]
7. Nachdem Sie Ihre Überwachungseinstellungen konfiguriert haben, können Sie das neue Feature Bedrohungserkennung (Vorschau) aktivieren und die E-Mail-Konten konfigurieren, an die Sicherheitswarnungen gesendet werden sollen. Mit der Bedrohungserkennung können Sie proaktive Warnungen bei anomalen Datenbankaktivitäten erhalten, die auf mögliche Sicherheitsbedrohungen hinweisen können. Weitere Informationen finden Sie unter [Bedrohungserkennung](sql-database-threat-detection.md).
8. Klicken Sie auf **Speichern**.

## <a name="auditing-geo-replicated-databases"></a>Überwachung von georeplizierten Datenbanken

Wenn Sie georeplizierte Datenbanken verwenden, können Sie die Überwachung je nach Überwachungstyp für die primäre Datenbank, die sekundäre Datenbank oder für beide einrichten.

**Tabellenüberwachung**: Sie können für jede der beiden Datenbanken (primär und sekundär) eine separate Richtlinie auf Datenbank- oder Serverebene konfigurieren.

**Blobüberwachung**: Befolgen Sie diese Anweisungen:

1. **Primäre Datenbank**: Aktivieren Sie die Blobüberwachung entweder auf dem Server oder in der Datenbank selbst.
2. **Sekundäre Datenbank**: Die Blobüberwachung kann nur in den Überwachungseinstellungen der primären Datenbank aktiviert und deaktiviert werden.

   * Aktivieren Sie die Blobüberwachung in der primären Datenbank. Die Blobüberwachung muss in der *primären Datenbank selbst* aktiviert werden, nicht auf dem Server.
   * Nachdem die Blobüberwachung in der primären Datenbank aktiviert wurde, wird sie auch in der sekundären Datenbank aktiv.

    > [!IMPORTANT]
    > Standardmäßig sind die Speichereinstellungen für die sekundäre Datenbank identisch mit den Einstellungen der primären Datenbank, wodurch regionsübergreifender Datenverkehr generiert wird. Sie können dies verhindern, indem Sie die Blobüberwachung auf dem sekundären Server aktivieren und in den Speichereinstellungen des sekundären Servers einen lokalen Speicherort konfigurieren. Dadurch wird der Speicherort für die sekundäre Datenbank überschrieben, und jede Datenbank speichert die Überwachungsprotokolle an einem lokalen Speicherort.  

## <a name="viewing-blob-auditing-logs"></a>Anzeigen von Blobüberwachungsprotokollen

Protokolle der Blobüberwachung werden als Sammlung von Blobdateien in einem Container namens **sqldbauditlogs** gespeichert.

Weitere Informationen zur Hierarchie der Speicherordner für Blobüberwachungsprotokolle, zur Benennungskonvention für Blobs sowie zum Protokollformat finden Sie in der [Formatreferenz für Blobüberwachungsprotokolle](https://go.microsoft.com/fwlink/?linkid=829599) (DOC-Datei zum Herunterladen).

Es gibt verschiedene Methoden zum Anzeigen von Blobüberwachungsprotokollen:

* Über das [Azure-Portal](https://portal.azure.com): Öffnen Sie die entsprechende Datenbank. Klicken Sie oben auf dem Blatt „Überwachung und Bedrohungserkennung“ der Datenbank auf **Überwachungsprotokolle anzeigen**.

    ![Navigationsbereich][10]

    Das Blatt „Überwachungsdatensätze“ wird geöffnet, auf dem Sie die Protokolle anzeigen können.

    - Sie können Protokolle bestimmter Daten anzeigen, indem Sie im oberen Bereich des Blatts mit den Überwachungsdatensätzen auf **Filter** klicken.
    - Sie können zwischen Überwachungsdatensätzen umschalten, die anhand der Server- oder der Datenbankrichtlinienüberwachung erstellt wurden.

    ![Navigationsbereich][11]
* Laden Sie Protokolldateien aus Ihrem Azure-Speicherblobcontainer über das Portal oder mithilfe eines Tools wie [Azure-Speicher-Explorer](http://storageexplorer.com/) herunter.

    Nachdem Sie die Protokolldatei lokal heruntergeladen haben, können Sie auf die Datei doppelklicken, um die Protokolle in SSMS zu öffnen, anzuzeigen und zu analysieren.

* Weitere Methoden:

   * Über Azure-Speicher-Explorer können Sie mehrere Dateien gleichzeitig herunterladen. Klicken Sie mit der rechten Maustaste auf einen bestimmten Unterordner (einen Unterordner, der alle Protokolldateien für ein bestimmtes Datum enthält), und wählen Sie die Option **Speicher unter**, um die Dateien in einem lokalen Ordner zu speichern.

       Nach dem Herunterladen mehrerer Dateien (oder der Dateien für einen ganzen Tag, wie oben beschrieben) können Sie diese lokal zusammenführen:

       **SSMS öffnen -> Datei -> Öffnen -> Zusammenführen -> Erweiterte Ereignisse -> Alle Dateien für die Zusammenführung auswählen**
   * Programmgesteuert:

     * [Leser für erweiterte Ereignisse C#-Bibliothek](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/)
     * [Abfragen von Dateien mit erweiterten Ereignissen mithilfe von PowerShell](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/)

   * Wir haben eine [Beispielanwendung](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) erstellt, die in Azure ausgeführt wird und öffentliche OMS-APIs verwendet, um SQL-Überwachungsprotokolle für die Nutzung über das OMS-Dashboard per Push an OMS zu übertragen.

## <a name="viewing-table-audit-logs"></a>Anzeigen von Tabellenüberwachungsprotokollen

Protokolle der Tabellenüberwachung werden als Sammlung von Azure-Speichertabellen mit dem Präfix **SQLDBAuditLogs** gespeichert.

Weitere Informationen zum Format der Protokolle für die Tabellenüberwachung finden Sie unter [Formatreferenz für Tabellenüberwachungsprotokolle (DOC-Datei zum Herunterladen)](http://go.microsoft.com/fwlink/?LinkId=506733).

Es gibt verschiedene Methoden zum Anzeigen von Tabellenüberwachungsprotokollen:

* Über das [Azure-Portal](https://portal.azure.com): Öffnen Sie die entsprechende Datenbank. Klicken Sie oben auf dem Blatt „Überwachung und Bedrohungserkennung“ der Datenbank auf **Überwachungsprotokolle anzeigen**.

    ![Navigationsbereich][10]

    Das Blatt „Überwachungsdatensätze“ wird geöffnet, auf dem Sie die Protokolle anzeigen können.

    * Sie können Protokolle bestimmter Daten anzeigen, indem Sie im oberen Bereich des Blatts mit den Überwachungsdatensätzen auf **Filter** klicken.
    * Sie können die Überwachungsprotokolle im Excel-Format herunterladen und anzeigen, indem Sie im oberen Bereich des Blatts mit den Überwachungsdatensätzen auf **In Excel öffnen** klicken.

    ![Navigationsbereich][12]

* Alternativ dazu steht eine vorkonfigurierte Berichtvorlage als [herunterladbares Excel-Arbeitsblatt](http://go.microsoft.com/fwlink/?LinkId=403540) zur Verfügung, mit dem Sie Protokolldaten schnell analysieren können. Damit Sie die Vorlage für Ihre Überwachungsprotokolle verwenden können, benötigen Sie Excel 2013 oder höher und Power Query (http://www.microsoft.com/download/details.aspx?id=39379).

* Sie können Ihre Überwachungsprotokolle auch mithilfe von Power Query direkt aus dem Azure-Speicherkonto in die Excel-Vorlage importieren. Anschließend können Sie Ihre Überwachungsdatensätze untersuchen und zusätzlich zu den Protokolldaten Dashboards und Berichte erstellen.

    ![Navigationsbereich][9]

## <a name="storage-key-regeneration"></a>Erneute Speicherschlüsselgenerierung
In einer Produktionsumgebung werden Sie Ihre Speicherschlüssel wahrscheinlich regelmäßig aktualisieren. Wenn Sie die Schlüssel aktualisieren, müssen Sie die Überwachungsrichtlinie neu speichern. Dieser Prozess verläuft wie folgt:

1. Ändern Sie auf dem Blatt mit den Speicherdetails den **Speicherzugriffsschlüssel** von *Primär* zu *Sekundär*, und klicken Sie dann unten auf **OK**. Klicken Sie anschließend oben auf dem Blatt für die Überwachungskonfiguration auf **SPEICHERN**.

    ![Navigationsbereich][6]
2. Wechseln Sie zum Blatt für die Speicherkonfiguration, und **generieren Sie erneut** den *primären Zugriffsschlüssel*.

    ![Navigationsbereich][8]
3. Wechseln Sie zurück zum Blatt für die Überwachungskonfiguration, ändern Sie den **Speicherzugriffsschlüssel** von *Sekundär* zu *Primär*, und klicken Sie dann unten auf **OK**. Klicken Sie anschließend oben auf dem Blatt für die Überwachungskonfiguration auf **SPEICHERN**.
4. Wechseln Sie zurück zum Blatt für die Speicherkonfiguration, und **generieren Sie erneut** den *sekundären Zugriffsschlüssel* (als Vorbereitung auf den nächsten Schlüsselaktualisierungszyklus).


<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation (PowerShell / REST API)]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)  

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[3-tbl]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on_table.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_audited_events.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_storage_key_regeneration.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_activity_log.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_regenerate_key.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_report_template.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_blob_view_audit_logs.png
[11]: ./media/sql-database-auditing-get-started/11_auditing_get_started_blob_audit_records.png
[12]: ./media/sql-database-auditing-get-started/12_auditing_get_started_table_audit_records.png

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe von PowerShell finden Sie unter [Configure database auditing using PowerShell](sql-database-auditing-powershell.md) (Konfigurieren der Datenbanküberwachung mithilfe von PowerShell).
* Eine Übersicht über die Überwachung finden Sie unter [Datenbanküberwachung](sql-database-auditing.md).


