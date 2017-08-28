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
ms.date: 08/14/2017
ms.author: jroth
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 67ba43f9456bbeffbf602067586143c4c68af672
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Verbinden mit SQL Server-Instanzen auf virtuellen Azure-Maschinen (Ressourcen-Manager)
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-sql-connect.md)
> * [Klassisch](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Übersicht

Dieses Thema beschreibt, wie eine Verbindung mit einer SQL Server-Instanz hergestellt wird, die auf einem virtuellen Azure-Computer ausgeführt wird. Es behandelt eine Reihe [allgemeiner Konnektivitätsszenarios](#connection-scenarios) und beschreibt dann [ausführliche Schritte für das Konfigurieren von SQL Server-Konnektivität auf einer Azure-VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Eine vollständige exemplarische Vorgehensweise, die Bereitstellung und Konnektivität behandelt, finden Sie unter [Bereitstellen eines virtuellen SQL Server-Computers in Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Konnektivitätsszenarien

Wie ein Client eine Verbindung mit SQL Server auf einem virtuellen Computer herstellt, ist abhängig vom Standort des Clients und von der Konfiguration des Netzwerks.

Wenn Sie einen virtuellen SQL Server-Computer über das Azure-Portal bereitstellen, können Sie die Art der **SQL-Konnektivität** angeben.

![SQL-Verbindungsoption „Öffentlich“ während der Bereitstellung](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Verfügbare Konnektivitätsoptionen:

| Option | Beschreibung |
|---|---|
| **Öffentlich** | Verbinden mit SQL Server über das Internet |
| **Privat** | Verbinden mit SQL Server innerhalb des gleichen virtuellen Netzwerks |
| **Lokal** | Lokales Verbinden mit SQL Server auf dem gleichen virtuellen Computer | 

In den folgenden Abschnitten werden die Optionen **Öffentlich** und **Privat** näher erläutert.

## <a name="connect-to-sql-server-over-the-internet"></a>Verbinden mit SQL Server über das Internet

Wenn Sie mit dem SQL Server-Datenbankmodul eine Verbindung über das Internet herstellen möchten, wählen Sie im Portal beim Bereitstellen für **SQL-Konnektivität** den Typ **Öffentlich** aus. Das Portal führt automatisch folgende Schritte aus:

* Es aktiviert das TCP/IP-Protokoll für SQL Server.
* Es konfiguriert eine Firewallregel, um den SQL Server-TCP-Port (standardmäßig 1433) zu öffnen.
* Es aktiviert die für den öffentlichen Zugriff erforderliche SQL Server-Authentifizierung.
* Es konfiguriert die Netzwerksicherheitsgruppe auf dem virtuellen Computer für sämtlichen TCP-Datenverkehr am SQL Server-Port.

> [!IMPORTANT]
> Durch die VM-Images für die Editionen SQL Server Developer und Express wird das TCP/IP-Protokoll nicht automatisch aktiviert. Für die Editionen Developer und Express müssen Sie den SQL Server-Konfigurations-Manager verwenden, um [das TCP/IP-Protokoll nach dem Erstellen des virtuellen Computers manuell zu aktivieren](#manualtcp).

Jeder Client mit Internetzugriff kann eine Verbindung mit der SQL Server-Instanz herstellen. Dazu muss entweder die öffentliche IP-Adresse des virtuellen Computers oder eine dieser IP-Adresse zugewiesene DNS-Bezeichnung angegeben werden. Wird der SQL Server-Port 1433 verwendet, müssen Sie ihn nicht in der Verbindungszeichenfolge angeben. Die folgende Verbindungszeichenfolge stellt eine Verbindung mit einem virtuellen SQL-Computer mit der DNS-Bezeichnung `sqlvmlabel.eastus.cloudapp.azure.com` unter Verwendung der SQL-Authentifizierung her. (Sie können auch die öffentliche IP-Adresse verwenden.)

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

Zwar wird so die Konnektivität für Clients über das Internet ermöglicht, dies bedeutet jedoch nicht, dass jeder Verbindungen mit ihrem SQL Server herstellen kann. Clients von außerhalb benötigen den richtigen Benutzernamen und das entsprechende Kennwort. Zur Erhöhung der Sicherheit können Sie allerdings den bekannten Port 1433 vermeiden. Wenn Sie beispielsweise SQL Server zum Lauschen an Port 1500 konfiguriert und entsprechende Regeln für die Firewall und die Netzwerksicherheitsgruppe eingerichtet haben, können Sie eine Verbindung herstellen, indem Sie die Portnummer an den Servernamen anfügen. Im folgenden Beispiel (das auf dem vorherigen Beispiel basiert) wird dem Servernamen die benutzerdefinierte Portnummer **1500** hinzugefügt:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> Wenn Sie SQL Server auf einem virtuellen Computer über das Internet abfragen, gelten für alle ausgehenden Daten des Azure-Datencenters die normalen [Preise für ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="connect-to-sql-server-within-a-virtual-network"></a>Verbinden mit SQL Server innerhalb eines virtuellen Netzwerks

Wenn Sie im Portal für **SQL-Konnektivität** den Typ **Privat** auswählen, werden die meisten Einstellungen genau wie beim Typ **Öffentlich** konfiguriert. Der einzige Unterschied ist, dass es keine Netzwerksicherheitsgruppen-Regel gibt, die externen Datenverkehr am SQL Server-Port (standardmäßig 1433) zulässt.

> [!IMPORTANT]
> Durch die VM-Images für die Editionen SQL Server Developer und Express wird das TCP/IP-Protokoll nicht automatisch aktiviert. Für die Editionen Developer und Express müssen Sie den SQL Server-Konfigurations-Manager verwenden, um [das TCP/IP-Protokoll nach dem Erstellen des virtuellen Computers manuell zu aktivieren](#manualtcp).

Private Konnektivität wird häufig in Verbindung mit [Virtual Network](../../../virtual-network/virtual-networks-overview.md) verwendet, was verschiedene Szenarien ermöglicht. Sie können Verbindungen mit VMs innerhalb des gleichen virtuellen Netzwerks herstellen, selbst wenn diese VMs in verschiedenen Ressourcengruppen ausgeführt werden. Mithilfe eines [Site-to-Site-VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md)können Sie eine hybride Architektur erstellen, die VMs mit lokalen Netzwerken und Computern verbindet.

Virtuelle Netzwerke ermöglichen auch das Einbinden Ihrer virtuellen Azure-Computer in eine Domäne. Dies ist die einzige Möglichkeit, wie Sie Windows-Authentifizierung mit SQL Server verwenden können. Für alle anderen Verbindungsszenarien muss SQL-Authentifizierung mit Benutzernamen und Kennwörtern verwendet werden.

Vorausgesetzt, Sie haben DNS in Ihrem virtuellen Netzwerk konfiguriert, können Sie eine Verbindung mit Ihrer SQL Server-Instanz herstellen, indem Sie den VM-Computernamen für SQL Server in der Verbindungszeichenfolge angeben. Im folgenden Beispiel wird zudem angenommen, dass darüber hinaus Windows-Authentifizierung konfiguriert und dem Benutzer Zugriff auf die SQL Server-Instanz erteilt wurde.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a> Ändern der SQL-Konnektivitätseinstellungen

Die Konnektivitätseinstellungen für den virtuellen SQL Server-Computer können im Azure-Portal geändert werden.

1. Wählen Sie im Azure-Portal die Option **Virtual Machines** aus.

2. Wählen Sie den virtuellen SQL Server-Computer aus.

3. Klicken Sie unter **Einstellungen** auf **SQL Server-Konfiguration**.

4. Legen Sie **SQL-Konnektivitätsebene** auf die gewünschte Einstellung fest. In diesem Bereich können Sie optional den SQL Server-Port oder die Einstellungen für die SQL-Authentifizierung ändern.

   ![Ändern der SQL-Konnektivität](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Warten Sie einige Minuten, bis das Update abgeschlossen ist.

   ![Aktualisierungsbenachrichtigung für den virtuellen SQL-Computer](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a> Aktivieren von TCP/IP für die Editionen Developer und Express

Wenn Sie die SQL Server-Konnektivitätseinstellungen ändern, wird das TCP/IP-Protokoll für die SQL Server-Editionen Developer und Express nicht automatisch aktiviert. In den folgenden Schritten wird die manuelle Aktivierung von TCP/IP erläutert, sodass Sie per IP-Adresse eine Remoteverbindung herstellen können.

Stellen Sie zunächst mit Remotedesktop eine Verbindung mit dem SQL Server-Computer her.

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Aktivieren Sie dann mithilfe des **SQL Server-Konfigurations-Managers** das TCP/IP-Protokoll.

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>Verbinden mit SSMS

Die folgenden Schritte zeigen, wie Sie eine optionale DNS-Bezeichnung für Ihren virtuellen Azure-Computer erstellen und anschließend eine Verbindung mit SQL Server Management Studio (SSMS) herstellen.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Nächste Schritte

Anweisungen zur Bereitstellung für diese Konnektivitätsschritte finden Sie unter [Bereitstellen eines virtuellen SQL Server-Computers in Azure](virtual-machines-windows-portal-sql-server-provision.md).

Weitere Informationen zum Ausführen von SQL Server auf virtuellen Azure-Computern finden Sie unter [SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).
