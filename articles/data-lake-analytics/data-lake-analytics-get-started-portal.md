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
ms.translationtype: HT
ms.sourcegitcommit: 99523f27fe43f07081bd43f5d563e554bda4426f
ms.openlocfilehash: 2722a2d72ed90ea0005362563ecaee30750c040a
ms.contentlocale: de-de
ms.lasthandoff: 08/05/2017

---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Enthält Informationen zur Verwendung des Azure-Portals zum Erstellen von Azure Data Lake Analytics-Konten, zum Definieren von Aufträgen in [U-SQL](data-lake-analytics-u-sql-get-started.md) und zum Übermitteln von Aufträgen an den Data Lake Analytics-Dienst. Weitere Informationen zu Data Lake Analytics finden Sie unter [Übersicht über Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird ein **Azure-Abonnement** benötigt. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos

Nun erstellen Sie gleichzeitig ein Data Lake Analytics- und ein Data Lake Store-Konto.  Dieser Schritt ist einfach und dauert nur etwa 60 Sekunden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu** >  **Daten und Analyse** > **Data Lake Analytics**.
3. Wählen Sie Werte für die folgenden Elemente:
   * **Name**: Geben Sie dem Data Lake Analytics-Konto einen Namen (nur Kleinbuchstaben und Zahlen sind zulässig).
   * **Abonnement**: Wählen Sie das Azure-Abonnement aus, das für das Analytics-Konto verwendet wird.
   * **Ressourcengruppe**. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe.
   * **Standort**: Wählen Sie ein Azure-Rechenzentrum für das Data Lake Analytics-Konto aus.
   * **Data Lake Store**: Führen Sie die Anweisungen zum Erstellen eines neuen Data Lake Store-Kontos aus, oder wählen Sie ein vorhandenes Konto aus. 
4. Wählen Sie optional einen Tarif für Ihr Data Lake Analytics-Konto aus.
5. Klicken Sie auf **Erstellen**. 


## <a name="your-first-u-sql-script"></a>Ihr erstes U-SQL-Skript

Der folgende Text ist ein sehr einfaches U-SQL-Skript. Es definiert lediglich ein kleines Dataset im Skript. Anschließend wird dieses Dataset als Datei mit dem Namen `/data.csv` in den standardmäßigen Data Lake Store geschrieben.

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

## <a name="submit-a-u-sql-job"></a>Senden eines U-SQL-Auftrags

1. Klicken Sie im Data Lake Analytics-Konto auf **Neuer Auftrag**.
2. Fügen Sie den Text des oben gezeigten U-SQL-Skripts ein. 
3. Klicken Sie auf **Auftrag senden**.   
4. Warten Sie, bis sich der Auftragsstatus in **Erfolgreich** ändert.
5. Falls für den Auftrag ein Fehler auftritt, helfen Ihnen die Informationen unter [Überwachung und Problembehandlung für Data Lake Analytics-Aufträge](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md) weiter.
6. Klicken Sie auf die Registerkarte **Ausgabe** und dann auf `data.csv`. 

## <a name="see-also"></a>Weitere Informationen

* Informationen zu den ersten Schritten der Entwicklung von U-SQL-Anwendungen finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).

