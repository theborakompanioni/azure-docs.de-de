---
title: Erstellen einer Linux-VM mit dem Azure-Portal | Microsoft Docs
description: Erstellen Sie eine Linux-VM mit dem Azure-Portal.
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cc5dc395-dc54-4402-8804-2bb15aba8ea2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/28/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b1446cd8892e14988ff428eaa03233f8e9aefb8a


---
# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Erstellen eines virtuellen Linux-Computers in Azure mithilfe des Portals
In diesem Artikel erfahren Sie, wie Sie mithilfe des [Azure-Portals](https://portal.azure.com/) einen virtuellen Linux-Computer erstellen.

Folgende Anforderungen müssen erfüllt sein:

* [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)
* [Dateien mit den öffentlichen und privaten SSH-Schlüsseln](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="sign-in"></a>Anmelden
Melden Sie sich mit Ihrer Azure-Kontoidentität beim Azure-Portal an, und klicken Sie links oben auf **+ Neu** :

![Bildschirm 1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

## <a name="choose-vm"></a>Virtuellen Computer auswählen
Klicken Sie im **Marketplace** auf **Virtual Machines** und dann in der Imageliste **Ausgewählte Apps** auf **Ubuntu Server 14.04 LTS**.  Vergewissern Sie sich im unteren Bereich, dass das Bereitstellungsmodell auf `Resource Manager` festgelegt ist, und klicken Sie dann auf **Erstellen**.

![Bildschirm 2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

## <a name="enter-vm-options"></a>VM-Optionen eingeben
Gehen Sie auf der Seite **Grundlagen** wie folgt vor:

* Geben Sie einen Namen für den virtuellen Computer ein.
* Geben Sie einen Benutzernamen für den Administratorbenutzer ein.
* Legen Sie den Authentifizierungstyp auf **Öffentlicher SSH-Schlüssel**
* Geben Sie Ihren öffentlichen SSH-Schlüssel als Zeichenfolge ein (aus dem Verzeichnis `~/.ssh/` ).
* Geben Sie einen Ressourcengruppennamen ein, oder wählen Sie eine vorhandene Gruppe aus.

Klicken Sie auf **OK**, um den Vorgang fortzusetzen und die VM-Größe auszuwählen. Das sollte dann in etwa wie der folgende Screenshot aussehen:

![Bildschirm 3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

## <a name="choose-vm-size"></a>VM-Größe auswählen
Wählen Sie als Größe **DS1** aus, um Ubuntu auf einem Premium-SSD-Datenträger zu installieren, und klicken Sie auf **Auswählen**, um die Einstellungen zu konfigurieren.

![Bildschirm 4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

## <a name="storage-and-network"></a>Speicher und Netzwerk
Behalten Sie unter **Einstellungen** die Standardwerte für Speicher und Netzwerk bei, und klicken Sie auf **OK**, um die Zusammenfassung anzuzeigen.  Durch Auswählen von „DS1“ wurde der Datenträgertyp auf „Premium-SSD“ festgelegt. Das **S** steht hierbei für SSD.

![Bildschirm 5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

## <a name="confirm-vm-settings-and-launch"></a>VM-Einstellungen bestätigen und starten
Überprüfen Sie die Einstellungen für den neuen virtuellen Ubuntu-Computer, und klicken Sie auf **OK**.

![Bildschirm 6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

## <a name="find-the-vm-nic"></a>VM-Netzwerkschnittstelle suchen
Öffnen Sie das Portal-Dashboard, und wählen Sie unter **Netzwerkschnittstellen** Ihre NIC aus.

![Bildschirm 7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

## <a name="find-the-public-ip"></a>Öffentliche IP-Adresse suchen
Öffnen Sie in den NIC-Einstellungen das Menü „Öffentliche IP-Adresse“.

![Bildschirm 8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

## <a name="ssh-to-the-vm"></a>SSH-Verbindung mit der VM
Stellen Sie mithilfe Ihres öffentlichen SSH-Schlüssels eine SSH-Verbindung mit der öffentlichen IP-Adresse her.  Auf einer Mac- oder Linux-Arbeitsstation können Sie vom Terminal eine direkte SSH-Verbindung herstellen. Auf einer Windows-Arbeitsstation müssen Sie PuTTY, MobaXTerm oder Cygwin zum Herstellen einer Verbindung mit Linux über SSH verwenden.  Wenn Sie Ihre Windows-Arbeitsstation noch nicht zum Herstellen von SSH-Verbindungen mit Linux vorbereitet haben, finden Sie hier ein Dokument mit entsprechenden Erläuterungen:

[Verwenden von SSH-Schlüsseln mit Windows in Azure](virtual-machines-linux-ssh-from-windows.md)

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun schnell eine Linux-VM zum Testen und für Demonstrationszwecke erstellt. Lesen Sie zum Erstellen eines virtuellen Linux-Computers, der an Ihre Infrastruktur angepasst ist, einen der folgenden Artikel:

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates.md)
* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
* [Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-create-cli-complete.md)




<!--HONumber=Nov16_HO2-->


