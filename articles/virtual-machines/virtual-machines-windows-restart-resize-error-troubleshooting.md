<properties
   pageTitle="Probleme beim Neustart oder Ändern der Größe eines virtuellen Computers | Microsoft Azure"
   description="Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Windows-Computers in Azure (Resource Manager-Bereitstellungsmodell)"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-windows"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.devlang="na"
   ms.workload="required"
   ms.date="05/12/2016"
   ms.author="delhan"/>

# Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Windows-Computers in Azure (Resource Manager-Bereitstellungsmodell)

> [AZURE.SELECTOR]
- [Klassisch](../articles/virtual-machines/virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
- [Ressourcen-Manager](../articles/virtual-machines/virtual-machines-windows-restart-resize-error-troubleshooting.md)

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

## Problem: Fehler beim Neustart eines vorhandenen virtuellen Computers

Sie versuchen, die Größe eines angehaltenen virtuellen Computers zu ändern, erhalten aber eine Zuordnungsfehlermeldung.

### Ursache

Die Anforderung zur Größenänderung des virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Der Cluster unterstützt jedoch nicht die angeforderte Größe des virtuellen Computers.

### Lösung

* Wiederholen Sie die Anforderung mit einer geringeren Größe des virtuellen Computers.

* Wenn die Größe des angeforderten virtuellen Computers nicht geändert werden kann:

  1. Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe.

    * Klicken Sie auf **Ressourcengruppen** > _Ihre Ressourcengruppe_ > **Ressourcen** > _Ihre Verfügbarkeitsgruppe_ > **Virtuelle Computer** > _Ihr virtueller Computer_ > **Beenden**.

  2. Nachdem alle virtuellen Computer beendet wurden, vergrößern Sie den gewünschten virtuellen Computer.
  3. Wählen Sie die Größe des virtuellen Computers aus, und klicken Sie auf **Starten**. Starten Sie dann jeden einzelnen beendeten virtuellen Computer.

<!---HONumber=AcomDC_0601_2016-->