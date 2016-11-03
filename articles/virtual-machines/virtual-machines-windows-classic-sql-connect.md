<properties
    pageTitle="Herstellen einer Verbindung mit einem virtuellen SQL Server-Computer (Klassisch)| Microsoft Azure"
    description="Erfahren Sie, wie eine Verbindung mit einer SQL Server-Instanz hergestellt wird, die auf einem virtuellen Computer in Azure ausgeführt wird. In diesem Thema wird das klassische Bereitstellungsmodell verwendet. Die Szenarien unterscheiden sich abhängig von der Netzwerkkonfiguration und dem Clientstandort."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />


# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Herstellen einer Verbindung mit einem virtuellen SQL Server-Computer in Azure (Klassische Bereitstellung)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-connect.md)
- [Klassisch](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Übersicht

Dieses Thema beschreibt, wie eine Verbindung mit einer SQL Server-Instanz hergestellt wird, die auf einem virtuellen Azure-Computer ausgeführt wird. Es behandelt eine Reihe [allgemeiner Konnektivitätsszenarios](#connection-scenarios) und beschreibt dann [ausführliche Schritte für das Konfigurieren von SQL Server-Konnektivität auf einer Azure-VM](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] Wenn Sie Resource Manager-VMs verwenden, finden Sie weitere Informationen unter [Verbinden mit SQL Server-Instanzen auf virtuellen Azure-Maschinen (Resource Manager)](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Konnektivitätsszenarien

Die Weise, wie ein Client Verbindungen mit SQL Server auf einem virtuellen Computer herstellt, unterscheidet sich je nach dem Standort des Clients und der Konfiguration des Computers und/oder Netzwerks. Zu diesen Szenarien gehören:

- [Verbinden mit SQL Server im selben Clouddienst](#connect-to-sql-server-in-the-same-cloud-service)
- [Verbinden mit SQL Server über das Internet](#connect-to-sql-server-over-the-internet)
- [Verbinden mit SQL Server innerhalb des gleichen virtuellen Netzwerks](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Bevor Sie mit einer dieser Methoden eine Verbindung herstellen, führen Sie die [Schritte in diesem Artikel zum Konfigurieren der Verbindung](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)aus.

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Verbinden mit SQL Server im selben Clouddienst

Im gleichen Clouddienst können mehrere virtuelle Computer erstellt werden. Das Konzept dieses Szenarios mit virtuellen Computern ist unter [Verbinden virtueller Computer mit einem virtuellen Netzwerk oder Clouddienst](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service)erläutert. Dieses Szenario bedeutet, dass ein Client auf einem virtuellen Computer versucht, eine Verbindung mit SQL Server auf einem anderen virtuellen Computer im selben Clouddienst herzustellen.

In diesem Szenario können Sie die Verbindung mit dem virtuellen Computer **Name** (im Portal auch als **Computername** oder **Hostname** dargestellt) herstellen. Dies ist der Name, den Sie für den virtuellen Computer während der Erstellung angegeben haben. Beispiel: Wenn Sie dem virtuellen SQL-Computer den Namen **mysqlvm**gegeben haben, könnte ein virtueller Clientcomputer im selben Clouddienst mit der folgenden Verbindungszeichenfolge eine Verbindung herstellen:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinden mit SQL Server über das Internet

Wenn Sie eine Verbindung mit Ihrem SQL Server-Datenbankmodul aus dem Internet herstellen möchten, müssen Sie einen VM-Endpunkt für eingehende TCP-Kommunikation erstellen. Dieser Azure-Konfigurationsschritt leitet den eingehenden Datenverkehr des TCP-Ports zu einem TCP-Port, auf den der virtuelle Computer zugreifen kann.

Um über das Internet eine Verbindung herzustellen, müssen Sie den DNS-Namen des virtuellen Computers und die Portnummer des Endpunkts für den virtuellen Computers verwenden (wird weiter unten in diesem Artikel konfiguriert). Um den DNS-Namen zu suchen, navigieren Sie zum Azure-Portal, und wählen Sie **Virtuelle Computer (klassisch)**aus. Wählen Sie dann den virtuellen Computer aus. Der **DNS-Name** wird im Abschnitt **Übersicht** angezeigt.

Beispiel: Ein klassischer virtueller Computer mit dem Namen **mysqlvm** mit einem DNS-Namen **mysqlvm7777.cloudapp.net** und dem Endpunkt **57500** für den virtuellen Computer. Wenn eine korrekt konfigurierte Verbindung vorhanden ist, könnte die folgende Verbindungszeichenfolge verwendet werden, um von überall über das Internet auf den virtuellen Computer zuzugreifen:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Zwar wird so die Konnektivität für Clients über das Internet ermöglicht, dies bedeutet jedoch nicht, dass jeder Verbindungen mit ihrem SQL Server herstellen kann. Clients von außerhalb benötigen den richtigen Benutzernamen und das entsprechende Kennwort. Zur weiteren Erhöhung der Sicherheit können Sie einen anderen als den bekannten Port 1433 für den öffentlichen Endpunkt des virtuellen Computers verwenden. Nach Möglichkeit sollten Sie dem Endpunkt auch eine ACL hinzufügen, um den Datenverkehr auf die von Ihnen zugelassenen Clients einzuschränken. Weitere Informationen zur Verwendung von ACLs für Endpunkte finden Sie unter [Verwalten der ACL für einen Endpunkt](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint)

>[AZURE.NOTE] Sie müssen beachten, dass beim Einsatz dieser Technik für die Kommunikation mit SQL Server für alle ausgehenden Daten aus dem Azure-Rechenzentrum normale [Preise für ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/)gelten.

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinden mit SQL Server innerhalb des gleichen virtuellen Netzwerks

[Virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) gibt die Möglichkeit zu weiteren Szenarien. Sie können Verbindungen mit VMs innerhalb des gleichen virtuellen Netzwerks herstellen, selbst wenn diese VMs in verschiedenen Clouddiensten ausgeführt werden. Mithilfe eines [Site-to-Site-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)können Sie eine hybride Architektur erstellen, die VMs mit lokalen Netzwerken und Computern verbindet.

Virtuelle Netzwerke ermöglichen auch das Einbinden Ihrer Azure-VMs in eine Domäne. Dies ist die einzige Möglichkeit, wie Sie Windows-Authentifizierung mit SQL Server verwenden können. Für alle anderen Verbindungsszenarien muss SQL-Authentifizierung mit Benutzernamen und Kennwörtern verwendet werden.

Wenn Sie eine Domänenumgebung mit Windows-Authentifizierung konfigurieren möchten, müssen Sie die Schritte in diesem Artikel zum Konfigurieren des öffentlichen Endpunkts oder der SQL-Authentifizierung und der entsprechenden Anmeldeinformationen nicht ausführen. In diesem Szenario können Sie eine Verbindung mit Ihrer SQL Server-Instanz herstellen, indem Sie den Namen des virtuellen SQL Server-Computers in der Verbindungszeichenfolge angeben. Im folgenden Beispiel wird angenommen, dass darüber hinaus Windows-Authentifizierung konfiguriert und dem Benutzer Zugriff auf die SQL Server-Instanz erteilt wurde.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Schritte zum Konfigurieren von SQL Server-Konnektivität in einer Azure-VM

Die folgenden Schritte veranschaulichen, wie Sie mithilfe von SQL Server Management Studio (SSMS) über das Internet eine Verbindung mit der SQL Server-Instanz herstellen. Mit den gleichen Schritten ermöglichen Sie jedoch auch den Zugriff der lokal und in Azure ausgeführten Anwendungen auf den virtuellen SQL Server-Computer.

Bevor Sie eine Verbindung mit der Instanz von SQL Server über einen anderen virtuellen Computer oder über das Internet herstellen können, müssen Sie folgende Aufgaben abschließen, die in den nächsten Abschnitten beschrieben werden:

- [Erstellen eines TCP-Endpunkts für den virtuellen Computer](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Öffnen der TCP-Ports in der Windows-Firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfigurieren von SQL Server für das Abhören des TCP-Protokolls](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfigurieren von SQL Server für die Authentifizierung mit gemischtem Modus](#configure-sql-server-for-mixed-mode-authentication)
- [Erstellen von Anmeldenamen für die SQL Server-Authentifizierung](#create-sql-server-authentication-logins)
- [Bestimmen des DNS-Namens des virtuellen Computers](#determine-the-dns-name-of-the-virtual-machine)
- [Verbinden mit dem Datenbankmodul von einem anderen Computer aus](#connect-to-the-database-engine-from-another-computer)

Der Verbindungspfad wird von folgendem Diagramm zusammengefasst:

![Verbinden mit einem virtuellen SQL Server-Computer](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie außerdem AlwaysOn-Verfügbarkeitsgruppen zum Erzielen von hoher Verfügbarkeit und Notfallwiederherstellung einsetzen möchten, sollten Sie die Implementierung eines Listeners in Erwägung ziehen. Datenbankclients stellen dann Verbindungen mit dem Listener anstelle direkter Verbindungen mit den SQL Server-Instanzen her. Der Listener leitet Clients auf das primäre Replikat in der Verfügbarkeitsgruppe um. Weitere Informationen finden Sie unter [Konfigurieren eines ILB-Listeners für AlwaysOn-Verfügbarkeitsgruppen in Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Es ist wichtig, alle bewährten Sicherheitsmethoden für SQL Server bei der Ausführung auf virtuellen Azure-Computern durchzuarbeiten. Weitere Informationen finden Sie unter [Sicherheitsüberlegungen für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-security.md).

[Sehen Sie sich den Lernpfad](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) für SQL Server auf virtuellen Azure-Computern an. 

Weitere Informationen zum Ausführen von SQL Server auf virtuellen Azure-Computern finden Sie unter [SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).



<!--HONumber=Oct16_HO2-->


