---
title: "Erste Schritte bei der Überwachung von Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Erste Schritte bei der Überwachung von SQL-Datenbank"
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
ms.date: 10/05/2016
ms.author: ronitr; giladm
translationtype: Human Translation
ms.sourcegitcommit: 506fd7b5cbf11dd4d9348962db5e06077953bd51
ms.openlocfilehash: 185c6aaa2db0820421c8ea3523865bc90a1475f7
ms.lasthandoff: 02/28/2017


---
# <a name="get-started-with-sql-database-auditing"></a>Erste Schritte bei der Überwachung von SQL-Datenbank
Die SQL-Datenbank-Überprüfung in Azure verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in Ihrem Azure-Speicherkonto.

Die Überwachung kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.

Die Überwachung ermöglicht und unterstützt die Einhaltung von Standards, garantiert diese aber nicht. Weitere Informationen zu Azure-Programmen, die die Einhaltung von Standards unterstützen, finden Sie im [Microsoft Azure-Vertrauenscenter](https://azure.microsoft.com/support/trust-center/compliance/).

* [Übersicht über die Überwachung von Azure SQL-Datenbank]
* [Einrichten der Überwachung für Ihre Datenbank]
* [Analysieren von Überwachungsprotokollen und -berichten]

## <a id="subheading-1"></a>Übersicht über die Überwachung von Azure SQL-Datenbank
Die Überwachung von SQL-Datenbank bietet folgende Möglichkeiten:

* **Beibehalten** eines Überwachungspfads von ausgewählten Ereignissen. Sie können Kategorien für zu protokollierende Datenbankaktionen und -ereignisse konfigurieren.
* **Melden** von Datenbankaktivitäten. Sie können vorkonfigurierte Berichte und ein Dashboard verwenden, um schnell mit der Berichterstattung über Aktivitäten und Ereignisse zu beginnen.
* **Analysieren** von Berichten. Sie können nach verdächtigen Ereignissen, ungewöhnliche Aktivitäten und Trends suchen.

Es gibt zwei **Überwachungsmethoden**:

* **Blobüberwachung**: Protokolle werden in Azure Blob Storage geschrieben. Dies ist eine neuere Überwachungsmethode, die eine **höhere Leistung** bietet, eine **detailliertere Überwachung auf Objektebene** unterstützt und **kostengünstiger** ist.
* **Tabellenüberwachung**: Protokolle werden in Azure Table Storage geschrieben.

> [!IMPORTANT]
> Die Einführung der neuen Blobüberwachung bedeutet eine wichtige Änderung bei der Vererbung der Serverüberwachungsrichtlinie von der Datenbank. Weitere Details finden Sie im Abschnitt [Unterschiede für Blobs/Tabellen bei der Vererbung der Serverüberwachungsrichtlinie](#subheading-8).

Sie können die Überwachung für verschiedene Arten von Ereigniskategorien konfigurieren, wie im Abschnitt [Einrichten der Überwachung für Ihre Datenbank](#subheading-2) erläutert.

<!--For each Event Category, auditing of **Success** and **Failure** operations are configured separately.-->

Eine Überwachungsrichtlinie kann für eine spezifische Datenbank oder als Standardserverrichtlinie definiert werden. Eine Standardrichtlinie für die Serverüberwachung wird auf alle vorhandenen und neu erstellten Datenbanken auf einem Server angewendet.

## <a id="subheading-2"></a>Einrichten der Überwachung für Ihre Datenbank
Der folgende Abschnitt beschreibt die Konfiguration der Überwachung über das Azure-Portal.

### <a id="subheading-2-1">Blobüberwachung</a>
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) unter https://portal.azure.com.
2. Navigieren Sie zum Blatt mit den Einstellungen der SQL-Datenbank-/SQL Server-Instanz, die Sie überwachen möchten. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung**.

    <a id="auditing-screenshot"></a>
    ![Navigationsbereich][1]
3. Auf dem Blatt für die Konfiguration der Datenbanküberwachung können Sie das Kontrollkästchen **Einstellungen vom Server erben** aktivieren, um anzugeben, dass diese Datenbank gemäß den Einstellungen des zugehörigen Servers überwacht wird. Wenn Sie diese Option aktivieren, wird der Link **Server-Überwachungseinstellungen anzeigen** angezeigt, den Sie verwenden können, um die Überwachungseinstellungen des Servers für diesen Kontext anzuzeigen oder zu ändern.

    ![Navigationsbereich][2]
4. Wenn Sie die Blobüberwachung (zusätzlich zur oder anstelle der Überwachung auf Serverebene) auf Datenbankebene aktivieren möchten, **deaktivieren** Sie die Option **Überwachungseinstellungen vom Server erben**, **aktivieren** Sie die Überwachung, und wählen Sie den Überwachungstyp **Blob** aus.

   > Wenn die Serverblobüberwachung aktiviert ist, existiert die konfigurierte Datenbanküberwachung parallel zur Serverblobüberwachung.  

    ![Navigationsbereich][3]
5. Wählen Sie **Speicherdetails** , um das Blatt „Speicherung von Überwachungsprotokollen“ zu öffnen. Wählen Sie das Azure-Speicherkonto aus, in dem die Protokolle gespeichert werden sollen, sowie die Aufbewahrungsdauer, nach der die alten Protokolle gelöscht werden. Klicken Sie anschließend unten auf **OK**. **Tipp:** Verwenden Sie für alle überwachten Datenbanken dasselbe Speicherkonto, um die Berichtvorlagen für die Überwachung optimal einzusetzen.

    <a id="storage-screenshot"></a>
    ![Navigationsbereich][4]
6. Wenn Sie die überwachten Ereignisse anpassen möchten, können Sie dies über PowerShell oder die REST-API tun. Weitere Informationen finden Sie im Abschnitt [Automatisierung (PowerShell / REST-API)](#subheading-7).
7. Klicken Sie auf **Speichern**.

### <a id="subheading-2-2">Tabellenüberwachung</a>

> Überprüfen Sie vor der Einrichtung der **Tabellenüberwachung**, ob Sie einen [„kompatiblen Client“](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) verwenden. Falls Sie strikte Firewalleinstellungen haben, beachten Sie außerdem, dass der [IP-Endpunkt Ihrer Datenbank geändert wird](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md), wenn Sie die Tabellenüberwachung aktivieren.


1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) unter https://portal.azure.com.
2. Navigieren Sie zum Blatt mit den Einstellungen der SQL-Datenbank-/SQL Server-Instanz, die Sie überwachen möchten. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung** (*[siehe Screenshot im Abschnitt „Blobüberwachung“](#auditing-screenshot)*).
3. Auf dem Blatt für die Konfiguration der Datenbanküberwachung können Sie das Kontrollkästchen **Einstellungen vom Server erben** aktivieren, um anzugeben, dass diese Datenbank gemäß den Einstellungen des zugehörigen Servers überwacht wird. Wenn Sie diese Option aktivieren, wird der Link **Server-Überwachungseinstellungen anzeigen** angezeigt, den Sie verwenden können, um die Überwachungseinstellungen des Servers für diesen Kontext anzuzeigen oder zu ändern.

    ![Navigationsbereich][2]
4. Wenn die Überwachungseinstellungen nicht vom Server geerbt werden sollen, **deaktivieren** Sie die Option **Überwachungseinstellungen vom Server erben**, **aktivieren** Sie die Überwachung, und wählen Sie den Überwachungstyp **Tabelle** aus.

    ![Navigationsbereich][3-tbl]
5. Wählen Sie **Speicherdetails** , um das Blatt „Speicherung von Überwachungsprotokollen“ zu öffnen. Wählen Sie das Azure-Speicherkonto aus, in dem die Protokolle gespeichert werden sollen, sowie die Aufbewahrungsdauer, nach der die alten Protokolle gelöscht werden. **Tipp:** Verwenden Sie für alle überwachten Datenbanken dasselbe Speicherkonto, um die Berichtvorlagen für die Überwachung optimal einzusetzen (*[siehe Screenshot im Abschnitt „Blobüberwachung“](#storage-screenshot)*).
6. Klicken Sie auf **Überwachte Ereignisse** , um anzupassen, welche Ereignisse überwacht werden sollen. Klicken Sie auf dem Blatt **Protokollierung nach Ereignis** auf **Erfolg** und **Fehler**, um alle Ereignisse zu protokollieren oder einzelne Ereigniskategorien auszuwählen.

   > Die Anpassung überwachter Ereignisse kann auch über PowerShell oder die REST-API erfolgen. Weitere Informationen finden Sie im Abschnitt [Automatisierung (PowerShell / REST-API)](#subheading-7).

    ![Navigationsbereich][5]
7. Nachdem Sie Ihre Überwachungseinstellungen konfiguriert haben, können Sie das neue Feature **Bedrohungserkennung** (Vorschau) aktivieren und die E-Mail-Konten konfigurieren, an die Sicherheitswarnungen gesendet werden sollen. Mit der Bedrohungserkennung können Sie proaktive Warnungen bei anomalen Datenbankaktivitäten erhalten, die auf mögliche Sicherheitsbedrohungen hinweisen können. Weitere Informationen finden Sie unter [Erste Schritte mit der Bedrohungserkennung](sql-database-threat-detection-get-started.md).
8. Klicken Sie auf **Speichern**.


## <a id="subheading-8"></a>Unterschiede für Blobs/Tabellen bei der Vererbung der Serverüberwachungsrichtlinie

###<a name="ablob-auditinga"></a><a>Blobüberwachung</a>

1. Wenn **die Serverblobüberwachung aktiviert ist**, **gilt diese immer für die Datenbank** (d.h. die Datenbank wird überwacht), unabhängig von:
    - Den Datenbanküberwachungseinstellungen
    - Der Aktivierung des Kontrollkästchens „Einstellungen vom Server erben“ auf dem Datenbankblatt

2. Der Aktivierung der Blobüberwachung in der Datenbank sowie auf dem Server. Hebt die Einstellungen der Serverblobüberwachung **nicht** auf und ändert sie nicht. Beide Überwachungen existieren nebeneinander. Das heißt, die Datenbank wird zweimal parallel überwacht (einmal anhand der Serverrichtlinie und einmal anhand der Datenbankrichtlinie).

    > [!NOTE]
    > Aktivieren Sie die Serverblobüberwachung und die Datenbankblobüberwachung nicht zusammen, es sei denn:
    > * Sie müssen ein anderes *Speicherkonto* oder eine andere *Vermerkdauer* für eine bestimmte Datenbank verwenden.
    > * Sie möchten andere Ereignistypen oder Kategorien für eine bestimmte Datenbank überwachen als die, die gerade für den Rest der Datenbanken auf diesem Server überwacht werden (z.B. wenn Tabelleneinfügungen nur für eine bestimmte Datenbank überwacht werden müssen).
    > <br><br>
    > Andernfalls **wird empfohlen, die Blobüberwachung nur auf Serverebene zu aktivieren** und die Überwachung auf Datenbankebene für alle Datenbanken deaktiviert zu lassen.

###<a name="atable-auditinga"></a><a>Tabellenüberwachung</a>

Wenn **die Tabellenüberwachung auf Serverebene aktiviert ist**, gilt diese nur für die Datenbank, wenn auf dem Datenbankblatt das Kontrollkästchen „Einstellungen vom Server erben“ aktiviert ist (Dies ist für alle vorhandenen und neu erstellten Datenbanken standardmäßig aktiviert).

- Ist das Kontrollkästchen *aktiviert*, **heben sämtliche Änderungen an den Überwachungseinstellungen in der Datenbank die Servereinstellungen für diese Datenbank auf**.

- Sind das Kontrollkästchen und die Überwachung auf Datenbankebene *deaktiviert*, wird die Datenbank nicht überwacht.

## <a id="subheading-3"></a>Analysieren von Überwachungsprotokollen und -berichten
Überwachungsprotokolle werden in dem Azure-Speicherkonto aggregiert, das Sie während der Einrichtung ausgewählt haben.

Sie können Überwachungsprotokolle mithilfe eines Tools wie [Azure-Speicher-Explorer](http://storageexplorer.com/) untersuchen.

Einzelheiten zur Analyse für die Überwachungsprotokolle der Typen **Blob** und **Tabelle** finden Sie unten.

### <a id="subheading-3-1">Blobüberwachung</a>
Protokolle der Blobüberwachung werden als Sammlung von Blobdateien in einem Container namens **sqldbauditlogs** gespeichert.

Weitere Informationen zur Hierarchie der Speicherordner für Blobüberwachungsprotokolle, zur Benennungskonvention für Blobs sowie zum Protokollformat finden Sie in der [Formatreferenz für Blobüberwachungsprotokolle](https://go.microsoft.com/fwlink/?linkid=829599) (DOC-Datei zum Herunterladen).

Es gibt verschiedene Methoden zum Anzeigen von Blobüberwachungsprotokollen:

1. Über das [Azure-Portal](https://portal.azure.com): Öffnen Sie die entsprechende Datenbank. Klicken Sie oben auf dem Blatt **Überwachung und Bedrohungserkennung** der Datenbank auf **Überwachungsprotokolle anzeigen**.

    ![Navigationsbereich][10]

    Das Blatt **Überwachungsdatensätze** wird geöffnet, auf dem Sie die Protokolle anzeigen können.

    - Sie können Protokolle bestimmter Daten anzeigen, indem Sie im oberen Bereich des Blatts mit den Überwachungsdatensätzen auf **Filter** klicken.
    - Sie können zwischen Überwachungsdatensätzen umschalten, die anhand der Server- oder der Datenbankrichtlinienüberwachung erstellt wurden.

    ![Navigationsbereich][11]
2. Laden Sie Protokolldateien aus Ihrem Azure Storage Blob-Container über das Portal oder mithilfe eines Tools wie [Azure-Speicher-Explorer](http://storageexplorer.com/) herunter.

    Nachdem Sie die Protokolldatei lokal heruntergeladen haben, können Sie auf die Datei doppelklicken, um die Protokolle in SSMS zu öffnen, anzuzeigen und zu analysieren.

    Weitere Methoden:

   * Über Azure-Speicher-Explorer können Sie **mehrere Dateien gleichzeitig herunterladen**. Klicken Sie mit der rechten Maustaste auf einen bestimmten Unterordner (z.B. einen Unterordner, der alle Protokolldateien für ein bestimmtes Datum enthält), und wählen Sie die Option „Speicher unter“, um die Dateien in einem lokalen Ordner zu speichern.

       Nach dem Herunterladen mehrerer Dateien (oder der Dateien für einen ganzen Tag, wie oben beschrieben) können Sie diese lokal zusammenführen:

       **SSMS öffnen -> Datei -> Öffnen -> Zusammenführen -> Erweiterte Ereignisse -> Alle Dateien für die Zusammenführung auswählen**
   * Programmgesteuert:

     * Leser für erweiterte Ereignisse **C#-Bibliothek** ([hier finden Sie weitere Informationen](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/))
     * Abfragen von Dateien mit erweiterten Ereignissen mithilfe von **PowerShell** ([hier finden Sie weitere Informationen](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/))

3. Wir haben eine **Beispielanwendung** erstellt, die in Azure ausgeführt wird und öffentliche OMS-APIs verwendet, um SQL-Überwachungsprotokolle für die Nutzung über das OMS-Dashboard per Push an OMS zu übertragen (Weitere Informationen finden Sie [hier](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration)).

### <a id="subheading-3-2">Tabellenüberwachung</a>
Protokolle der Tabellenüberwachung werden als Sammlung von Azure-Speichertabellen mit dem Präfix **SQLDBAuditLogs** gespeichert.

Weitere Informationen zum Format der Protokolle für die Tabellenüberwachung finden Sie unter [Formatreferenz für Tabellenüberwachungsprotokolle (DOC-Datei zum Herunterladen)](http://go.microsoft.com/fwlink/?LinkId=506733).

Es gibt verschiedene Methoden zum Anzeigen von Tabellenüberwachungsprotokollen:

1. Über das [Azure-Portal](https://portal.azure.com): Öffnen Sie die entsprechende Datenbank. Klicken Sie oben auf dem Blatt **Überwachung und Bedrohungserkennung** der Datenbank auf **Überwachungsprotokolle anzeigen**.

    ![Navigationsbereich][10]

    Das Blatt **Überwachungsdatensätze** wird geöffnet, auf dem Sie die Protokolle anzeigen können.

    * Sie können Protokolle bestimmter Daten anzeigen, indem Sie im oberen Bereich des Blatts mit den Überwachungsdatensätzen auf **Filter** klicken.
    * Sie können die Überwachungsprotokolle im Excel-Format herunterladen und anzeigen, indem Sie im oberen Bereich des Blatts mit den Überwachungsdatensätzen auf **In Excel öffnen** klicken.

    ![Navigationsbereich][12]

2. Alternativ dazu steht eine vorkonfigurierte Berichtvorlage als [herunterladbares Excel-Arbeitsblatt](http://go.microsoft.com/fwlink/?LinkId=403540) zur Verfügung, mit dem Sie Protokolldaten schnell analysieren können. Damit Sie die Vorlage für Ihre Überwachungsprotokolle verwenden können, benötigen Sie Excel 2013 oder höher und Power Query, das [hier](http://www.microsoft.com/download/details.aspx?id=39379)heruntergeladen werden kann.

    Sie können Ihre Überwachungsprotokolle auch mithilfe von Power Query direkt aus dem Azure-Speicherkonto in die Excel-Vorlage importieren. Anschließend können Sie Ihre Überwachungsdatensätze untersuchen und zusätzlich zu den Protokolldaten Dashboards und Berichte erstellen.

    ![Navigationsbereich][9]

## <a id="subheading-5"></a>Methoden zur Verwendung in der Produktion
<!--The description in this section refers to screen captures above.-->

### <a id="subheading-6">Überwachung von georeplizierten Datenbanken</a>
Wenn Sie georeplizierte Datenbanken verwenden, können Sie die Überwachung je nach Überwachungstyp für die primäre Datenbank, die sekundäre Datenbank oder für beide einrichten.

**Tabellenüberwachung**: Sie können für jede der beiden Datenbanken (primär und sekundär) eine separate Richtlinie auf Datenbank- oder Serverebene konfigurieren, wie im Abschnitt [Einrichten der Überwachung für Ihre Datenbank](#subheading-2-2) beschrieben.

**Blobüberwachung**: Befolgen Sie diese Anweisungen:

1. **Primäre Datenbank**: Aktivieren Sie die **Blobüberwachung** entweder auf dem Server oder in der Datenbank selbst, wie im Abschnitt [Einrichten der Überwachung für Ihre Datenbank](#subheading-2-1) beschrieben.
2. **Sekundäre Datenbank**: Die Blobüberwachung kann nur in den Überwachungseinstellungen der primären Datenbank aktiviert und deaktiviert werden.

   * Aktivieren Sie die **Blobüberwachung** in der **primären Datenbank**, wie im Abschnitt [Einrichten der Überwachung für Ihre Datenbank](#subheading-2-1) beschrieben. Die Blobüberwachung muss in der *primären Datenbank selbst* aktiviert werden, nicht auf dem Server.
   * Nachdem die Blobüberwachung in der primären Datenbank aktiviert wurde, wird sie auch in der sekundären Datenbank aktiv.

    > [!IMPORTANT]
    > Standardmäßig sind die **Speichereinstellungen** für die sekundäre Datenbank identisch mit den Einstellungen der primären Datenbank, wodurch **regionsübergreifender Datenverkehr** generiert wird. Sie können dies verhindern, indem Sie die Blobüberwachung auf dem **sekundären Server** aktivieren und in den Speichereinstellungen des sekundären Servers einen **lokalen Speicherort** konfigurieren. Dadurch wird der Speicherort für die sekundäre Datenbank überschrieben, und jede Datenbank speichert die Überwachungsprotokolle an einem lokalen Speicherort.  

<br>

### <a id="subheading-6">Erneute Speicherschlüsselgenerierung</a>
In einer Produktionsumgebung werden Sie Ihre Speicherschlüssel wahrscheinlich regelmäßig aktualisieren. Wenn Sie die Schlüssel aktualisieren, müssen Sie die Überwachungsrichtlinie neu speichern. Dieser Prozess verläuft wie folgt:

1. Ändern Sie auf dem Blatt mit den Speicherdetails den **Speicherzugriffsschlüssel** von *Primär* zu *Sekundär*, und klicken Sie dann unten auf **OK**. Klicken Sie anschließend oben auf dem Blatt für die Überwachungskonfiguration auf **SPEICHERN**.

    ![Navigationsbereich][6]
2. Wechseln Sie zum Blatt für die Speicherkonfiguration, und **generieren Sie erneut** den *primären Zugriffsschlüssel*.

    ![Navigationsbereich][8]
3. Wechseln Sie zurück zum Blatt für die Überwachungskonfiguration, ändern Sie den **Speicherzugriffsschlüssel** von *Sekundär* zu *Primär*, und klicken Sie dann unten auf **OK**. Klicken Sie anschließend oben auf dem Blatt für die Überwachungskonfiguration auf **SPEICHERN**.
4. Wechseln Sie zurück zum Blatt für die Speicherkonfiguration, und **generieren Sie erneut** den *sekundären Zugriffsschlüssel* (als Vorbereitung auf den nächsten Schlüsselaktualisierungszyklus).

## <a id="subheading-7"></a>Automatisierung (PowerShell / REST-API)
Sie können die Überwachung in Azure SQL-Datenbank auch mit den folgenden Automatisierungstools konfigurieren:

1. **PowerShell-Cmdlets**

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Remove-AzureRMSqlDatabaseAuditing][103]
   * [Remove-AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Use-AzureRMSqlServerAuditingPolicy][107]
2. **REST-API – Blobüberwachung**

   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Datenbankblobs](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Serverblobs](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Datenbankblobs](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Serverblobs](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [Abrufen des Ergebnisses eines Überwachungsvorgangs für Serverblobs](https://msdn.microsoft.com/library/azure/mt771862.aspx)
3. **REST-API – Tabellenüberwachung**

   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Datenbanken](https://msdn.microsoft.com/library/azure/mt604471.aspx)
   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Servern](https://msdn.microsoft.com/library/azure/mt604383.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Datenbanken](https://msdn.microsoft.com/library/azure/mt604381.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Servern](https://msdn.microsoft.com/library/azure/mt604382.aspx)

<!--Anchors-->
[Übersicht über die Überwachung von Azure SQL-Datenbank]: #subheading-1
[Einrichten der Überwachung für Ihre Datenbank]: #subheading-2
[Analysieren von Überwachungsprotokollen und -berichten]: #subheading-3
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

[101]: https://msdn.microsoft.com/library/azure/mt603731(v=azure.200).aspx
[102]: https://msdn.microsoft.com/library/azure/mt619329(v=azure.200).aspx
[103]: https://msdn.microsoft.com/library/azure/mt603796(v=azure.200).aspx
[104]: https://msdn.microsoft.com/library/azure/mt603574(v=azure.200).aspx
[105]: https://msdn.microsoft.com/library/azure/mt603531(v=azure.200).aspx
[106]: https://msdn.microsoft.com/library/azure/mt603794(v=azure.200).aspx
[107]: https://msdn.microsoft.com/library/azure/mt619353(v=azure.200).aspx
