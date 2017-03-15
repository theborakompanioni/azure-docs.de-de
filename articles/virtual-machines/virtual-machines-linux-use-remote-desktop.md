---
title: Verwenden von Remotedesktop mit einem virtuellen Linux-Computer in Azure | Microsoft-Dokumente
description: "Erfahren Sie, wie Sie Remotedesktop (XRDP) zum Herstellen einer Verbindung mit einem virtuellen Linux-Computer in Azure mithilfe von Tools mit grafischer Benutzeroberfläche installieren und konfigurieren."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: ff0c1a4f06888c09d31ac65caf75b3d9544e57f9
ms.lasthandoff: 03/08/2017


---
# <a name="install-and-configure-remote-desktop-to-connect-to-a-linux-vm-in-azure"></a>Installieren und Konfigurieren von Remotedesktop zum Herstellen einer Verbindung mit einem virtuellen Linux-Computer in Azure
Virtuelle Linux-Computer (Linux-VMs) in Azure werden normalerweise von der Befehlszeile aus mithilfe einer SSH-Verbindung (Secure Shell) verwaltet. Wenn Sie erst in Linux einsteigen oder schnell eine Fehlerbehandlung durchführen müssen, ist die Verwendung von Remotedesktop unter Umständen einfacher. Dieser Artikel erläutert im Detail die Installation und Konfiguration von Desktopumgebung ([XFCE](https://www.xfce.org)) und Remotedesktop ([XRDP](http://www.xrdp)) für Ihren virtuellen Linux-Computer mithilfe des Resource Manager-Bereitstellungsmodells. Ferner können Sie [diese Schritte für virtuelle Computer auch mit dem klassischen Bereitstellungsmodell ausführen](virtual-machines-linux-classic-remote-desktop.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).


## <a name="prerequisites"></a>Voraussetzungen
Für diesen Artikel ist ein vorhandener virtueller Linux-Computer in Azure erforderlich. Wenn Sie eine VM erstellen müssen, verwenden Sie eine der folgenden Methoden:

- [Azure CLI 2.0](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Azure CLI 1.0](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- Das [Azure-Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Sie müssen auch in einem [aktiven Azure-Konto](https://azure.microsoft.com/pricing/free-trial/) angemeldet sein.


## <a name="quick-commands"></a>Schnellbefehle
Wenn Sie die Aufgabe schnell erledigen müssen, finden Sie im folgenden Abschnitt ausführliche Beschreibungen der grundlegenden Befehle zum Installieren und Konfigurieren von Remotedesktop auf Ihrem virtuellen Computer. Ausführlichere Informationen und Kontext zu den einzelnen Schritten finden Sie im übrigen Dokument ([ab hier](#install-graphical-environment-on-linux-vm)).

Im folgenden Beispiel wird die schlanke [XFCE4](https://www.xfce.org/)-Desktopumgebung auf einem virtuellen Ubuntu-Computer installiert. Andere Distributionen und Desktopumgebungen weichen davon geringfügig ab. Verwenden Sie beispielsweise **yum**, um unter Red Hat Enterprise Linux zu installieren und passende **selinux**-Regeln zu konfigurieren, oder verwenden Sie **zypper** für die Installation unter SUSE.

Stellen Sie mithilfe des SSH-Netzwerkprotokolls eine Verbindung zu Ihrer VM her. Installieren Sie die XFCE-Desktopumgebung in folgender Weise:

```bash
sudo apt-get update
sudo apt-get install xfce4
```

Installieren Sie XRDP wie folgt:

```bash
sudo apt-get install xrdp
```

Konfigurieren Sie XRDP in folgender Weise für die Verwendung von XFCE als Ihrer Desktopumgebung:

```bash
echo xfce4-session >~/.xsession
```

Starten Sie den XRDP-Dienst neu:

```bash
sudo service xrdp restart
```

Legen Sie ein Kennwort für Ihr Benutzerkonto fest, wenn Sie aktuell nur einen SSH-Schlüssel für die Authentifizierung verwenden:

```bash
sudo passwd ops
```

Beenden Sie die SSH-Sitzung mit Ihrem virtuellen Linux-Computer. Verwenden Sie die Azure-CLI auf dem lokalen Computer, um eine Regel für eine Netzwerksicherheitsgruppe für die Zulassung von Remotedesktop-Datenverkehr zu erstellen. Verwenden Sie [az network nsg rule create](/cli/azure/network/nsg/rule#create) mit Azure CLI 2.0. Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRule` in `myNetworkSecurityGroup` erstellt, um Datenverkehr an TCP-Port 3389 zuzulassen:
    
```azurecli
az network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1010 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 3389 \
    --access allow
```

Sie können auch Azure CLI 1.0 verwenden:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1010 \
    --destination-port-range 3389 --access allow
```

Stellen Sie mithilfe Ihres bevorzugten Remotedesktopclients eine Verbindung mit Ihrem virtuellen Linux-Computer her.

![Stellen Sie eine Verbindung mit XRDP mithilfe Ihres Remotedesktopclients her](./media/virtual-machines-linux-use-remote-desktop/remote-desktop-client.png)

## <a name="install-a-desktop-environment-on-your-linux-vm"></a>Installieren Sie eine Desktopumgebung auf Ihrem virtuellen Linux-Computer
Auf den meisten virtuellen Linux-Computern in Azure ist standardmäßig keine Desktopumgebung installiert. Virtuelle Linux-Computer werden üblicherweise mithilfe von SSH-Verbindungen anstelle einer Desktopumgebung verwaltet. Unter Linux stehen verschiedene Desktopumgebungen zur Wahl. Je nach der gewählten Desktopumgebung werden 1 bis 2 GB Speicherplatz auf dem Datenträger und 5 bis 10 Minuten zum Installieren und Konfigurieren aller erforderlichen Pakete benötigt.

Im folgenden Beispiel wird die schlanke [XFCE4](https://www.xfce.org/)-Desktopumgebung auf einem virtuellen Ubuntu-Computer installiert. Die Befehle für andere Distributionen weichen geringfügig ab (verwenden Sie beispielsweise **yum**, um unter Red Hat Enterprise Linux zu installieren und passende **selinux**-Regeln zu konfigurieren, oder verwenden Sie **zypper** für die Installation unter SUSE).

Stellen Sie zuerst eine SSH-Verbindung mit Ihrer VM her. Im folgenden Beispiel wird mit dem Benutzernamen `ops` eine Verbindung mit dem virtuellen Computer `myvm.westus.cloudapp.azure.com` hergestellt:

```bash
ssh ops@myvm.westus.cloudapp.azure.com ~/.ssh/id_rsa.pub
```

Wenn Sie Windows verwenden und weitere Informationen zur Verwendung von SSH benötigen, lesen Sie [Verwenden von SSH-Schlüsseln mit Windows](virtual-machines-linux-ssh-from-windows.md).

Installieren Sie anschließend XFCE mithilfe von `apt`, wie hier dargestellt:

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a>Installieren und Konfigurieren eines Remotedesktopservers
Da Sie jetzt über eine installierte Remotedesktopumgebung verfügen, konfigurieren Sie einen Remotedesktopdienst, um nach eingehenden Verbindungen zu lauschen. [XRDP](http://xrdp.org) ist ein Open Source-RDP-Server (Remote Desktop Protocol), der in den meisten Linux-Distributionen verfügbar ist und gut mit XFCE funktioniert. Installieren Sie XRDP wie folgt auf Ihrem virtuellen Ubuntu-Computer:

```bash
sudo apt-get install xrdp
```

Teilen Sie XRDP beim Starten der Sitzung die verwendete Desktopumgebung mit. Konfigurieren Sie XRDP in folgender Weise für die Verwendung von XFCE als Ihrer Desktopumgebung:

```bash
echo xfce4-session >~/.xsession
```

Starten Sie den XRDP-Dienst wie folgt neu, damit die Änderungen wirksam werden:

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a>Festlegen eines Kennworts für das lokale Benutzerkonto
Wenn Sie beim Erstellen des virtuellen Computers ein Kennwort für Ihr Benutzerkonto erstellt haben, überspringen Sie diesen Schritt. Wenn Sie nur Authentifizierung per SSH-Schlüssel verwenden und kein Kennwort für das lokale Konto festgelegt ist, geben Sie ein Kennwort an, bevor Sie XRDP für die Anmeldung bei Ihrem virtuellen Computer verwenden. XRDP kann keine SSH-Schlüssel zur Authentifizierung annehmen. Im folgenden Beispiel wird ein Kennwort für das Benutzerkonto `ops` angegeben:

```bash
sudo passwd ops
```

> [!NOTE]
> Das Festlegen eines Kennworts bewirkt keine Aktualisierung Ihrer SSHD-Konfiguration für die Zulassung von Kennwortanmeldungen, wenn diese aktuell nicht zugelassen sind. Unter Sicherheitsgesichtspunkten kann es sinnvoll sein, Verbindungen mit Ihrem virtuellen Computer mithilfe eines SSH-Tunnels mit schlüsselbasierter Authentifizierung und dann erst die Verbindung mit XRDP herzustellen. In diesem Fall können Sie den folgenden Schritt zum Erstellen einer Netzwerksicherheitsgruppen-Regel zum Zulassen von Remotedesktopverkehr überspringen.


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a>Erstellen einer Netzwerksicherheitsgruppen-Regel für den Remotedesktop-Datenverkehr
Um zuzulassen, dass Remotedesktop-Datenverkehr Ihren virtuellen Linux-Computer erreicht, muss eine Netzwerksicherheitsgruppen-Regel erstellt werden, die TCP an Port 3389 an Ihren virtuellen Computer durchlässt. Weitere Informationen zu Netzwerksicherheitsgruppen-Regeln finden Sie unter [Was ist eine Netzwerksicherheitsgruppe?](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Alternativ können Sie [das Azure-Portal zum Erstellen einer Netzwerksicherheitsgruppen-Regel verwenden](virtual-machines-windows-nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

In den folgenden Beispielen wird eine Netzwerksicherheitgsgruppen-Regel mit dem Namen `myNetworkSecurityGroupRule` erstellt, damit Datenverkehr am `tcp`-Port `3389` zugelassen (`allow`) wird.

- Verwenden Sie [az network nsg rule create](/cli/azure/network/nsg/rule#create) mit Azure CLI 2.0:
    
    ```azurecli
    az network nsg rule create --resource-group myResourceGroup \
        --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
        --protocol tcp --direction inbound --priority 1010 \
        --source-address-prefix '*' --source-port-range '*' \
        --destination-address-prefix '*' --destination-port-range 3389 \
        --access allow
    ```

- Sie können auch Azure CLI 1.0 verwenden:

    ```azurecli
    azure network nsg rule create --resource-group myResourceGroup \
        --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
        --protocol tcp --direction inbound --priority 1010 \
        --destination-port-range 3389 --access allow
    ```

## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a>Herstellen einer Verbindung mit dem virtuellen Linux-Computer mithilfe eines Remotedesktopclients
Öffnen Sie Ihren lokalen Remotedesktopclient, und stellen Sie eine Verbindung mit der IP-Adresse oder dem DNS-Namen Ihres virtuellen Linux-Computers her. Geben Sie den Benutzernamen und das Kennwort des Benutzerkontos für Ihren virtuellen Computer wie folgt ein:

![Stellen Sie eine Verbindung mit XRDP mithilfe Ihres Remotedesktopclients her](./media/virtual-machines-linux-use-remote-desktop/remote-desktop-client.png)

Nach der Authentifizierung wird die XFCE-Desktopumgebung geladen. Sie sieht ähnlich wie das folgende Beispiel aus:

![XFCE-Desktopverbindung über XRDP](./media/virtual-machines-linux-use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a>Problembehandlung
Wenn Sie keine Verbindung mit Ihrem virtuellen Linux-Computer mithilfe eines Remotedesktopclients herstellen können, verwenden Sie `netstat` auf dem virtuellen Linux-Computer, um zu überprüfen, ob der virtuelle Computer nach RDP-Verbindungen lauscht, wie hier dargestellt:

```bash
sudo netstat -plnt | grep rdp
```

Das folgende Beispiel zeigt, dass der virtuelle Computer erwartungsgemäß an TCP-Port 3389 lauscht:

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

Wenn der XRDP-Dienst nicht lauscht, starten Sie den Dienst auf einem virtuellen Ubuntu-Computer in folgender Weise neu:

```bash
sudo service xrdp restart
```

Gehen Sie die Protokolle in `/var/log` auf Ihrem virtuellen Ubuntu-Computer auf Hinweise durch, warum der Dienst nicht reagiert. Sie können auch während eines Verbindungsversuchs per Remotedesktop das syslog-Protokoll überwachen, um eventuelle Fehler anzuzeigen:

```bash
tail -f /var/log/syslog
```

Andere Linux-Distributionen, wie etwa Red Hat Enterprise Linux und SUSE, verwenden möglicherweise andere Verfahren zum erneuten Starten von Diensten sowie andere Speicherorte für die zu überprüfenden Protokolldateien.

Wenn Sie in Ihrem Remotedesktopclient keine Antwort empfangen und keine Ereignisse im Systemprotokoll finden, zeigt dieses Verhalten an, dass der Remotedesktop-Datenverkehr den virtuellen Computer nicht erreicht. Überprüfen Sie die Regeln Ihrer Netzwerksicherheitsgruppe, um sicherzustellen, dass eine Regel in Kraft ist, die TCP an Port 3389 zulässt. Weitere Informationen finden Sie unter [Behandlung von Konnektivitätsproblemen mit Anwendungen](virtual-machines-linux-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Erstellen und Verwenden von SSH-Schlüsseln mit virtuellen Linux-Computern finden Sie unter [Erstellen von SSH-Schlüsseln für virtuelle Linux-Computer in Azure](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Informationen zum Verwenden von SSH unter Windows finden Sie unter [Verwenden von SSH-Schlüsseln mit Windows](virtual-machines-linux-ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


