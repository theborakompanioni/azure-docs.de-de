---
title: Verwalten von Computeleistung in Azure SQL Data Warehouse (Azure-Portal) | Microsoft Docs
description: "Azure-Portal-Aufgaben zum Verwalten von Computeleistung. Skalieren von Computeressourcen durch Anpassen der DWUs Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: f9814fc0011153a25489d60d696bb567edfcebc4
ms.openlocfilehash: a86b041424ce1757fd45c7a39350f4c4e66ee265


---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Verwalten von Computeleistung in Azure SQL Data Warehouse (Azure-Portal)
> [!div class="op_single_selector"]
> * [Übersicht](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a>Skalieren von Computeleistung
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

So ändern Sie Computeressourcen

1. Öffnen Sie das [Azure-Portal][Azure portal], öffnen Sie Ihre Datenbank, und klicken Sie auf **Skalieren**.

    ![Klicken Sie auf Skalieren.][1]
2. Verschieben Sie auf dem Blatt „Skalieren“ den Schieberegler nach links oder rechts, um die DWU-Einstellung zu ändern.

    ![Schieberegler bewegen][2]
3. Klicken Sie auf **Speichern**. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

    ![Klicken Sie auf Speichern.][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Anhalten von Computeressourcen
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

So halten Sie eine Datenbank an

1. Öffnen Sie das [Azure-Portal][Azure portal], und öffnen Sie Ihre Datenbank. Beachten Sie, dass der Status **Online**lautet.

    ![Onlinestatus][6]
2. Klicken Sie auf **Anhalten**, um Compute- und Arbeitsspeicherressourcen anzuhalten. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

    ![Anhalten bestätigen][7]
3. Während die Datenbank von SQL Data Warehouse gestartet wird, lautet der Status **Wird angehalten**.
4. Wenn der Status **Angehalten**lautet, ist der Anhaltevorgang abgeschlossen, und Ihnen werden keine DWUs mehr in Rechnung gestellt.

    ![Anhaltestatus][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Fortsetzen von Computeressourcen
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

So setzen Sie eine Datenbank fort

1. Öffnen Sie das [Azure-Portal][Azure portal], und öffnen Sie Ihre Datenbank. Beachten Sie, dass der Status **Angehalten**lautet.

    ![Datenbank anhalten][4]
2. Klicken Sie zum Fortsetzen der Datenbank auf **Starten**. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

    ![Fortsetzen bestätigen][5]
3. Während die Datenbank von SQL Data Warehouse gestartet wird, lautet der Status „Wird fortgesetzt“.
4. Wenn der Status **Online**lautet, ist die Datenbank bereit.

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Verwaltungsübersicht][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/



<!--HONumber=Jan17_HO3-->


