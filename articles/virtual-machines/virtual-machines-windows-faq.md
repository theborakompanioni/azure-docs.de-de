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
	ms.date="08/16/2016"
	ms.author="cynthn"/>

# Häufig gestellte Fragen zu virtuellen Windows-Computern 


Dieser Artikel enthält einige häufig gestellte Fragen zu virtuellen Windows-Computern, die in Azure mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. Die Linux-Version dieses Themas finden Sie unter [Häufig gestellte Fragen zu virtuellen Linux-Computern](virtual-machines-linux-faq.md).

## Was kann ich auf einem virtuellen Azure-Computer ausführen?

Alle Abonnenten können Serversoftware auf einem virtuellen Azure-Computer ausführen. Informationen zu den Supportrichtlinien für das Ausführen von Microsoft-Serversoftware in Azure finden Sie unter [Microsoft server software support for Azure Virtual Machines](https://support.microsoft.com/kb/2721672) (Unterstützung von Microsoft-Serversoftware für virtuelle Azure-Computer).

Bestimmte Versionen von Windows 7 und Windows 8.1 stehen für Azure-Abonnenten mit MSDN-Vorteilen und für Abonnenten von MSDN Dev/Test Pay-As-You-Go zu Entwicklungs- und Testzwecken bereit. Weitere Informationen, u.a. Anleitungen und Einschränkungen, finden Sie unter [Windows-Client-Images für MSDN-Abonnenten](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/).


## Wie viel Speicher kann mit einem virtuellen Computer verwendet werden?

Jeder Datenträger kann bis zu 1 TB groß sein. Die Anzahl der Datenträger, die Sie verwenden können, hängt von der Größe des virtuellen Computers ab. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md).

Azure-Speicherkonten bieten Speicher für Betriebssystem-Datenträger und Datenträger für Daten. Bei jedem Datenträger handelt es sich um eine VHD-Datei, die als Seiten-Blob gespeichert wird. Ausführliche Informationen zu Preisen finden Sie unter [Speicherpreisübersicht](https://azure.microsoft.com/pricing/details/storage/).


## Wie kann ich auf meinen virtuellen Computer zugreifen?

Richten Sie eine Remoteverbindung über Remotedesktopverbindung (Remote Desktop Connection, RDP) für einen virtuellen Windows-Computer ein. Anweisungen dazu finden Sie unter [Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](virtual-machines-windows-connect-logon.md). Es werden maximal zwei gleichzeitige Verbindungen unterstützt, es sei denn, der Server wurde als Sitzungshost für Remotedesktopdienste konfiguriert.


Wenn Probleme mit Remotedesktop auftreten, finden Sie weitere Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md).

Wenn Sie mit Hyper-V vertraut sind, suchen Sie möglicherweise nach einem ähnlichen Tool wie VMConnect. Azure bietet kein ähnliches Tool, da der Konsolenzugriff auf einem virtuellen Computer nicht unterstützt wird.

## Kann ich den temporären Datenträger (standardmäßig Laufwerk „D:“) verwenden, um Daten zu speichern?

Verwenden Sie den temporären Datenträger nicht zum Speichern von Daten. Dieser dient nur als temporärer Speicher, sodass das Risiko eines Verlusts von Daten besteht, die nicht wiederhergestellt werden können. Wenn ein virtueller Computer auf einen anderen Host verschoben wird, kann Datenverlust auftreten. Gründe für das Verschieben eines virtuellen Computers sind eine Änderung Größe des virtuellen Computers, Aktualisieren des Hosts oder ein Hardwarefehler auf dem Host.

Wenn eine Anwendung den Laufwerkbuchstaben „D:“ benötigt, können Sie die Laufwerkbuchstaben neu zuweisen, damit der temporäre Datenträger einen anderen Buchstaben als „D:“ verwendet. Anweisungen finden Sie unter [Ändern des Datenträgerbuchstabens des temporären Windows-Datenträgers](virtual-machines-windows-classic-change-drive-letter.md).

## Wie kann ich den Laufwerkbuchstaben des temporären Datenträgers ändern?

Sie können den Laufwerkbuchstaben ändern, indem Sie die Auslagerungsdatei verschieben und die Laufwerkbuchstaben neu zuweisen. Sie müssen dabei jedoch sicherstellen, dass Sie die Schritte in einer bestimmten Reihenfolge ausführen. Anweisungen finden Sie unter [Ändern des Datenträgerbuchstabens des temporären Windows-Datenträgers](virtual-machines-windows-classic-change-drive-letter.md).

## Kann ich einen vorhandenen virtuellen Computer zu einer Verfügbarkeitsgruppe hinzufügen?

Nein. Wenn der virtuelle Computer einer Verfügbarkeitsgruppe angehören soll, müssen Sie ihn innerhalb der Gruppe erstellen. Derzeit ist es nicht möglich, einen virtuellen Computer einer Verfügbarkeitsgruppe hinzuzufügen, nachdem er erstellt wurde.

## Kann ich einen virtuellen Computer in Azure hochladen?

Ja. Eine Anleitung hierzu finden Sie unter [Hochladen eines Windows-VM-Images in Microsoft Azure](virtual-machines-windows-upload-image.md).

## Kann ich die Größe des Betriebssystemlaufwerks ändern?

Ja. Anweisungen finden Sie unter [So erweitern Sie das Betriebssystemlaufwerk eines virtuellen Computers in einer Azure-Ressourcengruppe](virtual-machines-windows-expand-os-disk.md).

## Kann ich einen vorhandenen virtuellen Azure-Computer kopieren oder klonen?

Ja. Anweisungen hierzu finden Sie unter [Erstellen einer Kopie eines virtuellen Windows-Computers im Resource Manager-Bereitstellungsmodell](virtual-machines-windows-specialized-image.md).

## Warum werden die Regionen „Kanada, Mitte“ und „Kanada, Osten“ nicht in Azure Resource Manager angezeigt?

Die beiden neuen Regionen „Kanada, Mitte“ und „Kanada, Osten“ werden nicht automatisch für das Erstellen von virtuellen Computern im Rahmen von vorhandenen Azure-Abonnements registriert. Diese Registrierung erfolgt automatisch, wenn ein virtueller Computer mit Azure Resource Manager über das Azure-Portal für eine andere Region bereitgestellt wird. Nach der Bereitstellung eines virtuellen Computers in einer anderen Azure-Region sollten die neuen Regionen für nachfolgende virtuelle Computer verfügbar sein.

## Unterstützt Azure virtuelle Linux-Computer?

Ja. Unter [Erstellen eines virtuellen Linux-Computers in Azure mithilfe des Portals](virtual-machines-linux-quick-create-portal.md) finden Sie Informationen, wie Sie schnell einen virtuellen Linux-Computer erstellen und ausprobieren können.

## Kann ich meinem virtuellen Computer nach der Erstellung eine NIC hinzufügen?

Nein. Das Hinzufügen einer Netzwerkkarte ist nur zum Zeitpunkt der Erstellung möglich.

## Gibt es Anforderungen an den Computernamen?

Ja. Der Computername kann maximal 15 Zeichen lang sein. Weitere Informationen zur Benennung von Ressourcen finden Sie unter [Benennungsrichtlinien für die Infrastruktur](virtual-machines-windows-infrastructure-naming-guidelines.md).

## Welche Anforderungen an den Benutzernamen gelten beim Erstellen eines virtuellen Computers?

Benutzernamen können maximal 20 Zeichen lang sein und dürfen nicht mit einem Punkt („.“) enden.

Die folgenden Benutzernamen sind nicht zulässig:

<table>
	<tr>
		<td style="text-align:center">administrator </td><td style="text-align:center"> admin </td><td style="text-align:center"> user </td><td style="text-align:center"> user1</td>
	</tr>
	<tr>
		<td style="text-align:center">test </td><td style="text-align:center"> user2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> user3</td>
	</tr>
	<tr>
		<td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> a</td>
	</tr>
	<tr>
		<td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> aspnet</td>
	</tr>
	<tr>
		<td style="text-align:center">Sichern </td><td style="text-align:center"> console </td><td style="text-align:center"> david </td><td style="text-align:center"> guest</td>
	</tr>
	<tr>
		<td style="text-align:center">john </td><td style="text-align:center"> owner </td><td style="text-align:center"> root </td><td style="text-align:center"> server</td>
	</tr>
	<tr>
		<td style="text-align:center">sql </td><td style="text-align:center"> support </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
	</tr>
	<tr>
		<td style="text-align:center">test2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> user4 </td><td style="text-align:center"> user5</td>
	</tr>
</table>

## Welche Anforderungen an das Kennwort gelten beim Erstellen eines virtuellen Computers?

Kennwörter müssen 8 bis 123 Zeichen lang sein und 3 der 4 folgenden Komplexitätsanforderungen erfüllen:

- Kleinbuchstaben
- Großbuchstaben
- Eine Ziffer
- Ein Sonderzeichen (Übereinstimmung mit regulärem Ausdruck [\\W\_])

Die folgenden Kennwörter sind nicht zulässig:

<table>
	<tr>
		<td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Pa$$word</td>
	</tr>
	<tr>
		<td style="text-align:center">pass@word1</td><td style="text-align:center">Password!</td><td style="text-align:center">Password1</td><td style="text-align:center">Password22</td><td style="text-align:center">iloveyou!</td>
	</tr>
</table>

<!---HONumber=AcomDC_0928_2016-->