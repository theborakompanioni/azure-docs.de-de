<properties
   pageTitle="Verwalten von Skalierbarkeitstasks für Azure SQL Data Warehouse (Azure-Portal) | Microsoft Azure"
   description="Tasks im Azure-Portal zum horizontalen Hochskalieren der Leistung für Azure SQL Data Warehouse. Ändern Sie Computeressourcen durch Anpassen der DWUs. Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen."
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
   ms.date="04/28/2016"
   ms.author="barbkess;sonyama"/>

# Verwalten von Skalierbarkeitstasks für Azure SQL Data Warehouse (Azure-Portal)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-scalability.md)
- [Portal](sql-data-warehouse-manage-scale-out-tasks.md)
- [PowerShell](sql-data-warehouse-manage-scale-out-tasks-powershell.md)
- [REST](sql-data-warehouse-manage-scale-out-tasks-rest-api.md)
- [TSQL](sql-data-warehouse-manage-scale-out-tasks-tsql.md)


Skalieren Sie Computeressourcen und Arbeitsspeicher elastisch horizontal hoch, um die sich ändernden Anforderungen Ihrer Workload zu erfüllen, und sparen Sie Kosten, indem Sie in Zeiten geringerer Auslastung Ressourcen zurück skalieren.

Diese Sammlung von Tasks verwendet das Azure-Portal für folgende Aufgaben:

- Skalieren der Leistung durch Anpassen von DWUs
- Anhalten von Computeressourcen
- Fortsetzen von Computeressourcen

Informationen hierzu finden Sie unter [Übersicht über die Skalierbarkeit der Leistung][].

<a name="scale-performance-bk"></a>

## Skalieren der Leistung

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

1. Um Compute- und Speicherressourcen anzuhalten, klicken Sie auf **Anhalten**. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

    ![Anhalten bestätigen][7]

1. Während die Datenbank von SQL Data Warehouse gestartet wird, lautet der Status „Wird angehalten“.
2. Wenn der Status **Angehalten** lautet, ist der Vorgang abgeschlossen, und Ihnen werden keine DWUs mehr in Rechnung gestellt.

    ![Anhaltestatus][4]

<a name="resume-compute-bk"></a>

## Fortsetzen von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Fortsetzens in SQL Data Warehouse](../../includes/sql-data-warehouse-resume-description.md)]So setzen Sie eine Datenbank fort

1. Öffnen Sie das [Azure-Portal][], und öffnen Sie Ihre Datenbank. Beachten Sie, dass der Status **Angehalten** lautet. 

    ![Datenbank anhalten][4]

1. Klicken Sie zum Fortsetzen der Datenbank auf **Starten**. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

    ![Fortsetzen bestätigen][5]

1. Während die Datenbank von SQL Data Warehouse gestartet wird, lautet der Status „Wird fortgesetzt“.
2. Wenn der Status **online** lautet, ist die Datenbank bereit.

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## Nächste Schritte
Weitere Informationen finden Sie unter [Verwaltungsübersicht][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-scale-out-tasks/click-scale.png
[2]: ./media/sql-data-warehouse-manage-scale-out-tasks/move-slider.png
[3]: ./media/sql-data-warehouse-manage-scale-out-tasks/click-save.png
[4]: ./media/sql-data-warehouse-manage-scale-out-tasks/resume-database.png
[5]: ./media/sql-data-warehouse-manage-scale-out-tasks/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-scale-out-tasks/pause-database.png
[7]: ./media/sql-data-warehouse-manage-scale-out-tasks/pause-confirm.png

<!--Article references-->
[Verwaltungsübersicht]: ./sql-data-warehouse-overview-manage.md
[Übersicht über die Skalierbarkeit der Leistung]: ./sql-data-warehouse-overview-scalability.md

<!--MSDN references-->


<!--Other Web references-->

[Azure-Portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0504_2016-->