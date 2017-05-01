---
title: "Einrichten von hoher Verfügbarkeit für virtuelle Computer im Azure Resource Manager-Modus | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie, wie Sie eine AlwaysOn-Verfügbarkeitsgruppe mit virtuellen Computern im Azure Resource Manager-Modus erstellen."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 64e85527-d5c8-40d9-bbe2-13045d25fc68
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: d430febee23081b26eee0a68d4beb43228549f52
ms.lasthandoff: 04/11/2017


---
# <a name="configure-always-on-availability-groups-in-azure-virtual-machines-automatically-resource-manager"></a>Automatisches Konfigurieren einer AlwaysOn-Verfügbarkeitsgruppe in Azure Virtual Machines: Resource Manager

Dieses Tutorial zeigt Ihnen, wie Sie eine SQL Server-Verfügbarkeitsgruppe erstellen, die Azure Resource Manager-VMs nutzt. Das Tutorial verwendet Azure-Blätter zum Konfigurieren einer Vorlage. Während Sie dieses Tutorial absolvieren, können Sie die Standardeinstellungen überprüfen, erforderliche Einstellungen eingeben und die Blätter im Portal aktualisieren.

Im gesamten Tutorial wird eine SQL Server-Verfügbarkeitsgruppe in Azure Virtual Machines erstellt, die die folgenden Elemente enthält:

* Ein virtuelles Netzwerk, das mehrere Subnetze enthält, einschließlich eines Front-End- und eines Back-End-Subnetzes
* Zwei Domänencontroller, die eine Active Directory-Domäne aufweisen
* Zwei virtuelle Computer, auf denen SQL Server ausgeführt wird und die im Back-End-Subnetz bereitgestellt und der Active Directory-Domäne hinzugefügt wurden
* Einen Failovercluster aus drei Knoten mit dem Knotenmehrheit-Quorummodell
* Eine Verfügbarkeitsgruppe, die zwei Replikate einer Verfügbarkeitsdatenbank mit synchronem Commit aufweist

In der folgenden Abbildung ist die vollständige Lösung dargestellt.

![Test Lab-Architektur für Verfügbarkeitsgruppen in Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Alle Ressourcen in dieser Lösung gehören zu einer einzigen Ressourcengruppe.

Bevor Sie dieses Tutorial beginnen, prüfen Sie Folgendes:

* Sie besitzen bereits ein Azure-Abonnement. Falls Sie noch keines besitzen, können Sie sich für ein [Testkonto registrieren](http://azure.microsoft.com/pricing/free-trial/).
* Sie wissen bereits, wie ein virtueller SQL Server-Computer mithilfe der GUI aus dem Katalog virtueller Computer bereitgestellt wird. Weitere Informationen finden Sie unter [Bereitstellen eines virtuellen Computers mit SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md).
* Sie verfügen bereits über solide Kenntnisse über Verfügbarkeitsgruppen. Weitere Informationen finden Sie unter [AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Wenn Sie an der Verwendung von Verfügbarkeitsgruppen mit SharePoint interessiert sind, finden Sie Informationen hierzu unter [Konfigurieren von SQL Server 2012 AlwaysOn-Verfügbarkeitsgruppen für SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).
>
>

In diesem Tutorial verwenden Sie das Azure-Portal zu folgenden Zwecken:

* Auswählen der AlwaysOn-Vorlage aus dem Portal.
* Überprüfen der Vorlageneinstellungen und Aktualisieren einiger Konfigurationseinstellungen für Ihre Umgebung.
* Überwachen von Azure beim Erstellen der gesamten Umgebung.
* Herstellen einer Verbindung mit einem Domänencontroller und dann mit einem Server, auf dem SQL Server ausgeführt wird.

[!INCLUDE [availability-group-template](../../../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

## <a name="provision-the-cluster-from-the-gallery"></a>Bereitstellen des Clusters über den Katalog
Azure bietet ein Katalogimage für die gesamte Lösung. Gehen Sie wie folgt vor, um die Vorlage zu suchen:

1. Melden Sie sich mit Ihrem Konto beim Azure-Portal an.
2. Klicken Sie im Azure-Portal auf **+Neu**, um das Blatt **Neu** zu öffnen.
3. Suchen Sie auf dem Blatt **Neu** die Option **AlwaysOn**.
   ![Suchen der AlwaysOn-Vorlage](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
4. Suchen Sie in den Suchergebnissen nach **SQL Server AlwaysOn-Cluster**.
   ![AlwaysOn-Vorlage](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
5. Wählen Sie unter **Bereitstellungsmodell auswählen** die Option **Ressourcen-Manager** aus.

### <a name="basics"></a>Grundlagen
Klicken Sie auf **Grundlagen**, und konfigurieren Sie die folgenden Einstellungen:

* **Administratorbenutzername** ist ein Benutzerkonto mit Domänenadministratorberechtigungen und ein Mitglied der festen Serverrolle „sysadmin“ von SQL Server für beide Instanzen von SQL Server. Verwenden Sie für dieses Tutorial **DomainAdmin**.
* **Kennwort** ist das Kennwort für das Domänenadministratorkonto. Verwenden Sie ein komplexes Kennwort. Bestätigen Sie das Kennwort.
* **Abonnement** ist das Abonnement, das von Azure belastet wird, um alle für die Verfügbarkeitsgruppe bereitgestellten Ressourcen auszuführen. Gibt es für Ihr Konto mehrere Abonnements, können Sie ein anderes Abonnement angeben.
* **Ressourcengruppe** ist der Name der Gruppe, zu der alle Azure-Ressourcen gehören, die über diese Vorlage erstellt werden. Verwenden Sie für dieses Tutorial **SQL-HA-RG**. Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md#resource-groups).
* **Standort** ist die Azure-Region, in der die Ressourcen in diesem Tutorial erstellt werden. Wählen Sie eine Azure-Region aus.

Der folgende Screenshot zeigt ein ausgefülltes Blatt **Grundeinstellungen**:

![Grundlagen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

Klicken Sie auf **OK**.

### <a name="domain-and-network-settings"></a>Domänen- und Netzwerkeinstellungen
Mit dieser Azure-Katalogvorlage werden eine Domäne und Domänencontroller erstellt. Außerdem werden mit ihr ein Netzwerk und zwei Subnetze erstellt. Die Vorlage kann Server nicht in einer vorhandenen Domäne oder einem virtuellen Netzwerk erstellen. Im nächsten Schritt werden die Domänen- und Netzwerkeinstellungen konfiguriert.

Überprüfen Sie auf dem Blatt **Domänen- und Netzwerkeinstellungen** die voreingestellten Werte für die Domänen- und Netzwerkeinstellungen:

* **Gesamtstruktur-Stammdomänenname** ist der Domänenname für die Active Directory-Domäne, die den Cluster hostet. Verwenden Sie für das Tutorial **contoso.com**.
* **Name des virtuellen Netzwerks** ist der Netzwerkname für das virtuelle Azure-Netzwerk. Verwenden Sie für dieses Tutorial **autohaVNET**.
* **Domänencontroller-Subnetzname** ist der Name eines Teils des virtuellen Netzwerks, der den Domänencontroller hostet. Verwenden Sie **subnet-1**. Für dieses Subnetz wird das Adresspräfix **10.0.0.0/24** verwendet.
* **SQL Server-Subnetzname** ist der Name eines Teils des virtuellen Netzwerks, der die Server mit SQL Server und den Dateifreigabenzeugen hostet. Verwenden Sie **subnet-2**. Für dieses Subnetz wird das Adresspräfix **10.0.1.0/26** verwendet.

Weitere Informationen zu virtuellen Netzwerken in Azure finden Sie unter [Virtuelle Netzwerke im Überblick](../../../virtual-network/virtual-networks-overview.md).  

Die **Domänen- und Netzwerkeinstellungen** sollten wie im folgenden Screenshot aussehen:

![Domänen- und Netzwerkeinstellungen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Bei Bedarf können Sie diese Werte ändern. Verwenden Sie für dieses Tutorial die voreingestellten Werte.

Überprüfen Sie die Einstellungen, und klicken Sie dann auf **OK**.

### <a name="availability-group-settings"></a>Einstellungen der Verfügbarkeitsgruppe
Überprüfen Sie unter **Einstellungen der Verfügbarkeitsgruppe** die voreingestellten Werte für die Verfügbarkeitsgruppe und den Listener.

* **Name der Verfügbarkeitsgruppe** ist der Name der Clusterressource für die Verfügbarkeitsgruppe. Verwenden Sie für dieses Tutorial **Contoso-ag**.
* **Listenername der Verfügbarkeitsgruppe** wird vom Cluster und dem internen Lastenausgleichsmodul verwendet. Clients, die sich mit SQL Server verbinden, können diesen Namen für die Verbindung mit dem entsprechenden Replikat der Datenbank verwenden. Verwenden Sie für dieses Tutorial **Contoso-listener**.
* **Port des Verfügbarkeitsgruppenlisteners** gibt den TCP-Port des SQL Server-Listeners an. Verwenden Sie für dieses Tutorial den Standardport **1433**.

Bei Bedarf können Sie diese Werte ändern. Verwenden Sie für dieses Tutorial die voreingestellten Werte.  

![Einstellungen der Verfügbarkeitsgruppe](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

Klicken Sie auf **OK**.

### <a name="virtual-machine-size-storage-settings"></a>Größe des virtuellen Computers, Speichereinstellungen
Wählen Sie unter **Größe des virtuellen Computers, Speichereinstellungen** eine Größe für den virtuellen Computer mit SQL Server aus, und überprüfen Sie die anderen Einstellungen.

* **Größe des virtuellen Computers mit SQL Server** ist die Größe der beiden virtuellen Computer mit SQL Server. Wählen Sie eine für Ihre Workload angemessene Größe für die virtuellen Computer aus. Wenn Sie diese Umgebung für das Tutorial erstellen, verwenden Sie **DS2**. Wählen Sie für Produktionsworkloads für den virtuellen Computer eine Größe, die den jeweiligen Workload unterstützen kann. Viele Produktionsworkloads erfordern mindestens **DS4**. Die Vorlage erstellt zwei virtuelle Computer dieser Größe und installiert auf jedem SQL Server. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Azure installiert die Enterprise Edition von SQL Server. Die Kosten hängen von der Edition und der Größe des virtuellen Computers ab. Ausführliche Informationen zu aktuellen Kosten finden Sie unter [Virtual Machines – Preise](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).
>
>

* **Domänencontroller-VM-Größe** ist die Größe des virtuellen Computers für die Domänencontroller. Verwenden Sie für dieses Tutorial **D2**.
* **Dateifreigabenzeugen-VM-Größe** ist die Größe des virtuellen Computers für den Dateifreigabenzeugen. Verwenden Sie für dieses Tutorial **A1**.
* **SQL-Speicherkonto** ist der Name des Speicherkontos, auf dem die SQL Server-Daten und Betriebssystemdatenträger gespeichert werden. Verwenden Sie für dieses Tutorial **alwaysonsql01**.
* **DC-Speicherkonto** ist der Name des Speicherkontos für die Domänencontroller. Verwenden Sie für dieses Tutorial **alwaysondc01**.
* **Größe des Datenträgers für SQL Server-Daten in TB** ist die Größe des SQL Server-Datenträgers in TB. Geben Sie eine Zahl von 1 bis 4 ein. Verwenden Sie für dieses Tutorial **1**.
* **Speicheroptimierung** legt bestimmte Speicherkonfigurationseinstellungen basierend auf dem Workloadtyp für die SQL Server-VMs fest. Alle virtuellen Computer mit SQL Server in diesem Szenario verwenden Storage Premium, wobei der Azure-Datenträger-Hostcache als schreibgeschützt festgelegt ist. Darüber hinaus können Sie SQL Server-Einstellungen durch Auswahl einer der folgenden drei Einstellungen für die Workload optimieren:

  * **Allgemeine Workload** legt keine bestimmten Konfigurationseinstellungen fest.
  * **Transaktionsverarbeitung** legt die Ablaufverfolgungsflags 1117 und 1118 fest.
  * **Data Warehousing** legt die Ablaufverfolgungsflags 1117 und 610 fest.

Verwenden Sie für dieses Tutorial **Allgemeine Workload**.

![VM-Größe, Speichereinstellungen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

Überprüfen Sie die Einstellungen, und klicken Sie dann auf **OK**.

#### <a name="a-note-about-storage"></a>Ein Hinweis zum Speicher
Weitere Optimierungen hängen von der Größe der SQL Server-Datenträger ab. Für jedes Terabyte des Datenträgers fügt Azure 1 TB Storage Premium hinzu. Wenn ein Server 2 TB oder mehr benötigt, erstellt die Vorlage einen Speicherpool auf jedem virtuellen Computer mit SQL Server. Ein Speicherpool ist eine Form der Speichervirtualisierung, bei der mehrere Datenträger konfiguriert werden, um höhere Kapazität, Stabilität und Leistung bereitzustellen.  Die Vorlage erstellt dann einen Speicherplatz im Speicherpool und stellt dem Betriebssystem einen einzelnen Datenträger bereit. Die Vorlage bestimmt diesen Datenträger als Datenträger für SQL Server-Daten. Die Vorlage optimiert den Speicherpool für SQL Server mit den folgenden Einstellungen:

* Die Stripesetgröße ist die Interleaveeinstellung für den virtuellen Datenträger. Für Transaktionsworkloads werden 64 KB verwendet. Für Data Warehousing-Workloads werden 256 KB verwendet.
* Resilienz ist einfach (keine Resilienz).

> [!NOTE]
> Azure Storage Premium ist lokal redundant und behält drei Kopien der Daten innerhalb einer Region, sodass keine zusätzliche Resilienz beim Speicherpool erforderlich ist.
>
>

* Die Anzahl der Spalten entspricht der Anzahl von Datenträgern im Speicherpool.

Weitere Informationen zu Speicherplatz und Speicherpools finden Sie unter:

* [Speicherplätze – Übersicht](http://technet.microsoft.com/library/hh831739.aspx)
* [Windows Server-Sicherung und -Speicherpools](http://technet.microsoft.com/library/dn390929.aspx)

Weitere Informationen zu bewährten Methoden für die SQL Server-Konfiguration finden Sie unter [Bewährte Methoden zur Leistung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-performance.md).

### <a name="sql-server-settings"></a>SQL Server-Einstellungen
Überprüfen und ändern Sie unter **SQL Server-Einstellungen** das Präfix des Namens des virtuellen Computers mit SQL Server, die SQL Server-Version, das SQL Server-Dienstkonto und -Kennwort sowie den Wartungszeitplan für automatisches SQL-Patchen.

* **SQL Server-Namenspräfix** wird verwendet, um einen Namen für jeden virtuellen Computer mit SQL Server zu erstellen. Verwenden Sie für dieses Tutorial **sqlserver**. Entsprechend der Vorlage werden die virtuellen Computer mit SQL Server mit *sqlserver-0* und *sqlserver-1* benannt.
* **SQL Server-Version** ist die Version von SQL Server. Verwenden Sie für dieses Tutorial **SQL Server 2014**. Sie können auch **SQL Server 2012** oder **SQL Server 2016** wählen.
* **SQL Server-Dienstkonto-Benutzername** ist der Domänenkontoname für den SQL Server-Dienst. Verwenden Sie für dieses Tutorial **sqlservice**.
* **Kennwort** ist das Kennwort für das SQL Server-Dienstkonto.  Verwenden Sie ein komplexes Kennwort. Bestätigen Sie das Kennwort.
* **Wartungszeitplan für automatisches SQL-Patchen** gibt den Wochentag an, an dem Azure automatisch Patches auf die SQL Server-Instanzen anwendet. Geben Sie für dieses Tutorial **Sunday** ein.
* **Wartungsstartzeit für automatisches SQL-Patchen** ist die Uhrzeit für die Azure-Region, zu der das automatische Patchen beginnt.

> [!NOTE]
> Die Patchfenster der einzelnen virtuellen Computer sind mit einem Abstand von einer Stunde gestaffelt. Es wird jeweils nur ein virtueller Computer gepatcht, um eine Unterbrechung der Dienste zu vermeiden.
>
>

![SQL Server-Einstellungen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Überprüfen Sie die Einstellungen, und klicken Sie dann auf **OK**.

### <a name="summary"></a>Zusammenfassung
Auf der Zusammenfassungsseite überprüft Azure die Einstellungen. Sie können auch die Vorlage herunterladen. Überprüfen Sie die Zusammenfassung. Klicken Sie auf **OK**.

### <a name="buy"></a>Kaufen
Dieses letzte Blatt enthält **Nutzungsbedingungen** und die **Datenschutzrichtlinie**. Überprüfen Sie diese Informationen. Wenn Sie zum Erstellen der virtuellen Computer und aller anderen erforderlichen Ressourcen für die Verfügbarkeitsgruppe in Azure bereit sind, klicken Sie auf **Erstellen**.

Das Azure-Portal erstellt die Ressourcengruppe und alle Ressourcen.

## <a name="monitor-deployment"></a>Überwachen der Bereitstellung
Überwachen Sie den Fortschritt der Bereitstellung über das Azure-Portal. Ein Symbol für die Bereitstellung wird automatisch an das Dashboard des Azure-Portals geheftet.

![Azure-Dashboard](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

## <a name="connect-to-sql-server"></a>Herstellen einer Verbindung mit SQL Server
Die neuen Instanzen von SQL Server werden auf virtuellen Computern ausgeführt, die IP-Adressen mit Internetzugang haben. Sie können Remotedesktop (RDP) direkt für jeden virtuellen Computer mit SQL Server nutzen.

Gehen Sie folgendermaßen vor, um eine RDP-Verbindung mit einer SQL Server-Instanz herzustellen:

1. Vergewissern Sie sich im Dashboard des Azure-Portals, dass die Bereitstellung erfolgreich war.
2. Klicken Sie auf **Ressourcen**.
3. Klicken Sie auf dem Blatt **Ressourcen** auf **sqlserver-0**. Dies ist der Computername eines der virtuellen Computer mit SQL Server.
4. Klicken Sie auf dem Blatt für **sqlserver-0** auf **Verbinden**. Ihr Browser fragt, ob Sie das Remoteverbindungsobjekt öffnen oder speichern möchten. Klicken Sie auf **Öffnen**.
5. Mit **Remotedesktopverbindung** werden Sie möglicherweise darauf hingewiesen, dass der Herausgeber dieser Remoteverbindung nicht identifiziert werden kann. Klicken Sie auf **Verbinden**.
6. Die Windows-Sicherheit fordert Sie auf, zur Verbindung mit der IP-Adresse des primären Domänencontrollers Ihre Anmeldeinformationen einzugeben. Klicken Sie auf **Anderes Konto verwenden**. Geben Sie unter **Benutzername** die Zeichenfolge **contoso\DomainAdmin** ein. Sie haben dieses Konto konfiguriert, als Sie den Administratorbenutzernamen in der Vorlage festgelegt haben. Verwenden Sie das komplexe Kennwort, das Sie beim Konfigurieren der Vorlage ausgewählt haben.
7. Mit **Remotedesktop** werden Sie möglicherweise darauf hingewiesen, dass der Remotecomputer aufgrund von Problemen mit dem Sicherheitszertifikat nicht authentifiziert werden konnte. Der Name des Sicherheitszertifikats wird angezeigt. Wenn Sie gemäß dem Tutorial vorgegangen sind, lautet der Name **sqlserver-0.contoso.com**. Klicken Sie auf **Ja**.

Sie sind jetzt über eine RDP-Verbindung mit dem virtuellen Computer mit SQL Server verbunden. Sie können jetzt SQL Server-Management Studio öffnen, eine Verbindung mit der Standardinstanz von SQL Server herstellen und prüfen, ob die Verfügbarkeitsgruppe konfiguriert ist.

