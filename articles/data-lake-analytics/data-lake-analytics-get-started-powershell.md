---
title: Erste Schritte mit Azure Data Lake Analytics mithilfe von Azure PowerShell | Microsoft Docs
description: 'Verwenden Sie Azure PowerShell zum Erstellen eines Data Lake Analytics-Kontos, Erstellen eines Data Lake Analytics-Auftrags mit U-SQL und Senden des Auftrags. '
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 6985dff332928d704f30e167c3bddb62bcc6cac1
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Lernprogramm: Erste Schritte mit Azure Data Lake Analytics mithilfe von Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Es wird beschrieben, wie Sie Azure PowerShell zum Erstellen von Azure Data Lake Analytics-Konten verwenden und anschließend U-SQL-Aufträge senden und ausführen. Weitere Informationen zu Data Lake Analytics finden Sie unter [Übersicht über Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Eine Arbeitsstation mit Azure PowerShell**. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

## <a name="preparing-for-the-tutorial"></a>Vorbereiten des Tutorials
Für die Erstellung eines Data Lake Analytics-Kontos müssen Sie zuerst Folgendes definieren:

* **Azure-Ressourcengruppe**: Ein Data Lake Analytics-Konto muss innerhalb einer Azure-Ressourcengruppe erstellt werden.
* **Data Lake Analytics-Kontoname**: Der Data Lake-Kontoname darf nur Kleinbuchstaben und Zahlen enthalten.
* **Standort**: eines der Azure-Rechenzentren, die Data Lake Analytics unterstützen.
* **Data Lake Store-Standardkonto**: Jedes Data Lake Analytics-Konto verfügt über ein Data Lake Store-Standardkonto. Diese Konten müssen sich an demselben Standort befinden.

In den PowerShell-Codeausschnitten dieses Tutorials werden die folgenden Variablen zum Speichern dieser Informationen verwendet:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos

Wenn Sie nicht bereits über eine Ressourcengruppe verfügen, ist es erforderlich, eine Ressourcengruppe zu erstellen. 

```
New-AzureRmResourceGroup -Name  $rg -Location $location
```

Für jedes Data Lake Analytics-Konto ist ein Data Lake Store-Standardkonto erforderlich, das zum Speichern von Protokollen verwendet wird. Sie können ein vorhandenes Konto wiederverwenden oder ein neues Konto erstellen. 

```
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

Wenn eine Ressourcengruppe und ein Data Lake Store-Konto vorhanden sind, können Sie ein Data Lake Analytics-Konto erstellen.

```
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Abrufen von Informationen zu einem Data Lake Analytics-Konto

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>Senden eines U-SQL-Auftrags

Erstellen Sie eine Textdatei mit dem folgenden U-SQL-Skript:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Senden Sie das Skript.

```
Submit-AdlJob -AccountName $adla –ScriptPath "d:\test.usql"Submit
```

## <a name="monitor-u-sql-jobs"></a>Überwachen von U-SQL-Aufträgen

Listen Sie alle Aufträge des Kontos auf. Die Ausgabe enthält die derzeit ausgeführten Aufträge sowie die Aufträge, die vor Kurzem abgeschlossen wurden.

```
Get-AdlJob -Account $adla
```

Rufen Sie den Status eines bestimmten Auftrags ab.

```
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Anstatt immer wieder Get-AdlAnalyticsJob aufzurufen, bis ein Auftrag abgeschlossen ist, können Sie das Wait-AdlJob-Cmdlet verwenden.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Überprüfen Sie nach Abschluss des Auftrags, ob die Ausgabedatei vorhanden ist, indem Sie die Dateien eines Ordners auflisten.

```
Get-AdlStoreChildItem -Account $adls -Path "/"
```

Überprüfen Sie das Vorhandensein einer Datei.

```
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading-files"></a>Hoch- und Herunterladen von Dateien

Laden Sie die Ausgabe des U-SQL-Skripts herunter.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv"  -Destination "D:\data.csv"
```


Laden Sie eine Datei hoch, die als Eingabe für ein U-SQL-Skript verwendet werden soll.

```
Import-AdlStoreItem -AccountName $adls -Path "D:\data.tsv" -Destination "/data_copy.csv" 
```

## <a name="see-also"></a>Weitere Informationen
* Wenn Sie dasselbe Tutorial mit anderen Tools verwenden möchten, klicken Sie oben auf der Seite auf die Registerkartenauswahl.
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).

