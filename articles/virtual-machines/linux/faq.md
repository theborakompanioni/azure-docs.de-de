---
title: "Häufig gestellte Fragen zu Linux-VMs in Azure | Microsoft-Dokumentation"
description: "Hier finden Sie Antworten auf die häufigsten Fragen zu virtuellen Linux-Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: d579b55f07a2e710624b10fe77c17f449b09ab42
ms.lasthandoff: 04/03/2017


---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Häufig gestellte Fragen zu virtuellen Linux-Computern
Dieser Artikel enthält einige häufig gestellte Fragen zu virtuellen Linux-Computern, die in Azure mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. Die Windows-Version dieses Themas finden Sie unter [Häufig gestellte Fragen zu virtuellen Windows-Computern](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Was kann ich auf einem virtuellen Azure-Computer ausführen?
Alle Abonnenten können Serversoftware auf einem virtuellen Azure-Computer ausführen. Weitere Informationen finden Sie unter [Linux auf von Azure unterstützten Distributionen](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Wie viel Speicher kann mit einem virtuellen Computer verwendet werden?
Jeder Datenträger kann bis zu 1 TB groß sein. Die Anzahl der Datenträger, die Sie verwenden können, hängt von der Größe des virtuellen Computers ab. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure-Speicherkonten bieten Speicher für Betriebssystem-Datenträger und Datenträger für Daten. Bei jedem Datenträger handelt es sich um eine VHD-Datei, die als Seiten-Blob gespeichert wird. Ausführliche Informationen zu Preisen finden Sie unter [Speicherpreisübersicht](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Wie kann ich auf meinen virtuellen Computer zugreifen?
Stellen Sie eine Remoteverbindung mit Secure Shell (SSH) her, um sich beim virtuellen Computer anzumelden. Informationen zum Herstellen einer Verbindung finden Sie in den Anweisungen [für Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [für Linux und Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). SSH erlaubt standardmäßig maximal 10 gleichzeitige Verbindungen. Sie können diese Anzahl erhöhen, indem Sie die Konfigurationsdatei bearbeiten.

Wenn Probleme auftreten, finden Sie weitere Informationen unter [Problembehandlung für SSH-Verbindungen (Secure Shell)](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Kann ich den temporären Datenträger (/dev/sdb1) zum Speichern von Daten verwenden?
Verwenden Sie den temporären Datenträger (/dev/sdb1) nicht zum Speichern von Daten. Er dient nur zur temporären Speicherung. Sie riskieren den Verlust von Daten, die nicht wiederhergestellt werden können.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kann ich einen vorhandenen virtuellen Azure-Computer kopieren oder klonen?
Ja. Anweisungen hierzu finden Sie unter [Erstellen einer Kopie eines virtuellen Linux-Computers im Resource Manager-Bereitstellungsmodell](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Warum werden die Regionen „Kanada, Mitte“ und „Kanada, Osten“ nicht in Azure Resource Manager angezeigt?
Die beiden neuen Regionen „Kanada, Mitte“ und „Kanada, Osten“ werden nicht automatisch für das Erstellen von virtuellen Computern im Rahmen von vorhandenen Azure-Abonnements registriert. Diese Registrierung erfolgt automatisch, wenn ein virtueller Computer mit Azure Resource Manager über das Azure-Portal für eine andere Region bereitgestellt wird. Nach der Bereitstellung eines virtuellen Computers in einer anderen Azure-Region sollten die neuen Regionen für nachfolgende virtuelle Computer verfügbar sein.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kann ich meinem virtuellen Computer nach der Erstellung eine NIC hinzufügen?
Ja, dies ist jetzt möglich. Der virtuelle Computer muss zuerst beendet/freigegeben werden. Anschließend können Sie eine NIC hinzufügen oder entfernen (sofern es sich nicht um die letzte NIC auf dem virtuellen Computer handelt). 

## <a name="are-there-any-computer-name-requirements"></a>Gibt es Anforderungen an den Computernamen?
Ja. Der Computername kann maximal 64 Zeichen lang sein. Weitere Informationen zur Benennung von Ressourcen finden Sie unter [Benennungsrichtlinien für die Infrastruktur](infrastructure-naming-guidelines.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) .

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Welche Anforderungen an den Benutzernamen gelten beim Erstellen eines virtuellen Computers?
Benutzernamen müssen 1 bis 64 Zeichen lang sein.

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


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Welche Anforderungen an das Kennwort gelten beim Erstellen eines virtuellen Computers?
Kennwörter müssen 6 bis 72 Zeichen lang sein und 3 der 4 folgenden Komplexitätsanforderungen erfüllen:

* Kleinbuchstaben
* Großbuchstaben
* Eine Ziffer
* Ein Sonderzeichen (Übereinstimmung mit regulärem Ausdruck [\W_])

Die folgenden Kennwörter sind nicht zulässig:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$$word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Password!</td>
        <td style="text-align:center">Password1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">iloveyou!</td>
    </tr>
</table>

