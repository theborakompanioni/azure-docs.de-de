---
title: "Probleme beim Neustart oder Ändern der Größe eines virtuellen Computers | Microsoft-Dokumentation"
description: "Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Windows-Computers in Azure (klassisches Bereitstellungsmodell)"
services: virtual-machines-windows
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 01/10/2017
ms.devlang: na
ms.author: delhan
translationtype: Human Translation
ms.sourcegitcommit: 66b1bcdf0f79ff4743f466c3737696f53ef6a44c
ms.openlocfilehash: 6bae5c227fb45b1a6ed25c4eb7aac2048fc856c1


---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Windows-Computers in Azure (klassisches Bereitstellungsmodell)
> [!div class="op_single_selector"]
> * [Klassisch](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Ressourcen-Manager](../../virtual-machines-windows-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

Wenn Sie versuchen, einen beendeten virtuellen Azure-Computer zu starten oder die Größe eines vorhandenen virtuellen Azure-Computers zu ändern, tritt häufig ein Zuordnungsfehler auf. Dieser Fehler tritt auf, wenn in dem Cluster oder der Region keine Ressourcen verfügbar sind, oder wenn die angeforderte Größe des virtuellen Computers nicht unterstützt werden kann.

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../../../azure-resource-manager/resource-manager-deployment-model.md).  Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Sammeln von Überwachungsprotokollen
Sammeln Sie zur Problembehandlung zunächst die Überwachungsprotokolle, um den Fehler zu ermitteln, auf den das Problem zurückzuführen ist.

Klicken Sie im Azure-Portal auf **Durchsuchen** > **Virtuelle Computer** > *Ihr virtueller Windows-Computer* > **Einstellungen** > **Überwachungsprotokolle**.

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
Wenn beim Erstellen eines neuen virtuellen Windows-Computers in Azure Probleme auftreten, finden Sie Informationen unter [Behandeln von Bereitstellungsproblemen beim Erstellen eines neuen virtuellen Windows-Computers in Azure](../../virtual-machines-windows-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).




<!--HONumber=Nov16_HO4-->


