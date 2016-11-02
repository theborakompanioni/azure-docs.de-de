<properties
    pageTitle="Konfigurieren von DHCPv6 für virtuelle Linux-Computer | Microsoft Azure"
    description="Vorgehensweise beim Konfigurieren von DHCPv6 für virtuelle Linux-Computer."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6, Azure Load Balancer, dualer Stapel, öffentliche IP, natives IPv6, mobil, IoT"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>


# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfigurieren von DHCPv6 für virtuelle Linux-Computer

Bei einigen der Linux-VM-Images im Azure Marketplace ist DHCPv6 nicht standardmäßig konfiguriert. Zur Unterstützung von IPv6 muss DHCPv6 in der von Ihnen verwendeten Distribution des Linux-Betriebssystems konfiguriert werden. In unterschiedlichen Linux-Distributionen wird DHCPv6 unterschiedlich konfiguriert, da sie unterschiedliche Pakete verwenden.

>[AZURE.NOTE] Bei aktuellen SUSE Linux- und CoreOS-Images im Azure Marketplace ist DHCPv6 vorkonfiguriert. Wenn Sie diese Images verwenden, sind keine zusätzlichen Änderungen erforderlich.

Dieses Dokument beschreibt, wie DHCPv6 aktiviert wird, damit der virtuelle Linux-Computer eine IPv6-Adresse erhält.

>[AZURE.WARNING] Wenn Konfigurationsdateien für das Netzwerk nicht ordnungsgemäß bearbeitet werden, kann der Netzwerkzugriff auf Ihren virtuellen Computer unterbrochen werden. Es wird empfohlen, dass Sie Änderungen an der Konfiguration in Systemen außerhalb der Produktion testen. Die Anweisungen in diesem Artikel wurden mit den neuesten Versionen von Linux-Images im Azure Marketplace getestet. Lesen Sie die Dokumentation für Ihre spezielle Version von Linux, um detaillierte Anweisungen zu erhalten.

## <a name="ubuntu"></a>Ubuntu

1. Bearbeiten Sie die Datei `/etc/dhcp/dhclient6.conf` , und fügen Sie ihr folgende Zeile hinzu:

        timeout 10;

2. Bearbeiten Sie die Netzwerkkonfiguration für die Schnittstelle „eth0“ mit der folgenden Konfiguration:

    * Bearbeiten Sie unter **Ubuntu 12.04 und 14.04** die Datei `/etc/network/interfaces.d/eth0.cfg`.
    * Bearbeiten Sie unter **Ubuntu 16.04** die Datei `/etc/network/interfaces.d/50-cloud-init.cfg`.

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. Erneuern Sie die IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Bearbeiten Sie die Datei `/etc/dhcp/dhclient6.conf` , und fügen Sie ihr folgende Zeile hinzu:

        timeout 10;

2. Bearbeiten Sie die Datei `/etc/network/interfaces` , und fügen Sie ihr folgende Konfiguration hinzu:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Erneuern Sie die IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel-/-centos-/-oracle-linux"></a>RHEL/CentOS/Oracle Linux

1. Bearbeiten Sie die Datei `/etc/sysconfig/network` , und fügen Sie ihr folgenden Parameter hinzu:

        NETWORKING_IPV6=yes

2. Bearbeiten Sie die Datei `/etc/sysconfig/network-scripts/ifcfg-eth0` , und fügen Sie ihr folgende zwei Parameter hinzu:

        IPV6INIT=yes
        DHCPV6C=yes

3. Erneuern Sie die IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11-&-opensuse-13"></a>SLES 11/openSUSE 13

Bei aktuellen SLES- und openSUSE-Images in Azure ist DHCPv6 vorkonfiguriert. Wenn Sie diese Images verwenden, sind keine zusätzlichen Änderungen erforderlich. Wenn Sie einen virtuellen Computer basierend auf einem älteren oder benutzerdefinierten SUSE-Image verwenden, führen Sie die folgenden Schritte aus:

1. Installieren Sie bei Bedarf das Paket `dhcp-client` :

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Bearbeiten Sie die Datei `/etc/sysconfig/network/ifcfg-eth0` , und fügen Sie ihr folgenden Parameter hinzu:

        DHCLIENT6_MODE='managed'

3. Erneuern Sie die IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 und openSUSE Leap

Bei aktuellen SLES- und openSUSE-Images in Azure ist DHCPv6 vorkonfiguriert. Wenn Sie diese Images verwenden, sind keine zusätzlichen Änderungen erforderlich. Wenn Sie einen virtuellen Computer basierend auf einem älteren oder benutzerdefinierten SUSE-Image verwenden, führen Sie die folgenden Schritte aus:

1. Bearbeiten Sie die Datei `/etc/sysconfig/network/ifcfg-eth0` , und ersetzen Sie diesen Parameter

        #BOOTPROTO='dhcp4'

    durch den folgenden Wert:

        BOOTPROTO='dhcp'

2. Fügen Sie die folgenden Parameter zu `/etc/sysconfig/network/ifcfg-eth0`hinzu:

        DHCLIENT6_MODE='managed'

3. Erneuern Sie die IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Bei aktuellen CoreOS-Images in Azure ist DHCPv6 vorkonfiguriert. Wenn Sie diese Images verwenden, sind keine zusätzlichen Änderungen erforderlich. Wenn Sie einen virtuellen Computer basierend auf einem älteren oder benutzerdefinierten CoreOS-Image verwenden, führen Sie die folgenden Schritte aus:

1. Bearbeiten Sie die Datei `/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Erneuern Sie die IPv6-Adresse:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```



<!--HONumber=Oct16_HO2-->


