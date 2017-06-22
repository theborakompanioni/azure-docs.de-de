---
title: Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals | Microsoft Docs
description: 'Erfahren Sie, wie Sie das Azure-Portal zum Erstellen eines Data Lake Analytics-Kontos, Erstellen eines Data Lake Analytics-Auftrags mit U-SQL und Senden des Auftrags verwenden. '
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: eb85d8ef6b29605d7e26b0d2139a4a95c35141fb
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Tutorial: Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Enthält Informationen zur Verwendung des Azure-Portals zum Erstellen von Azure Data Lake Analytics-Konten, zum Definieren von Aufträgen in [U-SQL](data-lake-analytics-u-sql-get-started.md) und zum Übermitteln von Aufträgen an den Data Lake Analytics-Dienst. Weitere Informationen zu Data Lake Analytics finden Sie unter [Übersicht über Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird ein **Azure-Abonnement** benötigt. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/pricing/free-trial/)(in englischer Sprache).

## <a name="create-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos

Nun erstellen Sie gleichzeitig ein Data Lake Analytics- und ein Data Lake Store-Konto.  Dieser Schritt ist einfach und dauert nur etwa 60 Sekunden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu** >  **Intelligence + Analyse** > **Data Lake Analytics**.
3. Wählen Sie Werte für die folgenden Elemente:
   * **Name**: Geben Sie dem Data Lake Analytics-Konto einen Namen (nur Kleinbuchstaben und Zahlen sind zulässig).
   * **Abonnement**: Wählen Sie das Azure-Abonnement aus, das für das Analytics-Konto verwendet wird.
   * **Ressourcengruppe**. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe.
   * **Standort**: Wählen Sie ein Azure-Rechenzentrum für das Data Lake Analytics-Konto aus.
   * **Data Lake Store**: Führen Sie die Anweisungen zum Erstellen eines neuen Data Lake Store-Kontos aus, oder wählen Sie ein vorhandenes Konto aus. 
4. Wählen Sie optional einen Tarif für Ihr Data Lake Analytics-Konto aus.
5. Klicken Sie auf **Erstellen**. 

## <a name="create-and-submit-data-lake-analytics-jobs"></a>Erstellen und Übermitteln von Data Lake Analytics-Aufträgen
Nachdem Sie die Quelldaten vorbereitet haben, können Sie mit dem Entwickeln eines U-SQL-Skripts beginnen.  

**So senden Sie einen Auftrag**

1. Klicken Sie im Data Lake Analytics-Konto auf **Neuer Auftrag**.
2. Geben Sie den **Auftragsnamen**und das folgende U-SQL-Skript ein:

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
    TO "/Output/SearchLog-from-Data-Lake.csv"
    USING Outputters.Csv();
```


Mit diesem U-SQL-Skript wird die Quelldatei mithilfe von **Extractors.Tsv()** gelesen, und anschließend wird eine CSV-Datei mithilfe von **Outputters.Csv()** erstellt.

1. Klicken Sie auf **Auftrag senden**.   
2. Warten Sie, bis sich der Auftragsstatus in **Erfolgreich**ändert.
3. Falls für den Auftrag ein Fehler auftritt, helfen Ihnen die Informationen unter [Überwachung und Problembehandlung für Data Lake Analytics-Aufträge](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md) weiter.
4. Klicken Sie auf die Registerkarte **Ausgabe** und dann auf `SearchLog-from-Data-Lake.csv`. 

## <a name="see-also"></a>Weitere Informationen

* Informationen zu den ersten Schritten der Entwicklung von U-SQL-Anwendungen finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).

