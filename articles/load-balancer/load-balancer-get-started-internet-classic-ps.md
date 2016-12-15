---
title: Erste Schritte zum Erstellen eines Lastenausgleichs mit Internetzugriff im klassischen Modus mithilfe von PowerShell | Microsoft Docs
description: Erfahren Sie, wie Sie einen Load Balancer mit Internetzugriff im klassischen Modus mithilfe von PowerShell erstellen.
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: c40545833da86426d3e71955b8eb8627db3c1e4b
ms.openlocfilehash: 3855b32c8ff13f73cf91ea8973ed40a34a062224

---

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Erste Schritte zum Erstellen eines Load Balancers (klassisch) mit Internetzugriff in PowerShell

> [!div class="op_single_selector"]
> * [Klassisches Azure-Portal](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure-Befehlszeilenschnittstelle](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Bevor Sie mit Azure-Ressourcen arbeiten, sollten Sie wissen, dass Azure derzeit über zwei Bereitstellungsmodelle verfügt: die Bereitstellung mit dem Azure Resource Manager und die klassische Bereitstellung. Stellen Sie sicher, dass Sie die [Bereitstellungsmodelle und -tools](../azure-classic-rm.md) verstanden haben, bevor Sie mit Azure-Ressouren arbeiten. Zum Anzeigen der Dokumentation für verschiedene Tools klicken Sie auf die Registerkarten oben in diesem Artikel. Dieser Artikel gilt für das klassische Bereitstellungsmodell. Informationen zum Erstellen eines Load Balancers mit Internetzugriff unter Verwendung des Azure-Ressourcen-Managers finden Sie [hier](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a>Einrichten des Load Balancers mit PowerShell

Führen Sie zum Einrichten eines Load Balancers mithilfe von PowerShell die folgenden Schritte aus:

1. Wenn Sie Azure PowerShell zuvor noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) , und befolgen Sie die komplette Anleitung, um sich bei Azure anzumelden und Ihr Abonnement auszuwählen.
2. Nach dem Erstellen eines virtuellen Computers können Sie PowerShell-Cmdlets verwenden, um einem virtuellen Computer in demselben Clouddienst einen Load Balancer hinzuzufügen.

Im folgenden Beispiel fügen Sie eine Load Balancer-Gruppe namens „webfarm“ zum Clouddienst „mytestcloud“ (oder „myctestcloud.cloudapp.net“) hinzu. Dabei werden die Endpunkte für den Load Balancer den virtuellen Computern „web1“ und „web2“ hinzugefügt. Der Load Balancer empfängt Netzwerkdatenverkehr an Port 80 und führt für die durch den lokalen Endpunkt (in diesem Fall Port 80) festgelegten virtuellen Computer per TCP einen Lastenausgleich durch.

### <a name="step-1"></a>Schritt 1

Erstellen eines Endpunkts mit Lastenausgleich für den ersten virtuellen Computer „web1“

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a>Schritt 2

Erstellen eines weiteren Endpunkts für den zweiten virtuellen Computer „web2“ unter Verwendung desselben Namens für die Gruppe mit Lastenausgleich

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Entfernen eines virtuellen Computers aus einem Load Balancer

Sie können mithilfe von „Remove-AzureEndpoint“ den Endpunkt eines virtuellen Computers aus dem Load Balancer entfernen.

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a>Nächste Schritte

Sie können auch einen [internen Lastenausgleich erstellen](load-balancer-get-started-ilb-classic-ps.md) und die Art des [Verteilungsmodus](load-balancer-distribution-mode.md) für ein bestimmtes Datenverkehrsverhalten im Netzwerk konfigurieren.

Wenn es für Ihre Anwendung erforderlich ist, dass Verbindungen mit Servern hinter einem Lastenausgleich aufrechterhalten werden, informieren Sie sich über [TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md). Hier erfahren Sie mehr über das Verhalten von Leerlaufverbindungen bei der Verwendung des Azure-Lastenausgleichs.



<!--HONumber=Dec16_HO1-->


