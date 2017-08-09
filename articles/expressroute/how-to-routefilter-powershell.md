---
title: "Konfigurieren von Routenfiltern für Azure ExpressRoute-Verbindungen mit Microsoft-Peering: PowerShell | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie mithilfe von PowerShell Routenfilter für das Microsoft-Peering konfiguriert werden."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: ganesr;cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 174954a3b4345bc40a509f0078b760a728a7dcfd
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="configure-route-filters-for-microsoft-peering"></a>Konfigurieren von Routenfiltern für das Microsoft-Peering

Routenfilter stellen eine Möglichkeit dar, um eine Teilmenge von unterstützten Diensten durch das Microsoft-Peering zu nutzen. Die in diesem Artikel erläuterten Schritte unterstützen Sie bei der Konfiguration und Verwaltung von Routenfiltern für ExpressRoute-Verbindungen.

Das Microsoft-Peering ermöglicht den Zugriff auf Dynamics 365-Dienste und Office 365-Dienste wie Exchange Online, SharePoint Online und Skype for Business. Wenn das Microsoft-Peering in einer ExpressRoute-Verbindung konfiguriert ist, werden alle Präfixe im Zusammenhang mit diesen Diensten über BGP-Sitzungen angekündigt, die eingerichtet werden. Jedem Präfix wird zur Identifizierung des Diensts, der über das Präfix angeboten wird, ein BGP-Communitywert angefügt. Eine Liste der BGP-Communitywerte und der Dienste, denen sie zugeordnet sind, finden Sie unter [BGP-Communitys](expressroute-routing.md#bgp).

Wenn Konnektivität mit allen Diensten erforderlich ist, wird eine große Anzahl von Präfixen über BGP angekündigt. Dadurch erhöht sich deutlich die Größe der Routentabellen, die von Routern innerhalb Ihres Netzwerks verwaltet werden. Wenn Sie nur eine Teilmenge der Dienste nutzen möchten, die über das Microsoft-Peering angeboten werden, können Sie die Routentabellen auf zwei Arten verringern. Ihre Möglichkeiten:

- Sie filtern unerwünschte Präfixe heraus, indem Sie Routenfilter auf BGP-Communitys anwenden. Dies ist eine standardmäßige Vorgehensweise bei Netzwerken, die bei zahlreichen Netzwerken zum Einsatz kommt.

- Sie definieren Routenfilter und wenden sie auf Ihre ExpressRoute-Verbindung an. Ein Routenfilter ist eine neue Ressource, mit der Sie die Liste der Dienste, die Sie über das Microsoft-Peering nutzen möchten, auswählen können. ExpressRoute-Router senden lediglich die Liste der Präfixe, die den im Routenfilter identifizierten Diensten zugehörig sind.

### <a name="about"></a>Informationen zu Routenfiltern

Wenn das Microsoft-Peering für Ihre ExpressRoute-Verbindung konfiguriert ist, stellen die Microsoft-Edgerouter ein BGP-Sitzungspaar mit den Edgeroutern (Ihrem Edgerouter oder dem Ihres Konnektivitätsanbieters) her. Ihrem Netzwerk werden keine Routen angekündigt. Um Routenankündigungen für Ihr Netzwerk zu aktivieren, müssen Sie einen Routenfilter zuordnen.

Durch einen Routenfilter können Sie die Dienste identifizieren, die Sie über das Microsoft-Peering Ihrer ExpressRoute-Verbindung nutzen möchten. Im Wesentlichen handelt es sich um eine Whitelist aller BGP-Communitywerte. Sobald eine Routenfilterressource definiert und an eine ExpressRoute-Verbindung angefügt ist, werden Ihrem Netzwerk alle Präfixe angekündigt, die den BGP-Communitywerten zugeordnet sind.

Um Routenfilter mit Office 365-Diensten anfügen zu können, müssen Sie die Autorisierung zur Nutzung von Office 365-Diensten über ExpressRoute besitzen. Wenn Sie nicht zur Nutzung von Office 365-Diensten über ExpressRoute autorisiert sind, tritt beim Vorgang zum Anfügen von Routenfiltern ein Fehler auf. Weitere Informationen zum Autorisierungsvorgang finden Sie unter [Azure ExpressRoute für Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd). Für die Konnektivität mit Dynamics 365-Diensten ist KEINE vorherige Autorisierung erforderlich.

> [!IMPORTANT]
> Beim Microsoft-Peering von ExpressRoute-Verbindungen, die vor dem 1. August 2017 konfiguriert wurden, werden alle Dienstpräfixe über das Microsoft-Peering angekündigt, auch wenn keine Routenfilter definiert sind. Beim Microsoft-Peering von ExpressRoute-Verbindungen, die am oder nach dem 1. August 2017 konfiguriert wurden, werden Präfixe erst angekündigt, wenn der Verbindung ein Routenfilter hinzugefügt wurde.
> 
> 

### <a name="workflow"></a>Workflow

Um mit dem Microsoft-Peering eine Verbindung mit Diensten herstellen zu können, müssen Sie die folgenden Konfigurationsschritte durchführen:

- Sie benötigen eine aktive ExpressRoute-Verbindung, für die das Microsoft-Peering bereitgestellt ist. Zur Durchführung dieser Aufgaben können Sie folgende Anweisungen befolgen:
  - [Erstellen Sie eine ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md), und lassen Sie sie von Ihrem Konnektivitätsanbieter aktivieren, bevor Sie fortfahren. Die ExpressRoute-Verbindung muss den Zustand „Provisioned“ und „Enabled“ aufweisen.
  - [Erstellen Sie das Microsoft-Peering](expressroute-circuit-peerings.md), wenn Sie die BGP-Sitzung direkt verwalten. Überlassen Sie die Bereitstellung des Microsoft-Peerings für Ihre Verbindung wahlweise Ihrem Konnektivitätsanbieter.

-  Sie müssen einen Routenfilter erstellen und konfigurieren.
    - Identifizieren Sie die Dienste, die Sie über das Microsoft-Peering nutzen möchten.
    - Identifizieren Sie die Liste der BGP-Communitywerte, die den Diensten zugeordnet sind.
    - Erstellen Sie eine Regel, um die den BGP-Communitywerten entsprechende Präfixliste zuzulassen.

-  Sie müssen den Routenfilter der ExpressRoute-Verbindung anfügen.

## <a name="before-you-begin"></a>Voraussetzungen

Bevor Sie mit der Konfiguration beginnen, vergewissern Sie sich, dass Sie folgende Kriterien erfüllen:

 - Installieren Sie die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps).

  > [!NOTE]
  > Laden Sie die neueste Version aus dem PowerShell-Katalog statt mit dem Installationsprogramm herunter. Das Installationsprogramm unterstützt derzeit nicht die erforderlichen Cmdlets.
  > 

 - Lesen Sie vor Beginn der Konfiguration die Seiten zu den [Voraussetzungen](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md).

 - Sie benötigen eine aktive ExpressRoute-Verbindung. Führen Sie die Schritte zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) aus, und lassen Sie sie vom Konnektivitätsanbieter aktivieren, bevor Sie fortfahren. Die ExpressRoute-Verbindung muss den Zustand „Provisioned“ und „Enabled“ aufweisen.

 - Das Microsoft-Peering muss aktiviert sein. Befolgen Sie die Anweisungen unter [Erstellen und Ändern der Peeringkonfiguration](expressroute-circuit-peerings.md).

### <a name="log-in-to-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.

Bevor Sie mit dieser Konfiguration beginnen, müssen Sie sich beim Azure-Konto anmelden. Sie werden vom Cmdlet zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert. Nach dem Anmelden werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen.

Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

```powershell
Login-AzureRmAccount
```

Überprüfen Sie die Abonnements für das Konto, wenn Sie über mehrere Azure-Abonnements verfügen.

```powershell
Get-AzureRmSubscription
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="prefixes"></a>Schritt 1: Abrufen einer Liste von Präfixen und BGP-Communitywerten

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Abrufen einer Liste von BGP-Communitywerten

Rufen Sie mit dem folgenden Cmdlet die Liste von BGP-Communitywerten ab, die über das Microsoft-Peering zugänglichen Diensten zugeordnet sind, sowie die Liste von Präfixen, die ihnen zugeordnet sind:

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Erstellen einer Liste der zu verwendenden Werte

Erstellen Sie eine Liste von BGP-Communitywerten, die Sie im Routenfilter verwenden möchten. Der BGP-Communitywert für Dynamics 365-Dienste lautet beispielsweise „12076:5040“.

## <a name="filter"></a>Schritt 2: Erstellen eines Routenfilters und einer Filterregel

Ein Routenfilter kann nur eine Regel aufweisen, die zudem vom Typ „Zulassen“ sein muss. Diese Regel kann eine Liste von BGP-Communitywerten enthalten, die ihr zugeordnet sind.

### <a name="1-create-a-route-filter"></a>1. Erstellen eines Routenfilters

Erstellen Sie zunächst den Routenfilter. Mit dem Befehl „New-AzureRmRouteFilter“ wird nur eine Routenfilterressource erstellt. Nachdem Sie die Ressource erstellt haben, müssen Sie eine Regel erstellen und dem Routenfilterobjekt anfügen. Führen Sie den folgenden Befehl aus, um eine Routenfilterressource zu erstellen:

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a>2. Erstellen einer Filterregel

Sie können eine Reihe von BGP-Communitys als eine durch Komma getrennte Liste angeben, wie im Beispiel gezeigt wird. Führen Sie den folgenden Befehl aus, um eine neue Regel zu erstellen:
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-the-rule-to-the-route-filter"></a>3. Hinzufügen der Regel zum Routenfilter

Führen Sie den folgenden Befehl aus, um die Filterregel zum Routenfilter hinzuzufügen:
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="attach"></a>Schritt 3: Anfügen des Routenfilters zu einer ExpressRoute-Verbindung

Führen Sie den folgenden Befehl aus, um der ExpressRoute-Verbindung den Routenfilter anzufügen:

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -ExpressRouteCircuit $ckt -Name "MicrosoftPeering" -PeeringType MicrosoftPeering -PeerASN "BGPASNNumber" -PrimaryPeerAddressPrefix "A.A.A.A/30" -SecondaryPeerAddressPrefix "B.B.B.B/30" -VlanId "VLANNumber" -RouteFilter $routefilter
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="getproperties"></a>Abrufen der Eigenschaften eines Routenfilters

Um die Eigenschaften eines Routenfilters abzurufen, führen Sie die folgenden Schritte durch:

1. Führen Sie den folgenden Befehl aus, um die Routenfilterressource abzurufen:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. Rufen Sie die Routenfilterregeln für die Routenfilterressource ab, indem Sie folgenden Befehl ausführen:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

## <a name="updateproperties"></a>Aktualisieren der Eigenschaften eines Routenfilters

Wenn der Routenfilter bereits einer Verbindung angefügt ist, werden durch Updates der BGP-Communityliste automatisch entsprechende Änderungen an Präfixankündigungen über die eingerichteten BGP-Sitzungen vorgenommen. Sie können die BGP-Communityliste Ihres Routenfilters mit dem folgenden Befehl aktualisieren:

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="detach"></a>Trennen eines Routenfilters von einer ExpressRoute-Verbindung

Nachdem ein Routenfilter von der ExpressRoute-Verbindung getrennt wurde, werden keine Präfixe über die BGP-Sitzung angekündigt. Sie können einen Routenfilter mit dem folgenden Befehl von einer ExpressRoute-Verbindung trennen:
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="delete"></a>Löschen eines Routenfilters

Sie können einen Routenfilter nur löschen, wenn er keiner Verbindung angefügt wurde. Bevor Sie versuchen, diesen zu löschen, stellen Sie sicher, dass der Routenfilter keiner Verbindung zugeordnet ist. Sie können einen Routenfilter mit dem folgenden Befehl löschen:

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```
