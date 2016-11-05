---
title: Erstellen einer Linux-VM mit dem Azure-Portal | Microsoft Docs
description: Erstellen Sie eine Linux-VM mit dem Azure-Portal.
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: v-livech

---
# Erstellen eines virtuellen Linux-Computers in Azure mithilfe des Portals
In diesem Artikel erfahren Sie, wie Sie mithilfe des [Azure-Portals](https://portal.azure.com/) schnell einen virtuellen Linux-Computer erstellen. Die einzigen Voraussetzungen sind [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/) und [Dateien mit öffentlichen und privaten SSH-Schlüsseln](virtual-machines-linux-mac-create-ssh-keys.md).

1. Melden Sie sich mit Ihrer Azure-Kontoidentität beim Azure-Portal an, und klicken Sie links oben auf **+ Neu**:
   
    ![Bildschirm 1](../media/virtual-machines-linux-quick-create-portal/screen1.png)
2. Klicken Sie im **Marketplace** auf **Virtual Machines** und dann in der Imageliste **Ausgewählte Apps** auf **Ubuntu Server 14.04 LTS**. Vergewissern Sie sich im unteren Bereich, dass das Bereitstellungsmodell auf `Resource Manager` festgelegt ist, und klicken Sie dann auf **Erstellen**.
   
    ![Bildschirm 2](../media/virtual-machines-linux-quick-create-portal/screen2.png)
3. Gehen Sie auf der Seite **Grundlagen** wie folgt vor:
   
   * Geben Sie einen Namen für den virtuellen Computer ein.
   * Geben Sie einen Benutzernamen für den Administratorbenutzer ein.
   * Legen Sie den Authentifizierungstyp auf **Öffentlicher SSH-Schlüssel** fest.
   * Geben Sie Ihren öffentlichen SSH-Schlüssel als Zeichenfolge ein (aus dem Verzeichnis `~/.ssh/`).
   * Geben Sie einen Ressourcengruppennamen ein, oder wählen Sie eine vorhandene Gruppe aus.
     
     Klicken Sie auf **OK**, um den Vorgang fortzusetzen und die VM-Größe auszuwählen. Das sollte dann in etwa wie folgt aussehen:
     
     ![Bildschirm 3](../media/virtual-machines-linux-quick-create-portal/screen3.png)
4. Wählen Sie die Größe **DS1** aus, um Ubuntu auf einem Premium-SSD-Datenträger zu installieren, und klicken Sie auf **Auswählen**, um die Einstellungen zu konfigurieren.
   
    ![Bildschirm 4](../media/virtual-machines-linux-quick-create-portal/screen4.png)
5. Behalten Sie unter **Einstellungen** die Standardwerte für Speicher und Netzwerk bei, und klicken Sie auf **OK**, um die Zusammenfassung anzuzeigen. Durch Auswählen von „DS1“ wurde der Datenträgertyp auf „Premium-SSD“ festgelegt. Das **S** steht hierbei für SSD.
   
    ![Bildschirm 5](../media/virtual-machines-linux-quick-create-portal/screen5.png)
6. Überprüfen Sie die Einstellungen für den neuen virtuellen Ubuntu-Computer, und klicken Sie auf **OK**.
   
    ![Bildschirm 6](../media/virtual-machines-linux-quick-create-portal/screen6.png)
7. Öffnen Sie das Portal-Dashboard, und wählen Sie unter **Netzwerkschnittstellen** Ihre NIC aus.
   
    ![Bildschirm 7](../media/virtual-machines-linux-quick-create-portal/screen7.png)
8. Öffnen Sie in den NIC-Einstellungen das Menü „Öffentliche IP-Adresse“.
   
    ![Bildschirm 8](../media/virtual-machines-linux-quick-create-portal/screen8.png)
9. SSH-Verbindung über die öffentliche IP-Adresse mit Ihrem öffentlichen SSH-Schlüssel

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## Nächste Schritte
Sie haben nun schnell eine Linux-VM zum Testen und für Demonstrationszwecke erstellt. Lesen Sie zum Erstellen eines virtuellen Linux-Computers, der an Ihre Infrastruktur angepasst ist, einen der folgenden Artikel:

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates.md)
* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
* [Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-create-cli-complete.md)

<!---HONumber=AcomDC_0907_2016-->