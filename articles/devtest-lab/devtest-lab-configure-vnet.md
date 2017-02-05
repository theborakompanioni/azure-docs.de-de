---
title: Konfigurieren eines virtuellen Netzwerks in Azure DevTest Labs | Microsoft Docs
description: Erfahren Sie, wie Sie mit Azure DevTest Labs ein vorhandenes virtuelles Netzwerk und ein Subnetz konfigurieren und auf einem virtuellen Computer verwenden.
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1821aa2a3d6428ca98cad14b0bf5e8b37ec6cb9c


---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Konfigurieren eines virtuellen Netzwerks in Azure DevTest Labs
Wie im Artikel [Hinzufügen einer VM mit Artefakten zu einem Lab](devtest-lab-add-vm-with-artifacts.md)erläutert, können Sie beim Erstellen eines virtuellen Computers in einem Lab ein konfiguriertes virtuelles Netzwerk angeben. Ein Szenario hierfür wäre, wenn Sie von Ihren virtuellen Computern aus auf Ihre Unternehmensnetzwerkressourcen zugreifen müssen, und zwar über ein virtuelles Netzwerk, das mit ExpressRoute oder Site-to-Site-VPN konfiguriert wurde. In den folgenden Abschnitten wird veranschaulicht, wie Sie Ihr vorhandenes virtuelles Netzwerk in den Einstellungen für virtuelle Netzwerke eines Labs hinzufügen, sodass es beim Erstellen Ihrer virtuellen Computer zur Auswahl steht.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Konfigurieren eines virtuellen Netzwerks für ein Lab über das Azure-Portal
Die folgenden Schritte führen Sie durch das Hinzufügen eines vorhandenen virtuellen Netzwerks (und Subnetzes) zu einem Lab, sodass es beim Erstellen eines virtuellen Computers in dem gleichen Lab verwendet werden kann. 

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus. 
4. Wählen Sie auf dem Blatt des Labs **Konfiguration**aus.
5. Wählen Sie auf dem Blatt **Konfiguration** für das Lab die Option **Virtuelle Netzwerke** aus.
6. Auf dem Blatt **Virtuelle Netzwerke** sehen Sie eine Liste der virtuellen Netzwerke, die für das aktuelle Lab konfiguriert wurden, sowie das standardmäßige virtuelle Netzwerk, das für Ihr Lab erstellt wird. 
7. Wählen Sie **+ Hinzufügen**.
   
    ![Fügen Sie Ihrem Lab ein vorhandenes virtuelles Netzwerk hinzu.](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. Wählen Sie auf dem Blatt **Virtuelles Netzwerk** **[Virtuelles Netzwerk auswählen]**.
   
    ![Wählen Sie ein vorhandenes virtuelles Netzwerk aus.](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. Wählen Sie auf dem Blatt **Virtuelles Netzwerk wählen** das gewünschte virtuelle Netzwerk aus. Das Blatt zeigt alle virtuellen Netzwerke, die sich im Abonnement in derselben Region wie das Lab befinden.  
10. Nachdem Sie ein virtuelles Netzwerk ausgewählt haben, wird Ihnen wieder das Blatt **Virtuelles Netzwerk** angezeigt, und mehrere Felder sind aktiviert.  
    
     ![Wählen Sie ein vorhandenes virtuelles Netzwerk aus.](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
11. Geben Sie eine Beschreibung für Ihre Kombination aus virtuellem Netzwerk und Lab an.
12. Damit ein Subnetz bei der Lab-VM-Erstellung verwendet werden kann, aktivieren Sie die Option **USE IN VIRTUAL MACHINE CREATION**(BEIM ERSTELLEN VIRTUELLER COMPUTER VERWENDEN).
13. Um die Verwendung öffentlicher IP-Adressen in einem Subnetz zu ermöglichen, aktivieren Sie die Option **ALLOW PUBLIC IP**(ÖFFENTLICHE IP-ADRESSE ZULASSEN).
14. Geben Sie im Feld **MAX. VIRTUELLE COMPUTER PRO BENUTZER** die maximale Anzahl von virtuellen Computern pro Benutzer für jedes Subnetz an. Wenn eine unbeschränkte Anzahl von VMs verwendet werden soll, lassen Sie dieses Feld leer.
15. Wählen Sie **Speichern**aus.
16. Jetzt ist das virtuelle Netzwerk konfiguriert und kann beim Erstellen eines virtuellen Computers ausgewählt werden. 
    Informationen zum Erstellen eines virtuellen Computers und zum Angeben eines virtuellen Netzwerks finden Sie im Artikel [Hinzufügen einer VM mit Artefakten zu einem Lab](devtest-lab-add-vm-with-artifacts.md). 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihrem Lab das gewünschte virtuelle Netzwerk hinzugefügt haben, besteht der nächste Schritt darin, [Ihrem Lab einen virtuellen Computer hinzuzufügen](devtest-lab-add-vm-with-artifacts.md).




<!--HONumber=Nov16_HO3-->


