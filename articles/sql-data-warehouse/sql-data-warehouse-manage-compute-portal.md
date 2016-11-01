<properties
   pageTitle="Verwalten von Computeleistung in Azure SQL Data Warehouse (Azure-Portal) | Microsoft Azure"
   description="Azure-Portal-Aufgaben zum Verwalten von Computeleistung. Skalieren von Computeressourcen durch Anpassen der DWUs Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# Verwalten von Computeleistung in Azure SQL Data Warehouse (Azure-Portal)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Passen Sie die Leistung durch horizontales Hochskalieren der Computeressourcen und des Arbeitsspeichers an, um die wechselhaften Anforderungen Ihres Workloads zu erfüllen. Sparen Sie außerhalb von Spitzenzeiten Kosten, indem Sie die zur Verfügung stehenden Ressourcen verringern oder den Computevorgang vollständig pausieren.

Diese Sammlung von Tasks verwendet das Azure-Portal für folgende Aufgaben:

- Skalieren von Computeressourcen
- Anhalten von Computeressourcen
- Fortsetzen von Computeressourcen

Weitere Informationen finden Sie unter [Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht)][].

<a name="scale-performance-bk"></a> <a name="scale-compute-bk"></a>

## Skalieren von Computeleistung

[AZURE.INCLUDE [Beschreibung des Skalierens der DWUs in SQL Data Warehouse](../../includes/sql-data-warehouse-scale-dwus-description.md)]

So ändern Sie Computeressourcen

1. Öffnen Sie das [Azure-Portal][], öffnen Sie Ihre Datenbank, und klicken Sie auf **Skalieren**.

    ![Klicken Sie auf Skalieren.][1]

1. Verschieben Sie auf dem Blatt „Skalieren“ den Schieberegler nach links oder rechts, um die DWU-Einstellung zu ändern.

    ![Schieberegler bewegen][2]

1. Klicken Sie auf **Speichern**. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

    ![Klicken Sie auf Speichern.][3]

<a name="pause-compute-bk"></a>

## Anhalten von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Anhaltens in SQL Data Warehouse](../../includes/sql-data-warehouse-pause-description.md)]

So halten Sie eine Datenbank an

1. Öffnen Sie das [Azure-Portal][], und öffnen Sie Ihre Datenbank. Beachten Sie, dass der Status **Online** lautet.

    ![Onlinestatus][6]

1. Klicken Sie auf **Anhalten**, um Compute- und Speicherressourcen anzuhalten. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

    ![Anhalten bestätigen][7]

1. Während die Datenbank von SQL Data Warehouse gestartet wird, lautet der Status **Wird angehalten**.
2. Wenn der Status **Angehalten** lautet, ist der Anhaltevorgang abgeschlossen, und Ihnen werden keine DWUs mehr in Rechnung gestellt.

    ![Anhaltestatus][4]

<a name="resume-compute-bk"></a>

## Fortsetzen von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Fortsetzens in SQL Data Warehouse](../../includes/sql-data-warehouse-resume-description.md)] So setzen Sie eine Datenbank fort

1. Öffnen Sie das [Azure-Portal][], und öffnen Sie Ihre Datenbank. Beachten Sie, dass der Status **Angehalten** lautet.

    ![Datenbank anhalten][4]

1. Klicken Sie zum Fortsetzen der Datenbank auf **Starten**. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

    ![Fortsetzen bestätigen][5]

1. Während die Datenbank von SQL Data Warehouse gestartet wird, lautet der Status „Wird fortgesetzt“.
2. Wenn der Status **Online** lautet, ist die Datenbank bereit.

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## Nächste Schritte
Weitere Informationen finden Sie unter [Verwaltungsübersicht][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Verwaltungsübersicht]: ./sql-data-warehouse-overview-manage.md
[Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht)]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure-Portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0824_2016-->