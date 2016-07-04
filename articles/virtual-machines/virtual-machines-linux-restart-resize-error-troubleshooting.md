<properties
   pageTitle="Probleme beim Neustart oder Ändern der Größe eines virtuellen Computers | Microsoft Azure"
   description="Problembehandlung von Ressourcen-Manager-Bereitstellungsproblemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Linux-Computers in Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="06/16/2016"
   ms.author="delhan"/>

# Problembehandlung von Ressourcen-Manager-Bereitstellungsproblemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Linux-Computers in Azure

> [AZURE.SELECTOR]
- [Klassisch](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Ressourcen-Manager](../articles/virtual-machines/virtual-machines-linux-restart-resize-error-troubleshooting.md)

Wenn Sie versuchen, einen beendeten virtuellen Azure-Computer zu starten oder die Größe eines vorhandenen virtuellen Azure-Computers zu ändern, tritt häufig ein Zuordnungsfehler auf. Dieser Fehler tritt auf, wenn in dem Cluster oder der Region keine Ressourcen verfügbar sind, oder wenn die angeforderte Größe des virtuellen Computers nicht unterstützt werden kann.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Sammeln von Überwachungsprotokollen

Sammeln Sie zur Problembehandlung zunächst die Überwachungsprotokolle, um den Fehler zu ermitteln, auf den das Problem zurückzuführen ist. Die folgenden Links enthalten detaillierte Informationen zur Vorgehensweise:

[Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Überwachen von Vorgängen mit dem Ressourcen-Manager](../resource-group-audit.md)

## Problem: Fehler beim Starten eines angehaltenen virtuellen Computers

Sie versuchen, einen angehaltenen virtuellen Computer zu starten, erhalten aber eine Zuordnungsfehlermeldung.

### Ursache

Die Anforderung zum Start des angehaltenen virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Im Cluster ist jedoch nicht genügend freier Speicherplatz verfügbar, um die Anforderung zu erfüllen.

### Lösung

*	Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe, und starten Sie dann jeden virtuellen Computer neu.

  1. Klicken Sie auf **Ressourcengruppen** > _Ihre Ressourcengruppe_ > **Ressourcen** > _Ihre Verfügbarkeitsgruppe_ > **Virtuelle Computer** > _Ihr virtueller Computer_ > **Beenden**.

  2. Nachdem alle virtuellen Computer angehalten wurden, wählen Sie jeden einzelnen angehaltenen virtuellen Computer aus, und klicken Sie auf "Start".

*	Wiederholen Sie die Neustartanforderung zu einem späteren Zeitpunkt.

## Problem: Fehler beim Ändern der Größe eines vorhandenen virtuellen Computers

Sie versuchen, die Größe eines angehaltenen virtuellen Computers zu ändern, erhalten aber eine Zuordnungsfehlermeldung.

### Ursache

Die Anforderung zur Größenänderung des virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Der Cluster unterstützt jedoch nicht die angeforderte Größe des virtuellen Computers.

### Lösung

* Wiederholen Sie die Anforderung mit einer geringeren Größe des virtuellen Computers.

* Wenn die Größe des angeforderten virtuellen Computers nicht geändert werden kann:

  1. Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe.

    * Klicken Sie auf **Ressourcengruppen** > _Ihre Ressourcengruppe_ > **Ressourcen** > _Ihre Verfügbarkeitsgruppe_ > **Virtuelle Computer** > _Ihr virtueller Computer_ > **Beenden**.

  2. Nachdem alle virtuellen Computer beendet wurden, vergrößern Sie den gewünschten virtuellen Computer.
  3. Wählen Sie den virtuellen Computer mit veränderter Größe aus, und klicken Sie auf **Starten**. Starten Sie dann jeden einzelnen beendeten virtuellen Computer.

## Nächste Schritte

Wenn beim Erstellen eines neuen virtuellen Linux-Computers in Azure Probleme auftreten, finden Sie Informationen unter [Behandeln von Bereitstellungsproblemen beim Erstellen eines neuen virtuellen Linux-Computers in Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).

<!---HONumber=AcomDC_0622_2016-->