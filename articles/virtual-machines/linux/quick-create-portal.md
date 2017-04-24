---
title: "Azure-Schnellstart – Erstellen einer VM mit dem Portal | Microsoft-Dokumentation"
description: "Azure-Schnellstart – Erstellen einer VM mit dem Portal"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/13/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: ab29f01980bc7c3a8f12aaa55ff35baa3bf3f9fb
ms.lasthandoff: 04/15/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-portal"></a>Erstellen einer Linux-VM mit dem Azure-Portal

Virtuelle Azure-Computer können über das Azure-Portal erstellt werden. Diese Methode bietet eine browserbasierte Benutzeroberfläche zum Erstellen und Konfigurieren von virtuellen Computern und alle zugehörigen Ressourcen. In diesem Schnellstart wird beschrieben, wie Sie mit dem Azure-Portal einen virtuellen Computer erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-ssh-key-pair"></a>Erstellen eines SSH-Schlüsselpaars

Für diesen Schnellstart benötigen Sie ein SSH-Schlüsselpaar. Sie können diesen Schritt überspringen, wenn Sie bereits über ein vorhandenes SSH-Schlüsselpaar verfügen. Wenn Sie einen Windows-Computer verwenden, finden Sie [hier](ssh-from-windows.md) die richtige Anleitung. 

Führen Sie diesen Befehl über eine Bash-Shell aus, und befolgen Sie die Anweisungen auf dem Bildschirm. Die Ausgabe des Befehls enthält den Dateinamen der Datei mit dem öffentlichen Schlüssel. Sie benötigen den Inhalt dieser Datei beim Erstellen des virtuellen Computers.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie **Berechnen** auf dem Blatt **Neu**, wählen Sie **Ubuntu Server 16.04 LTS** auf dem Blatt **Berechnen**, und klicken Sie dann auf die Schaltfläche **Erstellen**.

3. Füllen Sie das Formular **Grundlagen** für die VM aus. Wählen Sie unter **Authentifizierungstyp** die Option **SSH**. Achten Sie beim Einfügen Ihres **Öffentlichen SSH-Schlüssels** darauf, alle führenden oder nachgestellten Leerzeichen zu entfernen. Erstellen Sie für **Ressourcengruppe** eine neue Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen erstellt und kollektiv verwaltet werden. Klicken Sie zum Abschluss auf **OK**.

    ![Eingeben grundlegender Informationen zu Ihrem virtuellen Computer im Portalblatt](./media/quick-create-portal/create-vm-portal-basic-blade.png)  

4. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. 

    ![Screenshot: VM-Größen](./media/quick-create-portal/create-linux-vm-portal-sizes.png)  

5. Wählen Sie auf dem Einstellungenblatt **Ja** unter **Verwaltete Datenträger verwenden**, behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**.

6. Klicken Sie auf der Zusammenfassungsseite auf **OK**, um die Bereitstellung des virtuellen Computers zu starten.

7. Um den Bereitstellungsstatus zu überwachen, klicken Sie auf die VM. Sie finden den virtuellen Computer auf dem Dashboard des Azure-Portals oder durch Auswählen von **Virtuelle Computer** im linken Menü. Nachdem der virtuelle Computer erstellt wurde, ändert sich der Status von **Wird bereitgestellt** in **Wird ausgeführt**.


## <a name="open-port-80-for-web-traffic"></a>Öffnen von Port 80 für Webdatenverkehr 

Standardmäßig sind für in Azure bereitgestellte virtuelle Linux-Computer nur eingehende SSH-Verbindungen zulässig. Wenn dieser virtuelle Computer als Webserver fungieren soll, muss Port 80 für Webdatenverkehr geöffnet werden. In diesem Schritt erfahren Sie, wie Sie eine Regel für eine Netzwerksicherheitsgruppe (NSG) erstellen, um eingehende Verbindungen an Port 80 zuzulassen.

1. Klicken Sie auf dem Blatt für den virtuellen Computer im Abschnitt **Zusammenfassung** auf den Namen der **Ressourcengruppe**.
2. Klicken Sie auf dem Blatt für die Ressourcengruppe in der Ressourcenliste auf die **Netzwerksicherheitsgruppe**. Der NSG-Name muss eine Kombination aus dem Namen des virtuellen Computers und dem Suffix „-nsg“ sein.
3. Klicken Sie auf die Überschrift **Eingangssicherheitsregel**, um die Eingangsregelliste zu öffnen. Die Liste müsste bereits eine Regel für RDP enthalten.
4. Klicken Sie auf **+ Hinzufügen**, um das Blatt **Eingangssicherheitsregel hinzufügen** zu öffnen.
5. Geben Sie unter **Name** den Namen **nginx** ein. Stellen Sie sicher, dass **Portbereich** auf 80 und **Aktion** auf **Zulassen** festgelegt ist. Klicken Sie auf **OK**.


## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine SSH-Verbindung mit dem virtuellen Computer her.

1. Klicken Sie auf dem Blatt des virtuellen Computers auf die Schaltfläche **Verbinden**. Die Verbindungsschaltfläche zeigt eine SSH-Verbindungszeichenfolge an, mit der Sie eine Verbindung mit dem virtuellen Computer herstellen können.

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png) 

2. Führen Sie den folgenden Befehl aus, um eine SSH-Sitzung zu erstellen. Ersetzen Sie die Verbindungszeichenfolge mit derjenigen, die Sie aus dem Azure-Portal kopiert haben.

```bash 
ssh <replace with IP address>
```

## <a name="install-nginx"></a>Installieren von NGINX

Verwenden Sie das folgende Bash-Skript, um Paketquellen zu aktualisieren und das neueste NGINX-Paket zu installieren. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a>Anzeigen der NGIX-Willkommensseite

Nachdem Sie NGINX installiert und Port 80 auf Ihrem virtuellen Computer für den Zugriff über das Internet geöffnet haben, können Sie nun mit einem Webbrowser Ihrer Wahl die NGINX-Standardwillkommensseite anzeigen. Verwenden Sie dabei den dokumentierten Wert von `publicIpAddress`, um die Standardseite zu besuchen. 

![NGINX-Standardwebsite](./media/quick-create-cli/nginx.png) 
## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigt werden, löschen Sie sie. Wählen Sie hierzu die Ressourcengruppe auf dem Blatt für virtuelle Computer aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer vollständigen Linux-Umgebung mit der Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure CLI-Beispiele für Linux-VMs](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

