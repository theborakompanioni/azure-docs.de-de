---
title: Manuelles Konfigurieren der Always On-Verfügbarkeitsgruppe auf virtuellen Azure-Computern – Microsoft Azure
description: Erstellen Sie eine AlwaysOn-Verfügbarkeitsgruppe mit virtuellen Azure-Computern. In diesem Tutorial werden die Benutzeroberfläche und Tools anstelle von Skripts verwendet.
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: timlt
editor: monicar
tags: azure-service-management

ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 09/22/2016
ms.author: MikeRayMSFT

---
# <a name="configure-always-on-availability-group-in-azure-vm-manually---resource-manager"></a>Manuelles Konfigurieren der AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern – Resource Manager
> [!div class="op_single_selector"]
> * [Resource Manager: Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
> * [Resource Manager: manuell](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
> * [Klassisch: Benutzeroberfläche](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
> * [Klassisch: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)
> 
> 

<br/>

In diesem End-to-End-Tutorial wird gezeigt, wie Sie SQL Server-Verfügbarkeitsgruppen auf virtuellen Azure Resource Manager-Maschinen implementieren. 

In diesem Tutorial erstellen Sie die folgenden Elemente:

* Ein virtuelles Netzwerk mit zwei Subnetzen (Front-End- und Back-End-Subnetz)
* Zwei Domänencontroller in einer Verfügbarkeitsgruppe mit einer Active Directory-Domäne 
* Zwei virtuelle SQL Server-Computern in einer Verfügbarkeitsgruppe, die im Back-End-Subnetz bereitgestellt werden und der AD-Domäne angehören
* Einen WSFC-Cluster aus 3 Knoten mit dem Knotenmehrheit-Quorummodell
* Einen internen Lastenausgleich zur Bereitstellung einer IP-Adresse für die Verfügbarkeitsgruppen
* Eine Verfügbarkeitsgruppe mit zwei Replikaten einer Verfügbarkeitsdatenbank mit synchronem Commit

Die folgende Abbildung ist eine grafische Darstellung der Lösung.

![Architektur für Verfügbarkeitsgruppe im Azure Resource Manager](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/00-EndstateSampleNoELB.png)

Dies ist eine mögliche Konfiguration. Sie können sie gemäß Ihren Anforderungen ändern. Beispielsweise können Sie die Anzahl von virtuellen Computern reduzieren, indem Sie einen Domänencontroller als Quorum-Dateifreigabenzeuge verwenden. Hierdurch wird die Anzahl von VMs für eine Verfügbarkeitsgruppe mit zwei Replikaten reduziert. Mit dieser Methode wird die Anzahl von virtuellen Computern in der Lösung um einen Computer verringert.

Das Durcharbeiten dieses Tutorials dauert einige Stunden, da Sie mehrere virtuelle Azure-Computer erstellen und konfigurieren müssen. Die gesamte Lösung kann auch automatisch erstellt werden. Das Azure-Portal enthält einen Katalog, der für AlwaysOn-Verfügbarkeitsgruppen mit einem Listener eingerichtet ist. Mit dieser Katalogeinrichtung wird alles automatisch konfiguriert, was Sie für Verfügbarkeitsgruppen benötigen. Weitere Informationen finden Sie unter [Portal – Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). 

[!INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

In diesem Tutorial wird Folgendes vorausgesetzt:

* Sie besitzen bereits ein Azure-Abonnement.
* Sie wissen bereits, wie ein virtueller SQL Server-Computer mithilfe der GUI aus dem virtuellen Computerkatalog bereitgestellt wird. Weitere Informationen finden Sie unter [Bereitstellen eines virtuellen Computers mit SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md)
* Sie verfügen bereits über solide Kenntnisse über Verfügbarkeitsgruppen. Weitere Informationen finden Sie unter [AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Wenn Sie an der Verwendung von Verfügbarkeitsgruppen mit SharePoint interessiert sind, helfen Ihnen die Informationen unter [Konfigurieren von SQL Server 2012 AlwaysOn-Verfügbarkeitsgruppen für SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx)weiter.
> 
> 

## <a name="create-resource-group"></a>Ressourcengruppe erstellen
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an. 
2. Klicken Sie auf **+Neu**, und geben Sie im Marketplace-Suchfenster** **die Zeichenfolge **Ressourcengruppe** ein.
   
   ![Ressourcengruppe](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/01-resourcegroupsymbol.png)
3. Klicken Sie auf **Ressourcengruppe** 
   
   ![Neue Ressourcengruppe](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/01-newresourcegroup.png)
4. Klicken Sie auf **Erstellen**. 
5. Geben Sie auf dem Blatt **Ressourcengruppe** unter **Ressourcengruppenname** die Zeichenfolge **SQL-HA-RG** ein.
6. Falls Sie über mehrere Azure-Abonnements verfügen, vergewissern Sie sich, dass es sich bei dem Abonnement um das Azure-Abonnement handelt, unter dem Sie die Verfügbarkeitsgruppe erstellen möchten. 
7. Wählen Sie einen Standort aus. Der Standort ist die Azure-Region, in der die Verfügbarkeitsgruppe erstellt werden soll. In diesem Tutorial erstellen wir alle Ressourcen an nur einem Azure-Standort. 
8. Vergewissern Sie sich, dass **An Dashboard anheften** aktiviert ist. Diese optionale Einstellung platziert eine Verknüpfung mit der Ressourcengruppe auf dem Dashboard des Azure-Portals. 
9. Klicken Sie auf **Erstellen** , um die Ressourcengruppe zu erstellen.

Azure erstellt die Ressourcengruppe und heftet eine Verknüpfung mit der Ressourcengruppe im Portal an.

## <a name="create-network-and-subnets"></a>Erstellen von Netzwerk und Subnetzen
Im nächsten Schritt werden die Netzwerke und Subnetze in der Azure-Ressourcengruppe erstellt.

Die Lösung verwendet ein virtuelles Netzwerk mit zwei Subnetzen. Weitere Informationen zu Netzwerken in Azure finden Sie unter [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md) .

So erstellen Sie das virtuelle Netzwerk:

1. Klicken Sie im Azure-Portal auf die neue Ressourcengruppe und anschließend auf **+** , um der Ressourcengruppe ein neues Element hinzuzufügen. Azure öffnet das Blatt **Alles** . 
   
   ![Neues Element](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/02-newiteminrg.png)
2. Suchen Sie nach **Virtuelles Netzwerk**.
   
   ![Suchen eines virtuellen Netzwerks](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/04-findvirtualnetwork.png)
3. Klicken Sie auf **Virtuelles Netzwerk**.
4. Klicken Sie auf dem Blatt **Virtuelles Netzwerk** auf das Bereitstellungsmodell **Resource Manager** und anschließend auf **Erstellen**.
   
   ![Virtuelles Netzwerk erstellen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/05-createvirtualnetwork.png)
5. Konfigurieren Sie auf dem Blatt **Virtuelles Netzwerk erstellen** das virtuelle Netzwerk. 
   
   Die folgende Tabelle enthält die Einstellungen für das virtuelle Netzwerk: 
   
   | **Field** | Wert |
   | --- | --- |
   | **Name** |autoHAVNET |
   | **Adressraum** |10.0.0.0/16 |
   | **Subnetzname** |Subnet-1 |
   | **Subnetzadressbereich** |10.0.0.0/24 |
   | **Abonnement** |Geben Sie das Abonnement an, das Sie verwenden möchten. Wenn Sie nur über ein einzelnes Abonnement verfügen, ist diese Option unter Umständen leer. |
   | **Standort** |Geben Sie den Azure-Standort an. |
   
   Ihr Adressraum und Ihr Subnetzadressbereich können sich von den Angaben in der Tabelle unterscheiden. Abhängig von Ihrem Abonnement gibt Azure einen verfügbaren Adressraum und den entsprechenden Subnetzadressbereich an. Ist kein geeigneter Adressraum verfügbar, verwenden Sie ein anderes Abonnement. 
6. Klicken Sie auf **Erstellen**
   
   ![Konfigurieren eines virtuellen Netzwerks](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/06-configurevirtualnetwork.png)

Azure zeigt wieder das Portaldashboard an und benachrichtigt Sie, wenn das neue Netzwerk erstellt wurde.

### <a name="create-the-second-subnet"></a>Erstellen des zweiten Subnetzes
Bis jetzt enthält das virtuelle Netzwerk ein Subnetz mit dem Namen „Subnet-1“. Dieses Subnetz wird von den Domänencontrollern verwendet. Die SQL Server-Instanzen verwenden ein zweites Subnetz namens **Subnet-2**. Gehen Sie zum Konfigurieren von „Subnet-2“ wie folgt vor:

1. Klicken Sie in Ihrem Dashboard auf die zuvor erstellte Ressourcengruppe **SQL-HA-RG**. Suchen Sie das Netzwerk in der Ressourcengruppe unter **Ressourcen**.
   
   Sollte **SQL-HA-RG** nicht angezeigt werden, klicken Sie auf **Ressourcengruppen**, und filtern Sie nach dem Namen der Ressourcengruppe.
2. Klicken Sie in der Liste mit den Ressourcen auf **autoHAVNET** . Azure öffnet das Blatt für die Netzwerkkonfiguration.
3. Klicken Sie im Bereich für das virtuelle Netzwerk **autoHAVNET** auf*Alle Einstellungen*.
4. Klicken Sie auf dem Blatt **Einstellungen** auf **Subnetze**.
   
   Hier sehen Sie das bereits erstellte Subnetz. 
   
   ![Konfigurieren eines virtuellen Netzwerks](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/07-addsubnet.png)
5. Erstellen Sie ein zweites Subnetz. Klicken Sie auf **+ Subnetz**. 
   
   Konfigurieren Sie das Subnetz auf dem Blatt **Subnetz hinzufügen**, indem Sie unter **Name** die Zeichenfolge **subnet-2** eingeben. Azure gibt automatisch einen gültigen **Adressbereich**an. Vergewissern Sie sich, dass dieser Adressbereich mindestens zehn Adressen umfasst. In einer Produktionsumgebung werden unter Umständen weitere Adressen benötigt. 
6. Klicken Sie auf **OK**.

![Konfigurieren eines virtuellen Netzwerks](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/08-configuresubnet.png)

Hier sehen Sie eine Zusammenfassung der Konfigurationseinstellungen für das virtuelle Netzwerk und die beiden Subnetze.

| **Field** | Wert |
| --- | --- |
| **Name** |**autoHAVNET** |
| **Adressraum** |Hängt von den verfügbaren Adressräumen Ihres Abonnements ab. Ein typischer Wert wäre etwa 10.0.0.0/16. |
| **Subnetzname** |**Subnet-1** |
| **Subnetzadressbereich** |Hängt von den verfügbaren Adressbereichen Ihres Abonnements ab. Ein typischer Wert wäre etwa 10.0.0.0/24. |
| **Subnetzname** |**Subnet-2** |
| **Subnetzadressbereich** |Hängt von den verfügbaren Adressbereichen Ihres Abonnements ab. Ein typischer Wert wäre etwa 10.0.1.0/24. |
| **Abonnement** |Geben Sie das Abonnement an, das Sie verwenden möchten. |
| **Ressourcengruppe** |**SQL-HA-RG** |
| **Standort** |Geben Sie den Speicherort an, den Sie auch für die Ressourcengruppe ausgewählt haben. |

## <a name="create-availability-sets"></a>Erstellen von Verfügbarkeitsgruppen
Vor dem Erstellen virtueller Computer müssen zunächst Verfügbarkeitsgruppen erstellt werden. Verfügbarkeitsgruppen verringern die Ausfallzeiten bei geplanten oder ungeplanten Wartungsereignissen. Eine Azure-Verfügbarkeitsgruppe ist eine logische Gruppe von Ressourcen, die Azure in physischen Fehlerdomänen und Updatedomänen platziert. Eine Fehlerdomäne stellt sicher, dass die Mitglieder der Verfügbarkeitsgruppe über eine separate Stromversorgung sowie über separate Netzwerkressourcen verfügen. Eine Updatedomäne stellt sicher, dass die Mitglieder der Verfügbarkeitsgruppe nicht gleichzeitig zu Wartungszwecken heruntergefahren werden. [Verwalten der Verfügbarkeit virtueller Computer](virtual-machines-windows-manage-availability.md).

Sie benötigen zwei Verfügbarkeitsgruppen: Eine für die Domänencontroller Eine für die SQL Server-Instanzen

Navigieren Sie zum Erstellen einer Verfügbarkeitsgruppe zur Ressourcengruppe, und klicken Sie auf **Hinzufügen**. Filtern Sie die Ergebnisse, indem Sie **Verfügbarkeitsgruppe**eingeben. Klicken Sie in den Ergebnissen auf **Verfügbarkeitsgruppe** . Klicken Sie auf **Erstellen**.

Konfigurieren Sie die beiden Verfügbarkeitsgruppen mit den Parametern in der folgenden Tabelle:

| **Field** | Verfügbarkeitsgruppe für Domänencontroller | Verfügbarkeitsgruppe für SQL Server |
| --- | --- | --- |
| **Name** |adavailabilitySet |sqlAvailabilitySet |
| **Ressourcengruppe** |SQL-HA-RG |SQL-HA-RG |
| **Fehlerdomänen** |3 |3 |
| **Updatedomänen** |5 |3 |

Kehren Sie nach Erstellung der Verfügbarkeitsgruppen zur Ressourcengruppe im Azure-Portal zurück.

## <a name="create-domain-controllers"></a>Erstellen von Domänencontrollern
Sie haben bereits das Netzwerk, Subnetze, Verfügbarkeitsgruppen und einen Lastenausgleich für Internetzugriff erstellt. Nun können Sie die virtuellen Computer für die Domänencontroller erstellen.

### <a name="create-the-virtual-machines-for-the-domain-controllers"></a>Erstellen der virtuellen Computer für die Domänencontroller
Kehren Sie zum Erstellen und Konfigurieren der Domänencontroller zur Ressourcengruppe **SQL-HA-RG** zurück.

1. Klicken Sie auf "Hinzufügen". Das Blatt **Alles** wird geöffnet.
2. Geben Sie **Windows Server 2012 R2 Datacenter**ein. 
3. Klicken Sie auf **Windows Server 2012 R2 Datencenter**. Vergewissern Sie sich auf dem Blatt **Windows Server 2012 R2 Datacenter**, dass das Bereitstellungsmodell **Resource Manager** lautet, und klicken Sie anschließend auf **Erstellen**. Azure öffnet das Blatt **Virtuellen Computer erstellen** . 

Führen Sie die obigen Schritte zweimal aus, um zwei virtuelle Computer zu erstellen. Benennen Sie die beiden virtuellen Computer wie folgt:

* ad-primary-dc
* ad-secondary-dc
  
  > [!NOTE]
  > **ad-secondary-dc** ist eine optionale Komponente zur Gewährleistung hoher Verfügbarkeit für Active Directory-Domänendienste. 
  > 
  > 

Die folgende Tabelle enthält die Einstellungen für die beiden Computer:

| **Field** | Wert |
| --- | --- |
| **Benutzername** |DomainAdmin |
| **Kennwort** |Contoso!0000 |
| **Abonnement** |*Ihr Abonnement* |
| **Ressourcengruppe** |SQL-HA-RG |
| **Standort** |*Ihr Standort* |
| **Größe** |D1_V2 (Standard) |
| **Speichertyp** |Standard |
| **Speicherkonto** |*Automatisch erstellt* |
| **Virtuelles Netzwerk** |autoHAVNET |
| **Subnetz** |Subnet-1 |
| **Öffentliche IP-Adresse** |*Gleicher Name wie der VM* |
| **Netzwerksicherheitsgruppen (NSG)** |*Gleicher Name wie der VM* |
| **Diagnose** |Aktiviert |
| **Diagnosespeicherkonto** |*Automatisch erstellt* |
| **Verfügbarkeitsgruppe** |adavailabilitySet |

> [!NOTE]
> Die Verfügbarkeitsgruppe für einen virtuellen Computer kann nach der Erstellung nicht mehr geändert werden.
> 
> 

Azure erstellt die virtuellen Computer.

Konfigurieren Sie nach der Erstellung der virtuellen Computer den Domänencontroller.

### <a name="configure-the-domain-controller"></a>Konfigurieren des Domänencontrollers
In den folgenden Schritten konfigurieren Sie den Computer **ad-primary-dc** als Domänencontroller für „corp.contoso.com“.

1. Öffnen Sie im Portal die Ressourcengruppe **SQL-HA-RG**, und wählen Sie den Computer **ad-primary-dc** aus. Klicken Sie auf dem Blatt **ad-primary-dc** auf **Verbinden**, um eine RDP-Datei für den Remotedesktopzugriff zu öffnen.
   
    ![Mit virtuellem Computer verbinden](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/20-connectrdp.png)
2. Melden Sie sich mit Ihrem konfigurierten Administratorkonto (**\DomainAdmin**) und Kennwort (**Contoso!0000**) an.
3. Standardmäßig sollte das Dashboard **Server-Manager** angezeigt werden.
4. Klicken Sie im Dashboard auf den Link **Rollen und Features hinzufügen** .
   
    !["Rollen hinzufügen" in Server-Explorer](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784623.png)
5. Wählen Sie **Weiter** aus, bis Sie zum Abschnitt **Serverrollen** gelangen.
6. Wählen Sie die Rollen **Active Directory Domain Services** und **DNS-Server** aus. Wenn Sie dazu aufgefordert werden, fügen Sie alle für diese Rollen erforderlichen, zusätzlichen Funktionen hinzu.
   
   > [!NOTE]
   > In Windows wird die Warnung angezeigt, dass keine statische IP-Adresse vorhanden ist. Wenn Sie die Konfiguration testen, klicken Sie auf "Weiter". Legen Sie in Produktionsszenarien die IP-Adresse über das Azure-Portal als statische Adresse fest, oder [verwenden Sie PowerShell, um die statische IP-Adresse des Domänencontrollercomputers festzulegen](../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Dialogfeld "Rollen hinzufügen"](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784624.png)
7. Klicken Sie auf **Weiter**, bis Sie zum Abschnitt **Bestätigung** gelangen. Aktivieren Sie das Kontrollkästchen **Zielserver bei Bedarf automatisch neu starten** .
8. Klicken Sie auf **Installieren**.
9. Nachdem die Installation der Funktionen abgeschlossen ist, kehren Sie zum Dashboard **Server-Manager** zurück.
10. Wählen Sie die neue Option **AD DS** im linken Bereich aus.
11. Klicken Sie in der gelben Warnungsleiste auf den Link **Mehr** .
    
     ![AD DS-Dialogfeld auf virtuellem DNS-Servercomputer](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784625.png)
12. Klicken Sie im Dialogfeld **Alle Serveraufgabendetails** in der Spalte **Aktion** auf **Server zu einem Domänencontroller heraufstufen**.
13. Verwenden Sie im **Konfigurations-Assistenten für Active Directory-Domänendienste**die folgenden Werte:

| **Seite** | Einstellung |
| --- | --- |
| **Bereitstellungskonfiguration** |**Neue Gesamtstruktur hinzufügen** = Aktiviert<br/>**Rootdomänenname** = corp.contoso.com |
| **Domänencontrolleroptionen** |**DSRM-Kennwort** = Contoso!0000<br/>**Kennwort bestätigen** = Contoso!0000 |

1. Klicken Sie auf **Weiter** , um die anderen Seiten des Assistenten zu durchlaufen. Vergewissern Sie sich, dass auf der Seite **Prüfung der erforderlichen Komponenten** die folgende Meldung angezeigt wird: **Alle erforderlichen Komponenten wurden erfolgreich überprüft.** Beachten Sie, dass Sie zwar alle zutreffenden Warnmeldungen überprüfen sollten, es aber möglich ist, die Installation fortzusetzen.
2. Klicken Sie auf **Installieren**. Der virtuelle Computer **ad-primary-dc** wird automatisch neu gestartet.

### <a name="configure-the-second-domain-controller"></a>Konfigurieren des zweiten Domänencontrollers
Nach dem Neustart des primären Domänencontrollers können Sie den zweiten Domänencontroller konfigurieren. Dieser optionale Schritt dient zur Gewährleistung einer hohen Verfügbarkeit. Für diesen Schritt benötigen Sie die private IP-Adresse für den Domänencontroller. Diese können Sie im Azure-Portal ermitteln. Gehen Sie wie folgt vor, um den zweiten Domänencontroller zu konfigurieren:

1. Melden Sie sich am Computer **ad-primary-dc** an. 
2. Öffnen Sie den Remotedesktop, und stellen Sie über die entsprechende IP-Adresse eine Verbindung mit dem sekundären Domänencontroller her. Sollte Ihnen die IP-Adresse des zweiten Domänencontrollers nicht bekannt sein, wechseln Sie zum Azure-Portal, und überprüfen Sie die Adresse, die der Netzwerkschnittstelle für den sekundären Domänencontroller zugewiesen ist. 
3. Legen Sie die bevorzugte DNS-Serveradresse auf die Adresse für den Domänencontroller fest. 
4. Starten Sie die RDP-Datei für den primären Domänencontroller (**ad-primary-dc**), und melden Sie sich beim virtuellen Computer mit Ihrem konfigurierten Administratorkonto (**BUILTIN\DomainAdmin**) und Kennwort (**Contoso!0000**) an.
5. Starten Sie auf dem primären Domänencontroller unter Verwendung der IP-Adresse einen Remotedesktop für **ad-secondary-dc** . Verwenden Sie das gleiche Konto und Kennwort.
6. Sobald Sie angemeldet sind, sollte das Dashboard **Server-Manager** angezeigt werden. Klicken Sie im linken Bereich auf **Lokaler Server** .
7. Wählen Sie den Link **IPv4-Adresse wird per DHCP zugewiesen, IPv6-fähig** aus.
8. Wählen Sie im Fenster **Netzwerkverbindungen** das Netzwerksymbol aus.
   
    ![Ändern des bevorzugten DNS-Servers für den virtuellen Computer](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784629.png)
9. Klicken Sie auf der Befehlsleiste auf **Einstellungen dieser Verbindung ändern** (abhängig von der Größe Ihres Fensters müssen Sie möglicherweise auf den Doppelpfeil nach rechts klicken, um diesen Befehl anzuzeigen).
10. Wählen Sie **Internetprotokoll Version 4 (TCP/IPv4)** aus, und klicken Sie auf „Eigenschaften“.
11. Wählen Sie „Folgende DNS-Serveradressen verwenden“ aus, und geben Sie unter **Bevorzugter DNS-Server**die Adresse des primären Domänencontrollers an.
12. Bei der Adresse handelt es sich um die Adresse, die einem virtuellen Computer im Subnetz „subnet-1“ im virtuellen Azure-Netzwerk zugewiesen ist. Dieser virtuelle Computer ist **ad-primary-dc**. Führen Sie an der Befehlszeile den Befehl **nslookup ad-primary-dc** aus, um die IP-Adresse von **ad-primary-dc** zu überprüfen.
    
     ![Verwenden von NSLOOKUP zum Suchen der IP-Adresse für den DC](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC664954.png)
    
    > [!NOTE]
    > Nach dem Festlegen des DNS wird die RDP-Sitzung unter Umständen an den Mitgliedsserver abgegeben. Starten Sie den virtuellen Computer in diesem Fall über das Azure-Portal neu.
    > 
    > 
13. Klicken Sie auf **OK** und anschließend auf **Schließen**, um die Änderungen zu übernehmen. Sie können den virtuellen Computer jetzt zu **corp.contoso.com**beitreten lassen.
14. Wiederholen Sie die Schritte, die Sie zum Erstellen des ersten Domänencontrollers ausgeführt haben, verwenden Sie aber im **Konfigurations-Assistenten für Active Directory-Domänendienste**folgende Werte:

| Seite | Einstellung |
| --- | --- |
| **Bereitstellungskonfiguration** |**Domänencontroller vorhandener Domäne hinzufügen** = Aktiviert<br/>**Stamm** = corp.contoso.com |
| **Domänencontrolleroptionen** |**DSRM-Kennwort** = Contoso!0000<br/>**Kennwort bestätigen** = Contoso!0000 |

### <a name="configure-domain-accounts"></a>Konfigurieren von Domänenkonten
In den nächsten Schritten werden die Active Directory-Konten (AD) für die spätere Verwendung konfiguriert.

1. Melden Sie sich wieder am Computer **ad-primary-dc** an.
2. Wählen Sie im **Server-Manager** die Option **Tools** aus, und klicken Sie dann auf **Active Directory-Verwaltungscenter**.
   
    ![Active Directory-Verwaltungscenter](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784626.png)
3. Geben Sie auf dem Blatt **Active Directory-Verwaltungscenter** select **corp (lokal)** aus.
4. Wählen Sie rechts im Aufgabenbereich** **die Option **Neu** aus, und klicken Sie dann auf **Benutzer**. Verwenden Sie folgende Einstellungen:

| Einstellung | Wert |
| --- | --- |
| **Vorname** |Installieren |
| **SamAccountName von Benutzer** |Installieren |
| **Kennwort** |Contoso!0000 |
| **Kennwort bestätigen** |Contoso!0000 |
| **Andere Kennwortoptionen** |Aktiviert |
| **Kennwort läuft nie ab** |Aktiviert |

1. Klicken Sie auf **OK**, um den Installationsbenutzer** **zu erstellen. Verwenden Sie dieses Konto zum Konfigurieren des Failoverclusters und der Verfügbarkeitsgruppe.
2. Erstellen Sie mit der gleichen Vorgehensweise zwei weitere Benutzer: **CORP\SQLSvc1** und **CORP\SQLSvc2**. Diese Konten werden von den SQL Server-Diensten verwendet. Erteilen Sie als Nächstes **CORP\Install** die erforderlichen Berechtigungen für das Konfigurieren von WSFC (Windows Service Failover Clustering).
3. Wählen Sie im **Active Directory-Verwaltungscenter** im linken Bereich die Option **corp (lokal)** aus. Klicken Sie dann rechts im Aufgabenbereich** **auf **Eigenschaften**.
   
    ![Eigenschaften des Benutzers CORP](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784627.png)
4. Wählen Sie **Erweiterungen** aus, und klicken Sie auf der Registerkarte **Sicherheit** auf die Schaltfläche **Erweitert**.
5. Gehen Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für corp** wie folgt vor: Klicken Sie auf **Hinzufügen**.
6. Klicken Sie auf **Prinzipal auswählen**. Suchen Sie dann nach **CORP\Install**. Klicken Sie auf **OK**.
7. Wählen Sie die Berechtigungen **Alle Eigenschaften lesen** und **Computerobjekte erstellen** aus.
   
    ![Berechtigungen des Benutzers CORP](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784628.png)
8. Klicken Sie auf **OK** und dann erneut auf **OK**. Schließen Sie das Eigenschaftenfenster von corp.

Nachdem Sie nun die Konfiguration von Active Directory und den Benutzerobjekten abgeschlossen haben, erstellen Sie zwei virtuelle SQL Server-Computer und einen virtuellen Computer als Zeugenserver. Führen Sie für alle drei Computer anschließend den Beitritt zur Domäne durch.

## <a name="create-sql-servers"></a>Erstellen von SQL Server-Computern
### <a name="create-and-configure-the-sql-server-vms"></a>Erstellen und Konfigurieren der virtuellen SQL Server-Computer
Im nächsten Schritt werden drei virtuelle Computer erstellt: zwei virtuelle SQL Server-Computer und ein WSFC-Clusterknoten. Kehren Sie zum Erstellen dieser virtuellen Computer jeweils zur Ressourcengruppe **SQL-HA-RG** zurück, klicken Sie auf **Hinzufügen**, suchen Sie nach dem entsprechenden Katalogelement (**Virtueller Computer**), und klicken Sie dann auf **Aus Katalog**. Verwenden Sie dann die Vorlagen aus der folgenden Tabelle, die Sie bei der Erstellung der virtuellen Computer unterstützen.

| Seite | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Wählen Sie das passende Katalogelement aus. |**Windows Server 2012 R2 Datacenter** |**SQL Server 2014 SP1 Enterprise unter Windows Server 2012 R2** |**SQL Server 2014 SP1 Enterprise unter Windows Server 2012 R2** |
| **Grundlagen** |**Name** = cluster-fsw<br/>**Benutzername** = DomainAdmin<br/>**Kennwort** = Contoso!0000<br/>**Abonnement** = Ihr Abonnement<br/>**Ressourcengruppe** = SQL-HA-RG<br/>**Standort** = Ihr Azure-Standort |**Name** = sqlserver-0<br/>**Benutzername** = DomainAdmin<br/>**Kennwort** = Contoso!0000<br/>**Abonnement** = Ihr Abonnement<br/>**Ressourcengruppe** = SQL-HA-RG<br/>**Standort** = Ihr Azure-Standort |**Name** = sqlserver-1<br/>**Benutzername** = DomainAdmin<br/>**Kennwort** = Contoso!0000<br/>**Abonnement** = Ihr Abonnement<br/>**Ressourcengruppe** = SQL-HA-RG<br/>**Standort** = Ihr Azure-Standort |
| Konfiguration des virtuellen Computers: **Größe** |DS1 (ein Kern, 3,5 GB Arbeitsspeicher) |**GRÖSSE** = DS2 (2 Kerne, 7 GB Arbeitsspeicher) |**GRÖSSE** = DS2 (2 Kerne, 7 GB Arbeitsspeicher) |
| Konfiguration des virtuellen Computers: **Einstellungen** |**Speicher** = Premium (SSD)<br/>**NETZWERKSUBNETZE** = autoHAVNET<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**Subnetz** = subnet-2(10.1.1.0/24)<br/>**Öffentliche IP-Adresse** = Keine<br/>**Netzwerksicherheitsgruppe** = Keine<br/>**Überwachung und Diagnose** = Aktiviert<br/>**Diagnosespeicherkonto** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = SqlAvailabilitySet<br/> |**Speicher** = Premium (SSD)<br/>**NETZWERKSUBNETZE** = autoHAVNET<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**Subnetz** = subnet-2(10.1.1.0/24)<br/>**Öffentliche IP-Adresse** = Keine<br/>**Netzwerksicherheitsgruppe** = Keine<br/>**Überwachung und Diagnose** = Aktiviert<br/>**Diagnosespeicherkonto** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = SqlAvailabilitySet<br/> |**Speicher** = Premium (SSD)<br/>**NETZWERKSUBNETZE** = autoHAVNET<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**Subnetz** = subnet-2(10.1.1.0/24)<br/>**Öffentliche IP-Adresse** = Keine<br/>**Netzwerksicherheitsgruppe** = Keine<br/>**Überwachung und Diagnose** = Aktiviert<br/>**Diagnosespeicherkonto** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = SqlAvailabilitySet<br/> |
| Konfiguration des virtuellen Computers: **SQL Server-Einstellungen** |Nicht zutreffend |**SQL-Konnektivität** = Privat (innerhalb von Virtual Network)<br/>**Port** = 1433<br/>**SQL-Authentifizierung** = Deaktiviert<br/>**Speicherkonfiguration** = Allgemein<br/>**Automatisiertes Patchen** = Sonntags, 2:00 Uhr<br/>**Automatisierte Sicherung** = Deaktiviert</br>**Azure Key Vault-Integration** = Deaktiviert |**SQL-Konnektivität** = Privat (innerhalb von Virtual Network)<br/>**Port** = 1433<br/>**SQL-Authentifizierung** = Deaktiviert<br/>**Speicherkonfiguration** = Allgemein<br/>**Automatisiertes Patchen** = Sonntags, 2:00 Uhr<br/>**Automatisierte Sicherung** = Deaktiviert</br>**Azure Key Vault-Integration** = Deaktiviert |

<br/>

> [!NOTE]
> Die vorherige Konfiguration legt die Verwendung virtueller Computer mit STANDARD-Tarif nahe, da Computer mit BASIC-Tarif keine Endpunkte mit Lastenausgleich unterstützen, die später für den Verfügbarkeitsgruppenlistener erforderlich sind. Außerdem sind die hier vorgeschlagenen Computergrößen für das Testen von Verfügbarkeitsgruppen in Azure-VMs vorgesehen. Die beste Leistung für Produktionsarbeitslasten finden Sie unter den Empfehlungen für die Größe und Konfiguration der SQL Server-Computer in [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md).
> 
> 

Sobald die drei virtuellen Computer vollständig bereitgestellt wurden, müssen Sie sie der Domäne **corp.contoso.com** hinzufügen und „CORP\Install“ Administratorrechte für die Computer gewähren.

Schreiben Sie sich der Einfachheit halber die virtuelle Azure-IP-Adresse der einzelnen virtuellen Computer auf. Rufen Sie die IP-Adresse für die einzelnen Server ab. Klicken Sie in der Azure-Ressourcengruppe „SQL-HA-RG“ auf die Ressource **autoHAVNET** . Auf dem Blatt **autoHAVNET** werden die IP-Adressen der einzelnen Computer in Ihrem Netzwerk angezeigt.

Notieren Sie sich die IP-Adressen für folgende Geräte: 

| VM-Rolle | Gerät | IP-Adresse |
| --- | --- | --- |
| Primärer Domänencontroller |ad-primary-dc | |
| Sekundärer Domänencontroller |ad-secondary-dc | |
| Cluster-Dateifreigabenzeuge |cluster-fsw | |
| SQL Server |sqlserver-0 | |
| SQL Server |sqlserver-1 | |

Verwenden Sie diese Adressen, um den DNS-Dienst für die einzelnen virtuellen Computer zu konfigurieren. Führen Sie für jeden der drei virtuellen Computer die folgenden Schritte aus:

1. Ändern Sie zunächst für jeden Mitgliedsserver die Adresse des bevorzugten DNS-Servers. 
2. Starten Sie die RDP-Datei für den primären Domänencontroller (**ad-primary-dc**), und melden Sie sich beim virtuellen Computer mit Ihrem konfigurierten Administratorkonto (**BUILTIN\DomainAdmin**) und Kennwort (**Contoso!0000**) an.
3. Starten Sie auf dem primären Domänencontroller unter Verwendung der IP-Adresse einen Remotedesktop für **sqlserver-0** . Verwenden Sie das gleiche Konto und Kennwort.
4. Sobald Sie angemeldet sind, sollte das Dashboard **Server-Manager** angezeigt werden. Klicken Sie im linken Bereich auf **Lokaler Server** .
5. Wählen Sie den Link **IPv4-Adresse wird per DHCP zugewiesen, IPv6-fähig** aus.
6. Wählen Sie im Fenster **Netzwerkverbindungen** das Netzwerksymbol aus.
   
    ![Ändern des bevorzugten DNS-Servers für den virtuellen Computer](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784629.png)
7. Klicken Sie auf der Befehlsleiste auf **Einstellungen dieser Verbindung ändern** (abhängig von der Größe Ihres Fensters müssen Sie möglicherweise auf den Doppelpfeil nach rechts klicken, um diesen Befehl anzuzeigen).
8. Wählen Sie **Internetprotokoll Version 4 (TCP/IPv4)** aus, und klicken Sie auf „Eigenschaften“.
9. Wählen Sie „Folgende DNS-Serveradressen verwenden“ aus, und geben Sie unter **Bevorzugter DNS-Server**die Adresse des primären Domänencontrollers an.
10. Bei der Adresse handelt es sich um die Adresse, die einem virtuellen Computer im Subnetz „subnet-1“ im virtuellen Azure-Netzwerk zugewiesen ist. Dieser virtuelle Computer ist **ad-primary-dc**. Führen Sie an der Befehlszeile den Befehl **nslookup ad-primary-dc** aus, um die IP-Adresse von **ad-primary-dc** zu überprüfen.
    
     ![Verwenden von NSLOOKUP zum Suchen der IP-Adresse für den DC](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC664954.png)
    
    > [!NOTE]
    > Nach dem Festlegen des DNS wird die RDP-Sitzung unter Umständen an den Mitgliedsserver abgegeben. Starten Sie den virtuellen Computer in diesem Fall über das Azure-Portal neu.
    > 
    > 
11. Klicken Sie auf **OK** und anschließend auf **Schließen**, um die Änderungen zu übernehmen. Sie können den virtuellen Computer jetzt zu **corp.contoso.com**beitreten lassen.
12. Klicken Sie im Fenster **Lokaler Server** auf den Link **ARBEITSGRUPPE**.
13. Klicken Sie im Abschnitt **Computername** auf **Ändern**.
14. Aktivieren Sie das Kontrollkästchen **Domäne**, und geben Sie in das Textfeld die Zeichenfolge **corp.contoso.com** ein. Klicken Sie auf **OK**.
15. Geben Sie im Popupdialogfeld **Windows-Sicherheit** die Anmeldeinformationen für das standardmäßige Domänenadministratorkonto (**CORP\DomainAdmin**) und das Kennwort (**Contoso!0000**) an.
16. Wenn die Meldung „Willkommen in der Domäne ‚corp.contoso.com‘“ angezeigt wird, klicken Sie auf **OK**.
17. Klicken Sie auf **Schließen**, und klicken Sie dann im Popupdialogfeld auf **Jetzt neu starten**.
18. Wiederholen Sie diese Schritte für den Dateifreigabenzeugen-Server und für die einzelnen SQL Server. 

### <a name="add-the-corp\install-user-as-an-administrator-on-each-cluster-vm:"></a>Fügen Sie auf jedem virtuellen Clustercomputer den Benutzer „Corp\Install“ als Administrator hinzu:
1. Warten Sie, bis der virtuelle Computer neu gestartet wurde, und starten Sie die RDP-Datei erneut über den primären Domänencontroller, um sich bei **sqlserver-0** mit dem Konto **BUILTIN\DomainAdmin** anzumelden.
2. Wählen Sie im **Server-Manager** die Option **Tools** aus, und klicken Sie dann auf **Computerverwaltung**.
   
    ![Computerverwaltung](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784630.png)
3. Erweitern Sie im Fenster **Computerverwaltung** die Option **Lokale Benutzer und Gruppen**, und wählen Sie dann **Gruppen** aus.
4. Doppelklicken Sie auf die Gruppe **Administratoren** .
5. Klicken Sie im Dialogfeld **Administratoreigenschaften** auf die Schaltfläche **Hinzufügen**.
6. Geben Sie den Benutzer **CORP\Install** ein, und klicken Sie dann auf **OK**. Geben Sie das Konto **DomainAdmin** und das Kennwort **Contoso!0000** an, wenn Sie zur Eingabe der Anmeldeinformationen aufgefordert werden.
7. Klicken Sie auf **OK**, um das Dialogfeld **Administratoreigenschaften** zu schließen.
8. Wiederholen Sie die oben genannten Schritte für **sqlserver-1** und **cluster-fsw**.

## <a name="create-the-cluster"></a>Cluster erstellen
### <a name="add-the-**failover-clustering**-feature-to-each-cluster-vm."></a>Fügen Sie jedem virtuellen Clustercomputer das Feature **Failoverclustering** hinzu.
1. Stellen Sie eine RDP-Verbindung mit **sqlserver-0**her.
2. Klicken Sie im Dashboard **Server-Manager** auf **Rollen und Features hinzufügen**.
3. Klicken Sie im **Assistenten zum Hinzufügen von Rollen und Features** auf **Weiter**, bis Sie zur Seite **Features** gelangen.
4. Wählen Sie **Failoverclustering**aus. Wenn Sie dazu aufgefordert werden, fügen Sie alle weiteren, abhängigen Features hinzu.
   
    ![Hinzufügen der Funktion "Failoverclustering" zum virtuellen Computer](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784631.png)
5. Klicken Sie auf **Weiter**, und klicken Sie dann auf der Seite **Bestätigung** auf **Installieren**.
6. Wenn die Installation des Features **Failoverclustering** abgeschlossen ist, klicken Sie auf **Schließen**.
7. Melden Sie sich bei dem virtuellen Computer ab.
8. Wiederholen Sie die Schritte in diesem Abschnitt für **sqlserver-1** und **cluster-fsw**.

Die virtuellen SQL Server-Computer sind jetzt bereitgestellt und werden ausgeführt, sie sind aber mit SQL Server mit Standardoptionen installiert.

### <a name="create-the-wsfc-cluster"></a>Erstellen des WSFC-Clusters
In diesem Abschnitt erstellen Sie den WSFC-Cluster, um die Verfügbarkeitsgruppe zu hosten. Mittlerweile sollten Sie Folgendes auf jedem der drei virtuellen Computer im WSFC-Cluster durchgeführt haben:

* Vollständige Bereitstellung in Azure
* Beitritt des virtuellen Computers zur Domäne
* Hinzufügen von **CORP\Install** zur lokalen Administratorgruppe
* Hinzufügen der Funktion "Failoverclustering"

All dies sind Voraussetzungen für jeden der virtuellen Computer, damit er dem WSFC-Cluster hinzugefügt werden kann.

Beachten Sie außerdem, dass sich das virtuelle Azure-Netzwerk nicht auf dieselbe Weise wie ein lokales Netzwerk verhält. Sie müssen die Cluster in der folgenden Reihenfolge erstellen:

1. Erstellen Sie auf einem der Knoten (**sqlserver-0**) einen Cluster mit einem einzelnen Knoten.
2. Legen Sie die IP-Adresse des Clusters in eine nicht verwendete IP-Adresse aus **sqlsubnet**fest.
3. Schalten Sie den Clusternamen online.
4. Fügen Sie die anderen Knoten (**sqlserver-1** und **cluster-fsw**) hinzu.

Führen Sie die unten angegebenen Schritte aus, um den Cluster zu konfigurieren.

1. Starten Sie die RDP-Datei für **sqlserver-0**, und melden Sie sich mit dem Domänenkonto **CORP\Install** an.
2. Wählen Sie im Dashboard **Server-Manager** die Option **Tools** aus, und klicken Sie dann auf **Failovercluster-Manager**.
3. Klicken Sie im linken Bereich mit der rechten Maustaste auf **Failovercluster-Manager**, und klicken Sie dann auf **Cluster erstellen**, wie unten gezeigt.
   
    ![Cluster erstellen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784632.png)
4. Erstellen Sie im Assistenten "Cluster erstellen" einen Cluster mit einem Knoten, indem Sie nacheinander die Seiten mit den nachfolgend aufgeführten Einstellungen durchlaufen:

| Seite | Einstellungen |
| --- | --- |
| Voraussetzungen |Standardwerte verwenden |
| Server auswählen |Geben Sie unter **Servernamen eingeben** die Zeichenfolge **sqlserver-0** ein, und klicken Sie auf **Hinzufügen**. |
| Validierungswarnung |Wählen Sie **Nein. Microsoft-Support für diesen Cluster nicht nötig. Validierungstests nicht durchführen. Beim Klicken auf "Weiter" Erstellung des Clusters fortsetzen.** aus. |
| Zugriffspunkt für die Clusterverwaltung |Geben Sie unter **Clustername** die Zeichenfolge **Cluster1** ein. |
| Bestätigung |Standardeinstellungen verwenden, es sei denn, Sie verwenden Speicherplätze. Siehe den Hinweis im Anschluss an diese Tabelle. |

> [!NOTE]
> Bei Verwendung von [Speicherplätzen](https://technet.microsoft.com/library/hh831739), die mehrere Datenträger zu Speicherpools gruppieren, müssen Sie auf der Seite **Bestätigung** das Kontrollkästchen **Der gesamte geeignete Speicher soll dem Cluster hinzugefügt werden** deaktivieren. Wenn Sie diese Option nicht deaktivieren, werden die virtuellen Festplatten von Windows während des Clusteringprozesses getrennt. Sie werden daher erst im Datenträger-Manager oder -Explorer angezeigt, nachdem die Speicherplätze aus dem Cluster entfernt und mithilfe der PowerShell erneut angefügt wurden.
> 
> 

Überprüfen Sie nun die Konfiguration des erstellten Clusters, und fügen Sie die restlichen Knoten hinzu. 

1. Scrollen Sie im mittleren Bereich nach unten bis zum Abschnitt **Hauptressourcen des Clusters**, und erweitern Sie die Details von **Name: Cluster1**. Die Ressource **Name** und **IP-Adresse** befinden sich im Zustand **Fehler**. Die IP-Adressressource kann nicht online geschaltet werden, da dem Cluster dieselbe IP-Adresse wie dem Computer selbst zugewiesen ist, wobei es sich damit um eine doppelte Adresse handelt.
2. Klicken Sie mit der rechten Maustaste auf die fehlerhafte Ressource **IP-Adresse**, und klicken Sie dann auf **Eigenschaften**.
   
    ![Eigenschaften des Clusters](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784633.png)
3. Wählen Sie **Statische IP-Adresse** aus, und geben Sie im Textfeld für die Adresse eine verfügbare Adresse aus „subnet-2“ an. Klicken Sie dann auf **OK**.
4. Klicken Sie im Abschnitt **Hauptressourcen des Clusters** mit der rechten Maustaste auf **Name: Cluster1**, und klicken Sie dann auf **Online schalten**. Warten Sie dann, bis beide Ressourcen online sind. Wenn die Clusternamensressource online ist, aktualisiert sie den DC-Server mit einem neuen AD-Computerkonto. Verwenden Sie dieses AD-Konto zum späteren Ausführen des Clusterdiensts der Verfügbarkeitsgruppe.
5. Schließlich fügen Sie die übrigen Knoten dem Cluster hinzu. Klicken Sie in der Browserstruktur mit der rechten Maustaste auf **Cluster1.corp.contoso.com**, und klicken Sie dann auf **Knoten hinzufügen**, wie unten dargestellt.
   
    ![Hinzufügen eines Knotens zum Cluster](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC784634.png)
6. Klicken Sie im **Assistenten „Knoten hinzufügen“** auf **Weiter**. Fügen Sie der Liste auf der Seite **Server auswählen** die Namen **sqlserver-1** und **cluster-fsw** hinzu, indem Sie jeweils unter **Servernamen eingeben** den Servernamen eingeben und anschließend auf **Hinzufügen** klicken. Wenn Sie fertig sind, klicken Sie auf **Weiter**.
7. Klicken Sie auf der Seite **Validierungswarnung** auf **Nein**. (In einem Produktionsszenario sollten Sie die Validierungstests ausführen.) Klicken Sie auf **Weiter**.
8. Klicken Sie auf der Seite **Bestätigung** auf **Weiter**, um die Knoten hinzuzufügen.
   
   > [!WARNING]
   > Bei Verwendung von [Speicherplätzen](https://technet.microsoft.com/library/hh831739), die mehrere Datenträger zu Speicherpools gruppieren, müssen Sie das Kontrollkästchen **Der gesamte geeignete Speicher soll dem Cluster hinzugefügt werden** deaktivieren. Wenn Sie diese Option nicht deaktivieren, werden die virtuellen Datenträger während des Clusteringprozesses getrennt. Als Folge daraus werden sie dann auch erst im Datenträger-Manager oder -Explorer angezeigt, nachdem die Speicherplätze aus dem Cluster entfernt und mithilfe der PowerShell erneut angefügt wurden.
   > 
   > 
9. Nachdem die Knoten dem Cluster hinzugefügt wurden, klicken Sie auf **Fertig stellen**. Im Failovercluster-Manager sollte nun angezeigt werden, dass Ihr Cluster über drei Knoten verfügt, und diese sollten im Container **Knoten** aufgelistet werden.
10. Melden Sie sich bei der Remotedesktopsitzung ab.

## <a name="configure-availability-groups"></a>Konfigurieren von Verfügbarkeitsgruppen
In diesem Abschnitt werden sowohl für **sqlserver-0** als auch für **sqlserver-1** folgende Aktionen durchgeführt:

* Hinzufügen von **CORP\Install** zur Standardinstanz von SQL Server als sysadmin-Rolle
* Öffnen der Firewall für den SQL Server-Remotezugriff für SQL Server, den Endpunkt für die Datenbankspiegelung und den Azure Load Balancer-Testport 
* Aktivieren der Funktion „Verfügbarkeitsgruppen“
* Ändern des SQL Server-Dienstkontos in **CORP\SQLSvc1** bzw. **CORP\SQLSvc2**

Diese Aktionen können in beliebiger Reihenfolge ausgeführt werden. Dennoch werden die folgenden Schritte in der vorgegebenen Reihenfolge durchlaufen. Führen Sie die Schritte sowohl für **sqlserver-0** als auch für **sqlserver-1** durch:

### <a name="add-installation-account-as-sysadmin-fixed-server-role-on-each-sql-server"></a>Hinzufügen des Installationskontos als feste SysAdmin-Serverrolle auf jedem SQL Server
1. Wenn Sie sich noch nicht von der Remotedesktopsitzung für den virtuellen Computer abgemeldet haben, tun Sie dies jetzt.
2. Starten Sie die RDP-Dateien für **sqlserver-0** und **sqlserver-1**, und melden Sie sich als **BUILTIN\DomainAdmin** an.
3. Starten Sie **SQL Server Management Studio**, und fügen Sie **CORP\Install** als Rolle vom Typ **SysAdmin** der Standardinstanz von SQL Server hinzu. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf **Anmeldungen**, und klicken Sie dann auf **Neue Anmeldung**.
4. Geben Sie **CORP\Install** in das Feld **Anmeldename** ein.
5. Wählen Sie auf der Seite **Serverrollen** die Option **sysadmin** aus. Klicken Sie dann auf **OK**. Nachdem die Anmeldung erstellt wurde, können Sie sie anzeigen, indem Sie die **Anmeldungen** in **Objekt-Explorer**.

### <a name="open-the-firewall-for-remote-access-to-sql-server-and-the-probe-port-on-each-sql-server"></a>Öffnen der Firewall für Remotezugriff auf SQL Server und den Testport auf jedem SQL Server
Für diese Lösung müssen auf jedem SQL Server zwei Firewallregeln konfiguriert werden. Die erste Regel ermöglicht eingehenden Zugriff auf SQL Server, die zweite ermöglicht eingehenden Zugriff auf den Load Balancer und den Listener. 

1. Als Nächstes erstellt Sie eine Firewallregel für SQL Server. Starten Sie über den **Startbildschirm** die **Windows-Firewall mit erweiterter Sicherheit**.
2. Klicken Sie im linken Bereich auf **Eingehende Regeln**. Klicken Sie im rechten Bereich auf **Neue Regel**.
3. Wählen Sie auf der Seite **Regeltyp** die Option **Programm** aus, und klicken Sie dann auf **Weiter**.
4. Wählen Sie auf der Seite **Programm** die Option **Dieser Programmpfad** aus, und geben Sie **%ProgramFiles%\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Binn\sqlservr.exe** in das Textfeld ein. (Wenn Sie diese Anleitung befolgen, aber SQL Server 2012 verwenden, lautet das SQL Server-Verzeichnis **MSSQL11. MSSQLSERVER**). Klicken Sie auf **Weiter**.
5. Lassen Sie auf der Seite **Aktion** die Option **Verbindung zulassen** aktiviert, und klicken Sie auf **Weiter**.
6. Akzeptieren Sie auf der Seite **Profil** die Standardeinstellungen, und klicken Sie auf **Weiter**.
7. Geben Sie auf der Seite **Name** im Textfeld **Name** einen Regelnamen an (Beispiel: **SQL Server (Programmregel)**), und klicken Sie anschließend auf **Fertig stellen**.
8. Erstellen Sie eine weitere eingehende Firewallregel für den Testport. Im Kontext dieses Tutorials wird eine eingehende Regel für den TCP-Port 59999 verwendet. Nennen Sie die Regel **SQL Server Listener**.

Führen Sie alle Schritte für beide SQL Server durch.

### <a name="enable-availability-groups-feature-on-each-sql-server"></a>Aktivieren der Funktion „Verfügbarkeitsgruppen“ für jeden SQL Server
Führen Sie diese Schritte für beide SQL Server durch. 

1. Als Nächstes aktivieren Sie die Funktion **AlwaysOn-Verfügbarkeitsgruppen** . Starten Sie über den **Startbildschirm** den **SQL Server-Konfigurations-Manager**.
2. Klicken Sie in der Browserstruktur auf **SQL Server-Dienste**, klicken Sie dann mit der rechten Maustaste auf den Dienst **SQL Server (MSSQLSERVER)**, und klicken Sie auf **Eigenschaften**.
3. Klicken Sie auf die Registerkarte **Hohe Verfügbarkeit mit AlwaysOn**, wählen Sie **AlwaysOn-Verfügbarkeitsgruppen aktivieren** aus, und klicken Sie dann auf **Übernehmen**, wie unten gezeigt. Klicken Sie im Popupdialogfenster auf **OK** , und lassen Sie das Eigenschaftenfenster noch geöffnet. Nachdem Sie das Dienstkonto geändert haben, starten Sie den SQL Server-Dienst neu.
   
    ![Aktivieren von AlwaysOn-Verfügbarkeitsgruppen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665520.gif)

### <a name="set-the-sql-server-service-account-on-each-sql-server"></a>Festlegen des SQL Server-Dienstkontos für jeden SQL Server
Führen Sie diese Schritte für beide SQL Server durch.

1. Als Nächstes ändern Sie das SQL Server-Dienstkonto. Klicken Sie auf die Registerkarte **Anmelden**, und geben Sie unter **Kontoname** entweder **CORP\SQLSvc1** (für **sqlserver-0**) oder **CORP\SQLSvc2** (für **sqlserver-1**) ein. Geben Sie dann das Kennwort ein, bestätigen Sie es, und klicken Sie auf **OK**.
2. Klicken Sie im Popupfenster auf **Ja** , um den SQL Server-Dienst neu zu starten. Nach dem Neustart des SQL Server-Diensts sind die Änderungen, die Sie im Eigenschaftenfenster vorgenommen haben, wirksam
3. Melden Sie sich bei den virtuellen Computern ab.

### <a name="create-the-availability-group"></a>Erstellen der Verfügbarkeitsgruppe
Sie sind jetzt bereit, um eine Verfügbarkeitsgruppe zu konfigurieren. Im Folgenden finden Sie einen Überblick der vorzunehmenden Aktionen:

* Erstellen einer neuen Datenbank (**MyDB1**) für **sqlserver-0**
* Erstellen sowohl einer vollständigen Sicherung als auch einer Transaktionsprotokollsicherung der Datenbank
* Wiederherstellen der vollständigen Sicherung und der Protokollsicherung auf **sqlserver-1** mit der Option **NORECOVERY**
* Erstellen der Verfügbarkeitsgruppe (**AG1**) mit synchronem Commit, automatischem Failover und lesbaren, sekundären Replikaten

### <a name="create-the-mydb1-database-on-sqlserver-0:"></a>Erstellen Sie die Datenbank „MyDB1“ für „sqlserver-0“:
1. Falls Sie sich noch nicht von den Remotedesktopsitzungen für **sqlserver-0** und **sqlserver-1** abgemeldet haben, führen Sie diesen Schritt jetzt aus.
2. Starten Sie die RDP-Datei für **sqlserver-0**, und melden Sie sich als **CORP\Install** an.
3. Erstellen Sie im **Datei-Explorer** unter **C:\** ein Verzeichnis namens **backup**. Dieses Verzeichnis verwenden Sie zum Sichern und Wiederherstellen Ihrer Datenbank.
4. Klicken Sie mit der rechten Maustaste auf das neue Verzeichnis, zeigen Sie auf **Freigeben für**, und klicken Sie dann auf **Bestimmte Personen**, wie unten gezeigt.
   
    ![Erstellen eines Sicherungsordners](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665521.gif)
5. Fügen Sie **CORP\SQLSvc1** hinzu, und erteilen Sie dafür die Berechtigung **Lesen/Schreiben**. Fügen Sie anschließend **CORP\SQLSvc2** hinzu, und erteilen Sie dafür die Berechtigung **Lesen/Schreiben** (wie unten dargestellt). Klicken Sie anschließend auf **Freigeben**. Nachdem der Dateifreigabeprozess abgeschlossen ist, klicken Sie auf **Fertig**.
   
    ![Erteilen von Berechtigungen für den Sicherungsordner](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665522.gif)
6. Erstellen Sie als Nächstes die Datenbank. Starten Sie **SQL Server Management Studio** über das** **Startmenü, und klicken Sie dann auf **Verbinden**, um eine Verbindung mit der Standardinstanz von SQL Server herzustellen.
7. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf **Datenbanken**, und klicken Sie dann auf **Neue Datenbank**.
8. Geben Sie unter **Datenbankname** den Namen **MyDB1** ein, und klicken Sie dann auf **OK**.

### <a name="take-a-full-backup-of-mydb1-and-restore-it-on-sqlserver-1:"></a>Erstellen Sie eine vollständige Sicherung von „MyDB1“, und stellen Sie sie auf „sqlserver-1“ wieder her:
1. Erstellen Sie als Nächstes eine vollständige Sicherung der Datenbank. Erweitern Sie im **Objekt-Explorer** den Eintrag **Datenbanken**, klicken Sie mit der rechten Maustaste auf **MyDB1**, zeigen Sie auf **Aufgaben**, und klicken Sie dann auf **Sichern**.
2. Lassen Sie im Abschnitt **Quelle** die Option **Sicherungstyp** auf **Vollständig** festgelegt. Klicken Sie im Abschnitt **Ziel** auf **Entfernen**, um den Standarddateipfad der Sicherungsdatei zu entfernen.
3. Klicken Sie im Abschnitt **Ziel** auf **Hinzufügen**.
4. Geben Sie im Textfeld **Dateiname** die Zeichenfolge **\\\\sqlserver-0\backup\MyDB1.bak** ein. Klicken Sie dann auf **OK** und anschließend erneut auf **OK**, um die Datenbank zu sichern. Wenn der Sicherungsvorgang abgeschlossen ist, klicken Sie wieder auf **OK** , um das Dialogfeld zu schließen.
5. Als Nächstes erstellen Sie eine Transaktionsprotokollsicherung der Datenbank. Erweitern Sie im **Objekt-Explorer** den Eintrag **Datenbanken**, klicken Sie mit der rechten Maustaste auf **MyDB1**, zeigen Sie auf **Aufgaben**, und klicken Sie dann auf **Sichern**.
6. Wählen Sie unter **Sicherungstyp** die Option **Transaktionsprotokoll** aus. Behalten Sie für den Dateipfad **Ziel** den von Ihnen zuvor angegebenen Pfad bei, und klicken Sie auf **OK**. Wenn der Sicherungsvorgang abgeschlossen ist, klicken Sie wieder auf **OK** .
7. Als Nächstes stellen Sie die vollständige Sicherung und die Transaktionsprotokollsicherung auf **sqlserver-1**wieder her. Starten Sie die RDP-Datei für **sqlserver-1**, und melden Sie sich als **CORP\Install** an. Lassen Sie die Remotedesktopsitzung für **sqlserver-0** geöffnet.
8. Starten Sie **SQL Server Management Studio** über das** **Startmenü, und klicken Sie dann auf **Verbinden**, um eine Verbindung mit der Standardinstanz von SQL Server herzustellen.
9. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf **Datenbanken**, und klicken Sie dann auf **Datenbank wiederherstellen**.
10. Wählen Sie im Abschnitt **Quelle** die Option **Gerät** aus, und klicken Sie dann auf die Schaltfläche mit den Auslassungspunkten (**...**) .
11. Klicken Sie unter **Sicherungsmedien auswählen** auf **Hinzufügen**.
12. Geben Sie unter „Speicherort der Sicherungsdatei“ den Pfad **\\\\sqlserver-0\backup** ein, und klicken Sie auf „Aktualisieren“. Wählen Sie „MyDB1.bak“ aus, klicken Sie auf „OK“, und klicken Sie dann erneut auf „OK“. Im Abschnitt "Wiederherzustellende Sicherungssätze" sollten jetzt die vollständige Sicherung und die Protokollsicherung angezeigt werden.
13. Wechseln Sie zur Seite "Optionen", wählen Sie dann in "Wiederherstellungsstatus" die Option "MIT NORECOVERY WIEDERHERSTELLEN" aus, und klicken Sie dann auf "OK", um die Datenbank wiederherzustellen. Wenn der Wiederherstellungsvorgang abgeschlossen ist, klicken Sie auf "OK".

### <a name="create-the-availability-group:"></a>Erstellen der Verfügbarkeitsgruppe:
1. Wechseln Sie zurück zur Remotedesktopsitzung für **sqlserver-0**. Klicken Sie im **Objekt-Explorer** in SSMS mit der rechten Maustaste auf **Hohe Verfügbarkeit mit AlwaysOn**, und klicken Sie dann auf **Assistent für neue Verfügbarkeitsgruppen**, wie unten dargestellt.
   
    ![Starten des Assistenten für neue Verfügbarkeitsgruppen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665523.gif)
2. Klicken Sie auf der Seite **Einführung** auf **Weiter**. Geben Sie auf der Seite **Namen der Verfügbarkeitsgruppe angeben** den Namen **AG1** in das Feld **Name der Verfügbarkeitsgruppe** ein, und klicken Sie dann erneut auf **Weiter**.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Namen der Verfügbarkeitsgruppe angeben](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665524.gif)
3. Wählen Sie auf der Seite **Datenbanken auswählen** den Eintrag **MyDB1** aus, und klicken Sie auf **Weiter**. Die Datenbank erfüllt die Voraussetzungen für eine Verfügbarkeitsgruppe, weil Sie mindestens eine vollständige Sicherung auf dem vorgesehenen primären Replikat erstellt haben.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Datenbanken auswählen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665525.gif)
4. Klicken Sie auf der Seite **Replikate angeben** auf **Replikat hinzufügen**.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Replikate angeben](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665526.png)
5. Das Dialogfeld **Verbindung mit Server herstellen** wird angezeigt. Geben Sie unter **Servername** die Zeichenfolge **sqlserver-1** ein, und klicken Sie dann auf **Verbinden**.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Verbindung mit dem Server herstellen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665527.png)
6. Auf der Seite **Replikate angeben** wird jetzt unter **Verfügbare Replikate** das Element **sqlserver-1** aufgeführt. Konfigurieren Sie die Replikate, wie unten gezeigt. 
   
    ![Assistent für neue Verfügbarkeitsgruppen, Replikate angeben (abgeschlossen)](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665528.png)
7. Klicken Sie auf **Endpunkte** , um den Endpunkt für die Datenbankspiegelung anzuzeigen, der von dieser Verfügbarkeitsgruppe verwendet werden soll. Jede Instanz von SQL Server muss über einen Endpunkt für die Datenbankspiegelung verfügen. Beachten Sie den TCP-Port, der vom Assistenten für diesen Endpunkt angegeben wird. Erstellen Sie für diesen TCP-Port auf jedem Server eine Firewallregel für eingehenden Datenverkehr.
   
    Wenn Sie fertig sind, klicken Sie auf **Weiter**.
8. Wählen Sie auf der Seite **Anfängliche Datensynchronisierung auswählen** die Option **Nur verknüpfen** aus, und klicken Sie dann auf **Weiter**. Sie haben bereits manuell eine Datensynchronisierung durchgeführt, als Sie die vollständige Sicherung und die Transaktionssicherung für **sqlserver-0** erstellt und diese auf **sqlserver-1** wiederhergestellt haben. Alternativ können Sie sich entscheiden, die Sicherungs- und Wiederherstellungsvorgänge nicht mit Ihrer Datenbank durchzuführen, und **Vollständig** auswählen, damit der „Assistent für neue Verfügbarkeitsgruppen“ die Datensynchronisierung für Sie ausführt. Hiervon wird jedoch bei sehr großen Datenbanken, wie sie in manchen Unternehmen vorhanden sind, abgeraten.
   
    ![Assistent für neue Verfügbarkeitsgruppen, anfängliche Datensynchronisierung auswählen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665529.png)
9. Klicken Sie auf der Seite **Validierung** auf **Weiter**. Diese Seite sollte ungefähr wie unten abgebildet aussehen. Es liegt eine Warnung für die Listenerkonfiguration vor, weil Sie keinen Listener für die Verfügbarkeitsgruppe konfiguriert haben. Diese Warnung können Sie ignorieren, weil in diesem Tutorial kein Listener konfiguriert wird. Der Listener wird in diesem Tutorial später noch erstellt.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Validierung](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665530.gif)
10. Klicken Sie auf der Seite **Zusammenfassung** auf **Fertig stellen**, und warten Sie dann, während der Assistent die neue Verfügbarkeitsgruppe konfiguriert. Auf der Seite **Status** können Sie auf **Weitere Details** klicken, um den detaillierten Status anzuzeigen. Überprüfen Sie nach Abschluss des Assistenten die Seite **Ergebnisse**, um sich zu vergewissern, dass die Verfügbarkeitsgruppe erfolgreich erstellt wurde (wie unten dargestellt), und klicken Sie dann auf **Schließen**, um den Assistenten zu beenden.
    
     ![Assistent für neue Verfügbarkeitsgruppen, Ergebnisse](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665531.png)
11. Erweitern Sie im **Objekt-Explorer** den Eintrag **Hohe Verfügbarkeit mit AlwaysOn**, und erweitern Sie dann **Verfügbarkeitsgruppen**. Sie sollten jetzt die neue Verfügbarkeitsgruppe in diesem Container sehen können. Klicken Sie mit der rechten Maustaste auf **AG1 (primär)**, und klicken Sie dann auf **Dashboard anzeigen**.
    
     ![Anzeigen des Verfügbarkeitsgruppen-Dashboards](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665532.png)
12. Ihr **AlwaysOn-Dashboard** sollte ähnlich wie unten dargestellt aussehen. Es werden die Replikate, der Failovermodus jedes Replikats und der Synchronisierungsstatus angezeigt.
    
     ![Verfügbarkeitsgruppen-Dashboard](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665533.png)
13. Kehren Sie zum **Server-Manager** zurück, wählen Sie **Tools** aus, und starten Sie dann den **Failovercluster-Manager**.
14. Erweitern Sie **Cluster1.corp.contoso.com**, und erweitern Sie dann **Dienste und Anwendungen**. Wählen Sie **Rollen** aus, und beachten Sie, dass die Verfügbarkeitsgruppenrolle **AG1** erstellt wurde. Beachten Sie, dass AG1 keine IP-Adresse besitzt, über die Datenbankclients eine Verbindung mit der Verfügbarkeitsgruppe herstellen können, weil Sie keinen Listener konfiguriert haben. Sie können direkt mit dem primären Knoten eine Verbindung herstellen, um Lese-/ Schreibvorgänge auszuführen, und mit dem sekundären Knoten für reine Leseabfragen.
    
     ![Verfügbarkeitsgruppe im Failovercluster-Manager](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual/IC665534.png)

> [!WARNING]
> Versuchen Sie nicht, ein Failover der Verfügbarkeitsgruppe aus dem Failovercluster-Manager heraus durchzuführen. Alle Failovervorgänge sollten aus dem **AlwaysOn-Dashboard** in SSMS ausgeführt werden. Weitere Informationen finden Sie unter [Einschränkungen für die Verwendung des WSFC-Failovercluster-Managers mit Verfügbarkeitsgruppen](https://msdn.microsoft.com/library/ff929171.aspx).
> 
> 

## <a name="configure-internal-load-balancer"></a>Konfigurieren des internen Load Balancers
Um eine direkte Verbindung mit der Verfügbarkeitsgruppe herzustellen, müssen Sie einen Lastenausgleich (Load Balancer) konfigurieren. Beim Lastenausgleich wird Clientdatenverkehr an die VM geleitet, die an die Listener-IP-Adresse und den Testport gebunden ist. In diesem Tutorial wird ein interner Lastenausgleich (Internal Load Balancer, ILB) verwendet. Per ILB wird es ermöglicht, dass für Datenverkehr eines virtuellen Netzwerks eine Verbindung mit SQL Server hergestellt wird. Für Anwendungen, die über das Internet eine Verbindung mit SQL Server herstellen, ist ein Lastenausgleich für Internetzugriff (externer Lastenausgleich) erforderlich. Weitere Informationen finden Sie unter [Übersicht über Azure Load Balancer](../load-balancer/load-balancer-overview.md).

> [!NOTE]
> Derzeit können Sie im Azure-Portal einen bestimmten Front-End-Port nur einmal für einen Lastenausgleich verwenden. Um den gleichen Port für alle Listener zu verwenden, können Sie mit PowerShell die Listener-IP-Adressen an den Load Balancer anfügen. Eine Anleitung finden Sie unter [Configure one or more Always On Availability Group Listeners - Resource Manager](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md)(Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern – Resource Manager). 
> 
> 

### <a name="create-the-load-balancer-in-azure"></a>Erstellen des Load Balancers in Azure
1. Navigieren Sie im Azure-Portal zu **SQL-HA-RG**, und klicken Sie auf **+ Hinzufügen**.
2. Suchen Sie nach **Load Balancer**. Wählen Sie den von Microsoft veröffentlichten Load Balancer aus, und klicken Sie auf **Erstellen**.
3. Konfigurieren Sie den Load Balancer mit folgenden Parametern: 

| Einstellung | Field |
| --- | --- |
| **Name** |sqlLB |
| **Schema** |Intern |
| **Virtuelles Netzwerk** |autoHAVNET |
| **Subnetz** |subnet-2. Dies ist die IP-Adresse, die Sie für den Listener in der Clusterressource festlegen. |
| **IP-Adresszuweisung** |Statisch |
| **IP-Adresse** |Verwenden Sie eine verfügbare Adresse aus „subnet-2“. |
| **Abonnement** |Verwenden Sie das gleiche Abonnement wie für alle anderen Ressourcen in dieser Lösung. |
| **Standort** |Verwenden Sie den gleichen Standort wie für alle anderen Ressourcen in dieser Lösung. |

Klicken Sie auf **Erstellen**.

Legen Sie für den Load Balancer folgende Einstellungen fest:

| Einstellung | Field |
| --- | --- |
| **Back-End-Pool** (Name) |sqlLBBE |
| **SQLLBBE – Verfügbarkeitsgruppe** |sqlAvailabilitySet |
| **SQLLBBE – Virtuelle Computer** |sqlserver-0, sqlserver-1 |
| **SQLLBBE – Verwendet von** |SQLAlwaysOnEndPointListener |
| **Test** (Name) |SQLAlwaysOnEndPointProbe |
| **Testprotokoll** |TCP |
| **Testport** |59999 (Hinweis: Sie können einen beliebigen nicht verwendeten Port verwenden.) |
| **Testintervall** |5 |
| **Fehlerschwellenwert für Test** |2 |
| **Test verwendet von** |SQLAlwaysOnEndPointListener |
| **Lastenausgleichsregeln** (Name) |SQLAlwaysOnEndPointListener |
| **Protokoll für Lastenausgleichsregeln** |TCP |
| **Port für Lastenausgleichsregeln** |1433 * |
| **Back-End-Port für Lastenausgleichsregeln** |1433 |
| **Test für Lastenausgleichsregeln** |SQLAlwaysOnEndPointProbe |
| **Sitzungspersistenz für Lastenausgleichsregeln** |Keine |
| **Leerlauftimeout für Lastenausgleichsregeln** |4 |
| **Floating IP (Direct Server Return) für Lastenausgleichsregeln** |Aktiviert |

> * 1433 ist der SQL Server-Standardport. Verwenden Sie ihn für den Front-End-Port der Standardinstanz. Wenn Sie mehrere Verfügbarkeitsgruppen benötigen, müssen Sie zusätzliche IP-Adressen für jede Verfügbarkeitsgruppe erstellen. Jede Verfügbarkeitsgruppe muss einen eigenen Front-End-Port aufweisen. Weitere Informationen finden Sie unter [Configure one or more Always On Availability Group Listeners - Resource Manager](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md)(Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern – Resource Manager).
> 
> [!NOTE]
> „Direct Server Return“ muss bei der Erstellung in den Lastenausgleichsregeln aktiviert sein.
> 
> 

Nachdem Sie den Load Balancer konfiguriert haben, können Sie den Listener für den Failovercluster konfigurieren. 

## <a name="configure-the-listener"></a>Konfigurieren des Listeners
Als Nächstes muss ein Verfügbarkeitsgruppenlistener für den Failovercluster konfiguriert werden. 

1. Stellen Sie eine RDP-Verbindung mit dem SQL Server zwischen „ad-primary-dc“ und „sqlserver-0“ her.
2. Notieren Sie sich im Failovercluster-Manager den Namen des Clusternetzwerks. Klicken Sie zum Ermitteln des Clusternetzwerknamens im linken Bereich des Failovercluster-Managers** **auf **Netzwerke**. Dieser Name wird im PowerShell-Skript in der `$ClusterNetworkName` -Variablen verwendet.
3. Erweitern Sie im Failovercluster-Manager den Clusternamen, und klicken Sie auf **Rollen**.
4. Klicken Sie im Bereich **Rollen** mit der rechten Maustaste auf den Verfügbarkeitsgruppennamen, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt** aus. 
5. Geben Sie unter **Name** die Zeichenfolge **aglistener** ein. Klicken Sie zweimal auf **Weiter** und anschließend auf **Fertig stellen**. Schalten Sie den Listener oder die Ressource jetzt noch nicht online.
6. Klicken Sie auf die Registerkarte **Ressourcen** , und erweitern Sie dann den Clientzugriffspunkt, den Sie gerade erstellt haben. Klicken Sie mit der rechten Maustaste auf die IP-Ressource, und klicken Sie auf „Eigenschaften“. Notieren Sie sich den Namen der IP-Adresse. Verwenden Sie diesen Namen im PowerShell-Skript in der Variablen `$IPResourceName` .
7. Klicken Sie unter **IP-Adresse** auf **Statische IP-Adresse**, und legen Sie die statische IP-Adresse auf die Adresse fest, die Sie auch im Azure-Portal für den Load Balancer **sqlLB** verwendet haben. Die gleiche IP-Adresse wird auch im PowerShell-Skript in der Variablen `$ILBIP` verwendet. 
8. Deaktivieren Sie NetBIOS für diese Adresse, und klicken Sie auf **OK**. 
9. Öffnen Sie auf dem Clusterknoten, der gerade das primäre Replikat hostet, eine PowerShell-ISE, und fügen Sie die folgenden Befehle in ein neues Skript ein.
   
    ```
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = "<X.X.X.X>" # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn> # In this sample we've using 59999 for the probe port. 
   
    Import-Module FailoverClusters
   
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
10. Aktualisieren Sie die Variablen, und führen Sie das PowerShell-Skript aus, um die IP-Adresse und den Port für den neuen Listener zu konfigurieren.
11. Klicken Sie im **Failovercluster-Manager** mit der rechten Maustaste auf die Verfügbarkeitsgruppenressource, und klicken Sie anschließend auf **Eigenschaften**. Legen Sie auf der Registerkarte **Abhängigkeiten** fest, dass die Ressourcengruppe vom Netzwerknamen des Listeners abhängig ist. 
12. Legen Sie die Port-Eigenschaft des Listeners auf 1433 fest. Öffnen Sie hierzu SQL Server Management Studio, klicken Sie mit der rechten Maustaste auf den Verfügbarkeitsgruppenlistener, und wählen Sie „Eigenschaften“ aus. Legen Sie **Port** auf 1433 fest. Sie sollten die gleiche Portnummer verwenden, die Sie für die SQL Server-Instanz konfiguriert haben. 
13. Nun können Sie den Listener online schalten. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Online schalten**.

### <a name="test-the-connection-to-the-listener"></a>Testen der Verbindung mit dem Listener
Gehen Sie wie folgt vor, um die Verbindung zu testen:

1. Stellen Sie eine RDP-Verbindung mit dem SQL Server her, der nicht das Replikat besitzt.
2. Testen Sie die Verbindung mithilfe des sqlcmd-Hilfsprogramms. Das folgende Skript stellt beispielsweise über den Listener eine sqlcmd-Verbindung mit Windows-Authentifizierung mit dem primären Replikat her:
   
    sqlcmd -S "<listenerName>" -E
   
   Wenn für den Listener ein anderer Port als 1433 verwendet wird, müssen Sie die Portnummer im Test angeben. Mit der folgenden Abfrage wird beispielsweise die Verbindung mit dem Listenernamen über Port 1435 getestet:
   
        sqlcmd -S "<listenerName>",1435 -E

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von SQL Server in Azure finden Sie unter [SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

<!--HONumber=Oct16_HO2-->


