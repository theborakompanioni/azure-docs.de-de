---
title: "Konfigurieren des Lastenausgleichs für SQL-AlwaysOn | Microsoft Docs"
description: "Konfigurieren des Load Balancers für SQL-Always On und Verwenden von PowerShell zum Erstellen des Load Balancers für die SQL-Implementierung"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 0c23ee550d8ac88994e8c7c54a33d348ffc24372
ms.openlocfilehash: 68aad6253f185d53fdd7f11c8660c7287ef12655

---

# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurieren des Lastenausgleichs für SQL-AlwaysOn 

SQL Server-AlwaysOn-Verfügbarkeitsgruppen können nun mit ILB ausgeführt werden. Verfügbarkeitsgruppen sind eine führende SQL Server-Lösung für hohe Verfügbarkeit und Notfallwiederherstellung. Der Verfügbarkeitsgruppenlistener ermöglicht Clientanwendungen eine nahtlose Verbindung mit dem primären Replikat unabhängig von der Anzahl der Replikate in der Konfiguration.

Der Listenername (DNS) wird einer IP-Adresse mit Lastenausgleich zugeordnet, und der Azure-Lastenausgleich leitet den eingehenden Datenverkehr nur an den primären Server in der Replikatgruppe.

Sie können ILB-Unterstützung für SQL Server-AlwaysOn-Endpunkte (Listener) verwenden. Sie steuern den Zugriff auf den Listener und können die IP-Adresse mit Lastenausgleich aus einem bestimmten Subnetz im virtuellen Netzwerk (VNet) auswählen.

Mithilfe von ILB auf dem Listener kann auf den SQL Server-Endpunkt (z. B. Server=tcp:ListenerName,1433;Database=DatabaseName) nur über Folgendes zugegriffen werden:

* Dienste und virtuelle Computer im gleichen virtuellen Netzwerk
* Dienste und virtuelle Computer aus dem lokalen Netzwerk
* Dienste und virtuelle Computer aus verbundenen VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Abbildung 1: SQL AlwaysOn konfiguriert mit Lastenausgleich für Internetzugriff

## <a name="add-internal-load-balancer-to-the-service"></a>Hinzufügen von internem Lastenausgleich zu einem Dienst

1. Im folgenden Beispiel konfigurieren wir ein virtuelles Netzwerk, das ein Subnetz namens „Subnet-1“ enthält:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Hinzufügen von Endpunkten mit Lastenausgleich für ILB auf jedem virtuellen Computer

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    Im Beispiel oben werden 2 virtuelle Computer namens "sqlsvc1" und "sqlsvc2" im Clouddienst "Sqlsvc" ausgeführt. Nach dem Erstellen von ILB mit dem Switch `DirectServerReturn` fügen Sie Endpunkte mit Lastenausgleich zu ILB hinzu, um SQL das Konfigurieren der Listener für Verfügbarkeitsgruppen zu ermöglichen.

Weitere Informationen zu SQL AlwaysOn finden Sie unter [Konfigurieren eines internen Load Balancers für eine AlwaysOn-Verfügbarkeitsgruppe in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Weitere Informationen
[Erste Schritte zum Konfigurieren des Lastenausgleichs für Internetverbindungen](load-balancer-get-started-internet-arm-ps.md)

[Erste Schritte zum Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Jan17_HO2-->


