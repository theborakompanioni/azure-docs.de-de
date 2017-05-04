---
title: 'Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell: PowerShell: Azure | Microsoft-Dokumentation'
description: Auf dieser Seite wird beschrieben, wie Sie mithilfe von PowerShell eine klassische Verbindung in das Resource Manager-Bereitstellungsmodell verschieben.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 08152836-23e7-42d1-9a56-8306b341cd91
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/03/2017
ms.author: ganesr;cherylmc
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 7f04ac92d7a9eff9db49ff493cdaa8693ce188c6
ms.lasthandoff: 04/27/2017


---
# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model-using-powershell"></a>Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell mithilfe von PowerShell

Damit Sie eine ExpressRoute-Verbindung sowohl für das klassische Bereitstellungsmodell als auch für das Resource Manager-Bereitstellungsmodell verwenden können, müssen Sie die Verbindung in das Resource Manager-Bereitstellungsmodell verschieben. In den folgenden Abschnitten werden die Schritte zum Verschieben der Verbindung mithilfe von PowerShell erläutert.

## <a name="before-you-begin"></a>Voraussetzungen
* Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Module (mindestens Version 1.0) besitzen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).
* Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
* Lesen Sie die unter [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-move.md) bereitgestellten Informationen. Stellen Sie sicher, dass Sie die Grenzwerte und Einschränkungen verstehen.
* Vergewissern Sie sich, dass die Verbindung im klassischen Bereitstellungsmodell voll funktionsfähig ist.
* Stellen Sie sicher, dass Sie über eine Ressourcengruppe verfügen, die im Resource Manager-Bereitstellungsmodell erstellt wurde.

## <a name="move-an-expressroute-circuit"></a>Verschieben einer ExpressRoute-Verbindung

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Schritt 1: Abrufen von Verbindungsdetails aus dem klassischen Bereitstellungsmodell
Melden Sie sich bei der klassischen Azure-Umgebung an, und rufen Sie den Dienstschlüssel ab.

1. Melden Sie sich beim Azure-Konto an.

        Add-AzureAccount

2. Wählen Sie das entsprechende Azure-Abonnement.

        Select-AzureSubscription "<Enter Subscription Name here>"

3. Importieren Sie die PowerShell-Module für Azure und ExpressRoute.

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

4. Verwenden Sie das Cmdlet weiter unten, um die Dienstschlüssel für alle ExpressRoute-Verbindungen abzurufen. Kopieren Sie nach dem Abrufen der Schlüssel den **Dienstschlüssel** der Verbindung, die Sie in das Resource Manager-Bereitstellungsmodell verschieben möchten.

        Get-AzureDedicatedCircuit

### <a name="step-2-sign-in-and-create-a-resource-group"></a>Schritt 2: Anmelden und Erstellen einer Ressourcengruppe
Melden Sie sich bei der Resource Manager-Umgebung an, und erstellen Sie eine neue Ressourcengruppe.

1. Melden Sie sich bei Ihrer Azure Resource Manager-Umgebung an.

        Login-AzureRmAccount

2. Wählen Sie das entsprechende Azure-Abonnement.

        Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

3. Ändern Sie den Codeausschnitt unten, um eine neue Ressourcengruppe zu erstellen, falls Sie noch keine besitzen.

        New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Schritt 3: Verschieben der ExpressRoute-Verbindung in das Resource Manager-Bereitstellungsmodell
Sie können Ihre ExpressRoute-Verbindung jetzt vom klassischen Bereitstellungsmodell in das Resource Manager-Bereitstellungsmodell verschieben. Bevor Sie fortfahren, lesen Sie die Informationen im Artikel [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-move.md).

Ändern Sie zum Verschieben den folgenden Codeausschnitt, und führen Sie ihn aus:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

> [!NOTE]
> Nachdem der Verschiebevorgang abgeschlossen wurde, wird der neue Name (der im vorherigen Cmdlet aufgeführt wurde), zum Verweis auf die Ressource verwendet. Die Verbindung wird im Grunde umbenannt.
> 

## <a name="modify-circuit-access"></a>Ändern des Verbindungszugriffs

### <a name="to-enable-expressroute-circuit-access-for-both-deployment-models"></a>So aktivieren Sie eine ExpressRoute-Verbindung für beide Bereitstellungsmodelle
Nachdem Sie die klassische ExpressRoute-Verbindung in das Resource Manager-Bereitstellungsmodell verschoben haben, können Sie den Zugriff auf beide Bereitstellungsmodelle aktivieren. Führen Sie die folgenden Cmdlets aus, um den Zugriff auf beide Bereitstellungsmodelle zu aktivieren:

1. Rufen Sie die Verbindungsdetails ab.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

2. Legen Sie „Klassische Vorgänge zulassen“ auf TRUE fest.

        $ckt.AllowClassicOperations = $true

3. Aktualisieren Sie die Verbindung. Nachdem dieser Vorgang erfolgreich abgeschlossen wurde, können Sie die Verbindung im klassischen Bereitstellungsmodell anzeigen.

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

4. Führen Sie das folgende Cmdlet aus, um die Details zur ExpressRoute-Verbindung abzurufen. Sie müssen in der Lage sein, den aufgeführten Dienstschlüssel anzuzeigen. 

        get-azurededicatedcircuit

5. Sie können jetzt Verknüpfungen mit der ExpressRoute-Verbindung mithilfe der Befehle des klassischen Bereitstellungsmodells für klassische VNets und mithilfe der Resource Manager-Befehle für Resource Manager-VNETs verwalten. In den folgenden Artikeln werden Informationen zum Verwalten von Verknüpfungen mit der ExpressRoute-Verbindung bereitgestellt:

    * [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen im Resource Manager-Bereitstellungsmodell](expressroute-howto-linkvnet-arm.md)
    * [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen im klassischen Bereitstellungsmodell](expressroute-howto-linkvnet-classic.md)

### <a name="to-disable-expressroute-circuit-access-to-the-classic-deployment-model"></a>So deaktivieren Sie die ExpressRoute-Verbindung für das klassische Bereitstellungsmodell
Führen Sie die folgenden Cmdlets aus, um den Zugriff auf das klassische Bereitstellungsmodell zu deaktivieren:

1. Rufen Sie die Details der ExpressRoute-Verbindung ab.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

2. Legen Sie „Klassische Vorgänge zulassen“ auf FALSE fest.

        $ckt.AllowClassicOperations = $false

3. Aktualisieren Sie die Verbindung. Nachdem dieser Vorgang erfolgreich abgeschlossen wurde, können Sie die Verbindung im klassischen Bereitstellungsmodell nicht mehr anzeigen.

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Ändern des Routings für Ihre ExpressRoute-Verbindung](expressroute-howto-routing-arm.md)
* [Verknüpfen Ihres virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)


