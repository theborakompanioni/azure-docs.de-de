---
title: Parametrisieren von U-SQL-Skripts in Azure Data Lake Analytics | Microsoft-Dokumentation
description: Erfahren Sie, wie U-SQL-Skripts in Azure Data Lake Analytics parametrisiert werden.
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: 6318fa8b14d8bb7d650522e2d96a5b1417afe3df
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="parameterizing-u-sql-scripts"></a>Parametrisieren von U-SQL-Skripts

Wenn Sie mit U-SQL-Skripts arbeiten, ist es sinnvoll, die Skripts zu parametrisieren. Durch die Verwendung von Parametern kann der Hauptteil unverändert bleiben, und die Ausführung des Skripts kann durch die Übergabe in separaten Parameterwerten gesteuert werden. In einem typischen Szenario verarbeitet ein Skript Daten über einen gewissen Zeitraum, und ein Entwickler möchte das Skript mit einem Anfangsdatum und einem Enddatum parametrisieren. 

Es gibt zwei Möglichkeiten, in denen Skripts parametrisiert werden:
* Das Skript ermöglicht das sichere Hinzufügen von Parametern am Anfang des Skripts. Diese Methode wird von U-SQL unterstützt.
* Eine Auftragsübermittlungs-API ermöglicht Aufrufern das Senden von Parametern. Diese Methode wird von U-SQL nicht unterstützt.

## <a name="example-scripts"></a>Beispielskripts

Nachstehend finden Sie ein einfaches erstes Skript:

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog
    TO "/output/data.csv"
    USING Outputters.Csv();
```    
   
In diesem Szenario parametrisieren wir ``Region`` durch Hinzufügen einer `DECLARE EXTERNAL`-Anweisung.

```
DECLARE EXTERNAL @TargetRegion = "en-us";

@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog
    TO "/output/data.csv"
    USING Outputters.Csv();
```    

Bei Ausführung filtert dieses Skript standardmäßig nur die Zeilen aus der Region `en-us`. Bisher haben wir einen Parameter im Skript definiert. Das Schlüsselwort `EXTERNAL` gibt an, dass ein Benutzer den Wert `@TargetRegion` durch Voranstellen einer DECLARE-Anweisung neu definieren kann, die den Parameter `@TargetRegion` definiert.

Das folgende Skript zeigt, wie das parametrisierte Skript aussieht, wenn ein Benutzer einen Wert für den Parameter angegeben hat. Dieses Skript filtert jetzt Zeilen mit der Region `en-gb`.

```
DECLARE @TargetRegion = "en-gb";

DECLARE EXTERNAL @TargetRegion = "en-us";

@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog
    TO "/output/data.csv"
    USING Outputters.Csv();
```    


## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Überwachung und Problembehandlung von Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

