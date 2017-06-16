---
title: " Verwalten eines in Azure ausgeführten Prozessservers (Klassisch) | Microsoft-Dokumentation"
description: In diesem Artikel wird beschrieben, wie ein Failbackprozessserver (Klassisch) in Azure eingerichtet wird.
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
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: anoopkv
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: cceda131fc9eaa74f6328c8dd68a8e3885877061
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017

---

# <a name="manage-a-process-server-running-in-azure-classic"></a>Verwalten eines in Azure ausgeführten Prozessservers (Classic)
> [!div class="op_single_selector"]
> * [Azure – klassisch](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [Ressourcen-Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

Beim Failback empfiehlt es sich den Prozessserver in Azure bereitzustellen, wenn die Wartezeit zwischen dem Azure Virtual Network und Ihrem lokalen Netzwerk hoch ist. In diesem Artikel wird beschrieben, wie Sie in Azure ausgeführte Prozessserver einrichten, konfigurieren und verwalten können.

> [!NOTE]
> Dieser Artikel gilt, wenn Sie beim Failover das klassische Bereitstellungsmodell für die virtuellen Computer verwendet haben. Wenn Sie Resource Manager als Bereitstellungsmodell ausgewählt haben, führen Sie die unter [Verwalten eines in Azure ausgeführten Prozessservers (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md) beschriebenen Schritte aus.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Bereitstellen eines Prozessservers in Azure

1. Erstellen Sie im Azure Marketplace mit dem **Microsoft Azure Site Recovery Process Server V2** einen virtuellen Computer. </br>
    ![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)
2. Wählen Sie das Bereitstellungsmodell **Klassisch** aus. </br>
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)
3. Wählen Sie unter „Create virtual machine wizard“ (Assistent zum Erstellen virtueller Computer) > „Grundeinstellungen“ das Abonnement und den Speicherort für das Failover des virtuellen Computers aus.</br>
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)
4. Der virtuelle Computer muss mit dem Azure Virtual Network verbunden sein, mit dem auch der virtuelle Computer verbunden ist, für den ein Failover ausgeführt wurde.</br>
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)
5. Wenn der virtuelle Prozessservercomputer bereitgestellt wurde, melden Sie sich an und registrieren Sie ihn beim Konfigurationsserver.

> [!NOTE]
> Damit Sie diesen Prozessserver für das Failback verwenden können, müssen Sie ihn beim lokalen Konfigurationsserver registrieren.

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>Registrieren des Prozessservers (in Azure ausgeführt) bei einem Konfigurationsserver (lokal ausgeführt)

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-the-process-server-to-latest-version"></a>Führen Sie ein Upgrade des Prozessservers auf die aktuelle Version durch.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-the-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>Aufheben der Registrierung des Prozessservers (in Azure ausgeführt) beim Konfigurationsserver (lokal ausgeführt)

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

