---
title: Verbinden mit virtuellen SQL Server-Computern (Resource Manager) | Microsoft Docs
description: "Erfahren Sie, wie eine Verbindung mit einer SQL Server-Instanz hergestellt wird, die auf einem virtuellen Computer in Azure ausgeführt wird. In diesem Thema wird das klassische Bereitstellungsmodell verwendet. Die Szenarien unterscheiden sich abhängig von der Netzwerkkonfiguration und dem Clientstandort."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/15/2016
ms.author: jroth
translationtype: Human Translation
ms.sourcegitcommit: 0c23ee550d8ac88994e8c7c54a33d348ffc24372
ms.openlocfilehash: 699d0c37adb9618827b6b39d74d09c2f9ba92913


---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Verbinden mit SQL Server-Instanzen auf virtuellen Azure-Maschinen (Ressourcen-Manager)
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-sql-connect.md)
> * [Klassisch](../sqlclassic/virtual-machines-windows-classic-sql-connect.md)
> 
> 

## <a name="overview"></a>Übersicht
Dieses Thema beschreibt, wie eine Verbindung mit einer SQL Server-Instanz hergestellt wird, die auf einem virtuellen Azure-Computer ausgeführt wird. Es behandelt eine Reihe [allgemeiner Konnektivitätsszenarios](#connection-scenarios) und beschreibt dann [ausführliche Schritte für das Konfigurieren von SQL Server-Konnektivität auf einer Azure-VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Den Artikel zum klassischen Bereitstellungsmodell finden Sie unter [Herstellen einer Verbindung mit einem virtuellen SQL Server-Computer in Azure (klassische Bereitstellung)](../sqlclassic/virtual-machines-windows-classic-sql-connect.md).

Eine vollständige exemplarische Vorgehensweise, die Bereitstellung und Konnektivität behandelt, finden Sie unter [Bereitstellen eines virtuellen SQL Server-Computers in Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Konnektivitätsszenarien
Die Weise, wie ein Client Verbindungen mit SQL Server auf einem virtuellen Computer herstellt, unterscheidet sich je nach dem Standort des Clients und der Konfiguration des Computers und/oder Netzwerks. Zu diesen Szenarien gehören:

* [Verbinden mit SQL Server über das Internet](#connect-to-sql-server-over-the-internet)
* [Verbinden mit SQL Server innerhalb des gleichen virtuellen Netzwerks](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinden mit SQL Server über das Internet
Wenn Sie über das Internet eine Verbindung mit Ihrem SQL Server-Datenbankmodul herstellen möchten, sind mehrere Schritte erforderlich: Konfigurieren der Firewall, Aktivieren der SQL-Authentifizierung und Konfigurieren der Netzwerksicherheitsgruppe. Sie benötigen eine Regel für die Netzwerksicherheitsgruppe, die TCP-Datenverkehr über Port 1433 zulässt.

Wenn Sie das Portal verwenden, um mit Resource Manager ein Image für virtuelle Computer mit SQL Server bereitzustellen, werden diese Schritte für Sie ausgeführt, wenn Sie die SQL-Verbindungsoption **Öffentlich** auswählen:

![SQL-Verbindungsoption „Öffentlich“ während der Bereitstellung](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Wurde diese Option während der Bereitstellung nicht ausgewählt, können Sie SQL Server und Ihre virtuellen Computer manuell konfigurieren. Führen Sie dazu die [Schritte in diesem Artikel zum manuellen Konfigurieren der Verbindung](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm) aus.

> [!NOTE]
> Mit dem VM-Image für die SQL Server Express Edition wird das TCP/IP-Protokoll nicht automatisch aktiviert. Für die Express Edition müssen Sie den SQL Server-Konfigurations-Manager verwenden, um [das TCP/IP-Protokoll nach dem Erstellen der VM manuell zu aktivieren](#configure-sql-server-to-listen-on-the-tcp-protocol) .
> 
> 

Nach der manuellen Konfiguration kann jeder Client mit Internetzugriff eine Verbindung mit der SQL Server-Instanz herstellen. Dazu muss entweder die öffentliche IP-Adresse der virtuellen Maschine oder die dieser IP-Adresse zugewiesene DNS-Bezeichnung angegeben werden. Wird der SQL Server-Port 1433 verwendet, müssen Sie ihn nicht in der Verbindungszeichenfolge angeben.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Zwar wird so die Konnektivität für Clients über das Internet ermöglicht, dies bedeutet jedoch nicht, dass jeder Verbindungen mit ihrem SQL Server herstellen kann. Clients von außerhalb benötigen den richtigen Benutzernamen und das entsprechende Kennwort. Um die Sicherheit weiter zu erhöhen, können Sie den bekannten Port 1433 vermeiden. Wenn Sie beispielsweise SQL Server zum Lauschen an Port 1500 konfiguriert und entsprechende Regeln für die Firewall und die Netzwerksicherheitsgruppe eingerichtet haben, können Sie eine Verbindung herstellen, indem Sie die Portnummer an den Servernamen anfügen. Siehe folgendes Beispiel:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

> [!NOTE]
> Sie müssen beachten, dass beim Einsatz dieser Technik für die Kommunikation mit SQL Server für alle ausgehenden Daten aus dem Azure-Rechenzentrum normale [Preise für ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/)gelten.
> 
> 

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinden mit SQL Server innerhalb des gleichen virtuellen Netzwerks
[Virtuelles Netzwerk](../../../virtual-network/virtual-networks-overview.md) gibt die Möglichkeit zu weiteren Szenarien. Sie können Verbindungen mit VMs innerhalb des gleichen virtuellen Netzwerks herstellen, selbst wenn diese VMs in verschiedenen Ressourcengruppen ausgeführt werden. Mithilfe eines [Site-to-Site-VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md)können Sie eine hybride Architektur erstellen, die VMs mit lokalen Netzwerken und Computern verbindet.

Virtuelle Netzwerke ermöglichen auch das Einbinden Ihrer Azure-VMs in eine Domäne. Dies ist die einzige Möglichkeit, wie Sie Windows-Authentifizierung mit SQL Server verwenden können. Für alle anderen Verbindungsszenarien muss SQL-Authentifizierung mit Benutzernamen und Kennwörtern verwendet werden.

Wenn Sie das Portal verwenden, um mit Resource Manager ein Image für virtuelle SQL Server-Computer bereitzustellen, werden die richtigen Firewallregeln für die Kommunikation im virtuellen Netzwerk eingerichtet, wenn Sie die SQL-Verbindungsoption **Privat** auswählen. Wurde diese Option während der Bereitstellung nicht ausgewählt, können Sie SQL Server und Ihre virtuellen Computer manuell konfigurieren. Führen Sie dazu die [Schritte in diesem Artikel zum manuellen Konfigurieren der Verbindung](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm) aus. Wenn Sie jedoch eine Domänenumgebung mit Windows-Authentifizierung konfigurieren möchten, brauchen Sie die Schritte in diesem Artikel zum Konfigurieren von SQL-Authentifizierung und der entsprechenden Anmeldeinformationen nicht auszuführen. Netzwerksicherheitsgruppen-Regeln für den Zugriff über das Internet müssen ebenfalls nicht konfiguriert werden.

> [!NOTE]
> Mit dem VM-Image für die SQL Server Express Edition wird das TCP/IP-Protokoll nicht automatisch aktiviert. Für die Express Edition müssen Sie den SQL Server-Konfigurations-Manager verwenden, um [das TCP/IP-Protokoll nach dem Erstellen der VM manuell zu aktivieren](#configure-sql-server-to-listen-on-the-tcp-protocol) .
> 
> 

Vorausgesetzt, Sie haben DNS in Ihrem virtuellen Netzwerk konfiguriert, können Sie eine Verbindung mit Ihrer SQL Server-Instanz herstellen, indem Sie den VM-Computernamen für SQL Server in der Verbindungszeichenfolge angeben. Im folgenden Beispiel wird zudem angenommen, dass darüber hinaus Windows-Authentifizierung konfiguriert und dem Benutzer Zugriff auf die SQL Server-Instanz erteilt wurde.

    "Server=mysqlvm;Integrated Security=true"

Beachten Sie, dass Sie in diesem Szenario auch die IP-Adresse der VM angeben könnten.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Schritte zum manuellen Konfigurieren von SQL Server-Konnektivität auf einer Azure-VM
Die folgenden Schritte veranschaulichen, wie Sie mithilfe von SQL Server Management Studio (SSMS) manuell die Konnektivität mit der SQL Server-Instanz einrichten und anschließend optional über das Internet eine Verbindung herstellen. Beachten Sie, dass viele dieser Schritte für Sie ausgeführt werden, wenn Sie die entsprechenden SQL Server-Konnektivitätsoptionen im Portal auswählen.

Bevor Sie eine Verbindung mit der Instanz von SQL Server über einen anderen virtuellen Computer oder über das Internet herstellen können, müssen Sie folgende Aufgaben abschließen, die in den nächsten Abschnitten beschrieben werden:

* [Öffnen der TCP-Ports in der Windows-Firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [Konfigurieren von SQL Server für das Abhören des TCP-Protokolls](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [Konfigurieren von SQL Server für die Authentifizierung mit gemischtem Modus](#configure-sql-server-for-mixed-mode-authentication)
* [Erstellen von Anmeldenamen für die SQL Server-Authentifizierung](#create-sql-server-authentication-logins)
* [Konfigurieren einer eingehenden Netzwerksicherheitsgruppen-Regel](#configure-a-network-security-group-inbound-rule-for-the-vm)
* [Konfigurieren einer DNS-Bezeichnung für die öffentliche IP-Adresse](#configure-a-dns-label-for-the-public-ip-address)
* [Verbinden mit dem Datenbankmodul von einem anderen Computer aus](#connect-to-the-database-engine-from-another-computer)

[!INCLUDE [Connect to SQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Nächste Schritte
Anweisungen zur Bereitstellung für diese Konnektivitätsschritte finden Sie unter [Bereitstellen eines virtuellen SQL Server-Computers in Azure](virtual-machines-windows-portal-sql-server-provision.md).

[Sehen Sie sich den Lernpfad für SQL Server auf virtuellen Azure-Computern an](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) .

Weitere Informationen zum Ausführen von SQL Server auf virtuellen Azure-Computern finden Sie unter [SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).




<!--HONumber=Jan17_HO2-->


