---
title: "Probleme beim Neustart oder Ändern der Größe eines virtuellen Computers | Microsoft-Dokumentation"
description: "Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Linux-Computers in Azure (klassisches Bereitstellungsmodell)"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 73f2672c-602e-4766-8948-2b180115d299
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: required
ms.date: 01/10/2017
ms.devlang: na
ms.author: delhan
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: c6d4ed45133dc3f4b1f3d17fb5a87d3bf77aa3f7
ms.lasthandoff: 04/03/2017


---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Linux-Computers in Azure (klassisches Bereitstellungsmodell)
> [!div class="op_single_selector"]
> * [Klassisch](restart-resize-error-troubleshooting.md)
> * [Ressourcen-Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

Wenn Sie versuchen, einen beendeten virtuellen Azure-Computer zu starten oder die Größe eines vorhandenen virtuellen Azure-Computers zu ändern, tritt häufig ein Zuordnungsfehler auf. Dieser Fehler tritt auf, wenn in dem Cluster oder der Region keine Ressourcen verfügbar sind, oder wenn die angeforderte Größe des virtuellen Computers nicht unterstützt werden kann.

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Die Resource Manager-Version finden Sie [hier](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Sammeln von Überwachungsprotokollen
Sammeln Sie zur Problembehandlung zunächst die Überwachungsprotokolle, um den Fehler zu ermitteln, auf den das Problem zurückzuführen ist.

Klicken Sie im Azure-Portal auf **Durchsuchen** > **Virtuelle Computer** > *Ihr virtueller Linux-Computer* > **Einstellungen** > **Überwachungsprotokolle**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Fehler beim Starten eines angehaltenen virtuellen Computers
Sie versuchen, einen angehaltenen virtuellen Computer zu starten, erhalten aber eine Zuordnungsfehlermeldung.

### <a name="cause"></a>Ursache
Die Anforderung zum Start des angehaltenen virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Im Cluster ist jedoch nicht genügend freier Speicherplatz verfügbar, um die Anforderung zu erfüllen.

### <a name="resolution"></a>Lösung
* Erstellen Sie einen neuen Clouddienst, und ordnen Sie ihn einer Region oder einem regionsbasierten virtuellen Netzwerk, jedoch keiner Affinitätsgruppe zu.
* Löschen Sie den angehaltenen virtuellen Computer.
* Erstellen Sie den virtuellen Computer durch Verwendung der Datenträger im neuen Clouddienst neu.
* Starten Sie den neu erstellten virtuellen Computer.

Sollte bei der Erstellung eines neuen Clouddiensts ein Fehler auftreten, wiederholen Sie den Vorgang zu einem späteren Zeitpunkt, oder ändern Sie die Region für den Clouddienst.

> [!IMPORTANT]
> Der neue Clouddienst trägt einen neuen Namen und eine neue VIP, sodass Sie diese Informationen für alle Abhängigkeiten ändern müssen, die diese Informationen für den vorhandenen Clouddienst verwenden.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Fehler beim Ändern der Größe eines vorhandenen virtuellen Computers
Sie versuchen, die Größe eines angehaltenen virtuellen Computers zu ändern, erhalten aber eine Zuordnungsfehlermeldung.

### <a name="cause"></a>Ursache
Die Anforderung zur Größenänderung des virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Der Cluster unterstützt jedoch nicht die angeforderte Größe des virtuellen Computers.

### <a name="resolution"></a>Lösung
Verringern Sie die angeforderte Größe des virtuellen Computers, und wiederholen Sie die Anforderung zum Ändern der Größe.

* Klicken Sie auf **Alle durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Computer* > **Einstellungen** > **Größe**. Ausführliche Schritte finden Sie unter [Ändern der Größe des virtuellen Computers](https://msdn.microsoft.com/library/dn168976.aspx).

Wenn die Größe des virtuellen Computers nicht verringert werden kann, führen Sie die folgenden Schritte aus:

* Erstellen Sie einen neuen Clouddienst, und stellen Sie dabei sicher, dass er weder mit einer Affinitätsgruppe verknüpft, noch einem virtuellen Netzwerk zugeordnet ist, das mit einer Affinitätsgruppe verknüpft ist.
* Erstellen Sie in diesem Clouddienst einen neuen größeren virtuellen Computer.

Sie können alle Ihre virtuellen Computer im gleichen Clouddienst zusammenführen. Wenn Ihr vorhandener Clouddienst einem regionsbasierten virtuellen Netzwerk zugeordnet ist, können Sie den neuen Clouddienst mit dem vorhandenen virtuellen Netzwerk verbinden.

Wenn der vorhandene Clouddienst nicht einem regionsbasierten virtuellen Netzwerk zugeordnet ist, müssen Sie die virtuellen Computer im vorhandenen Clouddienst löschen und über die zugehörigen Datenträger im neuen Clouddienst neu erstellen. Berücksichtigen Sie dabei allerdings, dass der neue Clouddienst einen neuen Namen und eine neue VIP besitzt und diese für alle Abhängigkeiten aktualisiert werden müssen, die derzeit diese Informationen für den vorhandenen Clouddienst verwenden.

## <a name="next-steps"></a>Nächste Schritte
Wenn beim Erstellen eines neuen virtuellen Linux-Computers in Azure Probleme auftreten, finden Sie Informationen unter [Behandeln von Bereitstellungsproblemen beim Erstellen eines neuen virtuellen Linux-Computers in Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


