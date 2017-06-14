---
title: "Probleme beim Neustart oder Ändern der Größe eines virtuellen Computers in Azure | Microsoft Docs"
description: "Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Windows-Computers in Azure (Resource Manager-Bereitstellungsmodell)"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 06/13/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: a99af3b7087d5d7af2687a080deb05ed566fdb63
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017


---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Behandeln von Bereitstellungsproblemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Windows-Computers in Azure
Wenn Sie versuchen, einen beendeten virtuellen Azure-Computer zu starten oder die Größe eines vorhandenen virtuellen Azure-Computers zu ändern, tritt häufig ein Zuordnungsfehler auf. Dieser Fehler tritt auf, wenn in dem Cluster oder der Region keine Ressourcen verfügbar sind, oder wenn die angeforderte Größe des virtuellen Computers nicht unterstützt werden kann.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Erfassen von Aktivitätsprotokollen
Sammeln Sie zur Problembehandlung zunächst die Aktivitätsprotokolle, um den Fehler zu ermitteln, auf den das Problem zurückzuführen ist. Die folgenden Links enthalten detaillierte Informationen zur Vorgehensweise:

[Anzeigen von Bereitstellungsvorgängen](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Anzeigen von Aktivitätsprotokollen zum Verwalten von Azure-Ressourcen](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Fehler beim Starten eines angehaltenen virtuellen Computers
Sie versuchen, einen angehaltenen virtuellen Computer zu starten, erhalten aber eine Zuordnungsfehlermeldung.

### <a name="cause"></a>Ursache
Die Anforderung zum Start des angehaltenen virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Im Cluster ist jedoch nicht genügend freier Speicherplatz verfügbar, um die Anforderung zu erfüllen.

### <a name="resolution"></a>Lösung
* Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe, und starten Sie dann jeden virtuellen Computer neu.
  
  1. Klicken Sie auf **Ressourcengruppen** > *Ihre Ressourcengruppe* > **Ressourcen** > *Ihre Verfügbarkeitsgruppe* > **Virtuelle Computer** > *Ihr virtueller Computer* > **Beenden**.
  2. Nachdem alle virtuellen Computer angehalten wurden, wählen Sie jeden einzelnen angehaltenen virtuellen Computer aus, und klicken Sie auf "Start".
* Wiederholen Sie die Neustartanforderung zu einem späteren Zeitpunkt.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Fehler beim Ändern der Größe eines vorhandenen virtuellen Computers
Sie versuchen, die Größe eines angehaltenen virtuellen Computers zu ändern, erhalten aber eine Zuordnungsfehlermeldung.

### <a name="cause"></a>Ursache
Die Anforderung zur Größenänderung des virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Der Cluster unterstützt jedoch nicht die angeforderte Größe des virtuellen Computers.

### <a name="resolution"></a>Lösung
* Wiederholen Sie die Anforderung mit einer geringeren Größe des virtuellen Computers.
* Wenn die Größe des angeforderten virtuellen Computers nicht geändert werden kann:
  
  1. Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe.
     
     * Klicken Sie auf **Ressourcengruppen** > *Ihre Ressourcengruppe* > **Ressourcen** > *Ihre Verfügbarkeitsgruppe* > **Virtuelle Computer** > *Ihr virtueller Computer* > **Beenden**.
  2. Nachdem alle virtuellen Computer beendet wurden, vergrößern Sie den gewünschten virtuellen Computer.
  3. Wählen Sie den virtuellen Computer mit der geänderten Größe aus, und klicken Sie auf **Starten**. Starten Sie dann jeden einzelnen beendeten virtuellen Computer.

## <a name="next-steps"></a>Nächste Schritte
Wenn beim Erstellen eines neuen virtuellen Windows-Computers in Azure Probleme auftreten, finden Sie Informationen unter [Behandeln von Bereitstellungsproblemen beim Erstellen eines neuen virtuellen Windows-Computers in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


