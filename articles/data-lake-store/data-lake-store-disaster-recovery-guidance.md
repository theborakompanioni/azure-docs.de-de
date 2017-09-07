---
title: "Leitfaden zur Notfallwiederherstellung für Azure Data Lake Store | Microsoft-Dokumentation"
description: "Leitfaden zur Notfallwiederherstellung für Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/30/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: f9ad730688433912a46d6e22bfc82063a9a60be3
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="disaster-recovery-guidance-for-data-in-data-lake-store"></a>Leitfaden zur Notfallwiederherstellung für Daten in Data Lake Store

Die Daten in Ihrem Azure Data Lake Store-Konto sind über automatisierte Replikate vor vorübergehenden Hardwareausfällen innerhalb einer Region geschützt. So wird für Dauerhaftigkeit und eine hohe Verfügbarkeit gesorgt, und die Vereinbarung zum Servicelevel (SLA) für Azure Data Lake Store wird erfüllt. In diesem Artikel wird beschrieben, wie Sie Ihre Daten weiter vor selten auftretenden regionsweiten Ausfällen oder dem versehentlichen Löschen schützen.

## <a name="disaster-recovery-guidance"></a>Leitfäden zur Notfallwiederherstellung
Es ist wichtig für jeden Kunden, den eigenen Notfallwiederherstellungsplan zu erstellen. Weitere Informationen zum Erstellen eines Notfallwiederherstellungsplans finden Sie in der Azure-Dokumentation weiter unten. Hier sind einige Ressourcen angegeben, die beim Erstellen Ihres eigenen Plans hilfreich sind.

* [Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Technischer Leitfaden zur Resilienz in Azure](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>Bewährte Methoden
Es ist ratsam, Ihre kritischen Daten in ein anderes Data Lake Store-Konto in einer anderen Region zu kopieren und die Häufigkeit des Vorgangs an die Anforderungen Ihres Plans für die Notfallwiederherstellung anzupassen. Es gibt viele verschiedene Methoden zum Kopieren von Daten, z.B. [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) oder [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md). Azure Data Factory ist ein nützlicher Dienst zum regelmäßigen Erstellen und Bereitstellen von Pipelines für die Datenverschiebung.

Wenn es zu einem regionalen Ausfall kommt, können Sie dann auf die Daten in der Region zugreifen, in die die Daten kopiert wurden. Sie können im [Azure-Dashboard für die Dienstintegrität](https://azure.microsoft.com/status/) den Azure-Dienststatus weltweit ermitteln.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Anleitung zur Wiederherstellung bei Datenbeschädigung oder unbeabsichtigtem Löschen
Azure Data Lake Store bietet dank automatisierter Replikate zwar Ausfallsicherheit, aber hiermit kann nicht verhindert werden, dass Ihre Anwendungen (oder Entwickler/Benutzer) Daten beschädigen oder versehentlich löschen.

### <a name="best-practices"></a>Bewährte Methoden
Zur Verhinderung des versehentlichen Löschens empfehlen wir Ihnen, zuerst die richtigen Zugriffsrichtlinien für Ihr Data Lake Store-Konto festzulegen.  Dabei werden [Azure-Ressourcensperren](../azure-resource-manager/resource-group-lock-resources.md) angewendet, um wichtige Ressourcen zu sperren, und mithilfe der verfügbaren [Data Lake Store-Sicherheitsfunktionen](data-lake-store-security-overview.md) wird Zugriffssteuerung auf Konto- und Dateiebene angewendet. Außerdem ist es ratsam, routinemäßig Kopien Ihrer kritischen Daten per [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) oder [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) in einem anderen Data Lake Store-Konto oder -Ordner oder Azure-Abonnement zu erstellen.  Diese Vorgehensweise kann verwendet werden, um Daten nach einer Beschädigung oder nach dem Löschen wiederherzustellen. Azure Data Factory ist ein nützlicher Dienst zum regelmäßigen Erstellen und Bereitstellen von Pipelines für die Datenverschiebung.

Organisationen können auch die [Diagnoseprotokollierung](data-lake-store-diagnostic-logs.md) für ihr Azure Data Lake Store-Konto aktivieren, um Überwachungsdaten zum Datenzugriff zu erfassen. Diese Daten liefern Informationen dazu, wer eine Datei ggf. gelöscht oder aktualisiert hat.

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit Azure Data Lake Store](data-lake-store-get-started-portal.md)
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)


