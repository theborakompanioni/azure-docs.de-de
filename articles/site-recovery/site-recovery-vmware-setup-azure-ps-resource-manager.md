---
title: " Verwalten eines in Azure ausgeführten Prozessservers (Resource Manager) | Microsoft-Dokumentations"
description: In diesem Artikel wird beschrieben, wie ein Failbackprozessserver (Resource Manager) in Azure eingerichtet wird.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 2/2/2017
ms.author: anoopkv
translationtype: Human Translation
ms.sourcegitcommit: bb24e1c79f6a77ec157fa05a8a212109ce8ae7ad
ms.openlocfilehash: 8ea724c49664ca4803b1c91aefb94b490bd3d283

---

# <a name="manage-a-process-server-running-in-azure-resource-manager"></a>Verwalten eines in Azure ausgeführten Prozessservers (Resource Manager)
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [Klassisch ](./site-recovery-vmware-setup-azure-ps-classic.md)

Beim Failback empfiehlt es sich, den Prozessserver in Azure bereitzustellen, wenn die Wartezeit zwischen dem Azure Virtual Network und Ihrem lokalen Netzwerk hoch ist. In diesem Artikel wird beschrieben, wie Sie in Azure ausgeführte Prozessserver einrichten, konfigurieren und verwalten können.

> [!NOTE]
> Dieser Artikel gilt, wenn Sie **Resource Manager** beim Failover als Bereitstellungsmodell für die virtuellen Computer verwendet haben. Wenn Sie **Klassisch** als Bereitstellungsmodell ausgewählt haben, führen Sie die unter [Verwalten eines in Azure ausgeführten Prozessservers (Klassisch)](./site-recovery-vmware-setup-azure-ps-classic.md) beschriebenen Schritte aus.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Bereitstellen eines Prozessservers in Azure
1. Wählen Sie im Tresor > **Site Recovery-Infrastruktur** (unter der Überschrift „Verwalten“) > **Konfigurationsserver** (unter der Überschrift „For VMware and Physical Machines“ (Für VMware und physische Computer)) den Konfigurationsserver aus.
2. Klicken Sie auf der daraufhin geöffneten Detailseite des Konfigurationsservers auf „+ Prozessserver“.

  ![Prozessserverkatalog hinzufügen](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  Wählen Sie auf der Seite **Prozessserver hinzufügen** die folgenden Werte aus:

  ![Prozessserver-Katalogelement hinzufügen](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|**Feldname**|**Wert**|
|-|-|
|Wählen Sie, wo Sie Ihren Prozessserver bereitstellen möchten.|Wählen Sie den Wert **Deploy a failback process server in Azure** (Bereitstellen eines Failbackprozessservers in Azure) aus. |
|Abonnement|Wählen Sie das Azure-Abonnement aus, in dem Sie das Failover der virtuellen Computer ausgeführt haben.|
|Ressourcengruppe|Sie können für die Bereitstellung dieses Prozessservers eine Ressourcengruppe erstellen oder den Prozessserver in einer vorhandenen Ressourcengruppe bereitstellen.|
|Ort|Wählen Sie das Azure-Rechenzentrum aus, auf das das Failover der virtuellen Computer erfolgt ist.|
|Azure-Netzwerk|Wählen Sie das Azure Virtual Network (VNet) aus, auf das das Failover der virtuellen Computer erfolgt ist. Wenn das Failover der virtuellen Computer auf mehrere Azure VNets erfolgt ist, benötigen Sie einen über VNet bereitgestellten Prozessserver.|

4. Geben Sie die restlichen Eigenschaften für den Prozessserver ein.

  ![Fügen Sie eine Prozessserverzusammenfassung hinzu.](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|**Feldname**|**Wert**|
|-|-|
|Servername|Anzeigename und Hostname für Ihren virtuellen Prozessservercomputer|
| Benutzername|Name des Benutzers, der Administrator für diesen virtuellen Computer wird|
|Speicherkonto|Name des Speicherkontos, in dem sich die dem virtuellen Datenträger des virtuellen Computers befinden|
|Subnetz|Das Subnetz des Azure VNet, mit dem der virtuellen Computer verbunden ist|
| IP-Adresse|Die IP-Adresse, die der Prozessserver nach dem Hochfahren übernehmen soll.|
5. Klicken Sie auf die Schaltfläche „OK“, um mit der Bereitstellung des virtuellen Prozessservercomputers zu beginnen.

> [!NOTE]
> Damit Sie diesen Prozessserver für das Failback verwenden können, müssen Sie ihn beim lokalen Konfigurationsserver registrieren.

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>Registrieren des Prozessservers (in Azure ausgeführt) bei einem Konfigurationsserver (lokal ausgeführt)

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-the-process-server-to-latest-version"></a>Führen Sie ein Upgrade des Prozessservers auf die aktuelle Version durch.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-the-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>Aufheben der Registrierung des Prozessservers (in Azure ausgeführt) beim Konfigurationsserver (lokal ausgeführt)

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]



<!--HONumber=Feb17_HO1-->


