---
title: Häufig gestellte Fragen zu virtuellen Linux-Computern | Microsoft Docs
description: Hier finden Sie Antworten auf die häufigsten Fragen zu virtuellen Linux-Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-management

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/16/2016
ms.author: cynthn

---
# Häufig gestellte Fragen zu virtuellen Linux-Computern
Dieser Artikel enthält einige häufig gestellte Fragen zu virtuellen Linux-Computern, die in Azure mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. Die Windows-Version dieses Themas finden Sie unter [Häufig gestellte Fragen zu virtuellen Windows-Computern](virtual-machines-windows-faq.md).

## Was kann ich auf einem virtuellen Azure-Computer ausführen?
Alle Abonnenten können Serversoftware auf einem virtuellen Azure-Computer ausführen. Weitere Informationen finden Sie unter [Linux auf von Azure unterstützten Verteilungen](virtual-machines-linux-endorsed-distros.md).

## Wie viel Speicher kann mit einem virtuellen Computer verwendet werden?
Jeder Datenträger kann bis zu 1 TB groß sein. Die Anzahl der Datenträger, die Sie verwenden können, hängt von der Größe des virtuellen Computers ab. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

Azure-Speicherkonten bieten Speicher für Betriebssystem-Datenträger und Datenträger für Daten. Bei jedem Datenträger handelt es sich um eine VHD-Datei, die als Seiten-Blob gespeichert wird. Ausführliche Informationen zu Preisen finden Sie unter [Speicherpreisübersicht](https://azure.microsoft.com/pricing/details/storage/).

## Wie kann ich auf meinen virtuellen Computer zugreifen?
Stellen Sie eine Remoteverbindung mit Secure Shell (SSH) her, um sich beim virtuellen Computer anzumelden. Weitere Informationen zum Herstellen einer Verbindung finden Sie in den Anweisungen [für Windows](virtual-machines-linux-ssh-from-windows.md) oder [für Linux und Mac](virtual-machines-linux-mac-create-ssh-keys.md). SSH erlaubt standardmäßig maximal 10 gleichzeitige Verbindungen. Sie können diese Anzahl erhöhen, indem Sie die Konfigurationsdatei bearbeiten.

Wenn Probleme auftreten, finden Sie weitere Informationen unter [Problembehandlung für SSH-Verbindungen (Secure Shell)](virtual-machines-linux-troubleshoot-ssh-connection.md).

## Kann ich den temporären Datenträger (/dev/sdb1) zum Speichern von Daten verwenden?
Verwenden Sie den temporären Datenträger (/dev/sdb1) nicht zum Speichern von Daten. Er dient nur zur temporären Speicherung. Sie riskieren den Verlust von Daten, die nicht wiederhergestellt werden können.

## Kann ich einen vorhandenen virtuellen Azure-Computer kopieren oder klonen?
Ja. Anweisungen hierzu finden Sie unter [Erstellen einer Kopie eines virtuellen Linux-Computers im Resource Manager-Bereitstellungsmodell](virtual-machines-linux-copy-vm.md).

## Warum werden die Regionen „Kanada, Mitte“ und „Kanada, Osten“ nicht in Azure Resource Manager angezeigt?
Die beiden neuen Regionen „Kanada, Mitte“ und „Kanada, Osten“ werden nicht automatisch für das Erstellen von virtuellen Computern im Rahmen von vorhandenen Azure-Abonnements registriert. Diese Registrierung erfolgt automatisch, wenn ein virtueller Computer mit Azure Resource Manager über das Azure-Portal für eine andere Region bereitgestellt wird. Nach der Bereitstellung eines virtuellen Computers in einer anderen Azure-Region sollten die neuen Regionen für nachfolgende virtuelle Computer verfügbar sein.

## Kann ich meinem virtuellen Computer nach der Erstellung eine NIC hinzufügen?
Nein. Das Hinzufügen einer Netzwerkkarte ist nur zum Zeitpunkt der Erstellung möglich.

## Gibt es Anforderungen an den Computernamen?
Ja. Der Computername kann maximal 64 Zeichen lang sein. Weitere Informationen zur Benennung von Ressourcen finden Sie unter [Benennungsrichtlinien für die Infrastruktur](virtual-machines-linux-infrastructure-naming-guidelines.md).

## Welche Anforderungen an den Benutzernamen gelten beim Erstellen eines virtuellen Computers?
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


## Welche Anforderungen an das Kennwort gelten beim Erstellen eines virtuellen Computers?
Kennwörter müssen 6 bis 72 Zeichen lang sein und 3 der 4 folgenden Komplexitätsanforderungen erfüllen:

* Kleinbuchstaben
* Großbuchstaben
* Eine Ziffer
* Ein Sonderzeichen (Übereinstimmung mit regulärem Ausdruck [\\W\_])

Die folgenden Kennwörter sind nicht zulässig:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Pa$$word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Password!</td><td style="text-align:center">Password1</td><td style="text-align:center">Password22</td><td style="text-align:center">iloveyou!</td>
    </tr>
</table>

<!---HONumber=AcomDC_0831_2016-->