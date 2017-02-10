---
title: "Probleme beim Neustart oder Ändern der Größe eines virtuellen Computers | Microsoft-Dokumentation"
description: "Problembehandlung von Ressourcen-Manager-Bereitstellungsproblemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Linux-Computers in Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 51f1610c-0290-464a-97d4-c2e485c7ae7f
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.workload: required
ms.date: 09/09/2016
ms.author: delhan
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: 9f85988268d4c832c46da1790c83d4a78141f489


---
# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Problembehandlung von Ressourcen-Manager-Bereitstellungsproblemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Linux-Computers in Azure
Wenn Sie versuchen, einen beendeten virtuellen Azure-Computer zu starten oder die Größe eines vorhandenen virtuellen Azure-Computers zu ändern, tritt häufig ein Zuordnungsfehler auf. Dieser Fehler tritt auf, wenn in dem Cluster oder der Region keine Ressourcen verfügbar sind, oder wenn die angeforderte Größe des virtuellen Computers nicht unterstützt werden kann.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Sammeln von Überwachungsprotokollen
Sammeln Sie zur Problembehandlung zunächst die Überwachungsprotokolle, um den Fehler zu ermitteln, auf den das Problem zurückzuführen ist. Die folgenden Links enthalten detaillierte Informationen zur Vorgehensweise:

[Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Überwachen von Vorgängen mit dem Ressourcen-Manager](../resource-group-audit.md)

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
Wenn beim Erstellen eines neuen virtuellen Linux-Computers in Azure Probleme auftreten, finden Sie Informationen unter [Behandeln von Bereitstellungsproblemen beim Erstellen eines neuen virtuellen Linux-Computers in Azure](virtual-machines-linux-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).




<!--HONumber=Nov16_HO3-->


