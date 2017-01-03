---
title: Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Netzwerke | Microsoft Docs
description: Hier erhalten Sie Informationen zur Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Netzwerke.
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: dbb749b056c36f16e921933fb8345211e3eaa697


---
# <a name="virtual-network--business-continuity"></a>Virtuelles Netzwerk: Geschäftskontinuität
## <a name="overview"></a>Übersicht
Ein virtuelles Netzwerk (VNet) ist eine logische Darstellung Ihres Netzwerks in der Cloud. Es ermöglicht Ihnen das Definieren Ihres eigenen privaten IP-Adressraums und das Segmentieren des Netzwerks in Subnetze. VNets dienen als Vertrauensstellungsgrenze beim Hosten von Computeressourcen wie Azure Virtual Machines und Cloud Services (Web-/Workerrollen). Ein VNet ermöglicht die direkte private IP-Kommunikation zwischen den darin gehosteten Ressourcen. Ein virtuelles Netzwerk kann auch über eine der Hybridoptionen wie VPN-Gateway oder ExpressRoute mit einem lokalen Netzwerk verknüpft werden.

Ein VNet wird innerhalb des Geltungsbereichs einer Region erstellt. Sie können VNets mit demselben Adressraum in zwei verschiedenen Regionen erstellen (z. B. USA, Osten und USA, Westen), sie aber nicht direkt mit einer anderen verbinden. 

## <a name="business-continuity"></a>Geschäftskontinuität
Es gibt verschiedene Möglichkeiten, warum Ihre Anwendung unterbrochen werden kann. Eine bestimmte Region kann aufgrund einer Naturkatastrophe oder eines Teilausfalls mehrerer Geräte und Dienste vollständig abgeschnitten sein. Die Auswirkung auf den VNet-Dienst unterscheidet sich in allen diesen Fällen.

**F: Was ist beim Ausfall einer gesamten Region, z.B. aufgrund einer Naturkatastrophe, zu tun? Was geschieht mit den virtuellen Netzwerken, die in der Region gehostet werden?**

A: Auf das virtuelle Netzwerk und die Ressourcen in der betroffenen Region kann während der Dienstunterbrechung nicht zugegriffen werden.

![Diagramm eines einfachen virtuellen Netzwerks](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**F: Was kann ich tun, um dasselbe virtuelle Netzwerk in einer anderen Region neu zu erstellen?**

A: Ein virtuelles Netzwerk (VNet) ist eine relativ schlanke Ressource. Sie können Azure-APIs zum Erstellen eines VNet mit dem gleichen Adressraum in einer anderen Region aufrufen. Um die gleiche Umgebung neu zu erstellen, die in der betroffenen Region vorhanden war, müssen Sie APIs aufrufen, um Ihre Clouddienste (Web-/Workerrollen) und virtuellen Computer erneut bereitzustellen. Sie müssen auch ein VPN-Gateway einrichten und die Verbindung mit Ihrem lokalen Netzwerk herstellen, sofern eine solche Verbindung vorhanden war (z.B. bei einer Hybridbereitstellung).

Die Anweisungen zum Erstellen eines VNet finden Sie [hier](virtual-networks-create-vnet-arm-pportal.md). 

**F: Kann ein Replikat eines VNet in einer bestimmten Region in einer anderen Region im Voraus neu erstellt werden?**

A: Ja, Sie können im Voraus zwei VNets erstellen, die denselben privaten IP-Adressraum und dieselben Ressourcen in zwei verschiedenen Regionen nutzen. Wenn ein Kunde im VNet Dienste mit Internetzugriff hostet, kann er Traffic Manager einrichten, um den Datenverkehr geografisch an die aktive Region zu leiten. Ein Kunde kann jedoch nicht zwei VNets mit demselben Adressraum mit dem lokalen Netzwerk verbinden, da dies Routingprobleme verursachen würde. Im Falle eines Notfalls und Verlusts eines VNet in einer Region kann ein Kunde das andere VNet in der verfügbaren Region mit dem übereinstimmenden Adressraum mit dem lokalen Netzwerk verbinden.

Die Anweisungen zum Erstellen eines VNet finden Sie [hier](virtual-networks-create-vnet-arm-pportal.md).




<!--HONumber=Nov16_HO3-->


