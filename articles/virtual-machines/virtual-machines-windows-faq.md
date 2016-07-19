<properties
	pageTitle="Häufig gestellte Fragen zu virtuellen Windows-Computern | Microsoft Azure"
	description="Hier finden Sie Antworten auf die häufigsten Fragen zu virtuellen Windows-Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-resource-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/16/2016"
	ms.author="cynthn"/>

# Häufig gestellte Fragen zu virtuellen Windows-Computern 


Dieser Artikel enthält die am häufigsten gestellten Fragen zu virtuellen Windows-Computern, die in Azure mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. Die Linux-Version dieses Themas finden Sie unter [Häufig gestellte Fragen zu virtuellen Linux-Computern](virtual-machines-linux-faq.md).

## Was kann ich auf einem virtuellen Azure-Computer ausführen?

Alle Abonnenten können Serversoftware auf einem virtuellen Azure-Computer ausführen. Informationen zu den Supportrichtlinien für das Ausführen von Microsoft-Serversoftware in Azure finden Sie unter [Microsoft server software support for Azure Virtual Machines](https://support.microsoft.com/kb/2721672) (Unterstützung von Microsoft-Serversoftware für virtuelle Azure-Computer).

Für Windows-Client-Images stehen bestimmte Versionen von Windows 7 und Windows 8.1 für MSDN-Abonnenten von Azure-Vorteilen und Abonnenten von MSDN-Entwicklung und Tests mit nutzungsbasierter Zahlung für Entwicklungs- und Testzwecke bereit. Weitere Informationen, u.a. Anleitungen und Einschränkungen, finden Sie unter [Windows-Client-Images für MSDN-Abonnenten](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/).


## Wie viel Speicher kann mit einem virtuellen Computer verwendet werden?

Jeder Datenträger kann bis zu 1 TB groß sein. Die Anzahl der Datenträger, die Sie verwenden können, hängt von der Größe des virtuellen Computers ab. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md).

Azure-Speicherkonten bieten Speicher für Betriebssystem-Datenträger und Datenträger für Daten. Bei jedem Datenträger handelt es sich um eine VHD-Datei, die als Seiten-Blob gespeichert wird. Ausführliche Informationen zu Preisen finden Sie unter [Speicherpreisübersicht](https://azure.microsoft.com/pricing/details/storage/).


## Wie kann ich auf meinen virtuellen Computer zugreifen?

Sie müssen eine Remoteverbindung über Remotedesktopverbindung (Remote Desktop Connection, RDP) für einen virtuellen Windows-Computer herstellen. Anweisungen dazu finden Sie unter [Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](virtual-machines-windows-connect-logon.md). Es werden maximal zwei gleichzeitige Verbindungen unterstützt, es sei denn, der Server wurde als Host für eine Remotedesktopdienste-Sitzung konfiguriert.


Wenn Probleme mit Remotedesktop auftreten, finden Sie weitere Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md).

Wenn Sie mit Hyper-V vertraut sind, suchen Sie möglicherweise nach einem ähnlichen Tool wie VMConnect. Azure bietet kein ähnliches Tool, da der Konsolenzugriff auf einem virtuellen Computer nicht unterstützt wird.

## Kann ich den temporären Datenträger (standardmäßig Laufwerk „D:“) verwenden, um Daten zu speichern?

Sie sollten keinen temporären Datenträger zum Speichern von Daten verwenden. Dieser dient nur als temporärer Speicher, sodass das Risiko eines Verlusts von Daten besteht, die nicht wiederhergestellt werden können. Dies kann vorkommen, wenn ein virtueller Computer auf einen anderen Host verschoben wird. Gründe für das Verschieben eines virtuellen Computers sind eine Änderung Größe des virtuellen Computers, Aktualisieren des Hosts oder ein Hardwarefehler auf dem Host.

Wenn eine Anwendung den Laufwerkbuchstaben „D:“ benötigt, können Sie die Laufwerkbuchstaben neu zuweisen, damit der temporäre Datenträger einen anderen Buchstaben als D: verwendet. Anweisungen finden Sie unter [Ändern des Datenträgerbuchstabens des temporären Windows-Datenträgers](virtual-machines-windows-classic-change-drive-letter.md).

## Wie kann ich den Laufwerkbuchstaben des temporären Datenträgers ändern?

Sie können auf einem virtuellen Windows-Computer den Laufwerkbuchstaben ändern, indem Sie die Auslagerungsdatei verschieben und die Laufwerkbuchstaben neu zuweisen. Sie müssen dabei jedoch sicherstellen, dass Sie die Schritte in einer bestimmten Reihenfolge ausführen. Anweisungen finden Sie unter [Ändern des Datenträgerbuchstabens des temporären Windows-Datenträgers](virtual-machines-windows-classic-change-drive-letter.md).

## Kann ich einen vorhandenen virtuellen Computer zu einer Verfügbarkeitsgruppe hinzufügen?

Nein. Wenn der virtuelle Computer einer Verfügbarkeitsgruppe angehören soll, müssen Sie ihn innerhalb der Gruppe erstellen. Derzeit ist es nicht möglich, einen virtuellen Computer einer Verfügbarkeitsgruppe hinzuzufügen, nachdem er erstellt wurde.

## Kann ich einen virtuellen Computer in Azure hochladen?

Ja. Eine Anleitung hierzu finden Sie unter [Hochladen eines Windows-VM-Images in Microsoft Azure](virtual-machines-windows-upload-image.md).

## Kann ich die Größe des Betriebssystemlaufwerks ändern?

Ja. Anweisungen finden Sie unter [So erweitern Sie das Betriebssystemlaufwerk eines virtuellen Computers in einer Azure-Ressourcengruppe](virtual-machines-windows-expand-os-disk.md).

## Kann ich einen vorhandenen virtuellen Azure-Computer kopieren oder klonen?

Ja. Anweisungen hierzu finden Sie unter [Erstellen einer Kopie eines virtuellen Windows-Computers im Resource Manager-Bereitstellungsmodell](virtual-machines-windows-specialized-image.md).

## Warum werden die Regionen „Kanada, Mitte“ und „Kanada, Osten“ nicht in Azure Resource Manager angezeigt?

Die beiden neuen Regionen „Kanada, Mitte“ und „Kanada, Osten“ werden nicht automatisch für das Erstellen von virtuellen Computern im Rahmen von vorhandenen Azure-Abonnements registriert. Diese Registrierung erfolgt automatisch, wenn ein virtueller Computer über das Azure-Portal für eine andere Region mit Azure Resource Manager bereitgestellt wird. Nach der Bereitstellung eines virtuellen Computers in einer anderen Azure-Region sollten die neuen Regionen für nachfolgende virtuelle Computer verfügbar sein.

## Unterstützt Azure virtuelle Linux-Computer?

Ja. Unter [Erstellen eines virtuellen Linux-Computers in Azure mithilfe des Portals](virtual-machines-linux-quick-create-portal.md) finden Sie Informationen, wie Sie schnell einen virtuellen Linux-Computer erstellen und ausprobieren können.

## Kann ich meinem virtuellen Computer nach der Erstellung eine NIC hinzufügen?

Nein. Dies ist derzeit nur zum Zeitpunkt der Erstellung möglich.

<!---HONumber=AcomDC_0706_2016-->