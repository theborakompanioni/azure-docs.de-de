<properties
	pageTitle="Häufig gestellte Fragen zu virtuellen Linux-Computern | Microsoft Azure"
	description="Hier finden Sie Antworten auf die häufigsten Fragen zu virtuellen Linux-Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-resource-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/16/2016"
	ms.author="cynthn"/>

# Häufig gestellte Fragen zu virtuellen Linux-Computern 


Dieser Artikel enthält die am häufigsten gestellten Fragen zu virtuellen Linux-Computern, die in Azure mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. Die Windows-Version dieses Themas finden Sie unter [Häufig gestellte Fragen zu virtuellen Windows-Computern](virtual-machines-windows-faq.md).

## Was kann ich auf einem virtuellen Azure-Computer ausführen?

Alle Abonnenten können Serversoftware auf einem virtuellen Azure-Computer ausführen. Weitere Informationen finden Sie unter [Linux auf von Azure unterstützten Verteilungen](virtual-machines-linux-endorsed-distros.md).


## Wie viel Speicher kann mit einem virtuellen Computer verwendet werden?

Jeder Datenträger kann bis zu 1 TB groß sein. Die Anzahl der Datenträger, die Sie verwenden können, hängt von der Größe des virtuellen Computers ab. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

Azure-Speicherkonten bieten Speicher für Betriebssystem-Datenträger und Datenträger für Daten. Bei jedem Datenträger handelt es sich um eine VHD-Datei, die als Seiten-Blob gespeichert wird. Ausführliche Informationen zu Preisen finden Sie unter [Speicherpreisübersicht](https://azure.microsoft.com/pricing/details/storage/).



## Wie kann ich auf meinen virtuellen Computer zugreifen?

Sie müssen eine Remoteverbindung mit Secure Shell (SSH) herstellen, um sich beim virtuellen Computer anzumelden. Weitere Informationen zum Herstellen einer Verbindung finden Sie in den Anweisungen [für Windows](virtual-machines-linux-ssh-from-windows.md) oder [für Linux und Mac](virtual-machines-linux-ssh-from-linux.md). SSH erlaubt standardmäßig maximal 10 gleichzeitige Verbindungen. Sie können diese Anzahl erhöhen, indem Sie die Konfigurationsdatei bearbeiten.


Wenn Probleme auftreten, finden Sie weitere Informationen unter [Problembehandlung für SSH-Verbindungen (Secure Shell)](virtual-machines-linux-troubleshoot-ssh-connection.md).

## Kann ich den temporären Datenträger (/dev/sdb1) zum Speichern von Daten verwenden?

Sie sollten keinen temporären Datenträger (/dev/sdb1) zum Speichern von Daten verwenden. Dieser dient nur als temporärer Speicher, sodass das Risiko eines Verlusts von Daten besteht, die nicht wiederhergestellt werden können.

## Kann ich einen vorhandenen virtuellen Azure-Computer kopieren oder klonen?

Ja. Anweisungen hierzu finden Sie unter [Erstellen einer Kopie eines virtuellen Linux-Computers im Resource Manager-Bereitstellungsmodell](virtual-machines-linux-specialized-image.md).

## Warum werden die Regionen „Kanada, Mitte“ und „Kanada, Osten“ nicht in Azure Resource Manager angezeigt?

Die beiden neuen Regionen „Kanada, Mitte“ und „Kanada, Osten“ werden nicht automatisch für das Erstellen von virtuellen Computern im Rahmen von vorhandenen Azure-Abonnements registriert. Diese Registrierung erfolgt automatisch, wenn ein virtueller Computer über das Azure-Portal für eine andere Region mit Azure Resource Manager bereitgestellt wird. Nach der Bereitstellung eines virtuellen Computers in einer anderen Azure-Region sollten die neuen Regionen für nachfolgende virtuelle Computer verfügbar sein.

<!---HONumber=AcomDC_0518_2016-->