---
title: "Einrichten von PowerShell zum Erstellen eines virtuellen Computers für den Marketplace | Microsoft Docs"
description: "Anweisungen zum Einrichten von Azure PowerShell und Verwenden als optionalen Prozessablauf zum Erstellen von VM-Images für die Bereitstellung und den Verkauf in Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 68be118bc40e3a62aad73cb43119f49415f5b6a9


---
# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Einrichten von Azure PowerShell zum Erstellen eines Angebots für Azure Marketplace
Ausführliche Informationen zum Einrichten von PowerShell in Azure finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). Ein einfacher Ansatz ist die Zertifikatmethode, bei der ein für die Authentifizierung benötigtes Zertifikat heruntergeladen und importiert wird. Um das erforderliche Zertifikat zu erhalten, verwenden Sie das Cmdlet **Get-AzurePublishSettingsFile** . Speichern Sie die Datei, wenn Sie dazu aufgefordert werden. Verwenden Sie das Cmdlet **Import-AzurePublishSettingsFile** , um das Zertifikat in eine PowerShell-Sitzung zu importieren.

Verwenden Sie zum Konfigurieren und Speichern der allgemeinen Microsoft Azure-Abonnementeinstellungen für die PowerShell-Sitzung die Cmdlets **Set-AzureSubscription** und **Select-AzureSubscription**:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Der erste Befehl ordnet dem Abonnement ein Standardspeicherkonto zu (das für einige VM-Bereitstellungsvorgänge erforderlich ist).  Die zweite richtet das Abonnement als das aktuelle ein (was von anderen Cmdlets erkannt wird).

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte: Veröffentlichen eines Angebots in Azure Marketplace](marketplace-publishing-getting-started.md)
* [Erstellen eines VM-Images für den Marketplace](marketplace-publishing-vm-image-creation.md)




<!--HONumber=Feb17_HO3-->


