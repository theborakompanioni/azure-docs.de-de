---
title: Erstellen einer Azure Network Watcher-Instanz | Microsoft Docs
description: "Diese Seite enthält die Schritte zum Erstellen einer Network Watcher-Instanz mit dem Portal und der Azure-REST-API."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 064c8f7c312aaac94f20224e99b9e29c641e0349
ms.openlocfilehash: 7099081e7294c64c1b9076b58a2c87f4f7ca7e44
ms.lasthandoff: 03/01/2017

---

# <a name="create-an-azure-network-watcher-instance"></a>Erstellen einer Azure Network Watcher-Instanz

Network Watcher ist ein regionaler Dienst, mit dem Sie Bedingungen auf der Ebene von Netzwerkszenarien in Azure überwachen und diagnostizieren können. Die Überwachung auf Szenarioebene erlaubt die umfassende Diagnose von Problemen auf Netzwerkebene. Die Tools zur Netzwerkdiagnose und -visualisierung von Network Watcher helfen Ihnen dabei, Ihr Netzwerk in Azure zu verstehen, Diagnosen durchzuführen und Einblicke zu gewinnen.

## <a name="register-the-preview-capability"></a>Registrieren der Vorschaufunktion

Network Watcher befindet sich derzeit in der Vorschau. Um die Features von Network Watcher zu verwenden, muss das Feature registriert werden. Führen Sie zu diesem Zweck das folgende PowerShell-Beispiel aus:

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
Register-AzureRmProviderFeature -FeatureName AllowNetworkWatcher -ProviderNamespace Microsoft.Network
```

Um zu überprüfen, ob die Registrierung erfolgreich war, führen Sie das folgende PowerShell-Beispiel aus:

```powershell
Get-AzureRmProviderFeature -FeatureName AllowNetworkWatcher -ProviderNamespace  Microsoft.Network
```

Wenn das Feature ordnungsgemäß registriert wurde, sollte die Ausgabe der folgenden ähneln:

```
FeatureName         ProviderName      RegistrationState
-----------         ------------      -----------------
AllowNetworkWatcher Microsoft.Network Registered
```

####<a name="instructions-for-cli"></a>Anweisungen für die Befehlszeilenschnittstelle

So führen Sie die Registrierung durch

```CLI
azure provider register Microsoft.Network
azure feature register  Microsoft.Network AllowNetworkWatcher
```
Um zu überprüfen, ob die Registrierung erfolgreich durchgeführt wurde, führen Sie den folgenden CLI-Befehl aus:

```CLI
azure feature show Microsoft.Network AllowNetworkWatcher
```

Wenn das Feature ordnungsgemäß registriert wurde, sollte die Ausgabe der folgenden ähneln:
```CLI
info:    Executing command feature show
data:    Feature Name:       AllowNetworkWatcher
data:    Provider Name:      Microsoft.Network
data:    Registration State: Registered
info:    feature show command OK
```

## <a name="create-a-network-watcher-in-the-portal"></a>Erstellen einer Network Watcher-Instanz im Portal

Navigieren Sie zu **Weitere Dienste** > **Netzwerk** > **Network Watcher**. Sie können alle Abonnements auswählen, für die Sie Network Watcher aktivieren möchten. Mit dieser Aktion erstellen Sie eine Network Watcher-Instanz in jeder verfügbaren Region.

![Erstellen einer Network Watcher-Instanz][1]

## <a name="create-a-network-watcher-with-powershell"></a>Erstellen einer Network Watcher-Instanz mit PowerShell

Um eine Network Watcher-Instanz zu erstellen, führen Sie das folgende Beispiel aus:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Erstellen einer Network Watcher-Instanz mit der REST-API

ARMClient dient zum Aufrufen der REST-API mithilfe von PowerShell. Sie finden [ARMClient auf Chocolatey](https://chocolatey.org/packages/ARMClient).

### <a name="log-in-with-armclient"></a>Anmelden mit ARMClient

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a>Erstellen der Network Watcher-Instanz

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, nachdem Sie über eine Instanz von Network Watcher verfügen, welche Features Ihnen zur Verfügung stehen:

* [Topologie](network-watcher-topology-overview.md)
* [Paketerfassung](network-watcher-packet-capture-overview.md)
* [IP-Datenflussüberprüfung](network-watcher-ip-flow-verify-overview.md)
* [Nächster Hop](network-watcher-next-hop-overview.md)
* [Sicherheitsgruppenansicht](network-watcher-security-group-view-overview.md)
* [NSG-Datenflussprotokollierung](network-watcher-nsg-flow-logging-overview.md)
* [Beheben von Problemen bei Virtual Network-Gateways](network-watcher-troubleshoot-overview.md)

Nachdem eine Network Watcher-Instanz erstellt wurde, kann die Paketerfassung konfiguriert werden. Gehen Sie dabei anhand des Artikels [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md) vor.

[1]: ./media/network-watcher-create/figure1.png












