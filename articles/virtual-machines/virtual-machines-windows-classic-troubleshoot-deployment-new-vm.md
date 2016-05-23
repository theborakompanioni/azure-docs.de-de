<properties
   pageTitle="Behandeln von Problemen beim Bereitstellen virtueller Windows-Computer (klassisches Bereitstellungsmodell) | Microsoft Azure"
   description="Behandeln Sie Probleme beim Erstellen eines neuen virtuellen Windows-Computers in Azure (klassisches Bereitstellungsmodell)."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="jiangchen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="05/06/2016"
  ms.author="cjiang"/>

# Behandeln von Problemen beim Erstellen eines neuen virtuellen Windows-Computers in Azure (klassisches Bereitstellungsmodell)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Modell.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Sammeln von Überwachungsprotokollen

Sammeln Sie zur Problembehandlung zunächst die Überwachungsprotokolle, um den Fehler zu ermitteln, auf den das Problem zurückzuführen ist.

Klicken Sie im Azure-Portal auf **Durchsuchen** > **Virtuelle Computer** > *Ihr virtueller Windows-Computer* > **Einstellungen** > **Überwachungsprotokolle**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y**: Bei einem generalisierten Windows-Betriebssystem, das mit der Generalisierungseinstellung hochgeladen und/oder erfasst wird, treten keine Fehler auf. Analog dazu gilt: Bei einem spezialisierten Windows-Betriebssystem, das mit der Spezialisierungseinstellung hochgeladen und/oder erfasst wird, treten keine Fehler auf.

**Uploadfehler:**

**N<sup>1</sup>:** Wenn ein generalisiertes Windows-Betriebssystem als spezialisiertes Betriebssystem hochgeladen wird, tritt bei der Bereitstellung ein Timeoutfehler auf, und der virtuelle Computer komm nicht über den OOBE-Bildschirm hinaus.

**N<sup>2</sup>:** Wenn ein spezialisiertes Windows-Betriebssystem als generalisiertes Betriebssystem hochgeladen wird, tritt ein Bereitstellungsfehler auf, und der virtuelle Computer komm nicht über den OOBE-Bildschirm hinaus, da der neue virtuelle Computer mit dem ursprünglichen Computernamen, Benutzernamen und Kennwort ausgeführt wird.

**Lösung:**

Laden Sie zur Behebung dieser Fehler die ursprüngliche (lokal verfügbare) virtuelle Festplatte mit der gleichen Einstellung (generalisiert/spezialisiert) hoch, die auch für das Betriebssystem verwendet wird. Beim Hochladen als generalisiertes Image muss zuerst "sysprep" ausgeführt werden. Weitere Informationen finden Sie unter [Erstellen und Hochladen einer Windows Server-VHD nach Azure](virtual-machines-windows-classic-createupload-vhd.md).

**Erfassungsfehler:**

**N<sup>3</sup>:** Wenn ein generalisiertes Windows-Betriebssystem als spezialisiertes Betriebssystem erfasst wird, tritt bei der Bereitstellung ein Timeoutfehler auf, da der ursprüngliche virtuelle Computer als generalisiert gekennzeichnet und somit nicht verwendbar ist.

**N<sup>4</sup>:** Wenn ein spezialisiertes Windows-Betriebssystem als generalisiertes Betriebssystem erfasst wird, tritt ein Bereitstellungsfehler auf, da der neue virtuelle Computer mit dem ursprünglichen Computernamen, Benutzernamen und Kennwort ausgeführt wird. Darüber hinaus ist der ursprüngliche virtuelle Computer als spezialisiert gekennzeichnet und somit nicht verwendbar.

**Lösung:**

Löschen Sie zur Behebung dieser Fehler das aktuelle Image über das Portal, und [erfassen Sie es auf der Grundlage der aktuellen VHDs erneut](virtual-machines-windows-classic-capture-image.md). Verwenden Sie dabei die gleiche Einstellung (generalisiert/spezialisiert), die auch für das Betriebssystem verwendet wird.

## Problem: Benutzerdefiniertes Image/Katalogimage/Marketplace-Image; Zuordnungsfehler
Dieser Fehler tritt auf, wenn die Anforderung für einen neuen virtuellen Computer an einen Cluster gesendet wird, der entweder nicht über genügend Speicherplatz verfügt oder die angeforderte VM-Größe nicht unterstützt. In einem einzelnen Clouddienst können nicht unterschiedliche VM-Serien verwendet werden. Wenn Sie also einen neuen virtuellen Computer mit einer Größe erstellen möchten, die vom Clouddienst nicht unterstützt wird, tritt ein Verarbeitungsfehler auf.

Je nach den Einschränkungen des Clouddiensts, den Sie zum Erstellen des neuen virtuellen Computers verwenden, liegt unter Umständen eine der folgenden Fehlerursachen vor:

**Ursache 1:** Der Clouddienst ist mit einem bestimmten Cluster oder mit einer Affinitätsgruppe (und dadurch designbedingt mit einem bestimmten Cluster) verknüpft. In dieser Affinitätsgruppe wird also versucht, neue Computerressourcen-Anforderungen in dem Cluster auszuführen, in dem auch die vorhandenen Ressourcen gehostet werden. Der Cluster unterstützt aber unter Umständen die angeforderte VM-Größe nicht oder verfügt nicht über genügend Speicherplatz, was einen Zuordnungsfehler zur Folge hat. Dies gilt unabhängig davon, ob die neuen Ressourcen über einen neuen Clouddienst oder einen vorhandenen Clouddienst erstellt werden.

**Lösung 1:**

- Erstellen Sie einen neuen Clouddienst, und ordnen Sie ihn einer Region oder einem regionsbasierten virtuellen Netzwerk zu.
- Erstellen Sie in dem neuen Clouddienst einen neuen virtuellen Computer. Sollte bei der Erstellung eines neuen Clouddiensts ein Fehler auftreten, wiederholen Sie den Vorgang zu einem späteren Zeitpunkt, oder ändern Sie die Region für den Clouddienst.

> [AZURE.IMPORTANT] Falls Sie erfolglos versucht haben, einen neuen virtuellen Computer in einem vorhandenen Clouddienst zu erstellen, und für Ihren neuen virtuellen Computer einen neuen Clouddienst erstellen mussten, haben Sie die Möglichkeit, alle Ihre virtuellen Computer im gleichen Clouddienst zusammenzufassen. Löschen Sie hierzu die virtuellen Computer im vorhandenen Clouddienst, und erfassen Sie sie auf der Grundlage ihrer Datenträger im neuen Clouddienst neu. Berücksichtigen Sie dabei allerdings, dass der neue Clouddienst einen neuen Namen und eine neue VIP besitzt und diese für alle Abhängigkeiten aktualisiert werden müssen, die derzeit diese Informationen für den vorhandenen Clouddienst verwenden.

**Ursache 2:** Der Clouddienst ist einem bestimmten virtuellen Netzwerk zugeordnet, das mit einer Affinitätsgruppe (und dadurch designbedingt mit einem bestimmten Cluster) verknüpft ist. In dieser Affinitätsgruppe wird versucht, alle neuen Computeressourcen-Anforderungen in dem Cluster auszuführen, in dem auch die vorhandenen Ressourcen gehostet werden. Der Cluster unterstützt aber unter Umständen die angeforderte VM-Größe nicht oder verfügt nicht über genügend Speicherplatz, was einen Zuordnungsfehler zur Folge hat. Dies gilt unabhängig davon, ob die neuen Ressourcen über einen neuen Clouddienst oder einen vorhandenen Clouddienst erstellt werden.

**Lösung 2:**

- Erstellen Sie ein neues regionales virtuelles Netzwerk.
- Erstellen Sie den neuen virtuellen Computer in dem neuen virtuellen Netzwerk.
- [Verbinden Sie Ihr vorhandenes virtuelles Netzwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) mit dem neuen virtuellen Netzwerk. Weitere Informationen zu regionalen virtuellen Netzwerken finden Sie [hier](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Alternativ dazu können Sie [Ihr auf einer Affinitätsgruppe basierendes virtuelles Netzwerk zu einem regionalen virtuellen Netzwerk migrieren](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/) und anschließend den neuen virtuellen Computer erstellen.

<!---HONumber=AcomDC_0511_2016-->