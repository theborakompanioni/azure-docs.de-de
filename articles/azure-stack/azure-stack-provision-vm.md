---
title: Erstellen eines virtuellen Testcomputers in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie als Cloudbetreiber einen virtuellen Testcomputer in Azure Stack bereitstellen.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: c86646e1-a12e-493f-b396-f17bfacd60c2
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/21/2017
ms.author: erikje
ms.translationtype: HT
ms.sourcegitcommit: 2812039649f7d2fb0705220854e4d8d0a031d31e
ms.openlocfilehash: 4e3f90fe35b7fb2509db0eb7a467305f4c4167ef
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="create-a-test-virtual-machine-in-azure-stack"></a>Erstellen eines virtuellen Testcomputers in Azure Stack
Als Cloudbetreiber können Sie einen virtuellen Testcomputer zum Überprüfen Ihrer Azure Stack-Bereitstellung erstellen.

> [!NOTE]
> Bevor Sie virtuelle Computer bereitstellen können, müssen Sie dem [Azure Stack-Marketplace das Windows Server 2016 Evaluation-Image hinzufügen](azure-stack-add-default-image.md).
> 
> 

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
1. Sie müssen sich auf dem Azure Stack Development Kit-Host beim Administratorportal (`https://adminportal.local.azurestack.external`) [anmelden](azure-stack-connect-azure-stack.md). Klicken Sie dann auf **Neu** > **Compute** > **Windows Server 2016 Datacenter Eval** > **Erstellen**.  
2. Geben Sie auf dem Blatt **Grundlagen** Werte für **Name**, **Benutzername** und **Kennwort** ein. Wählen Sie ein **Abonnement**aus. Erstellen Sie eine **Ressourcengruppe**, oder wählen Sie eine vorhandene aus, und klicken Sie anschließend auf **OK**.  
3. Klicken Sie auf dem Blatt **Größe auswählen** auf **A1 Standard** und anschließend auf **Auswählen**.  
4. Behalten Sie auf dem Blatt **Einstellungen** die Standardwerte bei, und klicken Sie auf **OK**.
5. Klicken Sie auf dem Blatt **Zusammenfassung** auf **OK**, um den virtuellen Computer zu erstellen.  
6. Klicken Sie zum Anzeigen Ihres neuen virtuellen Computers auf **Alle Ressourcen**, und suchen Sie dann nach dem virtuellen Computer. Klicken Sie auf seinen Namen.
    ![](media/azure-stack-provision-vm/image06.png)


## <a name="next-steps"></a>Nächste Schritte
[Verwenden des Administrator- und des Benutzerportals in Azure Stack](azure-stack-manage-portals.md)

