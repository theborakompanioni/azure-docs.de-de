---
title: Erstellen und Hochladen einer VHD-Datei mit Red Hat Enterprise Linux zur Verwendung in Azure
description: "Erfahren Sie, wie Sie eine virtuelle Azure-Festplatte (Virtual Hard Disk, VHD) erstellen und hochladen, die ein Red Hat-Linux-Betriebssystem enthält."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 6c6b8f72-32d3-47fa-be94-6cb54537c69f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: szark
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: bc04b875a2ff302cdc51f28071d57d891a62b7b1
ms.lasthandoff: 03/14/2017


---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Vorbereiten eines auf Red Hat basierenden virtuellen Computers für Azure
In diesem Artikel erfahren Sie, wie Sie einen auf Red Hat Enterprise Linux (RHEL) basierenden virtuellen Computer für die Verwendung in Azure vorbereiten. In diesem Artikel werden die RHEL-Versionen 6.7+ und 7.1+ behandelt. Darüber hinaus werden in diesem Artikel die Hypervisoren Hyper-V, KVM und VMware für die Vorbereitung vorgestellt. Weitere Informationen zu den Berechtigungsvoraussetzungen für die Teilnahme am Cloud Access-Programm von Red Hat finden Sie auf der [Red Hat Cloud Access-Website](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) und unter [Running RHEL on Azure](https://access.redhat.com/articles/1989673).

* [Vorbereiten eines virtuellen RHEL 6-Computers über Hyper-V-Manager](#prepare-a-rhel-6-virtual-machine-from-hyper-v-manager)
* [Vorbereiten eines virtuellen RHEL 7-Computers über Hyper-V-Manager](#prepare-a-rhel-7-virtual-machine-from-hyper-v-manager)
* [Vorbereiten eines virtuellen RHEL 6-Computers über KVM](#prepare-a-rhel-6-virtual-machine-from-kvm)
* [Vorbereiten eines virtuellen RHEL 7-Computers über KVM](#prepare-a-rhel-7-virtual-machine-from-kvm)
* [Vorbereiten eines virtuellen RHEL 6-Computers über VMWare](#prepare-a-rhel-6-virtual-machine-from-vmware)
* [Vorbereiten eines virtuellen RHEL 7-Computers über VMWare](#prepare-a-rhel-7-virtual-machine-from-vmware)
* [Vorbereiten eines virtuellen RHEL 7-Computers über eine Kickstart-Datei](#prepare-a-rhel-7-virtual-machine-from-a-kickstart-file)

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Vorbereiten eines auf Red Hat basierenden virtuellen Computers über Hyper-V-Manager

### <a name="prerequisites"></a>Voraussetzungen
In diesem Abschnitt wird davon ausgegangen, dass Sie bereits ein RHEL-Image aus einer ISO-Datei von der Red Hat-Website auf einer virtuellen Festplatte (VHD) installiert haben. Weitere Informationen zum Installieren eines Betriebssystemimages mit dem Hyper-V-Manager finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

**Installationshinweise zu RHEL**

* Das VHDX-Format wird in Azure noch nicht unterstützt, dafür jedoch **virtuelle Festplatten mit fester Größe**.  Sie können den Datenträger mit dem Hyper-V-Manager oder dem convert-vhd-Cmdlet in das VHD-Format konvertieren. Wenn Sie VirtualBox verwenden, bedeutet dies, dass Sie **Feste Größe** auswählen und nicht die Standardeinstellung, die dynamisch beim Erstellen des Datenträgers zugewiesen wird.
* Azure unterstützt nur virtuelle Computer der 1. Generation. Sie können virtuelle Computer der 1. Generation vom VHDX- in das VHD-Dateiformat und von einem dynamisch erweiterbaren Datenträger in einen Datenträger mit fester Größe konvertieren. Aber die Generation eines virtuellen Computers können Sie nicht ändern. Weitere Informationen finden Sie unter [Should I create a generation 1 or 2 virtual machine in Hyper-V?](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) (Sollte ich einen virtuellen Computer der 1. oder 2. Generation in Hyper-V erstellen?)
* Die maximal zulässige Größe für die virtuelle Festplatte beträgt 1.023 GB.
* Beim Installieren des Linux-Systems wird *empfohlen*, anstelle von LVM (bei vielen Installationen oftmals voreingestellt) die Standardpartitionen zu verwenden. Dadurch lässt sich vermeiden, dass ein LVM-Namenskonflikt mit geklonten virtuellen Computern auftritt, besonders dann, wenn ein BS-Datenträger zu Fehlerbehebungszwecken mit einem anderen identischen virtuellen Computer verbunden wird. [LVM](virtual-machines-linux-configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [RAID](virtual-machines-linux-configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) können auf Datenträgern verwendet werden.
* Kernel-Unterstützung für bereitgestellte UDF-Dateisysteme ist erforderlich. Beim ersten Start in Azure wird die Bereitstellungskonfiguration über UDF-formatierte Medien, die an den Gast angefügt sind, an den virtuellen Linux-Computer übergeben. Der Azure-Linux-Agent muss das UDF-Dateisystem bereitstellen können, um dessen Konfiguration zu lesen und die VM bereitzustellen.
* Linux-Kernelversionen unter 2.6.37 unterstützen keine NUMA auf Hyper-V mit größeren VM-Größen. Dieses Problem betrifft in erster Linie ältere Verteilungen, die den vorgeschalteten Red Hat 2.6.32 Kernel verwenden, und wurde in RHEL 6.6 (Kernel 2.6.32-504) behoben. Systeme, auf denen benutzerdefinierte Kernel ausgeführt werden, die älter als 2.6.37 sind, bzw. RHEL-basierte Kernel, die älter als 2.6.32-504 sind, müssen den Startparameter `numa=off` in der Kernel-Befehlszeile auf „grub.conf“ festlegen. Weitere Informationen finden Sie unter Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Konfigurieren Sie keine SWAP-Partition auf einem Betriebssystemdatenträger. Der Linux-Agent kann konfiguriert werden, eine Auslagerungsdatei auf dem temporären Ressourcendatenträger zu erstellen.  Weitere Informationen dazu finden Sie in den folgenden Schritten.
* Alle virtuellen Festplatten müssen eine Größe aufweisen, die ein Vielfaches von 1 MB ist.

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a>Vorbereiten eines virtuellen RHEL 6-Computers über Hyper-V-Manager

1. Wählen Sie im Hyper-V-Manager den virtuellen Computer aus.

2. Klicken Sie auf **Verbinden** , um ein Konsolenfenster für den virtuellen Computer zu öffnen.

3. In RHEL 6 kann NetworkManager zu Einschränkungen beim Azure Linux-Agent führen. Deinstallieren Sie dieses Paket, indem Sie den folgenden Befehl ausführen:
   
        # sudo rpm -e --nodeps NetworkManager

4. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network`, und fügen Sie ihr den folgenden Text hinzu:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network-scripts/ifcfg-eth0`, und fügen Sie ihr den folgenden Text hinzu:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Verschieben (oder entfernen) Sie die udev-Regeln, um eine Generierung statischer Regeln für die Ethernet-Schnittstelle zu vermeiden. Diese Regeln können beim Klonen eines virtuellen Computers unter Microsoft Azure oder Hyper-V zu Problemen führen:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Stellen Sie sicher, dass der Netzwerkdienst beim Booten startet, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

8. Registrieren Sie mit dem folgenden Befehl das Red Hat-Abonnement, um die Installation von Paketen aus dem RHEL-Repository zu ermöglichen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9. Das WALinuxAgent-Paket `WALinuxAgent-<version>` wurde in das Red Hat Extras-Repository übertragen. Aktivieren Sie das Extras-Repository, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Modifizieren Sie die Boot-Zeile des Kernels in Ihrer Grub-Konfiguration, um zusätzliche Kernel-Parameter für Azure einzubinden. Öffnen Sie dazu `/boot/grub/menu.lst` in einem Text-Editor, und stellen Sie sicher, dass der Standardkernel die folgenden Parameter enthält:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Dadurch wird zudem sichergestellt, dass alle Konsolennachrichten zum ersten seriellen Port gesendet werden. Dieser kann Azure bei der Behebung von Fehlern unterstützen.
    
    Neben den oben erwähnten Punkten wird es empfohlen, die folgenden Parameter zu *entfernen* :
    
        rhgb quiet crashkernel=auto
    
    Weder der Graphical Boot noch der Quiet Boot sind in einer Cloudumgebung nützlich, in der alle Protokolle an den seriellen Port gesendet werden sollen.  Die Option `crashkernel` kann bei Bedarf konfiguriert bleiben. Beachten Sie jedoch, dass dieser Parameter die Menge an verfügbarem Arbeitsspeicher im virtuellen Computer um 128 MB oder mehr reduziert, was bei kleineren virtuellen Computern problematisch sein kann.

    >[!Important]
    RHEL 6.5 und früher muss auch den Kernelparameter `numa=off` festlegen. Siehe dazu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

11. Stellen Sie sicher, dass der SSH-Server installiert und konfiguriert ist, damit er beim Booten hochfährt. Dies ist für gewöhnlich die Standardeinstellung. Ändern Sie die Datei „/etc/ssh/sshd_config“, sodass sie die folgende Zeile enthält:

        ClientAliveInterval 180

12. Installieren Sie den Azure Linux Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Durch eine Installation des WALinuxAgent-Pakets werden die NetworkManager- und NetworkManager-gnome-Pakete entfernt, sofern sie nicht bereits wie in Schritt 3 beschrieben entfernt wurden.

13. Richten Sie keinen SWAP-Raum auf dem BS-Datenträger ein.

    Der Azure Linux Agent kann den Auslagerungsbereich automatisch mit dem lokalen Ressourcendatenträger konfigurieren, der nach der Bereitstellung des virtuellen Computers in Azure mit dem virtuellen Computer verknüpft ist. Beachten Sie, dass der lokale Ressourcendatenträger ein temporärer Datenträger ist und geleert werden kann, wenn die Bereitstellung des virtuellen Computers aufgehoben wird. Ändern Sie nach dem Installieren des Azure Linux-Agents (siehe vorheriger Schritt) die folgenden Parameter in „/etc/waagent.conf“ entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Heben Sie das Abonnement (falls erforderlich) auf, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager unregister

15. Führen Sie die folgenden Befehle aus, um den virtuellen Computer zurückzusetzen und ihn für die Bereitstellung in Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klicken Sie im Hyper-V-Manager auf **Aktion > Herunterfahren**. Ihre Linux-VHD kann nun in Azure hochgeladen werden.


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Vorbereiten eines virtuellen RHEL 7-Computers über Hyper-V-Manager

1. Wählen Sie im Hyper-V-Manager den virtuellen Computer aus.

2. Klicken Sie auf **Verbinden** , um ein Konsolenfenster für den virtuellen Computer zu öffnen.

3. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network`, und fügen Sie ihr den folgenden Text hinzu:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network-scripts/ifcfg-eth0`, und fügen Sie ihr den folgenden Text hinzu:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Stellen Sie sicher, dass der Netzwerkdienst beim Booten startet, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

6. Registrieren Sie mit dem folgenden Befehl das Red Hat-Abonnement, um die Installation von Paketen aus dem RHEL-Repository zu ermöglichen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. Modifizieren Sie die Boot-Zeile des Kernels in Ihrer Grub-Konfiguration, um zusätzliche Kernel-Parameter für Azure einzubinden. Öffnen Sie dazu `/etc/default/grub` in einem Text-Editor, und bearbeiten Sie den Parameter `GRUB_CMDLINE_LINUX`. Beispiel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Dadurch wird zudem sichergestellt, dass alle Konsolennachrichten zum ersten seriellen Port gesendet werden. Dieser kann Azure bei der Behebung von Fehlern unterstützen. Außerdem werden die neuen RHEL 7-Benennungskonventionen für Netzwerkkarten deaktiviert. Neben den oben erwähnten Punkten wird es empfohlen, die folgenden Parameter zu *entfernen* :
   
        rhgb quiet crashkernel=auto
   
    Weder der Graphical Boot noch der Quiet Boot sind in einer Cloudumgebung nützlich, in der alle Protokolle an den seriellen Port gesendet werden sollen. Die Option `crashkernel` kann bei Bedarf konfiguriert bleiben. Beachten Sie jedoch, dass dieser Parameter die Menge an verfügbarem Arbeitsspeicher im virtuellen Computer um 128 MB oder mehr reduziert, was bei kleineren virtuellen Computern problematisch sein kann.

8. Nachdem Sie die Bearbeitung von `/etc/default/grub`abgeschlossen haben, führen Sie den folgenden Befehl zum erneuten Erstellen der GRUB-Konfiguration aus:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. Stellen Sie sicher, dass der SSH-Server installiert und konfiguriert ist, damit er beim Booten hochfährt. Dies ist für gewöhnlich die Standardeinstellung. Ergänzen Sie `/etc/ssh/sshd_config` um die folgende Zeile:

        ClientAliveInterval 180

10. Das WALinuxAgent-Paket `WALinuxAgent-<version>` wurde in das Red Hat Extras-Repository übertragen. Aktivieren Sie das Extras-Repository, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Installieren Sie den Azure Linux Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Richten Sie keinen SWAP-Raum auf dem BS-Datenträger ein. Der Azure Linux Agent kann den Auslagerungsbereich automatisch mit dem lokalen Ressourcendatenträger konfigurieren, der nach der Bereitstellung des virtuellen Computers in Azure mit dem virtuellen Computer verknüpft ist. Beachten Sie, dass der lokale Ressourcendatenträger ein temporärer Datenträger ist und geleert werden kann, wenn die Bereitstellung des virtuellen Computers aufgehoben wird. Passen Sie nach dem Installieren des Azure Linux-Agents (siehe vorheriger Schritt) die folgenden Parameter in `/etc/waagent.conf` entsprechend an:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Wenn Sie die Registrierung  des Abonnements aufheben möchten, führen Sie den folgenden Befehl aus:

        # sudo subscription-manager unregister

14. Führen Sie die folgenden Befehle aus, um den virtuellen Computer zurückzusetzen und ihn für die Bereitstellung in Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klicken Sie im Hyper-V-Manager auf **Aktion > Herunterfahren**. Ihre Linux-VHD kann nun in Azure hochgeladen werden.


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Vorbereiten eines auf Red Hat basierenden virtuellen Computers über KVM
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a>Vorbereiten eines virtuellen RHEL 6-Computers über KVM

1. Laden Sie das KVM-Image von RHEL 6 von der Red Hat-Website herunter.

2. Legen Sie ein Stammkennwort fest.

    Generieren Sie ein verschlüsseltes Kennwort, und kopieren Sie die Ausgabe des Befehls:

        # openssl passwd -1 changeme

    Legen Sie ein Stammkennwort mit Guestfish fest:
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Ändern Sie im zweiten Feld des Stammbenutzers "!!" in das verschlüsselte Kennwort.

3. Erstellen Sie in KVM einen virtuellen Computer aus dem qcow2-Image, legen Sie **qcow2** als Datenträgertyp fest, und legen Sie als Gerätemodell der virtuellen Netzwerkschnittstelle **virtio** fest. Starten Sie dann den virtuellen Computer, und melden Sie sich als Stammbenutzer an.

4. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network`, und fügen Sie ihr den folgenden Text hinzu:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network-scripts/ifcfg-eth0`, und fügen Sie ihr den folgenden Text hinzu:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Verschieben (oder entfernen) Sie die udev-Regeln, um eine Generierung statischer Regeln für die Ethernet-Schnittstelle zu vermeiden. Diese Regeln können beim Klonen eines virtuellen Computers unter Microsoft Azure oder Hyper-V zu Problemen führen:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Stellen Sie sicher, dass der Netzwerkdienst beim Booten startet, indem Sie den folgenden Befehl ausführen:

        # chkconfig network on

8. Registrieren Sie mit dem folgenden Befehl das Red Hat-Abonnement, um die Installation von Paketen aus dem RHEL-Repository zu ermöglichen:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9. Modifizieren Sie die Boot-Zeile des Kernels in Ihrer Grub-Konfiguration, um zusätzliche Kernel-Parameter für Azure einzubinden. Öffnen Sie dazu `/boot/grub/menu.lst` in einem Text-Editor, und stellen Sie sicher, dass der Standardkernel die folgenden Parameter enthält:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Dadurch wird zudem sichergestellt, dass alle Konsolennachrichten zum ersten seriellen Port gesendet werden. Dieser kann Azure bei der Behebung von Fehlern unterstützen.
    
    Neben den oben erwähnten Punkten wird es empfohlen, die folgenden Parameter zu *entfernen* :
    
        rhgb quiet crashkernel=auto
    
    Weder der Graphical Boot noch der Quiet Boot sind in einer Cloudumgebung nützlich, in der alle Protokolle an den seriellen Port gesendet werden sollen.  Die Option `crashkernel` kann bei Bedarf konfiguriert bleiben. Beachten Sie jedoch, dass dieser Parameter die Menge an verfügbarem Arbeitsspeicher im virtuellen Computer um 128 MB oder mehr reduziert, was bei kleineren virtuellen Computern problematisch sein kann.

    >[!Important]
    RHEL 6.5 und früher muss auch den Kernelparameter `numa=off` festlegen. Siehe dazu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

10. Fügen Sie Hyper-V-Module zu initramfs hinzu:  

    Bearbeiten Sie `/etc/dracut.conf` , und fügen Sie Inhalt hinzu:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Erstellen Sie „initramfs“ neu:

        # dracut -f -v

11. Deinstallieren Sie die Cloud-Initialisierung:

        # yum remove cloud-init

12. Stellen Sie sicher, dass der SSH-Server installiert und konfiguriert ist, damit er beim Starten hochfährt:

        # chkconfig sshd on

    Ändern Sie die Datei „/etc/ssh/sshd_config“, sodass sie die folgenden Zeilen enthält:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. Das WALinuxAgent-Paket `WALinuxAgent-<version>` wurde in das Red Hat Extras-Repository übertragen. Aktivieren Sie das Extras-Repository, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Installieren Sie den Azure Linux Agent, indem Sie den folgenden Befehl ausführen:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. Der Azure Linux Agent kann den Auslagerungsbereich automatisch mit dem lokalen Ressourcendatenträger konfigurieren, der nach der Bereitstellung des virtuellen Computers in Azure mit dem virtuellen Computer verknüpft ist. Beachten Sie, dass der lokale Ressourcendatenträger ein temporärer Datenträger ist und geleert werden kann, wenn die Bereitstellung des virtuellen Computers aufgehoben wird. Ändern Sie nach dem Installieren des Azure Linux-Agents (siehe vorheriger Schritt) die folgenden Parameter in **/etc/waagent.conf** entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Heben Sie das Abonnement (falls erforderlich) auf, indem Sie den folgenden Befehl ausführen:

        # subscription-manager unregister

17. Führen Sie die folgenden Befehle aus, um den virtuellen Computer zurückzusetzen und ihn für die Bereitstellung in Azure vorzubereiten:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Fahren Sie den virtuellen Computer in KVM herunter.

19. Konvertieren Sie das qcow2-Image in das VHD-Format:

    Konvertieren Sie das Bild zuerst in das raw-Format:

        # qemu-img convert -f qcow2 -O raw rhel-6.8.qcow2 rhel-6.8.raw

    Stellen Sie sicher, dass das RAW-Image auf 1 MB ausgerichtet ist. Runden Sie andernfalls die Größe auf 1 MB auf:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
            gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Konvertieren Sie den RAW-Datenträger in das VHD-Format mit festgelegter Größe:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a>Vorbereiten eines virtuellen RHEL 7-Computers über KVM

1. Laden Sie das KVM-Image von RHEL 7 von der Red Hat-Website herunter. In diesem Beispiel wird RHEL 7 verwendet.

2. Legen Sie ein Stammkennwort fest.

    Generieren Sie ein verschlüsseltes Kennwort, und kopieren Sie die Ausgabe des Befehls:

        # openssl passwd -1 changeme

    Legen Sie ein Stammkennwort mit Guestfish fest:

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Ändern Sie im zweiten Feld des Stammbenutzers "!!" in das verschlüsselte Kennwort.

3. Erstellen Sie in KVM einen virtuellen Computer aus dem qcow2-Image, legen Sie **qcow2** als Datenträgertyp fest, und legen Sie als Gerätemodell der virtuellen Netzwerkschnittstelle **virtio** fest. Starten Sie dann den virtuellen Computer, und melden Sie sich als Stammbenutzer an.

4. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network`, und fügen Sie ihr den folgenden Text hinzu:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network-scripts/ifcfg-eth0`, und fügen Sie ihr den folgenden Text hinzu:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. Stellen Sie sicher, dass der Netzwerkdienst beim Booten startet, indem Sie den folgenden Befehl ausführen:

        # chkconfig network on

7. Registrieren Sie mit dem folgenden Befehl das Red Hat-Abonnement, um die Installation von Paketen aus dem RHEL-Repository zu ermöglichen:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. Modifizieren Sie die Boot-Zeile des Kernels in Ihrer Grub-Konfiguration, um zusätzliche Kernel-Parameter für Azure einzubinden. Öffnen Sie dazu `/etc/default/grub` in einem Text-Editor, und bearbeiten Sie den Parameter `GRUB_CMDLINE_LINUX`. Beispiel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Dadurch wird zudem sichergestellt, dass alle Konsolennachrichten zum ersten seriellen Port gesendet werden. Dieser kann Azure bei der Behebung von Fehlern unterstützen. Außerdem werden die neuen RHEL 7-Benennungskonventionen für Netzwerkkarten deaktiviert. Neben den oben erwähnten Punkten wird es empfohlen, die folgenden Parameter zu *entfernen* :
   
        rhgb quiet crashkernel=auto
   
    Weder der Graphical Boot noch der Quiet Boot sind in einer Cloudumgebung nützlich, in der alle Protokolle an den seriellen Port gesendet werden sollen. Die Option `crashkernel` kann bei Bedarf konfiguriert bleiben. Beachten Sie jedoch, dass dieser Parameter die Menge an verfügbarem Arbeitsspeicher im virtuellen Computer um 128 MB oder mehr reduziert, was bei kleineren virtuellen Computern problematisch sein kann.

9. Nachdem Sie die Bearbeitung von `/etc/default/grub`abgeschlossen haben, führen Sie den folgenden Befehl zum erneuten Erstellen der GRUB-Konfiguration aus:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Fügen Sie Hyper-V-Module zu initramfs hinzu:

    Bearbeiten Sie `/etc/dracut.conf` , und fügen Sie Inhalt hinzu:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Erstellen Sie „initramfs“ neu:

        # dracut -f -v

11. Deinstallieren Sie die Cloud-Initialisierung:

        # yum remove cloud-init

12. Stellen Sie sicher, dass der SSH-Server installiert und konfiguriert ist, damit er beim Starten hochfährt:

        # systemctl enable sshd

Ändern Sie die Datei „/etc/ssh/sshd_config“, sodass sie die folgenden Zeilen enthält:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. Das WALinuxAgent-Paket `WALinuxAgent-<version>` wurde in das Red Hat Extras-Repository übertragen. Aktivieren Sie das Extras-Repository, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Installieren Sie den Azure Linux Agent, indem Sie den folgenden Befehl ausführen:

        # yum install WALinuxAgent

    Aktivieren Sie den Waagent-Dienst:

        # systemctl enable waagent.service

15. Richten Sie keinen SWAP-Raum auf dem BS-Datenträger ein. Der Azure Linux Agent kann den Auslagerungsbereich automatisch mit dem lokalen Ressourcendatenträger konfigurieren, der nach der Bereitstellung des virtuellen Computers in Azure mit dem virtuellen Computer verknüpft ist. Beachten Sie, dass der lokale Ressourcendatenträger ein temporärer Datenträger ist und geleert werden kann, wenn die Bereitstellung des virtuellen Computers aufgehoben wird. Passen Sie nach dem Installieren des Azure Linux-Agents (siehe vorheriger Schritt) die folgenden Parameter in `/etc/waagent.conf` entsprechend an:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Heben Sie das Abonnement (falls erforderlich) auf, indem Sie den folgenden Befehl ausführen:

        # subscription-manager unregister

17. Führen Sie die folgenden Befehle aus, um den virtuellen Computer zurückzusetzen und ihn für die Bereitstellung in Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Fahren Sie den virtuellen Computer in KVM herunter.

19. Konvertieren Sie das qcow2-Image in das VHD-Format:

    Konvertieren Sie das Bild zuerst in das raw-Format:

        # qemu-img convert -f qcow2 -O raw rhel-7.3.qcow2 rhel-7.3.raw

    Stellen Sie sicher, dass das RAW-Image auf 1 MB ausgerichtet ist. Runden Sie andernfalls die Größe auf 1 MB auf:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.3.raw" | \
            gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.3.raw $rounded_size

    Konvertieren Sie den RAW-Datenträger in das VHD-Format mit festgelegter Größe:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Vorbereiten eines auf Red Hat basierenden virtuellen Computers über VMware
### <a name="prerequisites"></a>Voraussetzungen
In diesem Abschnitt wird davon ausgegangen, dass Sie bereits einen virtuellen Computer von RHEL in VMware installiert haben. Weitere Informationen zum Installieren eines Betriebssystems in VMware finden Sie im [VMware-Installationshandbuch für Gastbetriebssysteme](http://partnerweb.vmware.com/GOSIG/home.html).

* Beim Installieren des Linux-Betriebssystems wird empfohlen, anstelle von LVM (bei vielen Installationen oftmals voreingestellt) die Standardpartitionen zu verwenden. Dadurch lässt sich vermeiden, dass ein LVM-Namenskonflikt mit geklonten virtuellen Computern auftritt, besonders dann, wenn ein BS-Datenträger zu Fehlerbehebungszwecken mit einem anderen virtuellen Computer verbunden wird. LVM oder RAID können bei Bedarf auf Datenträgern verwendet werden.
* Konfigurieren Sie keine SWAP-Partition auf einem Betriebssystemdatenträger. Sie können den Linux-Agent zur Erstellung einer Auslagerungsdatei auf dem temporären Ressourcendatenträger konfigurieren. Weitere Informationen dazu finden Sie in den folgenden Schritten.
* Wählen Sie beim Erstellen der virtuellen Festplatte **Virtuellen Datenträger als einzelne Datei speichern**aus.

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a>Vorbereiten eines virtuellen RHEL 6-Computers über VMWare
1. In RHEL 6 kann NetworkManager zu Einschränkungen beim Azure Linux-Agent führen. Deinstallieren Sie dieses Paket, indem Sie den folgenden Befehl ausführen:
   
        # sudo rpm -e --nodeps NetworkManager

2. Erstellen Sie eine Datei mit der Benennung **network** im Verzeichnis "/etc/sysconfig/", die den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network-scripts/ifcfg-eth0`, und fügen Sie ihr den folgenden Text hinzu:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4. Verschieben (oder entfernen) Sie die udev-Regeln, um eine Generierung statischer Regeln für die Ethernet-Schnittstelle zu vermeiden. Diese Regeln können beim Klonen eines virtuellen Computers unter Microsoft Azure oder Hyper-V zu Problemen führen:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

5. Stellen Sie sicher, dass der Netzwerkdienst beim Booten startet, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

6. Registrieren Sie mit dem folgenden Befehl das Red Hat-Abonnement, um die Installation von Paketen aus dem RHEL-Repository zu ermöglichen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. Das WALinuxAgent-Paket `WALinuxAgent-<version>` wurde in das Red Hat Extras-Repository übertragen. Aktivieren Sie das Extras-Repository, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8. Modifizieren Sie die Boot-Zeile des Kernels in Ihrer Grub-Konfiguration, um zusätzliche Kernel-Parameter für Azure einzubinden. Öffnen Sie dazu `/etc/default/grub` in einem Text-Editor, und bearbeiten Sie den Parameter `GRUB_CMDLINE_LINUX`. Beispiel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Dadurch wird zudem sichergestellt, dass alle Konsolennachrichten zum ersten seriellen Port gesendet werden. Dieser kann Azure bei der Behebung von Fehlern unterstützen. Außerdem werden die neuen RHEL 7-Benennungskonventionen für Netzwerkkarten deaktiviert. Neben den oben erwähnten Punkten wird es empfohlen, die folgenden Parameter zu *entfernen* :
   
        rhgb quiet crashkernel=auto
   
    Weder der Graphical Boot noch der Quiet Boot sind in einer Cloudumgebung nützlich, in der alle Protokolle an den seriellen Port gesendet werden sollen. Die Option `crashkernel` kann bei Bedarf konfiguriert bleiben. Beachten Sie jedoch, dass dieser Parameter die Menge an verfügbarem Arbeitsspeicher im virtuellen Computer um 128 MB oder mehr reduziert, was bei kleineren virtuellen Computern problematisch sein kann.

9. Fügen Sie Hyper-V-Module zu initramfs hinzu:

    Bearbeiten Sie `/etc/dracut.conf` , und fügen Sie Inhalt hinzu:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Erstellen Sie „initramfs“ neu:

        # dracut -f -v

10. Stellen Sie sicher, dass der SSH-Server installiert und konfiguriert ist, damit er beim Booten hochfährt. Dies ist für gewöhnlich die Standardeinstellung. Ergänzen Sie `/etc/ssh/sshd_config` um die folgende Zeile:

    ClientAliveInterval 180

11. Installieren Sie den Azure Linux Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Richten Sie keinen SWAP-Bereich auf dem Betriebssystemdatenträger ein: Der Azure Linux-Agent kann automatisch einen SWAP-Bereich auf dem lokalen Ressourcendatenträger konfigurieren, der nach der Bereitstellung der VM in Azure mit der VM verknüpft ist. Beachten Sie, dass der lokale Ressourcendatenträger ein temporärer Datenträger ist und geleert werden kann, wenn die Bereitstellung des virtuellen Computers aufgehoben wird. Passen Sie nach dem Installieren des Azure Linux-Agents (siehe vorheriger Schritt) die folgenden Parameter in `/etc/waagent.conf` entsprechend an:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Heben Sie das Abonnement (falls erforderlich) auf, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager unregister

14. Führen Sie die folgenden Befehle aus, um den virtuellen Computer zurückzusetzen und ihn für die Bereitstellung in Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Fahren Sie den virtuellen Computer herunter, und konvertieren Sie die VMDK-Datei in eine VHD-Datei.

    Konvertieren Sie das Bild zuerst in das raw-Format:

        # qemu-img convert -f vmdk -O raw rhel-6.8.vmdk rhel-6.8.raw

    Stellen Sie sicher, dass das RAW-Image auf 1 MB ausgerichtet ist. Runden Sie andernfalls die Größe auf 1 MB auf:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
            gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Konvertieren Sie den RAW-Datenträger in das VHD-Format mit festgelegter Größe:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Vorbereiten eines virtuellen RHEL 7-Computers über VMWare
1. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network`, und fügen Sie ihr den folgenden Text hinzu:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. Erstellen oder bearbeiten Sie die Datei `/etc/sysconfig/network-scripts/ifcfg-eth0`, und fügen Sie ihr den folgenden Text hinzu:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. Stellen Sie sicher, dass der Netzwerkdienst beim Booten startet, indem Sie den folgenden Befehl ausführen:

        # sudo chkconfig network on

4. Registrieren Sie mit dem folgenden Befehl das Red Hat-Abonnement, um die Installation von Paketen aus dem RHEL-Repository zu ermöglichen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. Modifizieren Sie die Boot-Zeile des Kernels in Ihrer Grub-Konfiguration, um zusätzliche Kernel-Parameter für Azure einzubinden. Öffnen Sie dazu `/etc/default/grub` in einem Text-Editor, und bearbeiten Sie den Parameter `GRUB_CMDLINE_LINUX`. Beispiel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Dadurch wird zudem sichergestellt, dass alle Konsolennachrichten zum ersten seriellen Port gesendet werden. Dieser kann Azure bei der Behebung von Fehlern unterstützen. Außerdem werden die neuen RHEL 7-Benennungskonventionen für Netzwerkkarten deaktiviert. Neben den oben erwähnten Punkten wird es empfohlen, die folgenden Parameter zu *entfernen* :
   
        rhgb quiet crashkernel=auto
   
    Weder der Graphical Boot noch der Quiet Boot sind in einer Cloudumgebung nützlich, in der alle Protokolle an den seriellen Port gesendet werden sollen. Die Option `crashkernel` kann bei Bedarf konfiguriert bleiben. Beachten Sie jedoch, dass dieser Parameter die Menge an verfügbarem Arbeitsspeicher im virtuellen Computer um 128 MB oder mehr reduziert, was bei kleineren virtuellen Computern problematisch sein kann.

6. Nachdem Sie die Bearbeitung von `/etc/default/grub`abgeschlossen haben, führen Sie den folgenden Befehl zum erneuten Erstellen der GRUB-Konfiguration aus:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. Fügen Sie Hyper-V-Module zu initramfs hinzu:

    Bearbeiten Sie `/etc/dracut.conf`und fügen Sie Inhalt hinzu:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Erstellen Sie „initramfs“ neu:

        # dracut -f -v

8. Stellen Sie sicher, dass der SSH-Server installiert und konfiguriert ist, damit er beim Booten hochfährt. Dies ist für gewöhnlich die Standardeinstellung. Ergänzen Sie `/etc/ssh/sshd_config` um die folgende Zeile:

        ClientAliveInterval 180

9. Das WALinuxAgent-Paket `WALinuxAgent-<version>` wurde in das Red Hat Extras-Repository übertragen. Aktivieren Sie das Extras-Repository, indem Sie den folgenden Befehl ausführen:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Installieren Sie den Azure Linux Agent, indem Sie den folgenden Befehl ausführen:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Richten Sie keinen SWAP-Raum auf dem BS-Datenträger ein. Der Azure Linux Agent kann den Auslagerungsbereich automatisch mit dem lokalen Ressourcendatenträger konfigurieren, der nach der Bereitstellung des virtuellen Computers in Azure mit dem virtuellen Computer verknüpft ist. Beachten Sie, dass der lokale Ressourcendatenträger ein temporärer Datenträger ist und geleert werden kann, wenn die Bereitstellung des virtuellen Computers aufgehoben wird. Passen Sie nach dem Installieren des Azure Linux-Agents (siehe vorheriger Schritt) die folgenden Parameter in `/etc/waagent.conf` entsprechend an:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Wenn Sie die Registrierung  des Abonnements aufheben möchten, führen Sie den folgenden Befehl aus:

        # sudo subscription-manager unregister

13. Führen Sie die folgenden Befehle aus, um den virtuellen Computer zurückzusetzen und ihn für die Bereitstellung in Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Fahren Sie den virtuellen Computer herunter, und konvertieren Sie die VMDK-Datei in das VHD-Format.

    Konvertieren Sie das Bild zuerst in das raw-Format:

        # qemu-img convert -f vmdk -O raw rhel-7.3.vmdk rhel-7.3.raw

    Stellen Sie sicher, dass das RAW-Image auf 1 MB ausgerichtet ist. Runden Sie andernfalls die Größe auf 1 MB auf:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.3.raw" | \
            gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.3.raw $rounded_size

    Konvertieren Sie den RAW-Datenträger in das VHD-Format mit festgelegter Größe:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Automatisches Vorbereiten eines auf Red Hat basierenden virtuellen Computers aus einer ISO-Datei mit einer Kickstart-Datei
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a>Vorbereiten eines virtuellen RHEL 7-Computers über eine Kickstart-Datei

1.  Erstellen Sie die Kickstart-Datei mit folgendem Inhalt, und speichern Sie die Datei. Weitere Informationen zur Kickstart-Installation finden Sie im [Kickstart-Installationshandbuch](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
          auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff

        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2. Legen Sie die Kickstart-Datei an einem Ort ab, auf den im gesamten Installationssystem zugegriffen werden kann.

3. Erstellen Sie in Hyper-V Manager einen neuen virtuellen Computer. Wählen Sie auf der Seite **Virtuelle Festplatte verbinden** die Option **Virtuelle Festplatte später zuordnen**, und schließen Sie den Assistenten für neue virtuelle Computer ab.

4. Öffnen Sie die VM-Einstellungen:

    a.  Ordnen Sie dem virtuellen Computer eine neue virtuelle Festplatte zu. Wählen Sie **VHD-Format** und **Feste Größe** aus.
    b.  Ordnen Sie die ISO-Installationsdatei dem DVD-Laufwerk zu.
    c.  Legen Sie fest, dass BIOS von der CD starten soll.

5. Starten Sie den virtuellen Computer. Wenn die Installationsanweisungen angezeigt werden, drücken Sie die **TAB-TASTE** , um die Startoptionen zu konfigurieren.

6. Geben Sie am Ende der Startoptionen `inst.ks=<the location of the kickstart file>` ein und drücken Sie die **EINGABETASTE**.

7. Warten Sie, bis die Installation abgeschlossen ist. Wenn sie abgeschlossen ist, wird der virtuelle Computer automatisch heruntergefahren. Ihre Linux-VHD kann nun in Azure hochgeladen werden.

## <a name="known-issues"></a>Bekannte Probleme
### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Der Hyper-V-Treiber konnte dem anfänglichen RAM-Datenträger nicht hinzugefügt werden, wenn ein anderer Hypervisor als ein Hyper-V-Hypervisor verwendet wurde.

In einigen Fällen enthalten Linux-Installationsprogramme möglicherweise keine Treiber für Hyper-V auf dem anfänglichen RAM-Datenträger („initrd“ oder „initramfs“), bis sie erkennen, dass sie eine Hyper-V-Umgebung ausführen.

Wenn ein anderes Virtualisierungssystem (z.B. Virtualbox oder Xen) zur Vorbereitung des Linux-Images verwendet wird, müssen Sie möglicherweise „initrd“ neu erstellen, um sicherzustellen, dass mindestens die Kernelmodule „hv_vmbus“ und „hv_storvsc“ auf dem anfänglichen RAM-Datenträger verfügbar sind. Dies ist ein bekanntes Problem, zumindest auf Systemen, die auf der Red Hat-Upstream-Verteilung basieren.

Um dieses Problem zu beheben, müssen Sie Hyper-V-Module zu „initramfs“ hinzufügen und das Archiv neu erstellen:

Bearbeiten Sie `/etc/dracut.conf` , und fügen Sie Inhalt hinzu:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

Erstellen Sie „initramfs“ neu:

        # dracut -f -v

Weitere Informationen finden Sie unter [Neuerstellung von „initramfs“](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Nächste Schritte
Sie können jetzt mit Ihrer Red Hat Enterprise Linux-VHD-Datei neue virtuelle Azure-Computer in Azure erstellen. Wenn Sie die VHD-Datei zum ersten Mal in Azure hochladen, führen Sie die Schritte 2 und 3 in [Erstellen und Hochladen einer virtuellen Festplatte, die das Linux-Betriebssystem enthält](virtual-machines-linux-classic-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)aus.

Weitere Informationen zu den Hypervisoren, die zum Ausführen von Red Hat Enterprise Linux zertifiziert sind, finden Sie auf der [Red Hat-Website](https://access.redhat.com/certified-hypervisors).

