---
title: "Regionsübergreifende Migration in Azure Data Lake Store | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die regionsübergreifende Migration für Azure Data Lake Store."
services: data-lake-store
documentationcenter: 
author: swums
manager: amitkul
editor: swums
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/27/2017
ms.author: stewu
translationtype: Human Translation
ms.sourcegitcommit: 7c28fda22a08ea40b15cf69351e1b0aff6bd0a95
ms.openlocfilehash: 34b449b251672619aec6e86b9343343a9404126a
ms.lasthandoff: 03/07/2017


---
# <a name="migrate-data-lake-store-across-regions"></a>Regionsübergreifende Migration in Azure Data Lake Store

Wenn Azure Data Lake Store in neuen Regionen verfügbar wird, möchten Sie vielleicht einer einmalige Migration durchführen, um die neue Region zu nutzen. Erfahren Sie, was bei der Planung und Durchführung der Migration zu berücksichtigen ist.

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Weitere Informationen finden Sie unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/).
* **Ein Azure Data Lake Store-Konto in zwei verschiedenen Regionen**. Weitere Informationen finden Sie unter [Erste Schritte mit Data Lake Store mithilfe des Azure-Portals](data-lake-store-get-started-portal.md).
* **Azure Data Factory** Weitere Informationen finden Sie unter [Einführung in Azure Data Factory](../data-factory/data-factory-introduction.md).


## <a name="migration-considerations"></a>Überlegungen zur Migration

Ermitteln Sie zuerst die Migrationsstrategie, die am besten zu Ihrer Anwendung passt, die Daten in Data Lake Store schreibt, liest oder verarbeitet. Rechnen Sie bei der Auswahl einer Strategie die Verfügbarkeitsanforderungen Ihrer Anwendung und die Ausfallzeit, die während der Migration auftritt, mit ein. Die einfachste Vorgehensweise könnte z.B. das Cloudmigrationsmodell per Lift & Shift sein. Bei dieser Vorgehensweise halten Sie die Anwendung in Ihrer vorhandenen Region an, während Ihre gesamten Daten in die neue Region kopiert werden. Wenn der Kopiervorgang abgeschlossen ist, können Sie Ihre Anwendung in der neuen Region wieder aufnehmen; löschen Sie anschließend das alte Data Lake Store-Konto. Eine Ausfallzeit während der Migration ist erforderlich.

Um Ausfallzeiten zu reduzieren, können Sie sofort damit beginnen, neue Daten in der neuen Region zu erfassen. Wenn die mindestens erforderlichen Daten vorhanden sind, führen Sie die Anwendung in der neuen Region aus. Im Hintergrund fahren Sie mit dem Kopieren älterer Daten aus dem alten Azure Data Lake Store-Konto in das neue Azure Data Lake Store-Konto in der neuen Region fort. Mithilfe dieses Ansatzes können Sie den Wechsel in die neue Region mit wenig Ausfallzeit durchführen. Wenn alle älteren Daten kopiert wurden, löschen Sie das alte Data Lake Store-Konto.

Weitere wichtige Details, die beim Planen der Migration zu berücksichtigen sind:

* **Datenvolumen**. Die Datenmenge (in Gigabyte, die Anzahl der Dateien und Ordnern usw.) wirkt sich auf die Zeit und Ressourcen aus, die für die Migration benötigt werden.

* **Name des Data Lake Store-Kontos**. Der neue Kontoname in der neuen Region muss global eindeutig sein. Beispielsweise könnte der Name Ihres alten Data Lake Store-Kontos in der Region „USA, Osten 2“ contosoeastus2.azuredatalakestore.net sein. Sie könnten Ihr neues Data Lake Store-Konto in der Region „EU, Norden“ contosonortheu.azuredatalakestore.net nennen.

* **Tools**. Es wird empfohlen, die [Kopieraktivität von Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) zu verwenden, um Data Lake Store-Dateien zu kopieren. Data Factory unterstützt das Verschieben von Daten mit hoher Leistung und Zuverlässigkeit. Bedenken Sie, dass Data Factory nur die Ordnerhierarchie und den Inhalt der Dateien kopiert. Sie müssen manuell alle Zugriffssteuerungslisten (access control lists, ACLs), die Sie im alten Konto verwenden, auf das neue Konto anwenden. Weitere Informationen, einschließlich der Leistungsziele für die günstigsten Szenarios, finden Sie unter [Handbuch zur Leistung und Optimierung der Kopieraktivität](../data-factory/data-factory-copy-activity-performance.md). Wenn Sie Daten schneller kopieren möchten, müssen Sie möglicherweise zusätzliche Einheiten für Clouddatenverschiebungen verwenden. Einige andere Tools, z.B. AdlCopy, unterstützen kein Kopieren von Daten zwischen Regionen.  

* **Bandbreitengebühren**. [Bandbreitengebühren](https://azure.microsoft.com/en-us/pricing/details/bandwidth/) werden geltend gemacht, da Daten aus einer Azure-Region hinaus übertragen werden.

* **ACLs für Ihre Daten**. Sichern Sie Ihre Daten in der neuen Region durch Anwenden von ACLs auf Dateien und Ordner. Weitere Informationen finden Sie unter [Schützen von Daten, die im Azure Data Lake Store gespeichert sind](data-lake-store-secure-data.md). Es wird empfohlen, dass Sie die Migration zum Aktualisieren und Anpassen Ihrer ACLs verwenden. Möglicherweise möchten die Einstellungen, die mit Ihrer aktuellen Einstellungen ähnliche verwenden. Sie können die ACLs, die auf alle Dateien angewendet werden, mithilfe vom Azure-Portal, [PowerShell-Cmdlets](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.datalakestore/v3.1.0/get-azurermdatalakestoreitempermission) oder SDKs anzeigen.  

* **Speicherort von Analysediensten**. Für optimale Leistung sollten sich Ihre Analysedienste, wie etwa Azure Data Lake Analytics oder Azure HDInsight, in der gleichen Region wie Ihre Daten befinden.  

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)

