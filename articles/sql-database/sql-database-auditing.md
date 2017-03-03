---
title: "Azure SQL-Datenbank – Überwachung | Microsoft Docs"
description: "Bei der Überwachung von Azure SQL-Datenbank werden Datenbankereignisse nachverfolgt und in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto geschrieben."
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
ms.sourcegitcommit: 5d51a5ef3387b4c00079547b0f44ffe1f96bd77c
ms.openlocfilehash: 8c9da10e6ebc67c464bf7b5e569cb2113a082d0c
ms.lasthandoff: 02/17/2017


---
# <a name="sql-database-auditing-concepts"></a>Konzepte der Überwachung von SQL-Datenbank
Bei der Überwachung von Azure SQL-Datenbank werden Datenbankereignisse nachverfolgt und in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto geschrieben.

Die Überwachung kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.

Die Überwachung ermöglicht und unterstützt die Einhaltung von Standards, garantiert diese aber nicht. Weitere Informationen zu Azure-Programmen, die die Einhaltung von Standards unterstützen, finden Sie im [Microsoft Azure-Vertrauenscenter](https://azure.microsoft.com/support/trust-center/compliance/).

* [Übersicht über die Überwachung von Azure SQL-Datenbank]
* [Einrichten der Überwachung für Ihre Datenbank]
* [Analysieren von Überwachungsprotokollen und -berichten]

## <a name="a-idsubheading-1aazure-sql-database-auditing-overview"></a><a id="subheading-1"></a>Übersicht über die Überwachung von Azure SQL-Datenbank
Die Überwachung von SQL-Datenbank bietet folgende Möglichkeiten:

* **Beibehalten** eines Überwachungspfads von ausgewählten Ereignissen. Sie können Kategorien für zu protokollierende Datenbankaktionen und -ereignisse konfigurieren.
* **Melden** von Datenbankaktivitäten. Sie können vorkonfigurierte Berichte und ein Dashboard verwenden, um schnell mit der Berichterstattung über Aktivitäten und Ereignisse zu beginnen.
* **Analysieren** von Berichten. Sie können nach verdächtigen Ereignissen, ungewöhnliche Aktivitäten und Trends suchen.

Es gibt zwei **Überwachungsmethoden**:

* **Blobüberwachung:** Protokolle werden in Azure Blob Storage geschrieben. Dies ist eine neuere Überwachungsmethode, die eine höhere Leistung bietet, eine detailliertere Überwachung auf Objektebene unterstützt und kostengünstiger ist.
* **Tabellenüberwachung:** Protokolle werden in Azure Table Storage geschrieben.

> [!IMPORTANT]
> Die Einführung der neuen Blobüberwachung bedeutet eine wichtige Änderung bei der Vererbung der Serverüberwachungsrichtlinie durch die Datenbank. 

Sie können die Überwachung für verschiedene Arten von Ereigniskategorien konfigurieren.

* Informationen zum Konfigurieren und Verwalten der Überwachung über das Azure-Portal finden Sie unter [Überwachung über das Azure-Portal](sql-database-auditing-portal.md).
* Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe von PowerShell finden Sie unter [Überwachung mithilfe von PowerShell](sql-database-auditing-powershell.md).
* Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe der REST-API finden Sie unter [Überwachung mithilfe der REST-API](sql-database-auditing-rest.md).

<!--For each Event Category, auditing of **Success** and **Failure** operations are configured separately.-->

Eine Überwachungsrichtlinie kann für eine spezifische Datenbank oder als Standardserverrichtlinie definiert werden. Eine Standardrichtlinie für die Serverüberwachung wird auf alle vorhandenen und neu erstellten Datenbanken auf einem Server angewendet.

## <a name="blob-auditing"></a>Blobüberwachung

Wenn die Serverblobüberwachung aktiviert ist, gilt sie immer für die Datenbank (d.h., alle Datenbanken auf dem Server werden überwacht), unabhängig von:
    - Den Datenbanküberwachungseinstellungen
    - Der Aktivierung des Kontrollkästchens „Einstellungen vom Server erben“ auf dem Datenbankblatt

> [!IMPORTANT]
> Der Aktivierung der Blobüberwachung in der Datenbank sowie auf dem Server. Diese hebt die Einstellungen der Serverblobüberwachung **nicht** auf und ändert sie nicht. Beide Überwachungen existieren nebeneinander. Das heißt, die Datenbank wird zweimal parallel überwacht (einmal anhand der Serverrichtlinie und einmal anhand der Datenbankrichtlinie).

Aktivieren Sie die Serverblobüberwachung und die Datenbankblobüberwachung nicht zusammen, es sei denn:
 * Sie müssen ein anderes *Speicherkonto* oder eine andere *Vermerkdauer* für eine bestimmte Datenbank verwenden.
 * Sie möchten andere Ereignistypen oder Kategorien für eine bestimmte Datenbank überwachen als die, die gerade für den Rest der Datenbanken auf diesem Server überwacht werden (z.B. wenn Tabelleneinfügungen nur für eine bestimmte Datenbank überwacht werden müssen).

Andernfalls wird empfohlen, die Blobüberwachung nur auf Serverebene zu aktivieren und die Überwachung auf Datenbankebene für alle Datenbanken deaktiviert zu lassen.

## <a name="table-auditing"></a>Tabellenüberwachung

Wenn die Tabellenüberwachung auf Serverebene aktiviert ist, gilt diese nur für die Datenbank, wenn auf dem Datenbankblatt das Kontrollkästchen „Einstellungen vom Server erben“ aktiviert ist. (Dies ist für alle vorhandenen und neu erstellten Datenbanken standardmäßig aktiviert.)

- Ist das Kontrollkästchen *aktiviert*, **heben sämtliche Änderungen an den Überwachungseinstellungen in der Datenbank die Servereinstellungen für diese Datenbank auf**.

- Sind das Kontrollkästchen und die Überwachung auf Datenbankebene *deaktiviert*, wird die Datenbank nicht überwacht.

## <a name="analyze-audit-logs-and-reports"></a>Analysieren von Überwachungsprotokollen und -berichten
Überwachungsprotokolle werden in dem Azure-Speicherkonto aggregiert, das Sie während der Einrichtung ausgewählt haben.

Sie können Überwachungsprotokolle mithilfe eines Tools wie [Azure-Speicher-Explorer](http://storageexplorer.com/) untersuchen.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Konfigurieren und Verwalten der Überwachung über das Azure-Portal finden Sie unter [Konfigurieren der Überwachung im Azure-Portal](sql-database-auditing-portal.md).
* Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe von PowerShell finden Sie unter [Konfigurieren der Überwachung mithilfe von PowerShell](sql-database-auditing-powershell.md).
* Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe der REST-API finden Sie unter [Konfigurieren der Überwachung mithilfe der REST-API](sql-database-auditing-rest.md).


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

