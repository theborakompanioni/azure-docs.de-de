---
title: Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell | Microsoft Docs
description: Auf dieser Seite wird beschrieben, wie Sie eine klassische Verbindung in das Resource Manager-Bereitstellungsmodell verschieben.
documentationcenter: na
services: expressroute
author: ganesr
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 08152836-23e7-42d1-9a56-8306b341cd91
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: ganesr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1c3bd8e01e02fb66bf5e04c307863bbe54176128


---
# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell
## <a name="configuration-prerequisites"></a>Konfigurationsvoraussetzungen
* Sie benötigen die neueste Version der Azure PowerShell-Module (mindestens Version 1.0).
* Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
* Bevor Sie fortfahren, lesen Sie die unter [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-move.md)bereitgestellten Informationen. Stellen Sie sicher, dass Sie alle Limits und Einschränkungen kennen und verstehen.
* Wenn Sie eine Azure ExpressRoute-Verbindung vom klassischen Bereitstellungsmodell zum Azure Resource Manager-Bereitstellungsmodell verschieben, muss die Verbindung im klassischen Bereitstellungsmodell vollständig konfiguriert worden und betriebsbereit sein.
* Stellen Sie sicher, dass Sie über eine Ressourcengruppe verfügen, die im Resource Manager-Bereitstellungsmodell erstellt wurde.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Verschieben der ExpressRoute-Verbindung in das Resource Manager-Bereitstellungsmodell
Sie müssen eine ExpressRoute-Verbindung in das Resource Manager-Modell verschieben, sodass Sie es sowohl im klassischen als auch im Resource Manager-Bereitstellungsmodell verwenden können. Hierzu können Sie die folgenden PowerShell-Befehle ausführen.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Schritt 1: Abrufen von Verbindungsdetails aus dem klassischen Bereitstellungsmodell
Sie müssen zunächst Informationen zu Ihrer ExpressRoute-Verbindung abrufen.

Melden Sie sich bei der klassischen Azure-Umgebung an, und rufen Sie den Dienstschlüssel ab. Sie können den folgenden PowerShell-Codeausschnitt zum Abrufen der Informationen verwenden:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Kopieren Sie den **Dienstschlüssel** der Verbindung, die Sie in das Resource Manager-Bereitstellungsmodell verschieben möchten.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Schritt 2: Anmelden bei der Resource Manager-Umgebung und Erstellen einer neuen Ressourcengruppe
Verwenden Sie den folgenden Codeausschnitt, um eine neue Ressourcengruppe zu erstellen:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Sie können auch eine vorhandene Ressourcengruppe verwenden, sofern verfügbar.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Schritt 3: Verschieben der ExpressRoute-Verbindung in das Resource Manager-Bereitstellungsmodell
Sie können Ihre ExpressRoute-Verbindung jetzt vom klassischen Bereitstellungsmodell in das Resource Manager-Bereitstellungsmodell verschieben. Bevor Sie fortfahren, lesen Sie die Informationen im Artikel [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-move.md) .

Verwenden Sie dazu den folgenden Codeausschnitt:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

> [!NOTE]
> Nachdem der Verschiebevorgang abgeschlossen wurde, wird der neue Name (der im vorherigen Cmdlet aufgeführt wurde), zum Verweis auf die Ressource verwendet. Die Verbindung wird im Grunde umbenannt.
> 
> 

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Aktivieren einer ExpressRoute-Verbindung für beide Bereitstellungsmodelle
Sie müssen Ihre ExpressRoute-Verbindung in das Resource Manager-Bereitstellungsmodell verschieben, bevor Sie den Zugriff auf das Bereitstellungsmodell steuern können.

Führen Sie das folgende Cmdlet aus, um den Zugriff auf beide Bereitstellungsmodelle zu aktivieren:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Nachdem dieser Vorgang erfolgreich abgeschlossen wurde, können Sie die Verbindung im klassischen Bereitstellungsmodell anzeigen.

Führen Sie den folgenden Befehl aus, um die Details zur ExpressRoute-Verbindung abzurufen:

    get-azurededicatedcircuit

Sie müssen in der Lage sein, den aufgeführten Dienstschlüssel anzuzeigen. Sie können jetzt Verknüpfungen mit der ExpressRoute-Verbindung mithilfe der Standardbefehle des klassischen Bereitstellungsmodells für klassische VNets und der ARM-Standardbefehle für ARM-VNETs verwalten. In den folgenden Artikeln werden Informationen zum Verwalten von Verknüpfungen mit der ExpressRoute-Verbindung bereitgestellt:

* [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen im Resource Manager-Bereitstellungsmodell](expressroute-howto-linkvnet-arm.md)
* [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen im klassischen Bereitstellungsmodell](expressroute-howto-linkvnet-classic.md)

## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>Deaktivieren der ExpressRoute-Verbindung für das klassische Bereitstellungsmodell
Führen Sie das folgende Cmdlet aus, um den Zugriff auf das klassische Bereitstellungsmodell zu deaktivieren:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Nachdem dieser Vorgang erfolgreich abgeschlossen wurde, können Sie die Verbindung im klassischen Bereitstellungsmodell nicht mehr anzeigen.

## <a name="next-steps"></a>Nächste Schritte
Führen Sie nach dem Erstellen Ihrer Verbindung folgende Vorgänge aus:

* [Erstellen und Ändern des Routings für Ihre ExpressRoute-Verbindung](expressroute-howto-routing-arm.md)
* [Verknüpfen Ihres virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)




<!--HONumber=Nov16_HO3-->


