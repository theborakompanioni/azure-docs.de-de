---
title: Herstellen einer Verbindung mit einem virtuellen Linux-Computer mithilfe von Remotedesktop| Microsoft Docs
description: "Erfahren Sie, wie Sie Remotedesktop zum Herstellen einer Verbindung mit einem virtuellen Microsoft Azure Linux-Computer für das klassische Bereitstellungsmodell installieren und konfigurieren."
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: mingzhan
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 68031d548bdbeda9a83d1bceaaea7c5bbcab3188
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017


---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>Herstellen einer Verbindung mit einem virtuellen Microsoft Azure Linux-Computer mithilfe von Remotedesktop
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Die aktualisierte Resource Manager-Version dieses Artikels finden Sie [hier](../use-remote-desktop.md).

## <a name="overview"></a>Übersicht
RDP (Remote Desktop Protocol) ist ein proprietäres Protokoll für Windows. Wie können wir mit RDP eine Remoteverbindung mit einem virtuellen Linux-Computer (Linux VM, Linux Virtual Machine) herstellen?

In dieser Anleitung erhalten Sie die Antwort! Sie finden Informationen zum Installieren und Konfigurieren von Xrdp auf Ihrem virtuellen Microsoft Azure Linux-Computer, wodurch Sie diesen mit Remotedesktop auf einem Windows-Computer verbinden können. Wir verwenden einen virtuellen Linux-Computer, der unter Ubuntu oder OpenSUSE ausgeführt wird, als Beispiel in dieser Anleitung.

Das Tool Xrdp ist ein Open-Source-RDP-Server, der Ihnen das Herstellen einer Verbindung zwischen Ihrem Linux-Server und Remotedesktop auf einem Windows-Computer ermöglicht. RDP bietet eine viel bessere Leistung als VNC (Virtual Network Computing). VNC rendert mit Grafiken in JPEG-Qualität und kann langsam sein, während RDP schnell und gestochen scharf rendert.

> [!NOTE]
> Sie müssen bereits über einen virtuellen Microsoft Azure-Computer verfügen, auf dem Linux ausgeführt wird. Informationen zum Erstellen und Einrichten eines virtuellen Linux-Computers finden Sie im [Tutorial zu virtuellen Azure-Linux-Computern](createportal.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Erstellen eines Endgeräts für Remotedesktop
Wir verwenden das Standardendgerät 3389 für Remotedesktop in diesem Dokument. Richten Sie wie unten dargestellt Endgerät 3389 als `Remote Desktop` für Ihren virtuellen Linux-Computer ein:

![image](./media/remote-desktop/endpoint-for-linux-server.png)

Wenn Sie nicht wissen, wie Sie ein Endgerät für Ihren virtuellen Computer einrichten, sehen Sie [diese Anleitung](setup-endpoints.md) ein.

## <a name="install-gnome-desktop"></a>Installation von Gnome Desktop
Stellen Sie über `putty` eine Verbindung mit Ihrem virtuellen Linux-Computer her, und installieren Sie `Gnome Desktop`.

Für Ubuntu verwenden Sie:

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


Für OpenSUSE verwenden Sie:

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a>Installation von Xrdp
Für Ubuntu verwenden Sie:

    #sudo apt-get install xrdp

Für OpenSUSE verwenden Sie:

> [!NOTE]
> Aktualisieren Sie die OpenSUSE-Version mit der Version, die Sie im folgenden Befehl verwenden. Das untenstehende Beispiel bezieht sich auf `OpenSUSE 13.2`.
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Starten von Xrdp und Festlegen des Xdrp-Dienstes beim Start
Für OpenSUSE verwenden Sie:

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

Für Ubuntu wird Xrdp nach der Installation zum Startzeitpunkt automatisch gestartet und aktiviert.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Verwenden Sie Xfce, wenn Sie eine neuere Ubuntu-Version als Ubuntu 12.04LTS verwenden.
Da die aktuelle Version von Xrdp für Ubuntu-Versionen nach Ubuntu 12.04LTS keine Unterstützung von Gnome Desktop bietet, verwenden wir stattdessen `xfce` Desktop.

Führen Sie zum Installieren von `xfce` diesen Befehl aus:

    #sudo apt-get install xubuntu-desktop

Aktivieren Sie anschließend `xfce` mit diesem Befehl:

    #echo xfce4-session >~/.xsession

Bearbeiten Sie die Konfigurationsdatei `/etc/xrdp/startwm.sh`:

    #sudo vi /etc/xrdp/startwm.sh   

Fügen Sie die Zeile `xfce4-session` vor der Zeile `/etc/X11/Xsession` hinzu.

Geben Sie für einen Neustart des Xrdp-Diensts Folgendes an:

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Herstellen einer Verbindung mit Ihrem virtuellen Linux-Computer auf einem Windows-Computer
Starten Sie auf einem Windows-Computer den Remotedesktop-Client, und geben Sie den DNS-Namen Ihres virtuellen Linux-Computers ein. Sie können auch zum Dashboard Ihres virtuellen Computers im Azure-Portal wechseln und auf `Connect` klicken, um eine Verbindung mit Ihrem virtuellen Linux-Computer herzustellen. In diesem Fall wird das folgende Anmeldefenster angezeigt:

![image](./media/remote-desktop/no2.png)

Melden Sie sich mit dem Benutzernamen und dem Kennwort Ihres virtuellen Linux-Computers an.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von Xrdp finden Sie unter [http://www.xrdp.org/](http://www.xrdp.org/).

