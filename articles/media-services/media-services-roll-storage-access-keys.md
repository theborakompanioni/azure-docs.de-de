---
title: "Aktualisieren von Media Services nach dem Austausch der Speicherzugriffsschlüssel | Microsoft Docs"
description: "In diesem Artikel lernen Sie, wie Sie Media Services nach dem Austausch der Speicherzugriffsschlüssel aktualisieren."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: milanga;cenkdin;juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: 8e0f0905748923177269b6d5df27be900770fece
ms.openlocfilehash: a2802dbf1d8a22c33b20dd4e71fcf26d9780f86a
ms.contentlocale: de-de
ms.lasthandoff: 01/27/2017

---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>Aktualisieren von Media Services nach dem Austausch der Speicherzugriffsschlüssel

Wenn Sie ein neues Azure Media Services-Konto (AMS) erstellen, müssen Sie auch ein Azure Storage-Konto auswählen, das zum Speichern Ihrer Medieninhalte verwendet wird. Sie können Ihrem Media Services-Konto mehrere Speicherkonten hinzufügen. In diesem Thema wird das Rotieren von Speicherschlüsseln gezeigt. Außerdem wird erläutert, wie einem Medienkonto Speicherkonten hinzugefügt werden. 

Um die in diesem Thema beschriebenen Aktionen auszuführen, müssen Sie [ARM-APIs](https://docs.microsoft.com/rest/api/media/mediaservice) und [PowerShell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media) verwenden.  Weitere Informationen finden Sie unter [Verwalten von Azure-Ressourcen mit PowerShell und Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).

## <a name="overview"></a>Übersicht

Wenn ein neues Speicherkonto erstellt wird, generiert Azure zwei 512-Bit-Speicherzugriffsschlüssel, die zum Authentifizieren des Zugriffs auf das Speicherkonto verwendet werden. Um Ihre Speicherverbindungen sicherer zu gestalten, sollten Sie Ihre Speicherzugriffsschlüssel in regelmäßigen Abständen neu generieren und austauschen. Zwei Zugriffsschlüssel (primär und sekundär) werden zugewiesen, damit Sie Verbindungen zum Speicherkonto mit einem Zugriffsschlüssel aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren. Dieser Vorgang wird als Austausch der Zugriffsschlüssel bezeichnet.

Für Media Services muss ein Speicherschlüssel bereitgestellt werden. Insbesondere hängen die Locators, mit denen Ihre Assets gestreamt oder heruntergeladen werden, vom angegebenen Speicherzugriffsschlüssel ab. Beim Erstellen eines AMS-Kontos wird standardmäßig die Abhängigkeit vom primären Speicherzugriffsschlüssel übernommen. Sie können als Benutzer aber den Speicherschlüssel von AMS aktualisieren. Sie müssen Media Services darüber informieren, welcher Schlüssel verwendet werden soll. Führen Sie dazu die Schritte in diesem Thema aus.  

>[!NOTE]
> Wenn Sie mehrere Speicherkonten verwenden, müssen Sie dieses Verfahren für jedes Speicherkonto einzeln ausführen. Die Reihenfolge beim Rotieren von Speicherschlüsseln ist nicht festgelegt. Sie können zuerst den sekundären Schlüssel und dann den primären Schlüssel rotieren oder auch umgekehrt.
>
> Sie sollten vor der Ausführung der in diesem Thema beschriebenen Schritte unter einem Produktionskonto zunächst einen Test unter einem Vorproduktionskonto durchführen.
>

## <a name="steps-to-rotate-storage-keys"></a>Schritte zum Rotieren von Speicherschlüsseln 
 
 1. Ändern Sie den primären Schlüssel des Speicherkontos über das PowerShell-Cmdlet oder über das [Azure](https://portal.azure.com/)-Portal.
 2. Rufen Sie das Cmdlet „Sync-AzureRmMediaServiceStorageKeys“ mit entsprechenden Parametern auf, um das Medienkonto zum Aufnehmen der Speicherkontoschlüssel zu zwingen.
 
    Das folgende Beispiel zeigt, wie Schlüssel mit Speicherkonten synchronisiert werden.
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. Warten Sie etwa eine Stunde ab. Stellen Sie sicher, dass die Streamingszenarien funktionieren.
 4. Ändern Sie den sekundären Schlüssel des Speicherkontos über das PowerShell-Cmdlet oder über das Azure-Portal.
 5. Rufen Sie „Sync-AzureRmMediaServiceStorageKeys“ mit entsprechenden Parametern in der PowerShell auf, um das Medienkonto zum Aufnehmen der neuen Speicherkontoschlüssel zu zwingen. 
 6. Warten Sie etwa eine Stunde ab. Stellen Sie sicher, dass die Streamingszenarien funktionieren.
 
### <a name="a-powershell-cmdlet-example"></a>Beispiel für ein PowerShell-Cmdlet 

Das folgende Beispiel veranschaulicht, wie das Speicherkonto abgerufen und mit dem AMS-Konto synchronisiert wird.

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a>Schritte zum Hinzufügen von Speicherkonten zu Ihrem AMS-Konto

Im folgenden Thema wird gezeigt, wie Ihrem AMS-Konto Speicherkonten hinzugefügt werden: [Anfügen mehrerer Speicherkonten an ein Media Services-Konto](meda-services-managing-multiple-storage-accounts.md).

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Danksagungen
Wir möchten folgenden Personen für ihre Beiträge zur Erstellung dieses Dokuments danken: Cenk Dingiloglu, Milan Gada, Seva Titov.

