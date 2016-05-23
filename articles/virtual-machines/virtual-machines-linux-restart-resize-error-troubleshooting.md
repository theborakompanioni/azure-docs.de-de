<properties
   pageTitle="Probleme beim Neustarten oder Ändern der Größe eines virtuellen Computers | Microsoft Azure"
   description="Problembehandlung von Ressourcen-Manager-Bereitstellungsproblemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Linux-Computers in Azure"
   services="virtual-machines"
   documentationCenter=""
   authors="delhan"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines"
   ms.topic="support-article"
   ms.tgt_pltfrm="virtual-machines"
   ms.devlang="na"
   ms.workload="required"
   ms.date="04/28/2016"
   ms.author="delhan"/>

# Problembehandlung von Ressourcen-Manager-Bereitstellungsproblemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Linux-Computers in Azure

> [AZURE.SELECTOR]
- [Klassisch](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Ressourcen-Manager](../articles/virtual-machines/virtual-machines-linux-arm-restart-resize-error-troubleshooting.md)

Wenn Sie versuchen, einen beendeten virtuellen Azure-Computer (VM) zu starten oder die Größe eines vorhandenen virtuellen Azure-Computers zu ändern, tritt häufig ein Zuordnungsfehler auf. Dieser Fehler tritt auf, wenn in dem Cluster oder der Region keine Ressourcen verfügbar sind oder wenn die angeforderte Größe des virtuellen Computers nicht unterstützt werden kann.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassisches Bereitstellungsmodell.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Sammeln von Überwachungsprotokollen

Sammeln Sie zur Problembehandlung zunächst die Überwachungsprotokolle, um den Fehler zu ermitteln, auf den das Problem zurückzuführen ist. Die folgenden Links enthalten detaillierte Informationen zum Prozess:

[Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Überwachen von Vorgängen mit dem Ressourcen-Manager](../resource-group-audit.md)

## Problem: Fehler beim Starten eines angehaltenen virtuellen Computers

Sie versuchen, einen angehaltenen virtuellen Computer zu starten, erhalten aber einen Zuordnungsfehler.

### Ursache

Die Anforderung zum Start des angehaltenen virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Der Cluster hat jedoch genug freien Speicherplatz verfügbar, um die Anforderung zu erfüllen.

### Lösung

*	Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe, und starten Sie dann jeden virtuellen Computer neu.

  1. Klicken Sie auf **Ressourcengruppen** > _Ihre Ressourcengruppe_ > **Ressourcen** > _Ihre Verfügbarkeitsgruppe_ > **Virtual Machines** > _Ihr virtueller Computer_ > **Beenden**.

  2. Nachdem alle virtuellen Computer angehalten wurden, wählen Sie jeden einzelnen angehaltenen virtuellen Computer aus, und klicken Sie auf "Start".

*	Wiederholen Sie die Neustartanforderung zu einem späteren Zeitpunkt.

## Problem: Fehler beim Neustart eines vorhandenen virtuellen Computers

Sie versuchen, die Größe eines angehaltenen virtuellen Computers zu ändern, erhalten aber einen Zuordnungsfehler.

### Ursache

Die Anforderung zur Größenänderung des virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Der Cluster unterstützt jedoch nicht die angeforderte Größe des virtuellen Computers.

### Lösung

* Wiederholen Sie die Anforderung mit einer geringeren Größe des virtuellen Computers.

* Wenn die Größe des angeforderten virtuellen Computers nicht geändert werden kann:

  1. Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe.
  
    * Klicken Sie auf **Ressourcengruppen** > _Ihre Ressourcengruppe_ > **Ressourcen** > _Ihre Verfügbarkeitsgruppe_ > **Virtual Machines** > _Ihr virtueller Computer_ > **Beenden**.

  2. Nachdem alle virtuellen Computer beendet wurden, vergrößern Sie den gewünschten virtuellen Computer.
  3. Wählen Sie die Größe des virtuellen Computers aus, und klicken Sie auf **Starten**. Starten Sie dann jeden einzelnen beendeten virtuellen Computer.

<!---HONumber=AcomDC_0511_2016-->