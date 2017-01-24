---
title: "Azure Resource Manager-Unterstützung für Load Balancer | Microsoft Docs"
description: "Verwenden von PowerShell für Load Balancer mit Azure Resource Manager. Verwenden von Vorlagen für Lastenausgleich"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: d0394f11-ee5a-4407-9d86-79c936297265
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: b0bbde6828ed556eef8063a56a35b325fee1865c
ms.openlocfilehash: d06c924f384a2684b5a91c202039c581796c1091

---

# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Verwenden der Azure Resource Manager-Unterstützung mit Azure Load Balancer

Azure Resource Manager ist das bevorzugte Verwaltungsframework für Dienste in Azure. Azure Load Balancer kann mithilfe von APIs und Tools auf Basis von Azure Resource Manager verwaltet werden.

## <a name="concepts"></a>Konzepte

Mit Resource Manager enthält Azure Load Balancer die folgenden untergeordneten Ressourcen:

* Front-End-IP-Konfiguration – ein Lastenausgleichsmodul kann ein oder mehrere Front-End-IP-Adressen umfassen, auch als virtuelle IPs (VIPs) bekannt. Diese IP-Adressen dienen als Eingang für den Datenverkehr.
* Back-End-Adresspool – IP-Adressen, die den Netzwerkschnittstellenkarten der virtuellen Computer zugeordnet sind, an die die Last verteilt wird.
* Lastenausgleichsregeln – eine Regeleigenschaft ordnet eine bestimmte IP-/Port-Kombination des Front-Ends einer IP-Adressen-/Port-Kombination des Back-Ends zu. Ein einzelnes Lastenausgleichsmodul kann mehrere Lastenausgleichsregeln umfassen. Jede Regel ist eine Kombination aus der Front-End-IP und dem Front-End-Port sowie der Back-End-IP und dem Back-End-Port von virtuellen Computern.
* Überprüfungen – Mit Überprüfungen können Sie die Integrität der VM-Instanzen nachverfolgen. Schlägt eine Integritätsüberprüfung fehl, wird die VM-Instanz automatisch aus der Rotation entfernt.
* Eingehende NAT-Regeln – NAT-Regeln definieren den Eingangsdatenverkehr, der durch die Front-End-IP fließt und an die Back-End-IP verteilt wird.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Schnellstartvorlagen

Mit dem Azure-Ressourcen-Manager können Sie Ihre Anwendungen mit einer deklarativen Vorlage bereitstellen. In einer Vorlage können Sie mehrere Dienste zusammen mit ihren Abhängigkeiten bereitstellen. Sie verwenden die gleiche Vorlage, um Ihre Anwendung während jeder Phase des Anwendungslebenszyklus wiederholt bereitzustellen.

Vorlagen können Definitionen für virtuelle Computer, virtuelle Netzwerke, Verfügbarkeitsgruppen, Netzwerkkarten, Speicherkonten, Lastenausgleichsmodule, Netzwerksicherheitsgruppen und öffentliche IP-Adressen umfassen. Mit Vorlagen können Sie alle Elemente erstellen, die Sie für eine komplexe Anwendung benötigen. Die Vorlagendatei kann zur Versionskontrolle und Zusammenarbeit in ein Content Management-System eingecheckt werden.

[Weitere Informationen zu Vorlagen](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Weitere Informationen zu Netzwerkressourcen](../virtual-network/resource-groups-networking.md)

Schnellstartvorlagen, die Azure Load Balancer verwenden, finden Sie in diesem [GitHub-Repository](https://github.com/Azure/azure-quickstart-templates) , das eine Reihe von in der Community erstellten Vorlagen enthält.

Beispiele für Vorlagen:

* [2 VMs in a Load Balancer and load balancing rules (in englischer Sprache)](http://go.microsoft.com/fwlink/?LinkId=544799)
* [2 VMs in a VNET with an Internal Load Balancer and Load Balancer rules (in englischer Sprache)](http://go.microsoft.com/fwlink/?LinkId=544800)
* [2 VMs in a Load Balancer and configure NAT rules on the LB (in englischer Sprache)](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Einrichten von Azure-Lastenausgleich per PowerShell oder CLI

Erste Schritte mit Cmdlets, Befehlszeilentools und REST-APIs von Azure Resource Manager

* [Azure Networking Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) können verwendet werden, um einen Lastenausgleich zu erstellen.
* [Erstellen eines Load Balancers mit dem Azure-Ressourcen-Manager](load-balancer-get-started-ilb-arm-ps.md)
* [Verwenden der Azure-CLI mit der Azure-Ressourcenverwaltung](../xplat-cli-azure-resource-manager.md)
* [Load Balancer REST APIs (in englischer Sprache)](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Nächste Schritte

Sie können auch einen [Lastenausgleich für Internetzugriff erstellen](load-balancer-get-started-internet-arm-ps.md) und die Art des [Verteilungsmodus](load-balancer-distribution-mode.md) des Lastenausgleichs für ein bestimmtes Datenverkehrsverhalten im Netzwerk konfigurieren.

Erfahren Sie, wie Sie [TCP-Leerlauftimeout-Einstellungen für ein Lastenausgleichsmodul](load-balancer-tcp-idle-timeout.md)verwalten. Dies ist wichtig, wenn Ihre Anwendung Verbindungen für Server hinter einem Lastenausgleichsmodul beibehalten muss.



<!--HONumber=Jan17_HO1-->


