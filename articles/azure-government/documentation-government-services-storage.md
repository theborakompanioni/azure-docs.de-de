---
title: Azure Government Storage | Microsoft Docs
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
ms.assetid: 83df022b-d791-4efb-9fdf-8afe47a885d5
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 12/22/2016
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: 831334f2b835d00aa3fd22292764b69e85d735a6
ms.openlocfilehash: a6b61df5884031eb5b53f983b834357bd23a622e


---
# <a name="azure-government-storage"></a>Azure Government Storage
## <a name="azure-storage"></a>Azure Storage
Einzelheiten zu diesem Dienst und seiner Verwendung finden Sie in der [öffentlichen Dokumentation zu Azure Storage](../storage/index.md).

### <a name="storage-service-availability-by-azure-government-region"></a>Speicherdienstverfügbarkeit nach Azure Government-Region

| Dienst | US Government, Virginia | US Government, Iowa | Hinweise
| --- | --- | --- | --- |
| [Blob Storage] (../storage/storage-introduction.md#blob-storage) |Allgemein verfügbar |Allgemein verfügbar |
| [Table Storage] (../storage/storage-introduction.md#table-storage) |Allgemein verfügbar  |Allgemein verfügbar |
| [Queue Storage] (../storage/storage-introduction.md#queue-storage) |Allgemein verfügbar | Allgemein verfügbar |
| [File Storage] (../storage/storage-introduction.md#file-storage) |Allgemein verfügbar |Allgemein verfügbar |
| [Hot/Cool Blob Storage] (../storage/storage-blob-storage-tiers.md) |NA |NA |
| [Storage Service Encryption] (../storage/storage-service-encryption.md) |Allgemein verfügbar |Allgemein verfügbar |
| [Storage Premium] (../storage/storage-premium-storage.md) |Allgemein verfügbar |NA | Umfasst das Erstellen virtueller Computer der DS-Serie. |
| [Blob-Import/Export] (../storage/storage-import-export-service.md) |Allgemein verfügbar |Allgemein verfügbar |
| [StorSimple] (../storsimple/storsimple-ova-overview.md) |Allgemein verfügbar |Allgemein verfügbar |

### <a name="variations"></a>Variationen
Die URLs für Speicherkonten in Azure Government unterscheiden sich:

| Service Type | Azure – Öffentlich | Azure Government |
| --- | --- | --- |
| Blob Storage |*.blob.core.windows.net |*.blob.core.usgovcloudapi.net |
| Warteschlangenspeicher |*.queue.core.windows.net |*.queue.core.usgovcloudapi.net |
| Tabellenspeicher |*.table.core.windows.net |*.table.core.usgovcloudapi.net |
| File Storage |*.file.core.windows.net |*.file.core.usgovcloudapi.net | 

> [!NOTE]
> Ihre gesamten Skripts und Ihr Code müssen die passenden Endpunkte berücksichtigen.  Weitere Informationen finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](../storage/storage-configure-connection-string.md). 
>
>

Weitere Informationen zu APIs finden Sie unter <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx"> CloudStorageAccount Constructor (CloudStorageAccount-Konstruktor)</a>.

Das Endpunkt-Suffix für diese Überladungen lautet core.usgovcloudapi.net.

> [!NOTE]
> [Microsoft Azure-Speicher-Explorer] (../vs-azure-tools-storage-manage-with-storage-explorer.md) bietet derzeit keine Unterstützung für das [Herstellen einer Verbindung mit einem Azure-Abonnement] (../vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-an-azure-subscription) durch Hinzufügen eines Kontos in Azure Government. Verwenden Sie andere Methoden zum [Herstellen einer Verbindung mit einem Speicherkonto] (../vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service).
Wählen Sie beim [Anfügen an ein externes Speicherkonto] (../vs-azure-tools-storage-manage-with-storage-explorer.md#attach-to-an-external-storage-account) in **Schritt 3** die Option **Other (specify below)** (Andere (unten angeben)) als Speicherendpunktdomäne, und geben Sie **core.usgovcloudapi.net** für Azure Government an.
>
>

> [!NOTE]
> Wenn Fehler 53 „Der Netzwerkpfad wurde nicht gefunden“ während der [Bereitstellung der Dateifreigabe] (.. / storage/storage-dotnet-how-to-use-files.md#mount-the-file-share) zurückgegeben wird, kann die Ursache dafür eine Firewall sein, die den ausgehenden Port blockiert. Versuchen Sie, die Dateifreigabe auf einem virtuellen Computer bereitzustellen, der sich im selben Azure-Abonnement wie das Speicherkonto befindet.
>
>

> [!NOTE]
> Verwenden Sie beim Bereitstellen des StorSimple Manager-Diensts die URLs https://portal.azure.us/ und https://manage.windowsazure.us/ für das Azure-Portal bzw. das klassische Portal. Anweisungen zum Bereitstellen von StorSimple Virtual Array finden Sie unter [Systemanforderungen für StorSimple Virtual Array] (../storsimple/storsimple-ova-system-requirements.md) und für die StorSimple 8000-Serie unter [StorSimple-Software, hohe Verfügbarkeit und Netzwerkanforderungen] (../storsimple/storsimple-system-requirements.md). Navigieren Sie im Menü links zum Abschnitt über die Bereitstellung. Die allgemeine StorSimple-Dokumentation finden Sie unter [Was ist StorSimple?] (../storsimple/index.md).
>
>

### <a name="considerations"></a>Überlegungen
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Storage:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Daten, die in einem Azure Storage-Produkt eingegeben, gespeichert und verarbeitet werden, können Daten enthalten, die Exportbestimmungen unterliegen. Statische Authentifikatoren wie Kennwörter und Smartcard-PINs für den Zugriff auf Komponenten der Azure-Plattform. Private Schlüssel von Zertifikaten, die für die Verwaltung von Komponenten der Azure-Plattform verwendet werden. Andere Sicherheitsinformationen/geheime Schlüssel, wie etwa Zertifikate, Verschlüsselungsschlüssel, Hauptschlüssel und Speicherschlüssel, die in Azure-Diensten gespeichert sind. |Azure Storage-Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten des Speicherprodukts eingegeben werden.  Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Ressourcengruppen, Deployment names (Bereitstellungsnamen), Ressourcennamen, Resourcentags |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.



<!--HONumber=Feb17_HO2-->


