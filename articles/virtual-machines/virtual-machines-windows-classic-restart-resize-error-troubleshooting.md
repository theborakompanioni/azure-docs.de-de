<properties
   pageTitle="Probleme beim Neustart oder Ändern der Größe eines virtuellen Computers | Microsoft Azure"
   description="Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Windows-Computers in Azure (klassisches Bereitstellungsmodell)"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-windows"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Windows-Computers in Azure (klassisches Bereitstellungsmodell)

> [AZURE.SELECTOR]
- [Klassisch](../articles/virtual-machines/virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
- [Ressourcen-Manager](../articles/virtual-machines/virtual-machines-windows-restart-resize-error-troubleshooting.md)

Wenn Sie versuchen, einen beendeten virtuellen Azure-Computer zu starten oder die Größe eines vorhandenen virtuellen Azure-Computers zu ändern, tritt häufig ein Zuordnungsfehler auf. Dieser Fehler tritt auf, wenn in dem Cluster oder der Region keine Ressourcen verfügbar sind, oder wenn die angeforderte Größe des virtuellen Computers nicht unterstützt werden kann.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Sammeln von Überwachungsprotokollen

Sammeln Sie zur Problembehandlung zunächst die Überwachungsprotokolle, um den Fehler zu ermitteln, auf den das Problem zurückzuführen ist.

Klicken Sie im Azure-Portal auf **Durchsuchen** > **Virtuelle Computer** > _Ihr virtueller Windows-Computer_ > **Einstellungen** > **Überwachungsprotokolle**.

## Problem: Fehler beim Starten eines angehaltenen virtuellen Computers

Sie versuchen, einen angehaltenen virtuellen Computer zu starten, erhalten aber eine Zuordnungsfehlermeldung.

### Ursache

Die Anforderung zum Start des angehaltenen virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Im Cluster ist jedoch nicht genügend freier Speicherplatz verfügbar, um die Anforderung zu erfüllen.

### Lösung

* Erstellen Sie einen neuen Clouddienst, und ordnen Sie ihn einer Region oder einem regionsbasierten virtuellen Netzwerk, jedoch keiner Affinitätsgruppe zu.

* Löschen Sie den angehaltenen virtuellen Computer.

* Erstellen Sie den virtuellen Computer durch Verwendung der Datenträger im neuen Clouddienst neu.

* Starten Sie den neu erstellten virtuellen Computer.

Sollte bei der Erstellung eines neuen Clouddiensts ein Fehler auftreten, wiederholen Sie den Vorgang zu einem späteren Zeitpunkt, oder ändern Sie die Region für den Clouddienst.

> [AZURE.IMPORTANT] Der neue Clouddienst trägt einen neuen Namen und eine neue VIP, sodass Sie diese Informationen für alle Abhängigkeiten ändern müssen, die diese Informationen für den vorhandenen Clouddienst verwenden.

## Problem: Fehler beim Ändern der Größe eines vorhandenen virtuellen Computers

Sie versuchen, die Größe eines angehaltenen virtuellen Computers zu ändern, erhalten aber eine Zuordnungsfehlermeldung.

### Ursache

Die Anforderung zur Größenänderung des virtuellen Computers muss im Originalcluster, der den Clouddienst hostet, ausgeführt werden. Der Cluster unterstützt jedoch nicht die angeforderte Größe des virtuellen Computers.

### Lösung

Verringern Sie die angeforderte Größe des virtuellen Computers, und wiederholen Sie die Anforderung zum Ändern der Größe.

* Klicken Sie auf **Alle durchsuchen** > **Virtuelle Computer (klassisch)** > _Ihr virtueller Computer_ > **Einstellungen** > **Größe**. Ausführliche Schritte finden Sie unter [Ändern der Größe des virtuellen Computers](https://msdn.microsoft.com/library/dn168976.aspx).

Wenn die Größe des virtuellen Computers nicht verringert werden kann, führen Sie die folgenden Schritte aus:

  * Erstellen Sie einen neuen Clouddienst, und stellen Sie dabei sicher, dass er weder mit einer Affinitätsgruppe verknüpft, noch einem virtuellen Netzwerk zugeordnet ist, das mit einer Affinitätsgruppe verknüpft ist.

  * Erstellen Sie in diesem Clouddienst einen neuen größeren virtuellen Computer.

Sie können alle Ihre virtuellen Computer im gleichen Clouddienst zusammenführen. Wenn Ihr vorhandener Clouddienst einem regionsbasierten virtuellen Netzwerk zugeordnet ist, können Sie den neuen Clouddienst mit dem vorhandenen virtuellen Netzwerk verbinden.

Wenn der vorhandene Clouddienst nicht einem regionsbasierten virtuellen Netzwerk zugeordnet ist, müssen Sie die virtuellen Computer im vorhandenen Clouddienst löschen und über die zugehörigen Datenträger im neuen Clouddienst neu erstellen. Berücksichtigen Sie dabei allerdings, dass der neue Clouddienst einen neuen Namen und eine neue VIP besitzt und diese für alle Abhängigkeiten aktualisiert werden müssen, die derzeit diese Informationen für den vorhandenen Clouddienst verwenden.

## Nächste Schritte

Wenn beim Erstellen eines neuen virtuellen Windows-Computers in Azure Probleme auftreten, finden Sie Informationen unter [Behandeln von Bereitstellungsproblemen beim Erstellen eines neuen virtuellen Windows-Computers in Azure](../virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md).

<!---HONumber=AcomDC_0921_2016-->