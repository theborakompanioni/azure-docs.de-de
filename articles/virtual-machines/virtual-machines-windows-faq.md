---
title: "Häufig gestellte Fragen zu virtuellen Windows-Computern in Azure | Microsoft-Dokumentation"
description: "Hier finden Sie Antworten auf die häufigsten Fragen zu virtuellen Windows-Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 757da816-a050-4889-a010-6f75d7978eb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/17/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 5e6ffbb8f1373f7170f87ad0e345a63cc20f08dd
ms.openlocfilehash: ae831f5d21e3f7d78a1c0307f34a220288acfaa5
ms.lasthandoff: 03/24/2017


---

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Häufig gestellte Fragen zu virtuellen Windows-Computern
Dieser Artikel enthält einige häufig gestellte Fragen zu virtuellen Windows-Computern, die in Azure mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. Die Linux-Version dieses Themas finden Sie unter [Häufig gestellte Fragen zu virtuellen Linux-Computern](virtual-machines-linux-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Was kann ich auf einem virtuellen Azure-Computer ausführen?
Alle Abonnenten können Serversoftware auf einem virtuellen Azure-Computer ausführen. Informationen zu den Supportrichtlinien für das Ausführen von Microsoft-Serversoftware in Azure finden Sie unter [Microsoft server software support for Azure Virtual Machines](https://support.microsoft.com/kb/2721672)

Bestimmte Versionen von Windows 7, Windows 8.1 und Windows 10 stehen für Azure-Abonnenten mit MSDN-Vorteilen und für Abonnenten von MSDN Dev/Test Pay-As-You-Go zu Entwicklungs- und Testzwecken bereit. Weitere Informationen, u.a. Anleitungen und Einschränkungen, finden Sie unter [Windows Client images for MSDN subscribers](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/) (Windows-Clientimages für MSDN-Abonnenten). 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Wie viel Speicher kann mit einem virtuellen Computer verwendet werden?
Jeder Datenträger kann bis zu 1 TB groß sein. Die Anzahl der Datenträger, die Sie verwenden können, hängt von der Größe des virtuellen Computers ab. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure-Speicherkonten bieten Speicher für Betriebssystem-Datenträger und Datenträger für Daten. Bei jedem Datenträger handelt es sich um eine VHD-Datei, die als Seiten-Blob gespeichert wird. Ausführliche Informationen zu Preisen finden Sie unter [Speicherpreisübersicht](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Wie kann ich auf meinen virtuellen Computer zugreifen?
Richten Sie eine Remoteverbindung über Remotedesktopverbindung (Remote Desktop Connection, RDP) für einen virtuellen Windows-Computer ein. Anweisungen dazu finden Sie unter [Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Es werden maximal zwei gleichzeitige Verbindungen unterstützt, es sei denn, der Server wurde als Sitzungshost für Remotedesktopdienste konfiguriert.  

Wenn Probleme mit Remotedesktop auftreten, finden Sie weitere Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Wenn Sie mit Hyper-V vertraut sind, suchen Sie möglicherweise nach einem ähnlichen Tool wie VMConnect. Azure bietet kein ähnliches Tool, da der Konsolenzugriff auf einem virtuellen Computer nicht unterstützt wird.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Kann ich den temporären Datenträger (standardmäßig Laufwerk „D:“) verwenden, um Daten zu speichern?
Verwenden Sie den temporären Datenträger nicht zum Speichern von Daten. Dieser dient nur als temporärer Speicher, sodass das Risiko eines Verlusts von Daten besteht, die nicht wiederhergestellt werden können. Wenn ein virtueller Computer auf einen anderen Host verschoben wird, kann Datenverlust auftreten. Gründe für das Verschieben eines virtuellen Computers sind eine Änderung Größe des virtuellen Computers, Aktualisieren des Hosts oder ein Hardwarefehler auf dem Host.

Wenn eine Anwendung den Laufwerkbuchstaben „D:“ benötigt, können Sie die Laufwerkbuchstaben neu zuweisen, damit der temporäre Datenträger einen anderen Buchstaben als „D:“ verwendet. Anweisungen finden Sie unter [Ändern des Datenträgerbuchstabens des temporären Windows-Datenträgers](virtual-machines-windows-change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).


## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Wie kann ich den Laufwerkbuchstaben des temporären Datenträgers ändern?
Sie können den Laufwerkbuchstaben ändern, indem Sie die Auslagerungsdatei verschieben und die Laufwerkbuchstaben neu zuweisen. Sie müssen dabei jedoch sicherstellen, dass Sie die Schritte in einer bestimmten Reihenfolge ausführen. Anweisungen finden Sie unter [Ändern des Datenträgerbuchstabens des temporären Windows-Datenträgers](virtual-machines-windows-change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kann ich einen vorhandenen virtuellen Computer zu einer Verfügbarkeitsgruppe hinzufügen?
Nein. Wenn der virtuelle Computer einer Verfügbarkeitsgruppe angehören soll, müssen Sie ihn innerhalb der Gruppe erstellen. Derzeit ist es nicht möglich, einen virtuellen Computer einer Verfügbarkeitsgruppe hinzuzufügen, nachdem er erstellt wurde.
## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Kann ich einen virtuellen Computer in Azure hochladen?
Ja. Anweisungen dazu finden Sie unter [Hochladen eines Windows-VM-Images in Microsoft Azure ](virtual-machines-windows-upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="can-i-resize-the-os-disk"></a>Kann ich die Größe des Betriebssystemlaufwerks ändern?
Ja. Anweisungen finden Sie unter [So erweitern Sie das Betriebssystemlaufwerk eines virtuellen Computers in einer Azure-Ressourcengruppe](virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kann ich einen vorhandenen virtuellen Azure-Computer kopieren oder klonen?
Ja. Anweisungen hierzu finden Sie unter [Create a copy of a specialized Windows VM running in Azure](virtual-machines-windows-vhd-copy.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Erstellen einer Kopie eines spezialisierten virtuellen Windows-Computers in Azure).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Warum werden die Regionen „Kanada, Mitte“ und „Kanada, Osten“ nicht in Azure Resource Manager angezeigt?

Die beiden neuen Regionen „Kanada, Mitte“ und „Kanada, Osten“ werden nicht automatisch für das Erstellen von virtuellen Computern im Rahmen von vorhandenen Azure-Abonnements registriert. Diese Registrierung erfolgt automatisch, wenn ein virtueller Computer mit Azure Resource Manager über das Azure-Portal für eine andere Region bereitgestellt wird. Nach der Bereitstellung eines virtuellen Computers in einer anderen Azure-Region sollten die neuen Regionen für nachfolgende virtuelle Computer verfügbar sein.

## <a name="does-azure-support-linux-vms"></a>Unterstützt Azure virtuelle Linux-Computer?
Ja. Unter [Erstellen eines virtuellen Linux-Computers in Azure mithilfe des Portals](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)finden Sie Informationen, wie Sie schnell einen virtuellen Linux-Computer erstellen und ausprobieren können.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kann ich meinem virtuellen Computer nach der Erstellung eine NIC hinzufügen?
Ja, dies ist jetzt möglich. Der virtuelle Computer muss zuerst beendet/freigegeben werden. Anschließend können Sie eine NIC hinzufügen oder entfernen (sofern es sich nicht um die letzte NIC auf dem virtuellen Computer handelt). 

## <a name="are-there-any-computer-name-requirements"></a>Gibt es Anforderungen an den Computernamen?
Ja. Der Computername kann maximal 15 Zeichen lang sein. Weitere Informationen zur Benennung von Ressourcen finden Sie unter [Benennungsrichtlinien für die Infrastruktur](virtual-machines-windows-infrastructure-naming-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) .

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Welche Anforderungen an den Benutzernamen gelten beim Erstellen eines virtuellen Computers?

Benutzernamen können maximal 20 Zeichen lang sein und dürfen nicht mit einem Punkt („.“) enden. 


Die folgenden Benutzernamen sind nicht zulässig:
<table>
    <tr>
        <td style="text-align:center">administrator </td><td style="text-align:center"> admin </td><td style="text-align:center"> user </td><td style="text-align:center"> user1</td>
    </tr>
    <tr>
        <td style="text-align:center">test </td><td style="text-align:center"> user2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> user3</td>
    </tr>    <tr>
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

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Welche Anforderungen an das Kennwort gelten beim Erstellen eines virtuellen Computers?
Kennwörter müssen 12 bis 123 Zeichen lang sein und 3 der 4 folgenden Komplexitätsanforderungen erfüllen:

* Kleinbuchstaben
* Großbuchstaben
* Eine Ziffer
* Ein Sonderzeichen (Übereinstimmung mit regulärem Ausdruck [\W_])

Die folgenden Kennwörter sind nicht zulässig:

<table>
    <tr>
        <td>abc@123 </td>
        <td>P@$$w0rd </td>
        <td>P@ssw0rd </td>
        <td>P@ssword123 </td>
        <td>Pa$$word </td>
    </tr>
    <tr>
        <td>pass@word1 </td>
        <td>Password! </td>
        <td>Password1 </td>
        <td>Password22 </td>
        <td>iloveyou! </td>
    </tr>
</table>

