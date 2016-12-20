---
title: "Häufig gestellte Fragen zu Azure Data Lake Store | Microsoft Docs"
description: "Enthält eine Anleitung zur Problembehandlung und Lösung von Problemen mit Azure Data Lake Store."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf7fd555-3e30-43ce-b28c-c3ad0a241fdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: f426cf4b91941f19714c33abc21ef0c777f5b233
ms.openlocfilehash: 38f3ca24b7fa136bdb4b64b6cae77078b7ea3c97


---
# <a name="frequently-asked-questions-for-azure-data-lake-store"></a>Häufig gestellte Fragen zu Azure Data Lake Store
In diesem Artikel sind die häufig gestellten Fragen zum Azure Data Lake Store enthalten.

## <a name="how-can-i-further-protect-my-data-from-region-wide-disasters-or-accidental-deletions"></a>Wie kann ich meine Daten weiterhin vor regionsweiten Notfällen oder unbeabsichtigten Löschen schützen?
Die Daten in Ihrem Azure Data Lake Store-Konto sind über automatisierte Replikate vor vorübergehenden Hardwareausfällen innerhalb einer Region geschützt. So wird für Dauerhaftigkeit und eine hohe Verfügbarkeit gesorgt, und die Vereinbarung zum Servicelevel (SLA) für Azure Data Lake Store wird erfüllt. Hier wird beschrieben, wie Sie Ihre Daten weiter vor selten auftretenden regionsweiten Ausfällen oder dem versehentlichen Löschen schützen.

### <a name="disaster-recovery-guidance"></a>Leitfäden zur Notfallwiederherstellung
Es ist wichtig für jeden Kunden, den eigenen Notfallwiederherstellungsplan zu erstellen. Weitere Informationen zum Erstellen eines Notfallwiederherstellungsplans finden Sie in der Azure-Dokumentation weiter unten. Hier sind einige Ressourcen angegeben, die beim Erstellen Ihres eigenen Plans hilfreich sind.

* [Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Technischer Leitfaden zur Resilienz in Azure](../resiliency/resiliency-technical-guidance.md)

#### <a name="best-practices"></a>Bewährte Methoden
Es ist ratsam, Ihre kritischen Daten in ein anderes Data Lake Store-Konto in einer anderen Region zu kopieren und die Häufigkeit des Vorgangs an die Anforderungen Ihres Plans für die Notfallwiederherstellung anzupassen. Es gibt viele verschiedene Methoden zum Kopieren von Daten, z.B. [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) oder [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md). Azure Data Factory ist ein nützlicher Dienst zum regelmäßigen Erstellen und Bereitstellen von Pipelines für die Datenverschiebung.

Wenn es zu einem regionalen Ausfall kommt, können Sie dann auf die Daten in der Region zugreifen, in die die Daten kopiert wurden. Sie können im [Azure-Dashboard für die Dienstintegrität](https://azure.microsoft.com/status/) den Azure-Dienststatus weltweit ermitteln.

### <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Anleitung zur Wiederherstellung bei Datenbeschädigung oder unbeabsichtigtem Löschen
Azure Data Lake Store bietet dank automatisierter Replikate zwar Ausfallsicherheit, aber hiermit kann nicht verhindert werden, dass Ihre Anwendungen (oder Entwickler/Benutzer) Daten beschädigen oder versehentlich löschen.

#### <a name="best-practices"></a>Bewährte Methoden
Zur Verhinderung des versehentlichen Löschens empfehlen wir Ihnen, zuerst die richtigen Zugriffsrichtlinien für Ihr Data Lake Store-Konto festzulegen, indem Sie die verfügbaren [Data Lake Store-Sicherheitsfunktionen](data-lake-store-security-overview.md) verwenden. Außerdem ist es ratsam, routinemäßig Kopien Ihrer kritischen Daten per [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) oder [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) in einem anderen Data Lake Store-Konto oder -Ordner oder Azure-Abonnement zu erstellen.  Diese Vorgehensweise kann verwendet werden, um Daten nach einer Beschädigung oder nach dem Löschen wiederherzustellen. Azure Data Factory ist ein nützlicher Dienst zum regelmäßigen Erstellen und Bereitstellen von Pipelines für die Datenverschiebung.

Organisationen können auch die [Diagnoseprotokollierung](data-lake-store-diagnostic-logs.md) für ihr Azure Data Lake Store-Konto aktivieren, um Überwachungsdaten zum Datenzugriff zu erfassen. Diese Daten liefern Informationen dazu, wer eine Datei ggf. gelöscht oder aktualisiert hat.

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit Azure Data Lake Store](data-lake-store-get-started-portal.md)
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)




<!--HONumber=Nov16_HO5-->


