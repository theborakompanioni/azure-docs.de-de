---
title: Verwenden von PowerShell-Cmdlets mit Azure RemoteApp | Microsoft Docs
description: Erfahren Sie, wie Sie Windows PowerShell-Cmdlets in Azure RemoteApp verwenden.
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 7d3d5ded-6f73-4de6-a8ac-c1d622e842a2
ms.service: remoteapp
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.translationtype: HT
ms.sourcegitcommit: 2812039649f7d2fb0705220854e4d8d0a031d31e
ms.openlocfilehash: e6338dc283b5a26f20a467df9582181381967931
ms.contentlocale: de-de
ms.lasthandoff: 07/22/2017

---
# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Verwenden von Windows PowerShell-Cmdlets mit Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

 Sie können die PowerShell-Cmdlets in Azure RemoteApp zur Bereitstellung und Verwaltung Ihrer Sammlungen verwenden. Die folgenden Informationen erleichtern Ihnen den Einsteig.

## <a name="get-the-cmdlets"></a>Abrufen der Cmdlets
- - -
Laden Sie [hier](http://go.microsoft.com/?linkid=9811175) zunächst die Azure PowerShell-Cmdlets herunter. Diese enthalten auch die RemoteApp-Cmdlets. 

Weitere Informationen finden Sie in der [Hilfe zu Azure RemoteApp-Cmdlets](/powershell/module/azure?view=azuresmps-3.7.0).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Konfigurieren Sie Azure-Cmdlets zur Verwendung Ihres Abonnements
- - -
Folgen Sie den Anweisungen [dieses Handbuchs](/powershell/azure/overview) , sodass Sie die Cmdlets für Ihr Azure-Abonnement verwenden können.

Für den Schnelleinstieg können Sie die folgenden Schritte ausführen:

1. Laden Sie die [Azure PowerShell-Cmdlets](http://go.microsoft.com/?linkid=9811175)herunter, und installieren Sie sie.
2. Starten Sie Microsoft Azure PowerShell.
3. Führen Sie **Add-AzureAccount** aus, um sich bei Ihrem Azure-Abonnement zu authentifizieren. Geben Sie bei der entsprechenden Aufforderung den Benutzernamen und das Kennwort ein, die Sie auch bei der Anmeldung beim Azure-Portal verwenden.  
4. Führen Sie **Get-AzureSubscription** aus, um die Ihrem Benutzerkonto zugeordneten Abonnements anzuzeigen. 
5. Führen Sie **Select-AzureSubscription -SubscriptionName &lt;Abonnementname&gt;** oder **Select-AzureSubscription -SubscriptionId &lt;Abonnement-ID&gt;** aus, um das zu verwendende Abonnement anzugeben.

Herzlichen Glückwunsch, damit ist die Azure PowerShell-Konsole konfiguriert und einsatzbereit. Beachten Sie, dass Sie die Schritte 2 bis 5 bei jedem Start der Azure PowerShell-Konsole wiederholen müssen.  


## <a name="list-all-collections"></a>Auflisten aller Sammlungen
- - -
     Get-AzureRemoteAppCollection

## <a name="delete-a-collection"></a>Löschen einer Sammlung
- - -
    Remove-AzureRemoteAppCollection <enter collection name>

Beispiel: `Remove-AzureRemoteAppCollection ContosoProduction`

## <a name="create-a-cloud-collection"></a>Erstellen einer Cloudsammlung
- - -
Es ist ganz leicht. Führen Sie den folgenden Befehl aus:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Microsoft Office 365-Anwendungen (Excel, OneNote, Outlook, PowerPoint, Visio und Word) werden vom oben aufgeführten Befehl automatisch veröffentlicht.

Die Erstellung der Sammlung dauert 30 Minuten oder länger, bis sie abgeschlossen ist. Dieser Befehl gibt daher eine Nachverfolgungs-ID zurück, die Sie wie folgt nutzen können:

    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Nach Abschluss der Sammlung können Sie Benutzer mit dem folgenden Befehl hinzufügen:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

Und Sie haben es geschafft! Dieser Benutzer sollte eine Verbindung zur Anwendung mit dem Azure RemoteApp-Client herstellen können, der sich [hier](https://www.remoteapp.windowsazure.com/)befindet.

## <a name="available-cmdlets"></a>Verfügbare Cmdlets
Es gibt viele weitere Befehle, für die es in Kürze eine Dokumentation gibt:

Grundlegende Sammlungs-Cmdlets für die RemoteApp: 

* New-AzureRemoteAppCollection
* Get-AzureRemoteAppCollection
* Set-AzureRemoteAppCollection
* Update-AzureRemoteAppCollection
* Remove-AzureRemoteAppCollection
* Add-AzureRemoteAppUser
* Get-AzureRemoteAppUser
* Remove-AzureRemoteAppUser
* Get-AzureRemoteAppSession
* Disconnect-AzureRemoteAppSession
* Invoke-AzureRemoteAppSessionLogoff
* Send-AzureRemoteAppSessionMessage
* Get-AzureRemoteAppProgram
* Get-AzureRemoteAppStartMenuProgram
* Publish-AzureRemoteAppProgram
* Unpublish-AzureRemoteAppProgram
* Get-AzureRemoteAppCollectionUsageDetails
* Get-AzureRemoteAppCollectionUsageSummary
* Get-AzureRemoteAppPlan

VM-Cmdlets für die RemoteApp:

* New-AzureRemoteAppVNet
* Get-AzureRemoteAppVNet
* Set-AzureRemoteAppVNet
* Remove-AzureRemoteAppVNet
* Get-AzureRemoteAppVpnDevice
* Get-- AzureRemoteAppVpnDeviceConfigScript
* Reset-AzureRemoteAppVpnSharedKey

Vorlagenimage-Cmdlets für die RemoteApp:

* New-AzureRemoteAppTemplateImage
* Get-AzureRemoteAppTemplateImage
* Rename-AzureRemoteAppTemplateImage
* Remove-AzureRemoteAppTemplateImage

Andere RemoteApp-Cmdlets:

* Get-AzureRemoteAppLocation
* Get-AzureRemoteAppWorkspace
* Set-AzureRemoteAppWorkspace
* Get-AzureRemoteAppOperationResult


