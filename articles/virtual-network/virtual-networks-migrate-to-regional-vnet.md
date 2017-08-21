---
title: "Migrieren eines (klassischen) virtuellen Azure-Netzwerks aus einer Affinitätsgruppe in eine Region | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie ein (klassisches) virtuelles Netzwerk aus einer Affinitätsgruppe in eine Region migrieren."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: b9b3bd0f2184ac85261166d5fe2ab67e1bf319d4
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-to-a-region"></a>Migrieren eines (klassischen) virtuellen Netzwerks aus einer Affinitätsgruppe in eine Region

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und klassische Bereitstellungen](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Resource Manager-Bereitstellungsmodells.

Mit einer Affinitätsgruppe können Sie sicherstellen, dass Ressourcen, die innerhalb der gleichen Affinitätsgruppe erstellt werden, physisch von Servern gehostet werden, die sich in geringem Abstand zueinander befinden. Auf diese Weise können die Ressourcen schneller miteinander kommunizieren. In der Vergangenheit waren Affinitätsgruppen eine Voraussetzung für die Erstellung (klassischer) virtueller Netzwerke. Damals konnte der Netzwerk-Manager-Dienst, der die (klassischen) virtuellen Netzwerke verwaltete, nur innerhalb einer Gruppe physischer Server oder innerhalb einer Skalierungseinheit verwendet werden. Aufgrund von Verbesserungen bei der Architektur konnte die Netzwerkverwaltung inzwischen jedoch auf eine Region ausgeweitet werden.

Deshalb ist die Verwendung von Affinitätsgruppen nicht mehr empfehlenswert und für (klassische) virtuelle Netzwerke auch nicht mehr erforderlich. Die Verwendung von Affinitätsgruppen für (klassische) virtuelle Netzwerke wird durch Regionen ersetzt. (Klassische) virtuelle Netzwerke mit regionaler Zuordnung werden als regionale virtuelle Netzwerke bezeichnet.

Es wird grundsätzlich von der Verwendung von Affinitätsgruppen abgeraten. Abgesehen von der virtuellen Netzwerkanforderung wurde mit Affinitätsgruppen auch sichergestellt, dass etwa (klassische) Compute- und (klassische) Speicherressourcen nah beieinander platziert wurden. Dank der aktuellen Netzwerkarchitektur von Azure ist dies jedoch nicht mehr erforderlich.

> [!IMPORTANT]
> Die Erstellung eines virtuellen Netzwerks mit Zuordnung zu einer Affinitätsgruppe ist zwar weiterhin technisch möglich, aber nicht mehr notwendig. Viele neue Funktionen von virtuellen Netzwerken wie etwa Netzwerksicherheitsgruppen stehen nur bei Verwendung eines regionalen virtuellen Netzwerks und nicht bei Verwendung virtueller Netzwerke mit Affinitätsgruppenzuordnung zur Verfügung.
> 
> 

## <a name="edit-the-network-configuration-file"></a>Bearbeiten Sie die Netzwerkkonfigurationsdatei

1. Exportieren Sie die Netzwerkkonfigurationsdatei. Um zu erfahren, wie Sie eine Netzwerkkonfigurationsdatei mit PowerShell oder der Azure-Befehlszeilenschnittstelle (CLI) 1.0 exportieren können, konsultieren Sie [Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md#export).
2. Bearbeiten Sie die Netzwerkkonfigurationsdatei, und ersetzen Sie dabei **AffinityGroup** durch **Location**. Geben Sie eine Azure-[Region](https://azure.microsoft.com/regions) für **Location** (Standort) an.
   
   > [!NOTE]
   > Geben Sie für **Location** die Region an, die Sie auch für die Affinitätsgruppe angegeben haben, die Ihrem (klassischen) virtuellen Netzwerk zugeordnet ist. Ist Ihr (klassisches) virtuelles Netzwerk also beispielsweise einer Affinitätsgruppe im Westen der USA zugeordnet, müssen Sie bei der Migration für **Location** ebenfalls den Westen der USA angeben. 
   > 
   > 
   
    Bearbeiten Sie die folgenden Zeilen Ihrer Netzwerkkonfigurationsdatei, und ersetzen Sie dabei die Werte durch Ihre eigenen Werte: 
   
    **Alter Wert:** \<VirtualNetworkSitename="VNetUSWest" AffinityGroup="VNetDemoAG"\> 
   
    **Neuer Wert:** \<VirtualNetworkSitename="VNetUSWest" Location="West US"\>
3. Speichern Sie die Änderungen, und [importieren](virtual-networks-using-network-configuration-file.md#import) Sie die Netzwerkkonfiguration in Azure.

> [!NOTE]
> Bei dieser Migration kommt es zu keinerlei Ausfallzeiten für Ihre Dienste.
> 
> 

## <a name="what-to-do-if-you-have-a-vm-classic-in-an-affinity-group"></a>Vorgehensweise bei Verwendung eines (klassischen) virtuellen Computers in einer Affinitätsgruppe
(Klassische) Virtuelle Computer, die sich derzeit in einer Affinitätsgruppe befinden, müssen nicht aus der Gruppe entfernt werden. Die Bereitstellung eines virtuellen Computers erfolgt in einer einzelnen Skalierungseinheit. Affinitätsgruppen können zwar den Satz verfügbarer VM-Größen für eine neue VM-Bereitstellung einschränken, alle vorhandenen und bereits bereitgestellten virtuellen Computer sind jedoch bereits auf den Satz von VM-Größen beschränkt, der in der entsprechenden Skalierungseinheit verfügbar ist. Da der virtuelle Computer bereits für eine Skalierungseinheit bereitgestellt wurde, hat das Entfernen eines virtuellen Computers aus der Affinitätsgruppe keine Auswirkung auf den virtuellen Computer.

