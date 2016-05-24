<properties
   pageTitle="Behandeln von Problemen beim Bereitstellen virtueller Linux-Computer (Ressourcen-Manager) | Microsoft Azure"
   description="Behandeln von Ressourcen-Manager-Bereitstellungsproblemen beim Erstellen eines neuen virtuellen Linux-Computers in Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="jiangchen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="05/06/2016"
  ms.author="cjiang"/>

# Behandeln von Ressourcen-Manager-Bereitstellungsproblemen beim Erstellen eines neuen virtuellen Linux-Computers in Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassisches Bereitstellungsmodell.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Sammeln von Überwachungsprotokollen

Sammeln Sie zur Problembehandlung zunächst die Überwachungsprotokolle, um den Fehler zu ermitteln, auf den das Problem zurückzuführen ist. Die folgenden Links enthalten detaillierte Informationen zum durchzuführenden Prozess:

[Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Überwachen von Vorgängen mit dem Ressourcen-Manager](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y**: Bei einem generalisierten Linux-Betriebssystem, das mit der Generalisierungseinstellung hochgeladen und/oder erfasst wird, treten keine Fehler auf. Analog dazu gilt: Bei einem spezialisierten Linux-Betriebssystem, das mit der Spezialisierungseinstellung hochgeladen und/oder erfasst wird, treten keine Fehler auf.

**Uploadfehler:**

**N<sup>1</sup>:** Wenn ein generalisiertes Linux-Betriebssystem als spezialisiertes Betriebssystem hochgeladen wird, tritt bei der Bereitstellung ein Timeoutfehler auf, da der virtuelle Computer nicht über die Bereitstellungsphase hinauskommt.

**N<sup>2</sup>:** Wenn ein spezialisiertes Linux-Betriebssystem als generalisiertes Betriebssystem hochgeladen wird, tritt ein Bereitstellungsfehler auf, da der neue virtuelle Computer mit dem ursprünglichen Computernamen, Benutzernamen und Kennwort ausgeführt wird.

**Lösung:**

Laden Sie zur Behebung dieser Fehler die ursprüngliche (lokal verfügbare) virtuelle Festplatte mit der gleichen Einstellung (generalisiert/spezialisiert) hoch, die auch für das Betriebssystem verwendet wird. Beim Hochladen als generalisiertes Image muss zuerst "-deprovision" ausgeführt werden.

**Erfassungsfehler:**

**N<sup>3</sup>:** Wenn ein generalisiertes Linux-Betriebssystem als spezialisiertes Betriebssystem erfasst wird, tritt bei der Bereitstellung ein Timeoutfehler auf, da der ursprüngliche virtuelle Computer als generalisiert gekennzeichnet und somit nicht verwendbar ist.

**N<sup>4</sup>:** Wenn ein spezialisiertes Linux-Betriebssystem als generalisiertes Betriebssystem erfasst wird, tritt ein Bereitstellungsfehler auf, da der neue virtuelle Computer mit dem ursprünglichen Computernamen, Benutzernamen und Kennwort ausgeführt wird. Darüber hinaus ist der ursprüngliche virtuelle Computer als spezialisiert gekennzeichnet und somit nicht verwendbar.

**Lösung:**

Löschen Sie zur Behebung dieser Fehler das aktuelle Image über das Portal, und [erfassen Sie es auf der Grundlage der aktuellen VHDs erneut](virtual-machines-linux-capture-image.md). Verwenden Sie dabei die gleiche Einstellung (generalisiert/spezialisiert), die auch für das Betriebssystem verwendet wird.

## Problem: Benutzerdefiniertes Image/Katalogimage/Marketplace-Image; Zuordnungsfehler
Dieser Fehler tritt auf, wenn die Anforderung für einen neuen virtuellen Computer an einen Cluster gesendet wird, der entweder nicht über genügend Speicherplatz verfügt oder die angeforderte VM-Größe nicht unterstützt.

**Ursache 1:** Der Cluster unterstützt nicht die angeforderte Größe des virtuellen Computers.

**Lösung 1:**

- Wiederholen Sie die Anforderung mit einer geringeren Größe des virtuellen Computers.
- Wenn die Größe des angeforderten virtuellen Computers nicht geändert werden kann:
  - Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe. Klicken Sie auf **Ressourcengruppen** > *Ihre Ressourcengruppe* > **Ressourcen** > *Ihre Verfügbarkeitsgruppe* > **Virtual Machines** > *Ihr virtueller Computer* > **Beenden**.
  - Nachdem alle virtuellen Computer beendet wurden, erstellen Sie den neuen virtuellen Computer in der gewünschten Größe.
  - Starten Sie zunächst den neuen virtuellen Computer, und wählen Sie dann alle angehaltenen virtuellen Computer aus, und klicken Sie auf **Starten**.

**Ursache 2:** Der Cluster besitzt keine freien Ressourcen.

**Lösung 2:**

- Wiederholen Sie die Anforderung zu einem späteren Zeitpunkt.
- Wenn der neue virtuelle Computer Teil einer anderen Verfügbarkeitsgruppe sein kann
  - Erstellen Sie einen neuen virtuellen Computer in einer anderen Verfügbarkeitsgruppe (in derselben Region).
  - Fügen Sie den neuen virtuellen Computer zum gleichen virtuellen Netzwerk hinzu.

<!---HONumber=AcomDC_0511_2016-->