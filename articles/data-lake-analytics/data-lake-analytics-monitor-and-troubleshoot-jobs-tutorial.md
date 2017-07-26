---
title: "Problembehandelung bei Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals | Microsoft Docs"
description: "Erfahren Sie, wie die Problembehandlung von Data Lake Analytics-Aufträgen im Azure-Portal erfolgt. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: b9c7453cc0a94f70d0098ed83e5f127832065a62
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a>Problembehandelung bei Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals
Erfahren Sie, wie die Problembehandlung von Data Lake Analytics-Aufträgen im Azure-Portal erfolgt.

In diesem Lernprogramm simulieren Sie das Problem einer fehlenden Quelldatei, das Sie anschließend im Azure-Portal beheben.

## <a name="submit-a-data-lake-analytics-job"></a>Übermitteln eines Data Lake Analytics-Auftrags

Übermitteln Sie den folgenden U-SQL-Auftrag:

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   TO "/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
Die im Skript definierte Quelldatei heißt **/Samples/Data/SearchLog.tsv1**, sollte aber **/Samples/Data/SearchLog.tsv** heißen.


## <a name="troubleshoot-the-job"></a>Beheben des Problems mit dem Auftrag

**So zeigen Sie alle Aufträge an**

1. Klicken Sie im Azure-Portal in der oberen linken Ecke auf **Microsoft Azure** .
2. Klicken Sie auf die Kachel mit Ihrem Data Lake Analytics-Kontonamen.  Die Zusammenfassung des Auftrags wird auf der Kachel **Auftragsverwaltung** gezeigt.

    ![Azure Data Lake Analytics – Auftragsverwaltung](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    Auf der Kachel „Auftragsverwaltung“ wird der Auftragsstatus gezeigt. Sie sehen, dass ein Auftrag nicht erfolgreich war.
3. Klicken Sie auf die Kachel **Auftragsverwaltung** , um die Aufträge anzuzeigen. Die Aufträge haben die Kategorien **Wird ausgeführt**, **In Warteschlange** und **Beendet**. Der fehlerhafte Auftrag sollte im Abschnitt **Beendet** angezeigt werden. Es muss der erste in der Liste sein. Wenn viele Aufträge vorhanden sind, können Sie auf **Filter** klicken, um Aufträge einfacher zu finden.

    ![Azure Data Lake Analytics – Filtern von Aufträgen](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Klicken Sie in der Liste auf den fehlerhaften Auftrag, um die Auftragsdetails in einem neuen Blatt zu öffnen:

    ![Azure Data Lake Analytics – Fehlerhafter Auftrag](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Beachten Sie die Schaltfläche **Erneut senden** . Nachdem Sie das Problem behoben haben, können Sie den Auftrag erneut senden.
5. Klicken Sie auf den markierten Teil im vorigen Bildschirmfoto, um die Fehlerdetails zu öffnen.  Die Ausgabe sollte etwa so aussehen:

    ![Azure Data Lake Analytics – Details zum fehlerhaften Auftrag](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Sie werden informiert, dass der Quellordner nicht gefunden wurde.
6. Klicken Sie auf **Skript duplizieren**.
7. Ändern Sie den Pfad **FROM** in Folgendes:

    „/Samples/Data/SearchLog.tsv“
8. Klicken Sie auf **Auftrag senden**.

## <a name="see-also"></a>Siehe auch
* [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md)
* [Erste Schritte mit Azure Data Lake Analytics mithilfe von Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Erste Schritte mit Azure Data Lake Analytics und U-SQL mithilfe von Visual Studio](data-lake-analytics-u-sql-get-started.md)
* [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md)

