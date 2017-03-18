---
title: "Azure Data Lake Store-Anleitung für regionsübergreifende Migration | Microsoft-Dokumentation"
description: "Azure Data Lake Store-Anleitung für regionsübergreifende Migration"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/27/2017
ms.author: stewu
translationtype: Human Translation
ms.sourcegitcommit: 3c83a76c589178d5a2a01ed4edd208e42fb2e83e
ms.openlocfilehash: 785f97a28c060f88a0e0f3a5aa9cf6555ed2b5a4
ms.lasthandoff: 02/22/2017


---
# <a name="guidance-to-migrate-azure-data-lake-store-across-regions"></a>Anleitung zum Migrieren von Azure Data Lake Store zwischen Regionen

Wenn Azure Data Lake Store in neuen Regionen verfügbar wird, möchten Sie vielleicht einer einmalige Migration durchführen, um eine neue Region zu nutzen.  Hier sind einige hilfreiche Informationen dazu, was bei der Planung und Durchführung der Migration zu berücksichtigen ist.

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Ein Azure Data Lake Store-Konto in zwei verschiedenen Regionen** Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit Azure Data Lake Store](data-lake-store-get-started-portal.md).
* **Azure Data Factory**  Weitere Informationen finden Sie unter [Einführung in Azure Data Factory](../data-factory/data-factory-introduction.md).


## <a name="guidance-for-migration"></a>Migrationsanleitung

Es wird empfohlen, zunächst die geeignete Migrationsstrategie für Ihre Anwendung zu identifizieren, die Daten in Azure Data Lake Store schreibt, liest oder verarbeitet. Bei der Auswahl einer Strategie sollten Sie die Verfügbarkeitsanforderungen (d.h. Ausfallzeiten) Ihrer Anwendung berücksichtigen, d.h. der einfachste Ansatz wäre das Verlagern Ihrer Daten („Lift and Shift“-Methode).  Dies bedeutet, dass Ihre Anwendungen in der alten Region angehalten werden, während alle Daten in die neue Region kopiert werden.  Nach Abschluss des Kopiervorgangs können Ihre Anwendung in der neuen Region fortsetzen und das alte Azure Data Lake Store-Konto löschen.  Während dieser Migration tritt jedoch eine Ausfallzeit auf.

Zum Reduzieren der Ausfallzeit können Sie alternativ auch sofort mit dem Erfassen neuer Daten in der neuen Region beginnen und Ihre Anwendungen in der neuen Region ausführen, sobald die mindestens erforderlichen Daten verfügbar sind.  Im Hintergrund können Sie ältere Daten aus dem alten Azure Data Lake Store-Konto in das neue Azure Data Lake Store-Konto in der neuen Region kopieren.  So können Sie den Wechsel zur neuen Region mit Bereich mit nur kurzer Ausfallzeit durchführen.  Sobald alle älteren Daten kopiert wurden, können Sie das alte ADLS-Konto löschen.

Weitere wichtige Details, die beim Planen der Migration zu berücksichtigen sind:

* **Datenmenge**: Die Datenmenge (GB, Anzahl von Dateien und Ordnern usw.) wirkt sich auf die Zeit und Ressourcen aus, die für das Verschieben erforderlich sind.

* **Azure Data Lake Store-Kontoname**: Der neue Kontoname in der neuen Region muss global eindeutig sein. Beispiel: Wenn „contosoeastus2.azuredatalakestore.net“ der Name des alten Speicherkontos in USA, Osten 2 ist, können Sie Ihrem Konto in EU, Norden den Namen „contosonortheu.azuredatalakestore.net“ geben.

* **Auswahl von Tools**: Für das Kopieren von Azure Data Lake Store-Dateien wird die [Azure Data Factory-Kopieraktivität](../data-factory/data-factory-azure-datalake-connector.md) empfohlen.   Azure Data Factory unterstützt das Verschieben von Daten mit hoher Leistung und Zuverlässigkeit.  Beachten Sie, dass Azure Data Factory nur die Ordnerhierarchie und den Inhalt der Dateien kopiert. Alle angewendeten Zugriffssteuerungslisten (ACLs) müssen im neuen Konto manuell angewendet werden.  Die [Azure Data Factory-Anleitungen zur Leistungsoptimierung](../data-factory/data-factory-copy-activity-performance.md) sind eine gute Referenz mit Leistungszielen für optimale Szenarien.  Wenn Sie Daten in kürzerer Zeit kopieren möchten, müssen Sie möglicherweise Einheiten für Clouddatenverschiebungen verwenden.  Beachten Sie, dass einige andere Tools wie ADLCopy das Kopieren von Daten zwischen Regionen nicht unterstützen.  

* **[Bandbreitengebühren](https://azure.microsoft.com/en-us/pricing/details/bandwidth/)**: Es fallen Gebühren an, da Daten aus einer Azure-Region übertragen werden.

* **ACLs für Ihre Daten**: Sichern Sie Ihre Daten in der neuen Region durch Anwenden von ACLs auf Dateien und Ordner.  Anleitungen dafür finden Sie [hier](data-lake-store-secure-data.md).  Es wird empfohlen, diese Migration als Gelegenheit zum Aktualisieren und Anpassen Ihrer ACLs zu nutzen.  Wenn Sie jedoch ähnliche Einstellungen verwenden möchten, können Sie die auf eine Datei angewendeten ACLs über das Portal, [PowerShell-Cmdlets](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.datalakestore/v3.1.0/get-azurermdatalakestoreitempermission) oder SDKs anzeigen.  

* **Standort von Analysediensten**: Für eine optimale Leistung sollten sich Ihre Analysedienste wie Data Lake Analytics oder HDInsight in der gleichen Region wie Ihre Daten befinden.  

## <a name="see-also"></a>Weitere Informationen
* [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)

