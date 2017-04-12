---
title: "SQL Server-Verfügbarkeitsgruppen – Azure Virtual Machines – Voraussetzungen | Microsoft-Dokumentation"
description: "Dieses Tutorial zeigt, wie die Voraussetzungen zum Erstellen einer SQL Server AlwaysOn-Verfügbarkeitsgruppe in Azure-VMs konfiguriert werden."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: c492db4c-3faa-4645-849f-5a1a663be55a
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 8073a2ed0b565b1fdd2685f9c0f69abf2a3fc10a
ms.lasthandoff: 03/31/2017


---

# <a name="complete-prerequisites-for-creating-always-on-availability-groups-in-azure-virtual-machines"></a>Erfüllen der Voraussetzungen für die Erstellung von AlwaysOn-Verfügbarkeitsgruppen in Azure Virtual Machines

In diesem Tutorial erfahren Sie, wie Sie die Voraussetzungen zum Erstellen einer [SQL Server AlwaysOn-Verfügbarkeitsgruppe in Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-tutorial.md) erfüllen. Wenn die Voraussetzungen erfüllt sind, haben Sie einen Domänencontroller, zwei SQL Server-Instanzen und einen Zeugenserver in einer einzigen Ressourcengruppe.

**Geschätzte Zeit**: Es dauert möglicherweise ein paar Stunden, um die Voraussetzungen zu erfüllen. Ein großer Teil dieser Zeit wird zum Erstellen virtueller Computer aufgewendet.

Das Diagramm veranschaulicht, was Sie im Rahmen dieses Tutorials erstellen.

![Verfügbarkeitsgruppe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="review-availability-group-documentation"></a>Einsehen der Verfügbarkeitsgruppendokumentation

Dieses Tutorial setzt Grundkenntnisse über SQL Server AlwaysOn-Verfügbarkeitsgruppen voraus. Wenn Sie nicht mit dieser Technologie vertraut sind, lesen Sie die [Übersicht über AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).


## <a name="create-an-azure-account"></a>Erstellen eines Azure-Kontos
* Sie benötigen ein Azure-Konto. Sie können entweder ein [kostenloses Azure-Konto erstellen](/pricing/free-trial/?WT.mc_id=A261C142F) oder [Visual Studio-Abonnementvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

## <a name="create-resource-group"></a>Ressourcengruppe erstellen
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an. 
2. Klicken Sie auf **+**, um ein neues Objekt im Portal zu erstellen.

   ![Neu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-portalplus.png)

1. Geben Sie **Ressourcengruppe** in das **Marketplace**-Suchfenster ein.
   
   ![Ressourcengruppe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroupsymbol.png)
1. Klicken Sie auf **Ressourcengruppe**.
4. Klicken Sie auf **Erstellen**. 
5. Geben Sie auf dem Blatt **Ressourcengruppe** unter **Ressourcengruppenname** einen Namen für die Ressourcengruppe ein. Geben Sie beispielsweise **sql-ha-rg** ein.
6. Falls Sie über mehrere Azure-Abonnements verfügen, vergewissern Sie sich, dass es sich bei dem Abonnement um das Azure-Abonnement handelt, unter dem Sie die Verfügbarkeitsgruppe erstellen möchten. 
7. Wählen Sie einen Standort aus. Der Standort ist die Azure-Region, in der die Verfügbarkeitsgruppe erstellt werden soll. In diesem Tutorial erstellen wir alle Ressourcen an nur einem Azure-Standort. 
8. Vergewissern Sie sich, dass **An Dashboard anheften** aktiviert ist. Diese optionale Einstellung platziert eine Verknüpfung mit der Ressourcengruppe auf dem Dashboard des Azure-Portals.

   ![Ressourcengruppe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroup.png)

9. Klicken Sie auf **Erstellen** , um die Ressourcengruppe zu erstellen.

Azure erstellt die Ressourcengruppe und heftet eine Verknüpfung mit der Ressourcengruppe im Portal an.

## <a name="create-network-and-subnets"></a>Erstellen von Netzwerk und Subnetzen
Im nächsten Schritt werden die Netzwerke und Subnetze in der Azure-Ressourcengruppe erstellt.

Die Lösung verwendet ein virtuelles Netzwerk mit zwei Subnetzen. Weitere Informationen zu Netzwerken in Azure finden Sie unter [Virtuelle Netzwerke im Überblick](../../../virtual-network/virtual-networks-overview.md) .

So erstellen Sie das virtuelle Netzwerk:

1. Klicken Sie im Azure-Portal in der Ressourcengruppe auf **+ Hinzufügen**. Azure öffnet das Blatt **Alles** . 
   
   ![Neues Element](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/02-newiteminrg.png)
2. Suchen Sie nach **Virtuelles Netzwerk**.
   
     ![Suchen eines virtuellen Netzwerks](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/04-findvirtualnetwork.png)
3. Klicken Sie auf **Virtuelles Netzwerk**.
4. Klicken Sie auf dem Blatt **Virtuelles Netzwerk** auf das Bereitstellungsmodell **Resource Manager** und anschließend auf **Erstellen**.
   
   Die folgende Tabelle enthält die Einstellungen für das virtuelle Netzwerk:
   
   | **Field** | Wert |
   | --- | --- |
   | **Name** |autoHAVNET |
   | **Adressraum** |10.33.0.0/24 |
   | **Subnetzname** |Administrator |
   | **Subnetzadressbereich** |10.33.0.0/29 |
   | **Abonnement** |Geben Sie das Abonnement an, das Sie verwenden möchten. Wenn Sie nur über ein einzelnes Abonnement verfügen, ist die Option **Abonnement** leer. |
   | **Standort** |Geben Sie den Azure-Standort an. |
   
   Ihr Adressraum und Ihr Subnetzadressbereich können sich von den Angaben in der Tabelle unterscheiden. Abhängig von Ihrem Abonnement schlägt das Portal einen verfügbaren Adressraum und den entsprechenden Subnetzadressbereich vor. Ist kein geeigneter Adressraum verfügbar, verwenden Sie ein anderes Abonnement. 

   Im Beispiel wird der Subnetzname **Admin** verwendet. Dieses Subnetz ist für die Domänencontroller bestimmt. 

6. Klicken Sie auf **Erstellen**.
   
   ![Konfigurieren eines virtuellen Netzwerks](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/06-configurevirtualnetwork.png)

Azure zeigt wieder das Portaldashboard an und benachrichtigt Sie, wenn das neue Netzwerk erstellt wurde.

### <a name="create-a-second-subnet"></a>Erstellen eines zweiten Subnetzes
Das neue virtuelle Netzwerk verfügt über ein Subnetz mit dem Namen **Admin**. Dieses Subnetz wird von den Domänencontrollern verwendet. Die SQL Server-Instanzen verwenden ein zweites Subnetz namens **SQL**. So konfigurieren Sie dieses Subnetz:

1. Klicken Sie in Ihrem Dashboard auf die zuvor erstellte Ressourcengruppe **SQL-HA-RG**. Suchen Sie das Netzwerk in der Ressourcengruppe unter **Ressourcen**.
   
    Sollte **SQL-HA-RG** nicht angezeigt werden, klicken Sie auf **Ressourcengruppen**, und filtern Sie nach dem Namen der Ressourcengruppe.
2. Klicken Sie in der Liste mit den Ressourcen auf **autoHAVNET** . Azure öffnet das Blatt für die Netzwerkkonfiguration.
3. Klicken Sie im Bereich für das virtuelle Netzwerk **autoHAVNET** auf **Alle Einstellungen**.
4. Klicken Sie auf dem Blatt **Einstellungen** auf **Subnetze**.
   
    Hier sehen Sie das bereits erstellte Subnetz. 
   
   ![Konfigurieren eines virtuellen Netzwerks](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/07-addsubnet.png)
5. Erstellen Sie ein zweites Subnetz. Klicken Sie auf **+ Subnetz**. 
6. Konfigurieren Sie das Subnetz auf dem Blatt **Subnetz hinzufügen**, indem Sie unter **Name** die Zeichenfolge **sqlsubnet** eingeben. Azure gibt automatisch einen gültigen **Adressbereich**an. Vergewissern Sie sich, dass dieser Adressbereich mindestens zehn Adressen umfasst. In einer Produktionsumgebung werden unter Umständen weitere Adressen benötigt. 
7. Klicken Sie auf **OK**.
   
    ![Konfigurieren eines virtuellen Netzwerks](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/08-configuresubnet.png)

In der folgenden Tabelle sind die Netzwerkkonfigurationseinstellungen zusammengefasst:

| **Field** | Wert |
| --- | --- |
| **Name** |**autoHAVNET** |
| **Adressraum** |Hängt von den verfügbaren Adressräumen Ihres Abonnements ab. Ein typischer Wert wäre etwa 10.0.0.0/16. |
| **Subnetzname** |**admin** |
| **Subnetzadressbereich** |Hängt von den verfügbaren Adressbereichen Ihres Abonnements ab. Ein typischer Wert wäre etwa 10.0.0.0/24. |
| **Subnetzname** |**sqlsubnet** |
| **Subnetzadressbereich** |Hängt von den verfügbaren Adressbereichen Ihres Abonnements ab. Ein typischer Wert wäre etwa 10.0.1.0/24. |
| **Abonnement** |Geben Sie das Abonnement an, das Sie verwenden möchten. |
| **Ressourcengruppe** |**SQL-HA-RG** |
| **Standort** |Geben Sie den Speicherort an, den Sie auch für die Ressourcengruppe ausgewählt haben. |

## <a name="create-availability-sets"></a>Erstellen von Verfügbarkeitsgruppen

Vor dem Erstellen virtueller Computer müssen zunächst Verfügbarkeitsgruppen erstellt werden. Verfügbarkeitsgruppen verringern die Ausfallzeiten bei geplanten oder ungeplanten Wartungsereignissen. Eine Azure-Verfügbarkeitsgruppe ist eine logische Gruppe von Ressourcen, die Azure in physischen Fehlerdomänen und Updatedomänen platziert. Eine Fehlerdomäne stellt sicher, dass die Mitglieder der Verfügbarkeitsgruppe über eine separate Stromversorgung sowie über separate Netzwerkressourcen verfügen. Eine Updatedomäne stellt sicher, dass die Mitglieder der Verfügbarkeitsgruppe nicht gleichzeitig zu Wartungszwecken heruntergefahren werden. [Verwalten der Verfügbarkeit virtueller Computer](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Sie benötigen zwei Verfügbarkeitsgruppen: Eine für die Domänencontroller Eine für die SQL Server-Instanzen

Navigieren Sie zum Erstellen einer Verfügbarkeitsgruppe zur Ressourcengruppe, und klicken Sie auf **Hinzufügen**. Filtern Sie die Ergebnisse, indem Sie **Verfügbarkeitsgruppe**eingeben. Klicken Sie in den Ergebnissen auf **Verfügbarkeitsgruppe** . Klicken Sie auf **Erstellen**.

Konfigurieren Sie die beiden Verfügbarkeitsgruppen mit den Parametern in der folgenden Tabelle:

| **Field** | Verfügbarkeitsgruppe für Domänencontroller | Verfügbarkeitsgruppe für SQL Server |
| --- | --- | --- |
| **Name** |adavailabilityset |sqlavailabilityset |
| **Ressourcengruppe** |SQL-HA-RG |SQL-HA-RG |
| **Fehlerdomänen** |3 |3 |
| **Updatedomänen** |5 |3 |

Kehren Sie nach Erstellung der Verfügbarkeitsgruppen zur Ressourcengruppe im Azure-Portal zurück.

## <a name="create-domain-controllers"></a>Erstellen von Domänencontrollern
Sie haben bereits das Netzwerk, Subnetze, Verfügbarkeitsgruppen und einen Lastenausgleich für Internetzugriff erstellt. Nun können Sie die virtuellen Computer für die Domänencontroller erstellen.

### <a name="create-the-virtual-machines-for-the-domain-controllers"></a>Erstellen der virtuellen Computer für die Domänencontroller
Kehren Sie zum Erstellen und Konfigurieren der Domänencontroller zur Ressourcengruppe **SQL-HA-RG** zurück.

1. Klicken Sie auf "Hinzufügen". Das Blatt **Alles** wird geöffnet.
2. Geben Sie **Windows Server 2016 Datacenter** ein. 
3. Klicken Sie auf **Windows Server 2016 Datacenter**. Vergewissern Sie sich auf dem Blatt **Windows Server 2016 Datacenter**, dass das Bereitstellungsmodell **Resource Manager** lautet, und klicken Sie anschließend auf **Erstellen**. Azure öffnet das Blatt **Virtuellen Computer erstellen** . 

Wiederholen Sie die vorhergehenden Schritte zum Erstellen von zwei virtuellen Computern. Benennen Sie die beiden virtuellen Computer wie folgt:

* ad-primary-dc
* ad-secondary-dc
  
  > [!NOTE]
  > **ad-secondary-dc** ist eine optionale Komponente zur Gewährleistung hoher Verfügbarkeit für Active Directory-Domänendienste. 
  > 
  > 

Die folgende Tabelle enthält die Einstellungen für die beiden Computer:

| **Field** | Wert |
| --- | --- |
| **VM-Datenträgertyp** |SSD |
| **Benutzername** |DomainAdmin |
| **Kennwort** |Contoso!0000 |
| **Abonnement** |*Ihr Abonnement* |
| **Ressourcengruppe** |SQL-HA-RG |
| **Standort** |*Ihr Standort* |
| **Größe** |DS1_V2 |
| **Speicherkonto** |*Automatisch erstellt* |
| **Virtuelles Netzwerk** |autoHAVNET |
| **Subnetz** |admin |
| **Öffentliche IP-Adresse** |*Gleicher Name wie der VM* |
| **Netzwerksicherheitsgruppen (NSG)** |*Gleicher Name wie der VM* |
| **Verfügbarkeitsgruppe** |adavailabilityset |
| **Diagnose** |Aktiviert |
| **Diagnosespeicherkonto** |*Automatisch erstellt* |


   >[!IMPORTANT]
   >Sie können einen virtuellen Computer nur bei seiner Erstellung in einer Verfügbarkeitsgruppe platzieren. Die Verfügbarkeitsgruppe für einen virtuellen Computer kann nach dessen Erstellung nicht mehr geändert werden. Siehe [Verwalten der Verfügbarkeit virtueller Computer](../manage-availability.md).

Azure erstellt die virtuellen Computer.

Konfigurieren Sie nach der Erstellung der virtuellen Computer den Domänencontroller.

### <a name="configure-the-domain-controller"></a>Konfigurieren des Domänencontrollers
In den folgenden Schritten konfigurieren Sie den Computer **ad-primary-dc** als Domänencontroller für „corp.contoso.com“.

1. Öffnen Sie im Portal die Ressourcengruppe **SQL-HA-RG**, und wählen Sie den Computer **ad-primary-dc** aus. Klicken Sie auf dem Blatt **ad-primary-dc** auf **Verbinden**, um eine RDP-Datei für den Remotedesktopzugriff zu öffnen.
   
    ![Mit virtuellem Computer verbinden](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/20-connectrdp.png)
2. Melden Sie sich mit Ihrem konfigurierten Administratorkonto (**\DomainAdmin**) und Kennwort (**Contoso!0000**) an.
3. Standardmäßig sollte das Dashboard **Server-Manager** angezeigt werden.
4. Klicken Sie im Dashboard auf den Link **Rollen und Features hinzufügen** .
   
    !["Rollen hinzufügen" in Server-Explorer](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Wählen Sie **Weiter** aus, bis Sie zum Abschnitt **Serverrollen** gelangen.
6. Wählen Sie die Rollen **Active Directory Domain Services** und **DNS-Server** aus. Wenn Sie dazu aufgefordert werden, fügen Sie alle für diese Rollen erforderlichen, zusätzlichen Funktionen hinzu.
   
   > [!NOTE]
   > In Windows wird die Warnung angezeigt, dass keine statische IP-Adresse vorhanden ist. Wenn Sie die Konfiguration testen, klicken Sie auf "Weiter". Legen Sie in Produktionsszenarien die IP-Adresse über das Azure-Portal als statische Adresse fest, oder [verwenden Sie PowerShell, um die statische IP-Adresse des Domänencontrollercomputers festzulegen](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Dialogfeld "Rollen hinzufügen"](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/23-addroles.png)
7. Klicken Sie auf **Weiter**, bis Sie zum Abschnitt **Bestätigung** gelangen. Aktivieren Sie das Kontrollkästchen **Zielserver bei Bedarf automatisch neu starten** .
8. Klicken Sie auf **Installieren**.
9. Nachdem die Installation der Funktionen abgeschlossen ist, kehren Sie zum Dashboard **Server-Manager** zurück.
10. Wählen Sie die neue Option **AD DS** im linken Bereich aus.
11. Klicken Sie in der gelben Warnungsleiste auf den Link **Mehr** .
    
    ![AD DS-Dialogfeld auf virtuellem DNS-Servercomputer](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/24-addsmore.png)
12. Klicken Sie im Dialogfeld **Alle Serveraufgabendetails** in der Spalte **Aktion** auf **Server zu einem Domänencontroller heraufstufen**.
13. Verwenden Sie im **Konfigurations-Assistenten für Active Directory-Domänendienste**die folgenden Werte:
    
    | **Seite** | Einstellung |
    | --- | --- |
    | **Bereitstellungskonfiguration** |**Neue Gesamtstruktur hinzufügen**<br/> **Rootdomänenname** = corp.contoso.com |
    | **Domänencontrolleroptionen** |**DSRM-Kennwort** = Contoso!0000<br/>**Kennwort bestätigen** = Contoso!0000 |
14. Klicken Sie auf **Weiter** , um die anderen Seiten des Assistenten zu durchlaufen. Vergewissern Sie sich, dass auf der Seite **Prüfung der erforderlichen Komponenten** die folgende Meldung angezeigt wird: **Alle erforderlichen Komponenten wurden erfolgreich überprüft.** Überprüfen Sie alle zutreffenden Warnmeldungen, aber es ist möglich, die Installation fortzusetzen.
15. Klicken Sie auf **Installieren**. Der virtuelle Computer **ad-primary-dc** wird automatisch neu gestartet.

### <a name="note-ip-address-of-primary-domain-controller"></a>Notieren Sie die IP-Adresse des primären Domänencontrollers.

Verwenden Sie den primären Domänencontroller für den DNS. Notieren Sie die IP-Adresse des primären Domänencontrollers.

Eine Möglichkeit, die IP-Adresse des primären Domänencontrollers abzurufen, ist der Weg über das Azure-Portal. 

1. Öffnen Sie im Azure-Portal die Ressourcengruppe. 

1. Klicken Sie auf den primären Domänencontroller.

1. Klicken Sie auf dem Blatt des primären Domänencontrollers auf **Netzwerkschnittstellen**.

![Neues Element](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/25-primarydcip.png)

Notieren Sie die private IP-Adresse für diesen Server. 

### <a name="configure-the-second-domain-controller"></a>Konfigurieren des zweiten Domänencontrollers
Nach dem Neustart des primären Domänencontrollers können Sie den zweiten Domänencontroller konfigurieren. Dieser optionale Schritt dient zur Gewährleistung einer hohen Verfügbarkeit. Gehen Sie wie folgt vor, um den zweiten Domänencontroller zu konfigurieren:

1. Öffnen Sie im Portal die Ressourcengruppe **SQL-HA-RG**, und wählen Sie den Computer **ad-secondary-dc** aus. Klicken Sie auf dem Blatt **ad-secondary-dc** auf **Verbinden**, um eine RDP-Datei für den Remotedesktopzugriff zu öffnen.
4. Melden Sie sich mit Ihrem konfigurierten Administratorkonto (**BUILTIN\DomainAdmin**) und Kennwort (**Contoso!0000**) bei der VM an.
3. Legen Sie die Adresse des Domänencontrollers als bevorzugte DNS-Serveradresse fest. 
1. Klicken Sie in **Netzwerk- und Freigabecenter** auf die Netzwerkschnittstelle. 
   ![NetworkInterface](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/26-networkinterface.png)

5. Klicken Sie auf **Eigenschaften**.
10. Wählen Sie **Internetprotokoll Version 4 (TCP/IPv4)** aus, und klicken Sie auf „Eigenschaften“.
11. Wählen Sie **Folgende DNS-Serveradressen verwenden** aus, und geben Sie unter **Bevorzugter DNS-Server** die Adresse des primären Domänencontrollers an.
1. Klicken Sie auf **OK** und anschließend auf **Schließen**, um die Änderungen zu übernehmen. Sie können den virtuellen Computer jetzt zu **corp.contoso.com**beitreten lassen.

   >[!IMPORTANT]
   >Wenn Sie die Verbindung mit dem Remotedesktop nach dem Ändern der DNS-Einstellung verlieren, wechseln Sie zum Azure-Portal, und starten Sie den virtuellen Computer neu.

1. Öffnen Sie über den Remotedesktop auf dem sekundären Domänencontroller das Dashboard **Server-Manager**.
4. Klicken Sie im Dashboard auf den Link **Rollen und Features hinzufügen** .
   
    !["Rollen hinzufügen" in Server-Explorer](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Wählen Sie **Weiter** aus, bis Sie zum Abschnitt **Serverrollen** gelangen.
6. Wählen Sie die Rollen **Active Directory Domain Services** und **DNS-Server** aus. Wenn Sie dazu aufgefordert werden, fügen Sie alle für diese Rollen erforderlichen, zusätzlichen Funktionen hinzu.

9. Nachdem die Installation der Funktionen abgeschlossen ist, kehren Sie zum Dashboard **Server-Manager** zurück.
10. Wählen Sie die neue Option **AD DS** im linken Bereich aus.
11. Klicken Sie in der gelben Warnungsleiste auf den Link **Mehr** .
12. Klicken Sie im Dialogfeld **Alle Serveraufgabendetails** in der Spalte **Aktion** auf **Server zu einem Domänencontroller heraufstufen**.
1. Wählen Sie unter **Bereitstellungskonfiguration** die Option **Domänencontroller vorhandener Domäne hinzufügen**. 
   ![NetworkInterface](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/28-deploymentconfig.png)

1. Klicken Sie auf **Auswählen...**.
1. Stellen Sie mithilfe des Administratorkontos (**CORP.CONTOSO.COM\domainadmin**) und Kennworts (**Contoso!0000**) eine Verbindung her.
1. Klicken Sie unter **Domäne aus der Gesamtstruktur auswählen** auf Ihre Domäne und dann auf **OK**. 

1. Verwenden Sie unter **Domänencontrolleroptionen** die Standardwerte, und legen Sie ein DSRM-Kennwort fest.

   >[!NOTE]
   >Die Seite **DNS-Optionen** warnt Sie möglicherweise, dass für diesen DNS-Server keine Delegierung erstellt werden kann. Sie können diese Warnung in nicht produktiven Umgebungen ignorieren. 
1. Klicken Sie auf **Weiter**, bis das Dialogfeld die **Voraussetzungsprüfung** erreicht. Klicken Sie dann auf **Weiter**.

Nachdem der Server die Änderungen an der Konfiguration abgeschlossen hat, starten Sie den Server neu. 

### <a name=DomainAccounts></a> Konfigurieren von Domänenkonten

In den nächsten Schritten werden die Active Directory-Konten (AD) konfiguriert. Die folgende Tabelle zeigt die Konten:

| |Installationskonto<br/> |sqlserver-0 <br/>SQL Server- und SQL Agent-Dienstkonto |sqlserver-1<br/>SQL Server- und SQL Agent-Dienstkonto
| --- | --- | --- | --- 
|**Vorname** |Installieren |SQLSvc1 | SQLSvc2
|**SamAccountName von Benutzer** |Installieren |SQLSvc1 | SQLSvc2

Führen Sie zum Erstellen jedes Kontos die folgenden Schritte aus. 

1. Melden Sie sich wieder am Computer **ad-primary-dc** an.
2. Wählen Sie im **Server-Manager** die Option **Tools** aus, und klicken Sie dann auf **Active Directory-Verwaltungscenter**.   
3. Wählen Sie **corp (local)** im linken Bereich.
4. Wählen Sie rechts im Aufgabenbereich****die Option **Neu** aus, und klicken Sie dann auf **Benutzer**. 
   ![Active Directory-Verwaltungscenter](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/29-addcnewuser.png)

   >[!TIP]
   >Legen Sie ein komplexes Kennwort für jedes Konto fest.<br/> Legen Sie für nicht produktive Umgebungen fest, dass das Benutzerkonto nie abläuft.

5. Klicken Sie auf **OK** , um den Benutzer zu erstellen. 
6. Wiederholen Sie die vorherigen Schritte für jedes der drei Konten. 

### <a name="grant-required-permissions-to-the-installation-account"></a>Erteilen der erforderlichen Berechtigungen für das Installationskonto
1. Wählen Sie im **Active Directory-Verwaltungscenter** im linken Bereich die Option **corp (lokal)** aus. Klicken Sie dann rechts im Aufgabenbereich****auf **Eigenschaften**.
   
    ![Eigenschaften des Benutzers CORP](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/31-addcproperties.png)
8. Wählen Sie **Erweiterungen** aus, und klicken Sie auf der Registerkarte **Sicherheit** auf die Schaltfläche **Erweitert**.
9. Gehen Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für corp** wie folgt vor: Klicken Sie auf **Hinzufügen**.
10. Klicken Sie auf **Prinzipal auswählen**. Suchen Sie dann nach **CORP\Install**. Klicken Sie auf **OK**.
11. Aktivieren Sie **Alle Eigenschaften lesen**.

1. Aktivieren Sie **Computerobjekte erstellen**.
    
     ![Berechtigungen des Benutzers CORP](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/33-addpermissions.png)
12. Klicken Sie auf **OK** und dann erneut auf **OK**. Schließen Sie das Eigenschaftenfenster von corp.

Nachdem Sie nun die Konfiguration von Active Directory und den Benutzerobjekten abgeschlossen haben, erstellen Sie zwei virtuelle SQL Server-Computer und einen virtuellen Computer als Zeugenserver. Führen Sie für alle drei Computer anschließend den Beitritt zur Domäne durch.

## <a name="create-sql-servers"></a>Erstellen von SQL Server-Computern
### <a name="create-and-configure-the-sql-server-vms"></a>Erstellen und Konfigurieren der virtuellen SQL Server-Computer
Im nächsten Schritt werden drei virtuelle Computer erstellt: zwei virtuelle SQL Server-Computer und ein virtueller Computer für einen zusätzlichen Clusterknoten. Kehren Sie zum Erstellen dieser virtuellen Computer jeweils zur Ressourcengruppe **SQL-HA-RG** zurück, klicken Sie auf **Hinzufügen**, suchen Sie nach dem entsprechenden Katalogelement (**Virtueller Computer**), und klicken Sie dann auf **Aus Katalog**. Verwenden Sie die Informationen in der folgenden Tabelle, die Sie bei der Erstellung der virtuellen Computer unterstützen:

| Seite | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Wählen Sie das passende Katalogelement aus. |**Windows Server 2016 Datacenter** |**SQL Server 2016 SP1 Enterprise unter Windows Server 2016** |**SQL Server 2016 SP1 Enterprise unter Windows Server 2016** |
| **Grundlagen** |**Name** = cluster-fsw<br/>**Benutzername** = DomainAdmin<br/>**Kennwort** = Contoso!0000<br/>**Abonnement** = Ihr Abonnement<br/>**Ressourcengruppe** = SQL-HA-RG<br/>**Standort** = Ihr Azure-Standort |**Name** = sqlserver-0<br/>**Benutzername** = DomainAdmin<br/>**Kennwort** = Contoso!0000<br/>**Abonnement** = Ihr Abonnement<br/>**Ressourcengruppe** = SQL-HA-RG<br/>**Standort** = Ihr Azure-Standort |**Name** = sqlserver-1<br/>**Benutzername** = DomainAdmin<br/>**Kennwort** = Contoso!0000<br/>**Abonnement** = Ihr Abonnement<br/>**Ressourcengruppe** = SQL-HA-RG<br/>**Standort** = Ihr Azure-Standort |
| Konfiguration des virtuellen Computers: **Größe** |DS1\_V2 (1 Kern, 3,5GB) |**GRÖSSE** = DS2\_V2 (2 Kerne, 7GB) |**GRÖSSE** = DS2\_V2 (2 Kerne, 7GB) |
| Konfiguration des virtuellen Computers: **Einstellungen** |**Speicher** = Premium (SSD)<br/>**NETZWERKSUBNETZE** = autoHAVNET<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**Subnetz** = sqlsubnet(10.1.1.0/24)<br/>**Öffentliche IP-Adresse** = Keine<br/>**Netzwerksicherheitsgruppe** = Keine<br/>**Überwachung und Diagnose** = Aktiviert<br/>**Diagnosespeicherkonto** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = SqlAvailabilitySet<br/> |**Speicher** = Premium (SSD)<br/>**NETZWERKSUBNETZE** = autoHAVNET<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**Subnetz** = sqlsubnet(10.1.1.0/24)<br/>**Öffentliche IP-Adresse** = Keine<br/>**Netzwerksicherheitsgruppe** = Keine<br/>**Überwachung und Diagnose** = Aktiviert<br/>**Diagnosespeicherkonto** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = SqlAvailabilitySet<br/> |**Speicher** = Premium (SSD)<br/>**NETZWERKSUBNETZE** = autoHAVNET<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**Subnetz** = sqlsubnet(10.1.1.0/24)<br/>**Öffentliche IP-Adresse** = Keine<br/>**Netzwerksicherheitsgruppe** = Keine<br/>**Überwachung und Diagnose** = Aktiviert<br/>**Diagnosespeicherkonto** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = SqlAvailabilitySet<br/> |
| Konfiguration des virtuellen Computers: **SQL Server-Einstellungen** |Nicht zutreffend |**SQL-Konnektivität** = Privat (innerhalb von Virtual Network)<br/>**Port** = 1433<br/>**SQL-Authentifizierung** = Deaktiviert<br/>**Speicherkonfiguration** = Allgemein<br/>**Automatisiertes Patchen** = Sonntags, 2:00 Uhr<br/>**Automatisierte Sicherung** = Deaktiviert</br>**Azure Key Vault-Integration** = Deaktiviert |**SQL-Konnektivität** = Privat (innerhalb von Virtual Network)<br/>**Port** = 1433<br/>**SQL-Authentifizierung** = Deaktiviert<br/>**Speicherkonfiguration** = Allgemein<br/>**Automatisiertes Patchen** = Sonntags, 2:00 Uhr<br/>**Automatisierte Sicherung** = Deaktiviert</br>**Azure Key Vault-Integration** = Deaktiviert |

<br/>

> [!NOTE]
> Die hier vorgeschlagenen Computergrößen sind für das Testen von Verfügbarkeitsgruppen in Azure-VMs vorgesehen. Die beste Leistung für Produktionsarbeitslasten finden Sie unter den Empfehlungen für die Größe und Konfiguration der SQL Server-Computer in [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> 
> 

Sobald die drei virtuellen Computer vollständig bereitgestellt wurden, müssen Sie sie der Domäne **corp.contoso.com** hinzufügen und „CORP\Install“ Administratorrechte für die Computer gewähren.

### <a name="set-dns-on-each-server"></a>Festlegen des DNS auf jedem Server
Ändern Sie zunächst für jeden Mitgliedsserver die Adresse des bevorzugten DNS-Servers. Folgen Sie diesen Schritten:

1. Öffnen Sie im Portal die Ressourcengruppe **SQL-HA-RG**, und wählen Sie den Computer **sqlserver-0** aus. Klicken Sie auf dem Blatt **sqlserver-0** auf **Verbinden**, um eine RDP-Datei für den Remotedesktopzugriff zu öffnen.
2. Melden Sie sich mit Ihrem konfigurierten Administratorkonto (**\DomainAdmin**) und Kennwort (**Contoso!0000**) an.
3. Standardmäßig sollte das Dashboard **Server-Manager** angezeigt werden. Klicken Sie im linken Bereich auf **Lokaler Server** .
5. Wählen Sie den Link **IPv4-Adresse wird per DHCP zugewiesen, IPv6-fähig** aus.
6. Wählen Sie im Fenster **Netzwerkverbindungen** das Netzwerksymbol aus.
7. Klicken Sie auf der Befehlsleiste auf **Einstellungen dieser Verbindung ändern**. Wenn Sie diese Option nicht sehen, klicken Sie auf den doppelten nach rechts weisenden Pfeil.
8. Wählen Sie **Internetprotokoll Version 4 (TCP/IPv4)** aus, und klicken Sie auf „Eigenschaften“.
9. Wählen Sie **Folgende DNS-Serveradressen verwenden** aus, und geben Sie unter **Bevorzugter DNS-Server** die Adresse des primären Domänencontrollers an.
   >[!TIP]
   >Um die IP-Adresse des Hauptknotens abzurufen, verwenden Sie `nslookup`.<br/>
   >Geben Sie an der Eingabeaufforderung `nslookup ad-primary-dc` ein. 
11. Klicken Sie auf **OK** und anschließend auf **Schließen**, um die Änderungen zu übernehmen. 

   >[!IMPORTANT]
   >Wenn Sie die Verbindung mit dem Remotedesktop nach dem Ändern der DNS-Einstellung verlieren, wechseln Sie zum Azure-Portal, und starten Sie den virtuellen Computer neu.

Wiederholen Sie diese Schritte für alle Server.

### <a name="joinDomain"></a>Fügen Sie die Server der Domäne hinzu.

Sie können den virtuellen Computer jetzt zu **corp.contoso.com**beitreten lassen. Führen Sie Folgendes für beide SQL Server-Instanzen und den Dateifreigabe-Zeugenserver aus: 

1. Stellen Sie mit **BUILTIN\DomainAdmin** eine Remoteverbindung mit dem virtuellen Computer her. 
1. Klicken Sie im **Server-Manager** auf **Lokaler Server**.
1. Klicken Sie auf den Link **WORKGROUP**.
1. Klicken Sie im Abschnitt **Computername** auf **Ändern**.
1. Aktivieren Sie das Kontrollkästchen **Domäne**, und geben Sie in das Textfeld die Zeichenfolge **corp.contoso.com** ein. Klicken Sie auf **OK**.
1. Geben Sie im Popupdialogfeld **Windows-Sicherheit** die Anmeldeinformationen für das standardmäßige Domänenadministratorkonto (**CORP\DomainAdmin**) und das Kennwort (**Contoso!0000**) an.
1. Wenn die Meldung „Willkommen in der Domäne ‚corp.contoso.com‘“ angezeigt wird, klicken Sie auf **OK**.
1. Klicken Sie auf **Schließen**, und klicken Sie dann im Popupdialogfeld auf **Jetzt neu starten**.



### <a name="add-the-corpinstall-user-as-an-administrator-on-each-cluster-vm"></a>Fügen Sie auf jedem virtuellen Clustercomputer den Benutzer „Corp\Install“ als Administrator hinzu:

Fügen Sie nach dem Neustart aller virtuellen Computer als Mitglied der Domäne **CORP\Install** als Mitglied der lokalen Administratorengruppe hinzu. 

1. Warten Sie, bis der virtuelle Computer neu gestartet wurde, und starten Sie die RDP-Datei erneut über den primären Domänencontroller, um sich bei **sqlserver-0** mit dem Konto **CORP\DomainAdmin** anzumelden.
   >[!TIP]
   >Stellen Sie sicher, dass Sie sich bei dem Domänenadministratorkonto anmelden. In den vorherigen Schritten haben Sie das BUILT IN-Administratorkonto verwendet. Nun, wo der Server sich in der Domäne befindet, verwenden Sie das Domänenkonto. Geben Sie in Ihrer RDP-Sitzung *DOMAIN*\\*Benutzername* an.

2. Wählen Sie im **Server-Manager** die Option **Tools** aus, und klicken Sie dann auf **Computerverwaltung**.
3. Erweitern Sie im Fenster **Computerverwaltung** die Option **Lokale Benutzer und Gruppen**, und wählen Sie dann **Gruppen** aus.
4. Doppelklicken Sie auf die Gruppe **Administratoren** .
5. Klicken Sie im Dialogfeld **Administratoreigenschaften** auf die Schaltfläche **Hinzufügen**.
6. Geben Sie den Benutzer **CORP\Install** ein, und klicken Sie dann auf **OK**. 
7. Klicken Sie auf **OK**, um das Dialogfeld **Administratoreigenschaften** zu schließen.
8. Wiederholen Sie die oben genannten Schritte für **sqlserver-1** und **cluster-fsw**.

### <a name="setServiceAccount"></a>Festlegen der SQL Server-Dienstkonten

Legen Sie auf jeder SQL Server-Instanz das SQL Server-Dienstkonto fest. Verwenden Sie die Konten, die Sie beim [Konfigurieren der Domänenkonten](#DomainAccounts) erstellt haben.

1. Öffnen Sie den **SQL Server-Konfigurations-Manager**.

1. Klicken Sie mit der rechten Maustaste auf den SQL Server-Dienst, und klicken Sie auf **Eigenschaften**.

1. Legen Sie Konto und Kennwort fest. 

1. Wiederholen Sie diese Schritte für die andere SQL Server-Instanz.  

Für SQL Server-Verfügbarkeitsgruppen muss jede SQL Server-Instanz als Domänenkonto ausgeführt werden. 

### <a name="create-login-on-each-sql-server-for-installation-account"></a>Erstellen einer Anmeldung für das Installationskonto auf jeder SQL Server-Instanz

Verwenden Sie das Installationskonto, um die Verfügbarkeitsgruppe zu konfigurieren. Dieses Konto muss auf jeder SQL Server-Instanz ein Mitglied der festen Serverrolle **sysadmin** sein. Mit folgenden Schritten erstellen Sie eine Anmeldung für das Installationskonto:

1. Stellen Sie mithilfe des *\<MachineName\>\DomainAdmin*-Kontos eine RDP-Verbindung mit dem Server her.

1. Öffnen Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit der lokalen SQL Server-Instanz her. 

1. Klicken Sie im **Objektexplorer** auf **Sicherheit**.

1. Klicken Sie mit der rechten Maustaste auf **Anmeldungen**. Klicken Sie auf **Neue Anmeldung...**.

1. Klicken Sie in **Anmeldung – Neu** auf **Suchen...**.

1. Klicken Sie auf **Standorte...**.

1. Geben Sie die Netzwerkanmeldeinformationen des Domänenadministrators ein. 

1. Verwenden Sie das Installationskonto.

1. Legen Sie die Anmeldung als Mitglied der festen Serverrolle **Sysadmin** fest. 

1. Klicken Sie auf OK. 

Wiederholen Sie die vorhergehenden Schritte für die andere SQL Server-Instanz. 

## <a name="add-failover-cluster-features-to-both-sql-servers"></a>Hinzufügen von Failovercluster-Features zu beiden SQL Server-Instanzen

Um die Failovercluster-Features hinzuzufügen, führen Sie die folgenden Schritte auf beiden SQL-Server-Instanzen aus:

1. Öffnen Sie über den Remotedesktop auf dem sekundären Domänencontroller das Dashboard **Server-Manager**.
4. Klicken Sie im Dashboard auf den Link **Rollen und Features hinzufügen** .
   
    !["Rollen hinzufügen" in Server-Explorer](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Wählen Sie **Weiter** aus, bis Sie zum Abschnitt **Serverfeatures** gelangen.
1. Wählen Sie in **Features** die Option **Failoverclustering**. 
1. Fügen Sie zusätzliche erforderliche Features hinzu. 
1. Klicken Sie auf „Installieren“, um alle Features hinzuzufügen.

Wiederholen Sie diese Schritte für die andere SQL Server-Instanz. 


## <a name="a-nameendpoint-firewall-configure-the-firewall-on-each-sql-server"></a><a name="endpoint-firewall"> Konfigurieren der Firewall auf jeder SQL-Server-Instanz

Für die Lösung müssen die folgenden TCP-Ports in der Firewall geöffnet sein:

- **SQL Server**<br/>
   Port 1433 für eine Standardinstanz von SQL Server. 
- **Azure Load Balancer-Test**<br/>
   Ein beliebiger verfügbarer Port. Für Beispiele wird häufig 59999 verwendet.
- **Datenbankspiegelungs-Endpunkt** <br/>
   Ein beliebiger verfügbarer Port. Für Beispiele wird häufig 5022 verwendet. 

Die Firewallports müssen auf beiden SQL-Server-Instanzen geöffnet sein.

Das Verfahren zum Öffnen von Ports hängt von der Firewalllösung ab, die Sie verwenden. Im nächsten Abschnitt wird erklärt, wie Sie die Ports in der Windows-Firewall zu öffnen. Öffnen Sie die erforderlichen Ports jeder SQL Server-Instanz. 

### <a name="open-a-tcp-port-on-a-firewall"></a>Öffnen eines TCP-Ports auf einer Firewall 

1. Starten Sie auf dem **Startbildschirm** der ersten SQL Server-Instanz die **Windows-Firewall mit erweiterter Sicherheit**.

2. Klicken Sie im linken Bereich auf **Eingehende Regeln**. Klicken Sie im rechten Bereich auf **Neue Regel**.

3. Wählen Sie Für **Regeltyp** die Option **Port**.

1. Geben Sie für den Port TCP an, und geben Sie die entsprechenden Portnummern ein. Siehe folgendes Beispiel:

   ![SQLFirewall](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/35-tcpports.png)

1. Klicken Sie auf **Weiter**. 

5. Lassen Sie auf der Seite **Aktion** die Option **Verbindung zulassen** aktiviert, und klicken Sie auf **Weiter**.

6. Akzeptieren Sie auf der Seite **Profil** die Standardeinstellungen, und klicken Sie auf **Weiter**.

7. Geben Sie auf der Seite **Name** im Textfeld **Name** einen Regelnamen an (Beispiel: **Azure Load Balancer-Test**), und klicken Sie anschließend auf **Fertig stellen**.

Wiederholen Sie diese Schritte auf der zweiten SQL Server-Instanz in gleicher Weise.



## <a name="next-steps"></a>Nächste Schritte

* [Create SQL Server Always On Availability Group on Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-tutorial.md) (Erstellen der SQL Server AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern)

