---
title: "Verbinden von HDInsight mit Ihrem lokalen Netzwerk – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie einen HDInsight-Cluster in einem virtuellen Azure-Netzwerk erstellen und ihn anschließend mit Ihrem lokalen Netzwerk verbinden. Sie erfahren außerdem, wie Sie unter Verwendung eines benutzerdefinierten DNS-Servers die Namensauflösung zwischen HDInsight und Ihrem lokalen Netzwerk konfigurieren."
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 6fc863010cc59e20e7d86ea9344489e574be75f2
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="connect-hdinsight-to-your-on-premise-network"></a>Verbinden von HDInsight mit Ihrem lokalen Netzwerk

Erfahren Sie, wie Sie HDInsight mit Ihrem lokalen Netzwerk verbinden, indem Sie virtuelle Azure-Netzwerke und ein VPN-Gateway einsetzen. In diesem Dokument erhalten Sie Informationen zur Planung der folgenden Aufgaben:

* Verwenden von HDInsight in einem virtuellen Azure-Netzwerk mit einer Verbindung mit Ihrem lokalen Netzwerk

* Konfigurieren der DNS-Namensauflösung zwischen dem virtuellen Netzwerk und Ihrem lokalen Netzwerk

* Konfigurieren von Netzwerksicherheitsgruppen zum Einschränken des Internetzugriffs auf HDInsight

* Ports, die von HDInsight im virtuellen Netzwerk bereitgestellt werden

## <a name="create-the-virtual-network-configuration"></a>Erstellen der Konfiguration des virtuellen Netzwerks

> [!IMPORTANT]
> Eine ausführliche Anleitung zum Herstellen einer Verbindung von HDInsight mit Ihrem lokalen Netzwerk mithilfe eines virtuellen Azure-Netzwerks finden Sie im Dokument [Verbinden von HDInsight mit Ihrem lokalen Netzwerk](connect-on-premises-network.md).

In den folgenden Dokumenten erfahren Sie, wie Sie ein virtuelles Azure-Netzwerk erstellen, das mit Ihrem lokalen Netzwerk verbunden wird:
    
* [Verwenden des Azure-Portals](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [Verwenden von Azure PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [Verwenden der Azure-Befehlszeilenschnittstelle](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a>Konfigurieren der Namensauflösung

Um HDInsight und den Ressourcen im angebundenen Netzwerk eine Kommunikation per Name zu ermöglichen, müssen Sie die folgenden Aufgaben ausführen:

* Erstellen eines benutzerdefinierten DNS-Servers im virtuellen Azure-Netzwerk

* Konfigurieren des virtuellen Netzwerks, damit anstelle des rekursiven Azure-Resolvers der benutzerdefinierte DNS-Server verwendet wird

* Konfigurieren der Weiterleitung zwischen dem benutzerdefinierten DNS-Server und Ihrem lokalen DNS-Server

Diese Konfiguration ermöglicht das folgende Verhalten:

* Anforderungen für vollqualifizierte Domänennamen, die das DNS-Suffix __für das virtuelle Netzwerk__ umfassen, werden an den benutzerdefinierten DNS-Server weitergeleitet. Der benutzerdefinierte DNS-Server leitet die Anforderungen dann an den rekursiven Azure-Resolver weiter, der die IP-Adresse zurückgibt.

* Alle anderen Anforderungen werden an den lokalen DNS-Server weitergeleitet. Selbst Anforderungen für öffentliche Internetressourcen, wie z.B. microsoft.com, werden zur Namensauflösung an den lokalen DNS-Server weitergeleitet.

Im folgenden Diagramm werden Anforderungen für Ressourcen, die auf das DNS-Suffix des virtuellen Netzwerks enden, als grüne Linien dargestellt. Die blauen Linien stehen für Anforderungen von Ressourcen im lokalen Netzwerk oder dem öffentlichen Internet.

![Diagramm zur Veranschaulichung der Auflösung von DNS-Anforderungen für die in diesem Dokument verwendete Konfiguration](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a>Erstellen eines benutzerdefinierten DNS-Servers

> [!IMPORTANT]
> Sie müssen den benutzerdefinierten DNS-Server erstellen und konfigurieren, bevor Sie HDInsight im virtuellen Netzwerk installieren.

Führen Sie die folgenden Schritte aus, um eine Linux-VM zu erstellen, die die DNS-Software [Bind](https://www.isc.org/downloads/bind/) verwendet:

> [!NOTE]
> Die nachfolgend aufgeführten Schritte gelten für die Erstellung eines virtuellen Azure-Computers über das [Azure-Portal](https://portal.azure.com). Informationen zu anderen Möglichkeiten zum Erstellen eines virtuellen Computers finden Sie in den Dokumenten [Erstellen eines virtuellen Computers – Azure CLI](../virtual-machines/linux/quick-create-cli.md) und [Erstellen eines virtuellen Computers – Azure PowerShell](../virtual-machines/linux/quick-create-portal.md).

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf __+__, __Compute__ und dann auf __Ubuntu Server 16.04 LTS__.

    ![Erstellen eines virtuellen Ubuntu-Computers](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. Geben Sie im Abschnitt __Grundeinstellungen__ die folgenden Informationen ein:

    * __Name__: Ein Anzeigename, über den dieser virtuelle Computer identifiziert wird. Beispiel: __DNSProxy__.
    * __Benutzername__: Der Name des SSH-Kontos.
    * __Öffentlicher SSH-Schlüssel__ oder __Kennwort__: Die Identifikationsmethode für das SSH-Konto. Wir empfehlen die Verwendung öffentlicher Schlüssel, weil diese sicherer sind. Weitere Informationen finden Sie unter [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](../virtual-machines/linux/mac-create-ssh-keys.md).
    * __Ressourcengruppe__: Klicken Sie auf __Vorhandene verwenden__, und wählen Sie dann die Ressourcengruppe aus, die das zuvor erstellte virtuelle Netzwerk enthält.
    * __Standort__: Wählen Sie denselben Standort aus wie für das virtuelle Netzwerk.

    ![Grundlegende Konfiguration des virtuellen Computers](./media/connect-on-premises-network/vm-basics.png)

    Übernehmen Sie für alle weitere Einstellungen die Standardwerte, und klicken Sie dann auf __OK__.

3. Wählen Sie im Abschnitt __Größe auswählen__ die VM-Größe aus. Im Rahmen dieses Tutorials wählen Sie die kleinste und kostengünstigste Option. Um fortzufahren, klicken Sie auf die Schaltfläche __Auswählen__.

4. Geben Sie im Abschnitt __Einstellungen__ die folgenden Informationen ein:

    * __Virtuelles Netzwerk__: Wählen Sie das zuvor erstellte virtuelle Netzwerk aus.

    * __Subnetz__: Wählen Sie das Standardsubnetz für das virtuelle Netzwerk aus. Wählen Sie __nicht__ das Subnetz aus, das vom VPN-Gateway verwendet wird.

    * __Diagnosespeicherkonto__: Wählen Sie entweder ein vorhandenes Speicherkonto aus, oder erstellen Sie ein neues Konto.

    ![Einstellungen für das virtuelle Netzwerk](./media/connect-on-premises-network/virtual-network-settings.png)

    Übernehmen Sie für alle weitere Einstellungen die Standardwerte, und klicken Sie dann auf __OK__, um den Vorgang fortzusetzen.

5. Wählen Sie auf dem Blatt __Kaufen__ die Schaltfläche __Kaufen__ aus, um den virtuellen Computer zu erstellen.

6. Sobald der virtuelle Computer erstellt wurde, wird der Abschnitt __Übersicht__ angezeigt. Klicken Sie in der Liste auf der linken Seite auf __Einstellungen__. Speichern Sie die Werte für __Öffentliche IP-Adresse__ und __Private IP-Adresse__. Sie werden im nächsten Abschnitt verwendet.

    ![Öffentliche und private IP-Adressen](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Installieren und Konfigurieren von Bind (DNS-Software)

1. Verwenden Sie SSH, um eine Verbindung mit der __öffentlichen IP-Adresse__ des virtuellen Computers herzustellen. Im folgenden Beispiel wird über die IP-Adresse 40.68.254.142 eine Verbindung mit dem virtuellen Computer hergestellt:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Ersetzen Sie `sshuser` durch das SSH-Benutzerkonto, der beim Erstellen des Clusters angegeben wurde.

    > [!NOTE]
    > Das Hilfsprogramm `ssh` kann auf verschiedene Weise erworben werden. Unter Linux, Unix und macOS gehört es zum Lieferumfang des Betriebssystems. Wenn Sie Windows verwenden, erwägen Sie eine der folgenden Optionen:
    >
    > * [Azure Cloud Shell](../cloud-shell/quickstart.md)
    > * [Bash auf Ubuntu unter Windows 10](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Verwenden Sie in der SSH-Sitzung die folgenden Befehle, um Bind zu installieren:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Um Bind zur Weiterleitung von Anforderungen zur Namensauflösung an Ihren lokalen DNS-Server zu konfigurieren, verwenden Sie den folgenden Text als Inhalt der Datei `/etc/bind/named.conf.options`:

        acl goodclients {
            10.0.0.0/16; # Replace with the IP address range of the virtual network
            10.1.0.0/16; # Replace with the IP address range of the on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with the IP address of the on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform to RFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > Ersetzen Sie die Werte im Abschnitt `goodclients` durch den IP-Adressbereich des virtuellen Netzwerks und des lokalen Netzwerks. In diesem Abschnitt werden die Adressen definiert, von denen dieser DNS-Server Anforderungen akzeptiert.
    >
    > Ersetzen Sie den Eintrag `192.168.0.1` im Abschnitt `forwarders` durch die IP-Adresse Ihres lokalen DNS-Servers. Dieser Eintrag routet DNS-Anforderungen zur Auflösung an Ihren lokalen DNS-Server.

    Verwenden Sie zum Bearbeiten dieser Datei den folgenden Befehl:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Zum Speichern der Datei drücken Sie __STRG+X__, __Y__ und dann die __EINGABETASTE__.

4. Verwenden Sie aus der SSH-Sitzung den folgenden Befehl:

    ```bash
    hostname -f
    ```

    Dieser Befehl gibt einen Wert zurück, der in etwa wie folgt aussieht:

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    Der Text `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` ist das __DNS-Suffix__ für dieses virtuelle Netzwerk. Speichern Sie diesen Wert, weil der später wieder verwendet wird.

5. Um Bind zum Auflösen von DNS-Namen für Ressourcen innerhalb des virtuellen Netzwerks zu konfigurieren, verwenden Sie den folgenden Text als Inhalt der Datei `/etc/bind/named.conf.local`:

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]
    > Sie müssen hierbei `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` durch das DNS-Suffix ersetzen, das Sie zuvor abgerufen haben.

    Verwenden Sie zum Bearbeiten dieser Datei den folgenden Befehl:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Zum Speichern der Datei drücken Sie __STRG+X__, __Y__ und dann die __EINGABETASTE__.

6. Verwenden Sie den folgenden Befehl, um Bind zu starten:

    ```bash
    sudo service bind9 restart
    ```

7. Um zu überprüfen, ob Bind die Namen der Ressourcen in Ihrem lokalen Netzwerk auflösen kann, verwenden Sie die folgenden Befehle:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > Ersetzen Sie `dns.mynetwork.net` durch den vollqualifizierten Domänennamen (FQDN) einer Ressource in Ihrem lokalen Netzwerk.
    >
    > Ersetzen Sie `10.0.0.4` durch die __interne IP-Adresse__ Ihres benutzerdefinierten DNS-Servers im virtuellen Netzwerk.

    Die Ausgabe ähnelt dem folgenden Text:

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Konfigurieren des virtuellen Netzwerks zur Verwendung des benutzerdefinierten DNS-Servers

Führen Sie die folgenden Schritte aus, damit das virtuelle Netzwerk anstelle des rekursiven Azure-Resolvers den benutzerdefinierten DNS-Server verwendet:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) das virtuelle Netzwerk aus, und klicken Sie dann auf __DNS-Server__.

2. Wählen Sie __Benutzerdefiniert__ aus, und geben Sie die __interne IP-Adresse__ des benutzerdefinierten DNS-Servers ein. Klicken Sie abschließend auf __Speichern__.

    ![Festlegen des benutzerdefinierten DNS-Servers für das Netzwerk](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-the-on-premises-dns-server"></a>Konfigurieren des lokalen DNS-Servers

Im vorherigen Abschnitt haben Sie den benutzerdefinierten DNS-Server so konfiguriert, dass Anforderungen an den lokalen DNS-Server weitergeleitet werden. Als Nächstes müssen Sie den lokalen DNS-Server so konfigurieren, dass Anforderungen an den benutzerdefinierten DNS-Server weitergeleitet werden.

Informationen dazu, wie Sie Ihren DNS-Server konfigurieren, finden Sie in der Dokumentation zu Ihrer DNS-Serversoftware. Suchen Sie hierbei nach den Schritten zum Konfigurieren einer __bedingten Weiterleitung__.

Bei einer bedingten Weiterleitung werden nur Anforderungen für ein bestimmtes DNS-Suffix weitergeleitet. In diesem Fall müssen Sie eine Weiterleitung für das DNS-Suffix des virtuellen Netzwerks konfigurieren. Anforderungen an dieses Suffix sollen an die IP-Adresse des benutzerdefinierten DNS-Servers weitergeleitet werden. 

Der folgende Text ist eine Beispielkonfiguration für eine bedingte Weiterleitung für die DNS-Software **Bind**:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server's internal IP address
    };

Informationen zur Verwendung von DNS in **Windows Server 2016** finden Sie in der [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone)-Dokumentation.

Nachdem Sie den lokalen DNS-Server konfiguriert haben, können Sie mithilfe von `nslookup` überprüfen, ob Namen im virtuellen Netzwerk aufgelöst werden können. Beispiel: 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

In diesem Beispiel wird der lokale DNS-Server unter 196.168.0.4 dazu verwendet, den Namen des benutzerdefinierten DNS-Servers aufzulösen. Ersetzen Sie die IP-Adresse durch die des lokalen DNS-Servers. Ersetzen Sie die Adresse `dnsproxy` durch den vollqualifizierten Domänennamen des benutzerdefinierten DNS-Servers.

## <a name="optional-control-network-traffic"></a>Optional: Steuern des Netzwerkdatenverkehrs

Sie können Netzwerksicherheitsgruppen (NSGs) oder benutzerdefinierte Routen dazu verwenden, den Netzwerkdatenverkehr zu steuern. NSGs ermöglichen das Filtern das eingehenden und ausgehenden Datenverkehrs, und Sie können Datenverkehr zulassen oder untersagen. Mithilfe von benutzerdefinierten Routen können Sie den Datenfluss zwischen den Ressourcen im virtuellen Netzwerk, dem Internet und dem lokalen Netzwerk steuern.

> [!WARNING]
> HDInsight erfordert eingehenden Zugriff auf bestimmte IP-Adressen in der Azure-Cloud sowie unbeschränkten ausgehenden Zugriff. Wenn Sie NSGs oder benutzerdefinierte Routen zum Steuern des Datenverkehrs verwenden, müssen Sie die folgenden Schritte ausführen:
>
> 1. Ermitteln Sie die IP-Adressen für den Standort, der Ihr lokales Netzwerk enthält. Eine Liste der benötigten IPs nach Standort finden Sie unter [Erforderliche IP-Adressen](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).
>
> 2. Lassen Sie eingehenden Datenverkehr von diesen IP-Adressen zu.
>
>    * __NSG__: Lassen Sie __eingehenden__ Datenverkehr auf Port __443__ aus dem __Internet__ zu.
>    * __Benutzerdefinierte Route__: Legen Sie für __Nächster Hop__ als Routentyp __Internet__ fest.

Ein Beispiel zur Verwendung von Azure PowerShell oder der Azure CLI zum Erstellen von NSGs finden Sie im Dokument [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg).

## <a name="create-the-hdinsight-cluster"></a>Erstellen des HDInsight-Clusters

> [!WARNING]
> Sie müssen den benutzerdefinierten DNS-Server konfigurieren, bevor Sie HDInsight im virtuellen Netzwerk installieren.

Folgen Sie den Anweisungen im Dokument [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](./hdinsight-hadoop-create-linux-clusters-portal.md), um einen HDInsight-Cluster zu erstellen.

> [!WARNING]
> * Während der Clustererstellung müssen Sie den Standort auswählen, der Ihr virtuelles Netzwerk enthält.
>
> * Im Abschnitt __Erweiterte Einstellungen__ der Konfiguration müssen Sie das zuvor erstellte virtuelle Netzwerk und das Subnetz auswählen.

## <a name="connecting-to-hdinsight"></a>Verbindungsherstellung mit HDInsight

In der Dokumentation zu HDInsight wird in den meisten Fällen vorausgesetzt, dass Sie über das Internet auf den Cluster zugreifen können. Dies würde z.B. bedeuten, dass Sie eine Verbindung mit dem Cluster unter https://CLUSTERNAME.azurehdinsight.net herstellen können. Diese Adresse verwendet das öffentliche Gateway, das nicht verfügbar ist, wenn Sie über NSGs oder benutzerdefinierte Routen den Zugriff aus dem Internet beschränken.

Zur direkten Verbindung mit HDInsight über das virtuelle Netzwerk führen Sie die folgenden Schritte aus:

1. Verwenden Sie eine der folgenden Methoden, um die internen FQDNs der HDInsight-Clusterknoten zu ermitteln:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

2. Informationen dazu, wie Sie den Port ermitteln, auf dem ein Dienst verfügbar ist, finden Sie im Dokument [Von HDInsight verwendete Ports und URIs](./hdinsight-hadoop-port-settings-for-services.md).

    > [!IMPORTANT]
    > Einige der auf den Hauptknoten gehosteten Dienste sind zu jedem Zeitpunkt nur auf einem Knoten aktiv. Wenn Sie versuchen, auf einen Dienst auf einem der Hauptknoten zuzugreifen und ein Fehler auftritt, wechseln Sie zum anderen Hauptknoten.
    >
    > Beispielsweise ist Ambari immer nur auf einem Hauptknoten aktiv. Wenn Sie versuchen, auf dem Hauptknoten auf Ambari zuzugreifen und ein 404-Fehler zurückgegeben wird, wird Ambari auf dem anderen Hauptknoten ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Verwendung von HDInsight in einem virtuellen Netzwerk finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](./hdinsight-extend-hadoop-virtual-network.md).

* Weitere Informationen zu virtuellen Azure-Netzwerken finden Sie in der [Übersicht zu virtuellen Azure-Netzwerken](../virtual-network/virtual-networks-overview.md).

* Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie in der [Übersicht zu Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).

* Weitere Informationen zu benutzerdefinierten Routen finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md).

