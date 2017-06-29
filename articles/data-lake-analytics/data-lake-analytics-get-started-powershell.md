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
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: faf17bcac66a70fc78bb171e172886fd2dcadca8
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Erste Schritte mit Azure Data Lake Analytics mithilfe von Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Es wird beschrieben, wie Sie Azure PowerShell zum Erstellen von Azure Data Lake Analytics-Konten verwenden und anschließend U-SQL-Aufträge senden und ausführen. Weitere Informationen zu Data Lake Analytics finden Sie unter [Übersicht über Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure Data Lake Analytics-Konto**. Weitere Informationen finden Sie unter [Erste Schritte mit Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).
* **Eine Arbeitsstation mit Azure PowerShell**. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

## <a name="log-in-to-azure"></a>Anmelden an Azure

In diesem Tutorial wird davon ausgegangen, dass Sie bereits mit der Verwendung von Azure PowerShell vertraut sind. Insbesondere müssen Sie wissen, wie Sie sich bei Azure anmelden. Wenn Sie Hilfe benötigen, finden Sie weitere Informationen unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps).

So melden Sie sich mit einem Abonnementnamen an:

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

Anstelle des Abonnementnamens können Sie für die Anmeldung auch eine Abonnement-ID verwenden:

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

Bei erfolgreicher Durchführung ähnelt die Ausgabe dieses Befehls dem folgenden Text:

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-the-tutorial"></a>Vorbereiten des Tutorials

In den PowerShell-Codeausschnitten dieses Tutorials werden die folgenden Variablen zum Speichern dieser Informationen verwendet:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Abrufen von Informationen zu einem Data Lake Analytics-Konto

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>Senden eines U-SQL-Auftrags

Erstellen Sie eine PowerShell-Variable, die das U-SQL-Skript enthalten soll.

```
$script = @"
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

"@
```

Senden Sie das Skript.

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

Alternativ können Sie das Skript als Datei speichern und mit dem folgenden Befehl senden:

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


Rufen Sie den Status eines bestimmten Auftrags ab. Verwenden Sie dieses Cmdlet weiterhin, bis Sie feststellen, dass der Auftrag abgeschlossen ist.

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Anstatt immer wieder Get-AdlAnalyticsJob aufzurufen, bis ein Auftrag abgeschlossen ist, können Sie das Wait-AdlJob-Cmdlet verwenden.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Laden Sie die Ausgabedatei herunter.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a>Weitere Informationen
* Wenn Sie dasselbe Tutorial mit anderen Tools verwenden möchten, klicken Sie oben auf der Seite auf die Registerkartenauswahl.
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).

