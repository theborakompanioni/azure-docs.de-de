---
title: Azure Data Lake Analytics--Kontingentgrenzen | Microsoft Docs
description: "Erfahren Sie, wie Sie Kontingentgrenzen im ADLA-Konto (Azure Data Lake Analytics) anpassen und erhöhen. Diese Grenzen zu kennen, kann dabei helfen, das Verhalten von U-SQL-Aufträgen besser zu verstehen. Es handelt sich hierbei um weiche Grenzen. Wenden Sie sich an uns, um diese Höchstgrenzen jederzeit zu erhöhen."
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/16/2016
ms.author: omidm
translationtype: Human Translation
ms.sourcegitcommit: ce76fb1feaa38ff20ccc873114541a015126a1ad
ms.openlocfilehash: cffafbc20294c235060b03a75f6d1bee712115d1


---
# <a name="azure-data-lake-analytics-quota-limits"></a>Azure Data Lake Analytics-Kontingentgrenzen
Erfahren Sie, wie Sie Kontingentgrenzen im ADLA-Konto (Azure Data Lake Analytics) anpassen und erhöhen. Diese Grenzen zu kennen, kann dabei helfen, das Verhalten von U-SQL-Aufträgen besser zu verstehen. Es handelt sich hierbei um weiche Grenzen. Wenden Sie sich an uns, um diese Höchstgrenzen jederzeit zu erhöhen.

**Voraussetzungen**

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/pricing/free-trial/)(in englischer Sprache).
* **Data Lake Analytics-Konto**. Siehe [Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals](https://azure.microsoft.com/en-us/documentation/articles/data-lake-analytics-get-started-portal/#create-adl-analytics-account).
* **Grundlegende Kenntnisse des Data Lake Analytics-Auftragsprozesses**. Siehe [Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals](https://azure.microsoft.com/en-us/documentation/articles/data-lake-analytics-get-started-portal/).

<!-- ################################ -->
<!-- ################################ -->
## <a name="quota-limits"></a>Kontingentgrenzen:
In der folgenden Liste werden die derzeitigen Kontingentgrenzen des Systems aufgeführt:

**Azure-Abonnementgrenzen:** Für Azure-Abonnements gilt die folgende Begrenzung:
* **Maximale Anzahl von ADLA-Konten pro Abonnement:** 5. Dies ist die maximale Anzahl von ADLA-Konten, die Sie pro Abonnement erstellen können. Beim Versuch, ein sechstes ADLA-Konto zu erstellen, erhalten Sie den Fehler „Sie haben die maximale Anzahl von Data Lake Analytics-Konten (5) in (Region) für das Abonnement (Name) erreicht“. Sie können dies problemlos lösen, indem Sie entweder verwendete ADLA-Konten für Ihr Abonnement löschen oder sich durch Erstellen eines Support-Tickets an uns wenden.

**ADLA-Kontobegrenzungen:**
* **Maximale Anzahl von Analytics-Einheiten (AUs) pro Konto:** 250. Dies ist die maximale Anzahl von AUs, die in Ihrem Konto gleichzeitig ausgeführt werden können. Die Gesamtzahl der ausgeführten AUs für Ihre Aufträge dürfen nicht darüber hinausgehen. Bei einer Überschreitung dieses Werts werden neuere Aufträge automatisch in die Warteschlange gestellt. Beispiel:
    * Sie führen einen Auftrag mit 250 AUs aus. Wenn Sie den zweiten Auftrag übermitteln, befindet sich dieser in der Auftragswarteschlange, bis der erste Auftrag abgeschlossen ist.
    * Sie führen bereits fünf Aufträge mit jeweils 50 AUs aus. Wenn Sie einen sechsten Auftrag mit 20 AUs übermitteln, bleibt dieser in der Auftragswarteschlange und wird erst ausgeführt, sobald 20 AUs verfügbar sind.
* **Maximale Anzahl gleichzeitiger U-SQL-Aufträge pro Konto:** 20. Dies ist die maximale Anzahl von Aufträgen, die in Ihrem Konto gleichzeitig ausgeführt werden können. Bei einer Überschreitung dieses Werts werden neuere Aufträge automatisch in die Warteschlange gestellt.

**So passen Sie ADLA-Kontingentgrenzen pro Konto an:**
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie das bereits erstellte ADLA-Konto aus.
3. Klicken Sie auf **Eigenschaften**.
4. Passen Sie **Parallelität** und **Gleichzeitige Aufträge** Ihren Anforderungen entsprechend an.

    ![Portalblatt für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

**So erhöhen Sie die maximalen Kontingentgrenzen:**
1. Öffnen Sie eine Supportanfrage im Azure-Portal.

    ![Portalblatt für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Portalblatt für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Wählen Sie als Problemtyp **Kontingent** aus.
3. Wählen Sie Ihr **Abonnement** aus (stellen Sie sicher, dass es kein Testabonnement ist).
4. Wählen Sie als Kontingenttypen **Data Lake Analytics** aus.

    ![Portalblatt für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5.  Erläutern Sie auf dem Problemblatt Ihre Anforderung für die Erhöhung der Grenze, und nennen Sie **Details**, aus welchem Grund Sie diese zusätzliche Kapazität benötigen.

    ![Portalblatt für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6.  Überprüfen Sie Ihre Kontaktinformationen, und erstellen Sie die Supportanfrage.

Wir werden Ihre Anfrage überprüfen und versuchen, Ihren Geschäftsanforderungen schnellstmöglich nachzukommen.

## <a name="see-also"></a>Siehe auch
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Erste Schritte mit Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md)
* [Verwalten von Azure Data Lake Analytics mithilfe von Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
* [Überwachung und Problembehandlung von Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)



<!--HONumber=Nov16_HO3-->


