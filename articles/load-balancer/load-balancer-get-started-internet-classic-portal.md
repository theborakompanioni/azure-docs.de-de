---
title: "Erstellen eines Lastenausgleichs für den Internetzugriff – klassisches Azure-Portal | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie im klassischen Azure-Portal einen Load Balancer mit Internetzugriff in einem klassischen Bereitstellungsmodell erstellen
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: a022154f5eca6de2d2dbfc1b9aa30d2ea0a7d650

---

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Erste Schritte zum Erstellen eines Load Balancers mit Internetzugriff (klassisch) im klassischen Azure-Portal

> [!div class="op_single_selector"]
> * [Klassisches Azure-Portal](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure-Befehlszeilenschnittstelle](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Bevor Sie mit Azure-Ressourcen arbeiten, sollten Sie wissen, dass Azure derzeit über zwei Bereitstellungsmodelle verfügt: die Bereitstellung mit dem Azure Resource Manager und die klassische Bereitstellung. Stellen Sie sicher, dass Sie die [Bereitstellungsmodelle und -tools](../azure-classic-rm.md) verstanden haben, bevor Sie mit Azure-Ressouren arbeiten. Zum Anzeigen der Dokumentation für verschiedene Tools klicken Sie auf die Registerkarten oben in diesem Artikel. Dieser Artikel gilt für das klassische Bereitstellungsmodell. Informationen zum Erstellen eines Load Balancers mit Internetzugriff unter Verwendung des Azure-Ressourcen-Managers finden Sie [hier](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Einrichten des Lastenausgleichs für Internetverbindungen für virtuelle Computer

Um Lastenausgleich für Netzwerkverkehr aus dem Internet auf virtuellen Computern eines Clouddiensts auszuführen, müssen Sie eine Gruppe mit Lastenausgleich erstellen. Dieses Verfahren setzt voraus, dass Sie die virtuellen Computer bereits erstellt haben und diese sich im selben Clouddienst befinden.

**So konfigurieren Sie eine Gruppe mit Lastenausgleich für virtuelle Computer**

1. Klicken Sie im klassischen Azure-Portal auf **Virtuelle Computer**und anschließend auf den Namen eines virtuellen Computers in der Gruppe mit Lastenausgleich.
2. Klicken Sie auf **Endpunkte** und anschließend auf **Hinzufügen**.
3. Klicken Sie auf der Seite **Endpunkt zu einem virtuellen Computer hinzufügen** auf den rechten Pfeil.
4. Gehen Sie auf der Seite **Geben Sie die Details des Endpunktes ein** folgendermaßen vor:

   * Geben Sie unter **Name**einen Namen für den Endpunkt ein, oder wählen Sie einen der vordefinierten Endpunkte für gängige Protokolle aus der Liste aus.
   * Wählen Sie unter **Protokoll**das Protokoll aus, das für den Endpunkttyp erforderlich ist: TCP oder UDP.
   * Geben Sie unter **Öffentlicher Port**und "Privater Port" die Portnummern ein, die der virtuelle Computer verwenden soll. Sie können die Regeln für privaten Port und Firewall auf dem virtuellen Computer verwenden, um Datenverkehr entsprechend Ihren Anforderungen umzuleiten. Der private Port kann mit dem öffentlichen Port identisch sein. Für einen Endpunkt für Webverkehr (HTTP) könnten Sie zum Beispiel Port 80 sowohl für den öffentlichen als auch für den privaten Port verwenden.

5. Klicken Sie auf **Satz mit Lastenausgleich erstellen**und klicken Sie auf den Pfeil nach rechts.
6. Geben Sie auf der Seite **Gruppe mit Lastenausgleich konfigurieren** einen Namen für die Gruppe mit Lastenausgleich ein, und weisen Sie dann die Werte für das Testverhalten des Azure-Lastenausgleichsmoduls zu. Das Lastenausgleichsmodul führt Tests durch, um zu ermitteln, ob die virtuellen Computer in einem Satz mit Lastenausgleich empfangsbereit für eingehenden Datenverkehr sind.
7. Klicken Sie auf das Häkchen, um den Endpunkt mit Lastenausgleich zu erstellen. In der Spalte **Name der Gruppe mit Lastenausgleich** auf der Seite **Endpunkte** für den virtuellen Computer wird **Ja** angezeigt.
8. Klicken Sie im Portal auf **Virtuelle Computer**, auf den Namen eines weiteren virtuellen Computers in der Gruppe mit Lastenausgleich, auf **Endpunkte** und dann auf **Hinzufügen**.
9. Klicken Sie auf der Seite **Einem virtuellen Computer einen Endpunkt hinzufügen** auf **Add endpoint to an existing load-balanced set** (Einer vorhandenen Gruppe mit Lastenausgleich einen Endpunkt hinzufügen), wählen Sie den Namen der Gruppe mit Lastenausgleich aus, und klicken Sie auf den Pfeil nach rechts.
10. Geben Sie auf der Seite **Geben Sie die Details des Endpunktes ein** einen Namen für den Endpunkt ein, und klicken Sie auf das Häkchen.

Wiederholen Sie die Schritte 8 bis 10 für die weiteren virtuellen Computer in der Gruppe mit Lastenausgleich.

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Jan17_HO4-->


