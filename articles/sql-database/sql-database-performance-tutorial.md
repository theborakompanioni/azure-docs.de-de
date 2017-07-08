---
title: Behandeln von Leistungsproblemen und Optimieren der Datenbank | Microsoft-Dokumentation
description: "Enthält Empfehlungen zur Leistung Ihrer SQL-Datenbank und eine Beschreibung dazu, wie Sie Einblicke in die Leistung der Abfragen erhalten, die für Ihre Datenbank ausgeführt werden."
metakeywords: azure sql database performance monitoring recommendation
services: sql-database
documentationcenter: 
manager: jhubbard
author: jan-eng
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,monitor & tune
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: janeng
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: f9ae96cdc80c347593f229cb2fce3f2d4d8e7caf
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a>Behandeln von Leistungsproblemen und Optimieren der Datenbank

Fehlende Indizes und falsch optimierte Abfragen sind häufige Ursachen für eine schlechte Datenbankleistung. In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Prüfen, Anwenden und Zurücksetzen von Empfehlungen zur Leistungsverbesserung
> * Suchen nach Abfragen mit starker Ressourcennutzung
> * Suchen nach Abfragen mit langer Laufzeit

> Sie benötigen für eine Datenbank mit Leistungsproblemen – z.B. einem fehlenden Index – eine kontinuierliche Workload, um eine Empfehlung zu erhalten.
>

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="review-and-apply-a-recommendation"></a>Prüfen und Anwenden einer Empfehlung

Führen Sie diese Schritte aus, um für Ihre Datenbank eine Empfehlung des Systems anzuwenden:

1. Klicken Sie auf dem Blatt „Datenbank“ auf das Menü **Leistungsempfehlungen**.

    ![Leistungsempfehlung](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. Wählen Sie in der Liste mit den Empfehlungen eine aktive Empfehlung aus. In diesem Beispiel ist dies CREATE INDEX.

    ![Auswählen einer Empfehlung](./media/sql-database-performance-tutorial/create_index.png)

3. Wenden Sie die Empfehlung an, indem Sie auf die Schaltfläche **Übernehmen** klicken. Optional können Sie die Details zur Empfehlung und das auszuführende T-SQL-Skript anzeigen, indem Sie auf die Schaltfläche **Skript anzeigen** klicken.

    ![Anwenden einer Empfehlung](./media/sql-database-performance-tutorial/apply.png)

4. [Optional] Aktivieren Sie die Funktion, bei der die automatische Optimierung für Empfehlungen automatisch angewendet wird.

    ![Automatische Optimierung](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a>Zurücksetzen einer Empfehlung

Der Database Advisor überwacht jede implementierte Empfehlung. Wenn eine Empfehlung für die Workload nicht zu einer Verbesserung führt, wird sie automatisch zurückgesetzt. Das manuelle Zurücksetzen einer Empfehlung ist möglich, aber in den meisten Fällen nicht erforderlich. Gehen Sie wie folgt vor, um eine Empfehlung zurückzusetzen:

1. Navigieren Sie zum Menü mit den Leistungsempfehlungen, und wählen Sie eine der angewendeten Empfehlungen aus.

    ![Auswählen einer Empfehlung](./media/sql-database-performance-tutorial/select.png)

2. Klicken Sie in der Detailansicht auf **Zurücksetzen**.

    ![Zurücksetzen der Empfehlung](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-the-query-that-consumes-the-most-resources"></a>Suchen nach der Abfrage, die die meisten Ressourcen verbraucht

Führen Sie diese Schritte aus, um die Abfrage zu ermitteln, die die meisten Ressourcen verbraucht:

1. Klicken Sie auf dem Blatt „Datenbank“ auf das Menü **Query Performance Insight**.

    ![Query Insights](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. Wählen Sie einen Ressourcentyp aus.

    ![Query Insights](./media/sql-database-performance-tutorial/select_resource_type.png)

3. Wählen Sie die erste Abfrage in der Tabelle.

    ![Query Insights](./media/sql-database-performance-tutorial/select_query.png)

4. Prüfen Sie die Details der Abfrage.

    ![Query Insights](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-the-longest-running-query"></a>Suchen nach der Abfrage mit der längsten Ausführungsdauer

1. Navigieren Sie zu Query Performance Insight, und wählen Sie die Registerkarte **Abfragen mit langer Ausführungszeit**.

    ![Query Insights](./media/sql-database-performance-tutorial/long_running.png)

3. Wählen Sie die erste Abfrage in der Tabelle.

    ![Query Insights](./media/sql-database-performance-tutorial/select_first_query.png)

4. Prüfen Sie die Details der Abfrage.

    ![Query Insights](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a>Nächste Schritte 
Fehlende Indizes und falsch optimierte Abfragen sind häufige Ursachen für eine schlechte Datenbankleistung. In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Prüfen, Anwenden und Zurücksetzen von Empfehlungen zur Leistungsverbesserung
> * Suchen nach Abfragen mit starker Ressourcennutzung
> * Suchen nach Abfragen mit langer Laufzeit

[Tipps zur Optimierung der SQL-Datenbankleistung](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)

