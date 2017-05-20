---
title: "Konfigurieren von Always On-Verfügbarkeitsgruppen auf virtuellen Azure-Computern (klassisch) | Microsoft-Dokumentation"
description: "Erstellen Sie eine Always On-Verfügbarkeitsgruppe mit virtuellen Azure-Computern. In diesem Tutorial werden die Benutzeroberfläche und Tools anstelle von Skripts verwendet."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 8d48a3d2-79f8-4ab0-9327-2f30ee0b2ff1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d9afb1554158a4d88b7f161c62fa51c1bf61a7d
ms.openlocfilehash: b360fe9f28eeb9b10c82fce729165b1b572ac3c6
ms.contentlocale: de-de
ms.lasthandoff: 04/12/2017


---
# <a name="configure-always-on-availability-group-in-azure-virtual-machines-classic"></a>Konfigurieren von Always On-Verfügbarkeitsgruppen auf virtuellen Azure-Computern (klassisch)
> [!div class="op_single_selector"]
> * [Klassisch: Benutzeroberfläche](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klassisch: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Bevor Sie beginnen, sollten Sie berücksichtigen, dass Sie diese Aufgabe jetzt im Azure Resource Manager-Modell durchführen können. Wir empfehlen das Azure Resource Manager-Modell für neue Bereitstellungen. Weitere Informationen finden Sie unter [SQL Server Always On-Verfügbarkeitsgruppen auf virtuellen Azure-Computern](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT] 
> Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel erläutert die Verwendung des klassischen Bereitstellungsmodells. 

Informationen zum Durchführen dieser Aufgabe unter dem Azure Resource Manager-Modell finden Sie unter [SQL Server-Always On-Verfügbarkeitsgruppen auf virtuellen Azure-Computern](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

In diesem End-to-End-Tutorial erfahren Sie, wie Sie Verfügbarkeitsgruppen mithilfe von SQL Server Always On implementieren, das auf virtuellen Computern in Azure ausgeführt wird.

Am Ende des Tutorials besteht Ihre SQL Server AlwaysOn-Lösung in Azure aus folgenden Elementen:

* Ein virtuelles Netzwerk, das mehrere Subnetze enthält sowie ein Front-End- und eines Back-End-Subnetz umfasst
* Ein Domänencontroller mit einer Active Directory-Domäne (Azure AD)
* Zwei virtuelle Computer, auf denen SQL Server ausgeführt wird und die im Back-End-Subnetz bereitgestellt und der Azure AD-Domäne hinzugefügt wurden
* Ein Failovercluster aus drei Knoten mit dem Knotenmehrheit-Quorummodell
* Eine Verfügbarkeitsgruppe, die zwei Replikate einer Verfügbarkeitsdatenbank mit synchronem Commit aufweist

Die folgende Abbildung ist eine grafische Darstellung der Lösung.

![Test Lab-Architektur für Verfügbarkeitsgruppen in Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Beachten Sie, dass dies nur eine mögliche Konfiguration ist. Sie können z. B. die Anzahl der virtuellen Computer für eine Verfügbarkeitsgruppe mit zwei Replikaten minimieren. Diese Konfiguration spart Computestunden in Azure, indem der Domänencontroller als Freigabenzeuge der Quorumdatei in einem Cluster mit zwei Knoten verwendet wird. Diese Methode verringert die Anzahl der virtuellen Computer gegenüber der dargestellten Konfiguration um einen Computer.

In diesem Tutorial wird Folgendes vorausgesetzt:

* Sie besitzen bereits ein Azure-Abonnement.
* Sie wissen bereits, wie Sie mithilfe der GUI aus dem Katalog virtueller Computer einen klassischen virtuellen Computer bereitstellen, auf dem SQL Server ausgeführt wird.
* Sie verfügen bereits über solide Kenntnisse über Always On-Verfügbarkeitsgruppen. Weitere Informationen finden Sie unter [AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Informationen zur Verwendung von Always On-Verfügbarkeitsgruppen mit SharePoint finden Sie unter [Konfigurieren von SQL Server 2012 Always On-Verfügbarkeitsgruppen für SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).
> 
> 

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Erstellen des virtuellen Netzwerks und des Domänencontrollerservers
Sie beginnen mit einem neuen Azure-Testkonto. Nachdem Sie Ihr Konto eingerichtet haben, sollten Sie sich im Startbildschirm des klassischen Azure-Portals befinden.

1. Klicken Sie in der linken unteren Ecke der Seite auf die Schaltfläche **Neu**, wie auf dem folgenden Bildschirmfoto gezeigt.
   
    ![Klicken Sie im Portal auf "Neu"](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)
2. Klicken Sie auf **Network Services** > **Virtuelles Netzwerk** > **Benutzerdefiniert erstellen**, wie auf dem folgenden Bildschirmfoto gezeigt.
   
    ![Virtuelles Netzwerk erstellen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)
3. Erstellen Sie im Dialogfeld **VIRTUELLES NETZWERK ERSTELLEN** ein neues virtuelles Netzwerk, indem Sie nacheinander die Seiten mit den in der folgenden Tabelle aufgeführten Einstellungen durchlaufen. 
   
   | Seite | Einstellungen |
   | --- | --- |
   | Details zum virtuellen Netzwerk |**NAME = ContosoNET**<br/>**REGION = USA, Westen** |
   | DNS-Server und VPN-Konnektivität |Keine |
   | Adressräume des virtuellen Netzwerks |Die Einstellungen sind auf dem folgenden Bildschirmfoto zu sehen: ![Virtuelles Netzwerk erstellen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png) |
4. Erstellen Sie den virtuellen Computer, der als Domänencontroller (DC) verwendet werden soll. Klicken Sie auf **Neu** > **Compute** > **Virtueller Computer** > **Aus Katalog**, wie auf dem folgenden Bildschirmfoto gezeigt.
   
    ![Erstellen eines virtuellen Computers](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)
5. Konfigurieren Sie im Dialogfeld **VIRTUELLEN COMPUTER ERSTELLEN** einen neuen virtuellen Computer, indem Sie nacheinander die Seiten mit den in der folgenden Tabelle aufgeführten Einstellungen durchlaufen. 
   
   | Seite | Einstellungen |
   | --- | --- |
   | Betriebssystem des virtuellen Computers auswählen |Windows Server 2012 R2 Datacenter |
   | Konfiguration des virtuellen Computers |**VERÖFFENTLICHUNGSDATUM DER VERSION** = (neuestes)<br/>**NAME DES VIRTUELLEN COMPUTERS** = ContosoDC<br/>**STUFE** = STANDARD<br/>**GRÖSSE** = A2 (2 Kerne)<br/>**NEUER BENUTZERNAME** = AzureAdmin<br/>**NEUES KENNWORT** = Contoso!000<br/>**BESTÄTIGEN** = Contoso!000 |
   | Konfiguration des virtuellen Computers |**CLOUDDIENST** = einen neuen Clouddienst erstellen<br/>**DNS-NAME DES CLOUDDIENSTS** = ein eindeutiger Clouddienstname<br/>**DNS-NAME** = ein eindeutiger Name (z.B.: ContosoDC123)<br/>**REGION/AFFINITÄTSGRUPPE/VIRTUELLES NETZWERK** = ContosoNET<br/>**VIRTUELLES NETZWERK – SUBNETZE** = Back(10.10.2.0/24)<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = (Keine) |
   | Virtueller Computer - Optionen |Standardwerte verwenden |

Nachdem Sie den neuen virtuellen Computer konfiguriert haben, warten Sie, bis der virtuelle Computer bereitgestellt wurde. Dieser Vorgang nimmt einige Zeit in Anspruch. Wenn Sie im klassischen Azure-Portal auf die Registerkarte **Virtueller Computer** klicken, können Sie sehen, wie ContosoDC die Zustände von **Wird gestartet (Bereitstellung)** zu **Beendet**, **Wird gestartet**, **Wird ausgeführt (Bereitstellung)** und schließlich **Wird ausgeführt** durchläuft.

Der DC-Server ist jetzt erfolgreich bereitgestellt. Als Nächstes konfigurieren Sie die Active Directory-Domäne auf diesem DC-Server.

## <a name="configure-the-domain-controller"></a>Konfigurieren des Domänencontrollers
In den folgenden Schritten konfigurieren Sie den Computer "ContosoDC" als Domänencontroller für "corp.contoso.com".

1. Wählen Sie im Portal den Computer **ContosoDC** aus. Klicken Sie auf der Registerkarte **Dashboard** auf **Verbinden**, um eine RDP-Datei für den Remotedesktopzugriff zu öffnen.
   
    ![Herstellen einer Verbindung mit einem virtuellen Computer](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)
2. Melden Sie sich mit Ihrem konfigurierten Administratorkonto (**\AzureAdmin**) und Kennwort (**Contoso!000**) an.
3. Standardmäßig sollte das Dashboard **Server-Manager** angezeigt werden.
4. Klicken Sie im Dashboard auf den Link **Rollen und Features hinzufügen** .
   
    !["Rollen hinzufügen" in Server-Explorer](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)
5. Klicken Sie auf **Weiter**, bis Sie zum Abschnitt **Serverrollen** gelangen.
6. Wählen Sie die Rollen **Active Directory Domain Services** und **DNS-Server** aus. Fügen Sie weitere Features hinzu, die für diese Rollen erforderlich sind, wenn Sie dazu aufgefordert werden.
   
   > [!NOTE]
   > Sie erhalten eine Validierungswarnung, dass keine statische IP-Adresse vorhanden ist. Wenn Sie die Konfiguration testen, klicken Sie auf **Weiter**. In Produktionsszenarien [verwenden Sie PowerShell, um die statische IP-Adresse des Domänencontrollercomputers festzulegen](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Dialogfeld "Rollen hinzufügen"](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)
7. Klicken Sie auf **Weiter**, bis Sie zum Abschnitt **Bestätigung** gelangen. Aktivieren Sie das Kontrollkästchen **Zielserver bei Bedarf automatisch neu starten**.
8. Klicken Sie auf **Installieren**.
9. Nachdem die Installation der Features abgeschlossen ist, kehren Sie zum Dashboard **Server-Manager** zurück.
10. Wählen Sie die neue Option **AD DS** im linken Bereich aus.
11. Klicken Sie in der gelben Warnungsleiste auf den Link **Mehr** .
    
     ![Dialogfeld „AD DS“ auf virtuellem Computer für DNS-Server](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)
12. Klicken Sie im Dialogfeld **Alle Serveraufgabendetails** in der Spalte **Aktion** auf **Server zu einem Domänencontroller heraufstufen**.
13. Verwenden Sie im **Konfigurations-Assistenten für Active Directory-Domänendienste**die folgenden Werte:
    
    | Seite | Einstellung |
    | --- | --- |
    | Bereitstellungskonfiguration |**Neue Gesamtstruktur hinzufügen** = Aktiviert<br/>**Rootdomänenname** = corp.contoso.com |
    | Domänencontrolleroptionen |**Kennwort** = Contoso!000<br/>**Kennwort bestätigen** = Contoso!000 |
14. Klicken Sie auf **Weiter** , um die anderen Seiten des Assistenten zu durchlaufen. Vergewissern Sie sich, dass auf der Seite **Prüfung der erforderlichen Komponenten** die folgende Meldung angezeigt wird: **Alle erforderlichen Komponenten wurden erfolgreich überprüft.** Beachten Sie, dass Sie zwar alle zutreffenden Warnmeldungen überprüfen sollten, es aber möglich ist, die Installation fortzusetzen.
15. Klicken Sie auf **Installieren**. Der virtuelle Computer **ContosoDC** wird automatisch neu gestartet.

## <a name="configure-domain-accounts"></a>Konfigurieren von Domänenkonten
In den nächsten Schritten werden die Active Directory-Konten für die spätere Verwendung konfiguriert.

1. Melden Sie sich wieder bei dem Computer **ContosoDC** an.
2. Klicken Sie im **Server-Manager** auf **Tools** > **Active Directory-Verwaltungscenter**.
   
    ![Active Directory-Verwaltungscenter](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)
3. Wählen Sie im **Active Directory-Verwaltungscenter** im linken Bereich die Option **corp (lokal)** aus.
4. Klicken Sie im rechten Bereich **Aufgaben** auf **Neu** > **Benutzer**. Verwenden Sie folgende Einstellungen:
   
   | Einstellung | Wert |
   | --- | --- |
   | **Vorname** |Installieren |
   | **SamAccountName von Benutzer** |Installieren |
   | **Kennwort** |Contoso!000 |
   | **Kennwort bestätigen** |Contoso!000 |
   | **Andere Kennwortoptionen** |Aktiviert |
   | **Kennwort läuft nie ab** |Aktiviert |
5. Klicken Sie auf **OK**, um den **Installationsbenutzer** zu erstellen. Dieses Konto wird zum Konfigurieren des Failoverclusters und der Verfügbarkeitsgruppe verwendet.
6. Erstellen Sie mit der gleichen Vorgehensweise zwei weitere Benutzer: **CORP\SQLSvc1** und **CORP\SQLSvc2**. Diese Konten werden für SQL Server-Instanzen verwendet. Als Nächstes müssen Sie **CORP\Install** die erforderlichen Berechtigungen zum Konfigurieren des Windows-Failoverclustering erteilen.
7. Klicken Sie im **Active Directory-Verwaltungscenter** im linken Bereich auf die Option **corp (lokal)**. Klicken Sie dann rechts im **Aufgabenbereich** auf **Eigenschaften**.
   
    ![Eigenschaften des Benutzers CORP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)
8. Wählen Sie **Erweiterungen** aus, und klicken Sie auf der Registerkarte **Sicherheit** auf die Schaltfläche **Erweitert**.
9. Klicken Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für corp** auf **Hinzufügen**.
10. Klicken Sie auf **Prinzipal auswählen**, suchen Sie nach **CORP\Install** und klicken Sie dann auf **OK**.
11. Wählen Sie die Berechtigungen **Alle Eigenschaften lesen** und **Computerobjekte erstellen** aus.
    
     ![Berechtigungen des Benutzers CORP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)
12. Klicken Sie auf **OK** und dann erneut auf **OK**. Schließen Sie das Eigenschaftenfenster von corp.

Nachdem Sie nun die Konfiguration von Active Directory und den Benutzerobjekten abgeschlossen haben, erstellen drei virtuelle SQL Server-Computer und lassen sie dieser Domäne beitreten.

## <a name="create-the-sql-server-virtual-machines"></a>Erstellen Sie die virtuellen SQL Server-Computer.
Erstellen Sie drei virtuelle Computer. Einer ist für einen Clusterknoten und zwei sind für SQL Server vorgesehen. Um diese virtuellen Computer zu erstellen, kehren Sie zum klassischen Azure-Portal zurück. Klicken Sie auf **Neu** > **Compute** > **Virtueller Computer** > **Aus Katalog**. Verwenden Sie dann die Vorlagen aus der folgenden Tabelle, die Sie bei der Erstellung der virtuellen Computer unterstützen.

| Seite | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Betriebssystem des virtuellen Computers auswählen |**Windows Server 2012 R2 Datacenter** |**SQL Server 2014 RTM Enterprise** |**SQL Server 2014 RTM Enterprise** |
| Konfiguration des virtuellen Computers |**VERÖFFENTLICHUNGSDATUM DER VERSION** = (neuestes)<br/>**NAME DES VIRTUELLEN COMPUTERS** = ContosoWSFCNode<br/>**STUFE** = STANDARD<br/>**GRÖSSE** = A2 (2 Kerne)<br/>**NEUER BENUTZERNAME** = AzureAdmin<br/>**NEUES KENNWORT** = Contoso!000<br/>**BESTÄTIGEN** = Contoso!000 |**VERÖFFENTLICHUNGSDATUM DER VERSION** = (neuestes)<br/>**NAME DES VIRTUELLEN COMPUTERS** = ContosoSQL1<br/>**STUFE** = STANDARD<br/>**GRÖSSE** = A3 (4 Kerne)<br/>**NEUER BENUTZERNAME** = AzureAdmin<br/>**NEUES KENNWORT** = Contoso!000<br/>**BESTÄTIGEN** = Contoso!000 |**VERÖFFENTLICHUNGSDATUM DER VERSION** = (neuestes)<br/>**NAME DES VIRTUELLEN COMPUTERS** = ContosoSQL2<br/>**STUFE** = STANDARD<br/>**GRÖSSE** = A3 (4 Kerne)<br/>**NEUER BENUTZERNAME** = AzureAdmin<br/>**NEUES KENNWORT** = Contoso!000<br/>**BESTÄTIGEN** = Contoso!000 |
| Konfiguration des virtuellen Computers |**CLOUDDIENST** = zuvor erstellter eindeutiger DNS-Name des Clouddiensts (z.B.: ContosoDC123)<br/>**REGION/AFFINITÄTSGRUPPE/VIRTUELLES NETZWERK** = ContosoNET<br/>**VIRTUELLES NETZWERK – SUBNETZE** = Back(10.10.2.0/24)<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = Verfügbarkeitsgruppe erstellen<br/>**NAME DER VERFÜGBARKEITSGRUPPE** = SQLHADR |**CLOUDDIENST** = zuvor erstellter eindeutiger DNS-Name des Clouddiensts (z.B.: ContosoDC123)<br/>**REGION/AFFINITÄTSGRUPPE/VIRTUELLES NETZWERK** = ContosoNET<br/>**VIRTUELLES NETZWERK – SUBNETZE** = Back(10.10.2.0/24)<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = SQLHADR (Sie können die Verfügbarkeitsgruppe auch konfigurieren, nachdem der Computer erstellt wurde. Alle drei Computer sollten der Verfügbarkeitsgruppe SQLHADR zugewiesen werden.) |**CLOUDDIENST** = zuvor erstellter eindeutiger DNS-Name des Clouddiensts (z.B.: ContosoDC123)<br/>**REGION/AFFINITÄTSGRUPPE/VIRTUELLES NETZWERK** = ContosoNET<br/>**VIRTUELLES NETZWERK – SUBNETZE** = Back(10.10.2.0/24)<br/>**SPEICHERKONTO** = Automatisch generiertes Speicherkonto verwenden<br/>**VERFÜGBARKEITSGRUPPE** = SQLHADR (Sie können die Verfügbarkeitsgruppe auch konfigurieren, nachdem der Computer erstellt wurde. Alle drei Computer sollten der Verfügbarkeitsgruppe SQLHADR zugewiesen werden.) |
| Virtueller Computer - Optionen |Standardwerte verwenden |Standardwerte verwenden |Standardwerte verwenden |

<br/>

> [!NOTE]
> Die vorherige Konfiguration legt die Verwendung virtueller Computer mit STANDARD-Tarif nahe, da Computer mit BASIC-Tarif keine Endpunkte mit Lastenausgleich unterstützen. Sie benötigen Endpunkte mit Lastenausgleich später, um einen Listener für eine Verfügbarkeitsgruppe zu erstellen. Außerdem sind die hier vorgeschlagenen Computergrößen für das Testen von Verfügbarkeitsgruppen in virtuellen Azure-Computern vorgesehen. Die beste Leistung für Produktionsarbeitslasten finden Sie unter den Empfehlungen für die Größe und Konfiguration der SQL Server-Computer in [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](../sql/virtual-machines-windows-sql-performance.md).
> 
> 

Nachdem die drei virtuellen Computer vollständig bereitgestellt wurden, müssen Sie sie der Domäne **corp.contoso.com** hinzufügen und „CORP\Install“ Administratorrechte für die Computer gewähren. Verwenden Sie hierzu die folgenden Schritte für jeden der drei virtuellen Computer.

1. Ändern Sie zunächst die Adresse des bevorzugte DNS-Servers. Laden Sie die RDP-Datei jedes virtuellen Computers in Ihr lokales Verzeichnis herunter, indem Sie den virtuellen Computer in der Liste auswählen und dann auf die Schaltfläche **Verbinden** klicken. Klicken Sie zum Auswählen eines virtuellen Computers auf eine beliebige Stelle, jedoch nicht auf die erste Zelle in der Zeile, wie auf folgendem Bildschirmfoto gezeigt.
   
    ![Herunterladen der RDP-Datei](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)
2. Öffnen Sie die RDP-Datei, die Sie heruntergeladen haben, und melden Sie sich mit Ihrem konfigurierten Administratorkonto (**BUILTIN\AzureAdmin**) und dem Kennwort (**Contoso!000**) beim virtuellen Computer an.
3. Nachdem Sie angemeldet sind, sollte das Dashboard **Server-Manager** angezeigt werden. Klicken Sie im linken Bereich auf **Lokaler Server** .
4. Klicken Sie auf den Link **IPv4-Adresse wird per DHCP zugewiesen, IPv6-fähig**.
5. Klicken Sie im Dialogfeld **Netzwerkverbindungen** auf das Netzwerksymbol.
   
    ![Ändern des bevorzugten DNS-Servers des virtuellen Computers](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)
6. Klicken Sie auf der Befehlsleiste auf **Einstellungen dieser Verbindung ändern**. (Abhängig von der Größe Ihres Fensters müssen Sie möglicherweise auf den Doppelpfeil nach rechts klicken, um diesen Befehl anzuzeigen).
7. Wählen Sie **Internetprotokoll Version 4 (TCP/IPv4)** aus, und klicken Sie dann auf **Eigenschaften**.
8. Wählen Sie **Folgende DNS-Serveradressen verwenden** aus, und geben Sie dann im Feld **Bevorzugter DNS-Server** die Adresse **10.10.2.4** an.
9. Die Adresse **10.10.2.4** ist die einem virtuellen Computer im Subnetz „10.10.2.0/24“ zugewiesene Adresse in einem virtuellen Azure-Netzwerk. Dieser virtuelle Computer ist **ContosoDC**. Um die IP-Adresse von **ContosoDC** zu überprüfen, verwenden Sie **nslookup contosodc** im Befehlszeilenfenster, wie auf folgendem Bildschirmfoto gezeigt.
   
    ![Verwenden von NSLOOKUP zum Suchen der IP-Adresse für den Domänencontroller](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)
10. Klicken Sie auf **OK** > **Schließen**, um die Änderungen zu übernehmen. Sie können den virtuellen Computer jetzt zu **corp.contoso.com** hinzufügen.
11. Klicken Sie im Fenster **Lokaler Server** auf den Link **ARBEITSGRUPPE**.
12. Klicken Sie im Abschnitt **Computername** auf **Ändern**.
13. Aktivieren Sie das Kontrollkästchen **Domäne**, geben Sie in das Textfeld die Zeichenfolge **corp.contoso.com** ein, und klicken Sie dann auf **OK**.
14. Geben Sie im Dialogfeld **Windows-Sicherheit** die Anmeldeinformationen für das standardmäßige Domänenadministratorkonto (**CORP\AzureAdmin**) und das Kennwort (**Contoso!000**) an.
15. Wenn die Meldung „Willkommen in der Domäne ‚corp.contoso.com‘“ angezeigt wird, klicken Sie auf **OK**.
16. Klicken Sie im Dialogfeld auf **Schließen** > **Jetzt neu starten**.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-virtual-machine"></a>Fügen Sie auf jedem virtuellen Computer den Benutzer „Corp\Install“ als Administrator hinzu.
1. Warten Sie, bis der virtuelle Computer neu gestartet wurde, und öffnen Sie dann die RDP-Datei erneut, um sich beim virtuellen Computer mit dem Konto **BUILTIN\AzureAdmin** anzumelden.
2. Klicken Sie im **Server-Manager** auf **Tools** > **Computerverwaltung**.
   
    ![Computerverwaltung](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)
3. Erweitern Sie im Dialogfeld **Computerverwaltung** die Option **Lokale Benutzer und Gruppen**, und klicken Sie dann auf **Gruppen**.
4. Doppelklicken Sie auf die Gruppe **Administratoren** .
5. Klicken Sie im Dialogfeld **Administratoreigenschaften** auf die Schaltfläche **Hinzufügen**.
6. Geben Sie den Benutzer **CORP\Install** ein, und klicken Sie dann auf **OK**. Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, verwenden Sie das Konto **AzureAdmin** mit dem Kennwort **Contoso!000**.
7. Klicken Sie auf **OK**, um das Dialogfeld **Administratoreigenschaften** zu schließen.

### <a name="add-the-failover-clustering-feature-to-each-virtual-machine"></a>Hinzufügen des Features „Failoverclustering“ zu jedem virtuellen Computer
1. Klicken Sie im Dashboard **Server-Manager** auf **Rollen und Features hinzufügen**.
2. Klicken Sie im **Assistenten zum Hinzufügen von Rollen und Features** auf **Weiter**, bis Sie zur Seite **Features** gelangen.
3. Wählen Sie **Failoverclustering**aus. Wenn Sie dazu aufgefordert werden, fügen Sie alle weiteren, abhängigen Features hinzu.
   
    ![Hinzufügen des Features „Failoverclustering“ zum virtuellen Computer](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)
4. Klicken Sie auf **Weiter**, und klicken Sie dann auf der Seite **Bestätigung** auf **Installieren**.
5. Wenn die Installation des Features **Failoverclustering** abgeschlossen ist, klicken Sie auf **Schließen**.
6. Melden Sie sich bei dem virtuellen Computer ab.
7. Wiederholen Sie die Schritte in diesem Abschnitt für alle drei Server: **ContosoWSFCNode**, **ContosoSQL1** und **ContosoSQL2**.

Die virtuellen SQL Server-Computer sind jetzt bereitgestellt und werden ausgeführt, verfügen aber jeweils über die Standardoptionen für SQL Server.

## <a name="create-the-failover-cluster"></a>Erstellen des Failoverclusters
In diesem Abschnitt erstellen Sie den Failovercluster, der die später erstellte Verfügbarkeitsgruppe hosten soll. Mittlerweile sollten Sie Folgendes auf jedem der drei virtuellen Computer, die im Failovercluster verwendet werden sollen, erledigt haben:

* Vollständige Bereitstellung des virtuellen Computers in Azure
* Beitritt des virtuellen Computers zur Domäne
* Hinzufügen von **CORP\Install** zur lokalen Administratorgruppe
* Hinzufügen des Features „Failoverclustering“

All dies sind Voraussetzungen für jeden der virtuellen Computer, damit er dem Failovercluster hinzugefügt werden kann.

Beachten Sie außerdem, dass sich das virtuelle Azure-Netzwerk nicht auf dieselbe Weise wie ein lokales Netzwerk verhält. Sie müssen die Cluster in der folgenden Reihenfolge erstellen:

1. Erstellen Sie einen Einzelknotencluster auf einem Knoten (**ContosoSQL1**).
2. Ändern Sie die IP-Adresse des Clusters in eine nicht verwendete IP-Adresse (**10.10.2.101**).
3. Schalten Sie den Clusternamen online.
4. Fügen Sie die anderen Knoten hinzu (**ContosoSQL2** und **ContosoWSFCNode**).

Gehen Sie folgendermaßen vor, um die Aufgaben durchzuführen, mit denen der Cluster vollständig konfiguriert wird.

1. Öffnen Sie die RDP-Datei für **ContosoSQL1**, und melden Sie sich mit dem Domänenkonto **CORP\Install** an.
2. Klicken Sie im Dashboard **Server-Manager** auf **Tools** > **Failovercluster-Manager**.
3. Klicken Sie im linken Bereich mit der rechten Maustaste auf **Failovercluster-Manager**, und klicken Sie dann auf **Cluster erstellen**, wie auf dem folgenden Bildschirmfoto gezeigt.
   
    ![Cluster erstellen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)
4. Erstellen Sie im Clustererstellungs-Assistenten einen Cluster mit einem einzelnen Knoten, indem Sie die Seiten durchlaufen und dabei die Einstellungen aus der folgenden Tabelle verwenden:
   
   | Seite | Einstellungen |
   | --- | --- |
   | Voraussetzungen |Standardwerte verwenden |
   | Server auswählen |Geben Sie **ContosoSQL1** unter **Servernamen eingeben** ein, und klicken Sie auf **Hinzufügen**. |
   | Validierungswarnung |Wählen Sie **Nein. Microsoft-Support für diesen Cluster nicht nötig. Validierungstests nicht durchführen. Beim Klicken auf "Weiter" Erstellung des Clusters fortsetzen.** aus. |
   | Zugriffspunkt für die Clusterverwaltung |Geben Sie unter **Clustername** die Zeichenfolge **Cluster1** ein. |
   | Bestätigung |Standardeinstellungen verwenden, es sei denn, Sie verwenden Speicherplätze. Beachten Sie die Warnung im Anschluss an diese Tabelle. |
   
   > [!WARNING]
   > Bei Verwendung von [Speicherplätzen](https://technet.microsoft.com/library/hh831739), die mehrere Datenträger zu Speicherpools gruppieren, müssen Sie das Kontrollkästchen **Der gesamte geeignete Speicher soll dem Cluster hinzugefügt werden** auf der Seite **Bestätigung** deaktivieren. Wenn Sie diese Option nicht deaktivieren, werden die virtuellen Datenträger während des Clusteringprozesses getrennt. Als Folge daraus werden sie dann auch erst im Datenträger-Manager oder -Explorer angezeigt, nachdem die Speicherplätze aus dem Cluster entfernt und mithilfe von PowerShell erneut angefügt wurden.
   > 
   > 
5. Erweitern Sie im linken Bereich **Failovercluster-Manager**, und klicken Sie dann auf **Cluster1.corp.contoso.com**.
6. Scrollen Sie im mittleren Bereich nach unten bis zum Abschnitt **Hauptressourcen des Clusters**, und erweitern Sie die Details von **Name: Cluster1**. Die Ressource **Name** und **IP-Adresse** befinden sich im Zustand **Fehler**. Die IP-Adressressource kann nicht online geschaltet werden, da dem Cluster dieselbe IP-Adresse wie dem Computer selbst zugewiesen ist, wobei es sich damit um eine doppelte Adresse handelt.
7. Klicken Sie mit der rechten Maustaste auf die fehlerhafte Ressource **IP-Adresse**, und klicken Sie dann auf **Eigenschaften**.
   
    ![Clustereigenschaften](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)
8. Wählen Sie **Statische IP-Adresse** aus, geben Sie in das Textfeld **Adresse** den Wert **10.10.2.101** ein, und klicken Sie dann auf **OK**.
9. Klicken Sie im Abschnitt **Hauptressourcen des Clusters** mit der rechten Maustaste auf **Name: Cluster1**, und klicken Sie dann auf **Online schalten**. Warten Sie, bis beide Ressourcen online sind. Wenn die Clusternamensressource online ist, wird der DC-Server mit einem neuen Active Directory-Computerkonto aktualisiert. Dieses Active Directory-Konto wird später zum Ausführen des Clusterdiensts der Verfügbarkeitsgruppe verwendet.
10. Fügen Sie die übrigen Knoten dem Cluster hinzu. Klicken Sie in der Browserstruktur mit der rechten Maustaste auf **Cluster1.corp.contoso.com**, und klicken Sie dann auf **Knoten hinzufügen**, wie auf dem folgenden Bildschirmfoto dargestellt.
    
     ![Hinzufügen von Knoten zum Cluster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)
11. Klicken Sie im **Assistenten „Knoten hinzufügen“** auf **Weiter**, fügen Sie auf der Seite **Server auswählen** die Namen **ContosoSQL2** und **ContosoWSFCNode** zur Liste hinzu, indem Sie im Feld **Servernamen eingeben** den Servernamen eingeben und dann auf **Hinzufügen** klicken. Wenn Sie fertig sind, klicken Sie auf **Weiter**.
12. Klicken Sie auf der Seite **Validierungswarnung** auf **Nein**, obwohl Sie in einem Produktionsszenario entsprechende Validierungstests ausführen sollten. Klicken Sie auf **Weiter**.
13. Klicken Sie auf der Seite **Bestätigung** auf **Weiter**, um die Knoten hinzuzufügen.
    
    > [!WARNING]
    > Bei Verwendung von [Speicherplätzen](https://technet.microsoft.com/library/hh831739), die mehrere Datenträger zu Speicherpools gruppieren, müssen Sie das Kontrollkästchen **Der gesamte geeignete Speicher soll dem Cluster hinzugefügt werden** deaktivieren. Wenn Sie diese Option nicht deaktivieren, werden die virtuellen Datenträger während des Clusteringprozesses getrennt. Als Folge daraus werden sie dann auch erst im Datenträger-Manager oder -Explorer angezeigt, nachdem die Speicherplätze aus dem Cluster entfernt und mithilfe der PowerShell erneut angefügt wurden.
    > 
    > 
14. Nachdem die Knoten dem Cluster hinzugefügt wurden, klicken Sie auf **Fertig stellen**. Im Failovercluster-Manager sollte nun angezeigt werden, dass Ihr Cluster über drei Knoten verfügt, und diese sollten im Container **Knoten** aufgelistet werden.
15. Melden Sie sich bei der Remotedesktopsitzung ab.

## <a name="prepare-the-sql-server-instances-for-availability-groups"></a>Vorbereiten der SQL Server-Instanzen für die Verfügbarkeitsgruppe
In diesem Abschnitt führen Sie die folgenden Aktionen sowohl mit **ContosoSQL1** als auch mit **ContosoSQL2** durch:

* Hinzufügen einer Anmeldung für **NT-AUTORITÄT\System** mit Festlegung der erforderlichen Berechtigungen auf der Standardinstanz von SQL Server
* Hinzufügen von **CORP\Install** zur Standardinstanz von SQL Server als sysadmin-Rolle
* Öffnen Sie die Firewall für den Remotezugriff auf SQL Server.
* Aktivieren des Features „Always On-Verfügbarkeitsgruppen“
* Ändern des SQL Server-Dienstkontos in **CORP\SQLSvc1** bzw. **CORP\SQLSvc2**

Diese Aktionen können in beliebiger Reihenfolge ausgeführt werden. Dennoch werden die folgenden Schritte in der vorgegebenen Reihenfolge durchlaufen. Führen Sie die Schritte sowohl für **ContosoSQL1** als auch für **ContosoSQL2** aus:

1. Wenn Sie sich noch nicht von der Remotedesktopsitzung für den virtuellen Computer abgemeldet haben, tun Sie dies jetzt.
2. Öffnen Sie die RDP-Dateien für **ContosoSQL1** und **ContosoSQL2**, und melden Sie sich als **BUILTIN\AzureAdmin** an.
3. Fügen Sie **NT-AUTORITÄT\System** zu den SQL Server-Anmeldungen mit den erforderlichen Berechtigungen hinzu. Öffnen Sie **SQL Server Management Studio**.
4. Klicken Sie auf **Verbinden** , um eine Verbindung mit der Standardinstanz von SQL Server herzustellen.
5. Erweitern Sie im **Objekt-Explorer** den Eintrag **Sicherheit**, und erweitern Sie dann **Anmeldungen**.
6. Klicken Sie mit der rechten Maustaste auf die Anmeldung **NT-AUTORITÄT\System**, und klicken Sie dann auf **Eigenschaften**.
7. Wählen Sie auf der Seite **Sicherungsfähige Elemente** für den lokalen Server für die folgenden Berechtigungen die Option **Erteilen** aus, und klicken Sie dann auf **OK**.
   
   * Beliebige Verfügbarkeitsgruppe ändern
   * SQL verbinden
   * Serverstatus anzeigen
8. Fügen Sie **CORP\Install** als **sysadmin**-Rolle zur Standardinstanz von SQL Server hinzu. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf **Anmeldungen**, und klicken Sie dann auf **Neue Anmeldung**.
9. Geben Sie **CORP\Install** in das Feld **Anmeldename** ein.
10. Wählen Sie auf der Seite **Serverrollen** die Option **Sysadmin** aus, und klicken Sie dann auf **OK**. Nachdem Sie die Anmeldung erstellt haben, können Sie sie anzeigen, indem Sie **Anmeldungen** in **Objekt-Explorer** erweitern.
11. Öffnen Sie auf dem **Startbildschirm** die Option **Windows-Firewall mit erweiterter Sicherheit**, um eine Firewallregel für SQL Server zu erstellen.
12. Klicken Sie im linken Bereich auf **Eingehende Regeln**. Klicken Sie im rechten Bereich auf **Neue Regel**.
13. Klicken Sie auf der Seite **Regeltyp** auf **Programm** > **Weiter**.
14. Wählen Sie auf der Seite **Programm** die Option **Dieser Programmpfad** aus. Geben Sie im Textfeld **%ProgramFiles%\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Binn\sqlservr.exe** ein, und klicken Sie dann auf **Weiter**. Wenn Sie diese Anleitung befolgen, aber SQL Server 2012 verwenden, lautet das SQL Server-Verzeichnis **MSSQL11.MSSQLSERVER**.
15. Lassen Sie auf der Seite **Aktion** die Option **Verbindung zulassen** aktiviert, und klicken Sie dann auf **Weiter**.
16. Akzeptieren Sie auf der Seite **Profil** die Standardeinstellungen, und klicken Sie dann auf **Weiter**.
17. Geben Sie auf der Seite **Name** im Textfeld **Name** einen Regelnamen an (Beispiel: **SQL Server (Programmregel)**), und klicken Sie anschließend auf **Fertig stellen**.
18. Öffnen Sie zum Aktivieren des Features **Always On-Verfügbarkeitsgruppe** auf dem **Startbildschirm** die Option **SQL Server-Konfigurations-Manager**.
19. Klicken Sie in der Browserstruktur auf **SQL Server-Dienste**, klicken Sie dann mit der rechten Maustaste auf den Dienst **SQL Server (MSSQLSERVER)**, und klicken Sie anschließend auf **Eigenschaften**.
20. Klicken Sie auf die Registerkarte **Hohe Verfügbarkeit mit Always On**, wählen Sie **Always On-Verfügbarkeitsgruppen aktivieren** aus (wie auf dem folgenden Bildschirmfoto gezeigt), und klicken Sie dann auf **Übernehmen**. Klicken Sie im Dialogfeld auf **OK**, und lassen Sie das **Eigenschaftenfenster** noch geöffnet. Nachdem Sie das Dienstkonto geändert haben, starten Sie den SQL Server-Dienst neu.
    
     ![Aktivieren von AlwaysOn-Verfügbarkeitsgruppen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)
21. Klicken Sie zum Ändern des SQL Server-Dienstkontos auf die Registerkarte **Anmelden**, und geben Sie **CORP\SQLSvc1** (für **ContosoSQL1**) oder **CORP\SQLSvc2** (für **ContosoSQL2**) in **Kontoname** ein. Geben Sie anschließend das Kennwort ein, und bestätigen Sie es, und klicken Sie dann auf **OK**.
22. Klicken Sie im sich öffnenden Dialogfeld auf **Ja**, um den SQL Server-Dienst neu zu starten. Nach dem Neustart des SQL Server-Diensts sind die Änderungen, die Sie im Dialogfeld **Eigenschaften** vorgenommen haben, wirksam
23. Melden Sie sich bei den virtuellen Computern ab.

## <a name="create-the-availability-group"></a>Erstellen der Verfügbarkeitsgruppe
Sie sind jetzt bereit, um eine Verfügbarkeitsgruppe zu konfigurieren. Im Folgenden finden Sie einen Überblick der vorzunehmenden Aktionen:

* Erstellen einer neuen Datenbank (**MyDB1**) auf **ContosoSQL1**
* Erstellen sowohl einer vollständigen Sicherung als auch einer Transaktionsprotokollsicherung der Datenbank
* Wiederherstellen der vollständigen Sicherung und der Protokollsicherung auf **ContosoSQL2** mit der Option **NORECOVERY**
* Erstellen der Verfügbarkeitsgruppe (**AG1**) mit synchronem Commit, automatischem Failover und lesbaren, sekundären Replikaten

### <a name="create-the-mydb1-database-on-contososql1"></a>Erstellen der Datenbank „MyDB1“ auf „ContosoSQL1“
1. Wenn Sie sich noch nicht von den Remotedesktopsitzungen für **ContosoSQL1** und **ContosoSQL2** abgemeldet haben, tun Sie dies jetzt.
2. Öffnen Sie die RDP-Datei für **ContosoSQL1**, und melden Sie sich als **CORP\Install** an.
3. Erstellen Sie im **Datei-Explorer** unter **C:\\** ein Verzeichnis namens **backup**. Dieses Verzeichnis verwenden Sie zum Sichern und Wiederherstellen Ihrer Datenbank.
4. Klicken Sie mit der rechten Maustaste auf das neue Verzeichnis, zeigen Sie auf **Freigeben für**, und klicken Sie dann auf **Bestimmte Personen**, wie auf dem folgenden Bildschirmfoto gezeigt.
   
    ![Erstellen eines Sicherungsordners](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)
5. Fügen Sie **CORP\SQLSvc1** hinzu, und erteilen Sie dafür die Berechtigung **Lesen/Schreiben**. Fügen Sie anschließend **CORP\SQLSvc2** hinzu, und erteilen Sie dafür die Berechtigung **Lesen**, wie auf dem folgenden Bildschirmfoto gezeigt, und klicken Sie dann auf **Freigeben**. Nachdem der Dateifreigabeprozess abgeschlossen ist, klicken Sie auf **Fertig**.
   
    ![Erteilen von Berechtigungen für den Sicherungsordner](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)
6. Starten Sie zum Erstellen der Datenbank die Option **SQL Server Management Studio** über das **Startmenü**, und klicken Sie dann auf **Verbinden**, um eine Verbindung mit der Standardinstanz von SQL Server herzustellen.
7. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf **Datenbanken**, und klicken Sie dann auf **Neue Datenbank**.
8. Geben Sie unter **Datenbankname** den Namen **MyDB1** ein, und klicken Sie dann auf **OK**.

### <a name="make-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Erstellen einer vollständigen Sicherung von MyDB1 und die anschließende Wiederherstellung auf ContosoSQL2
1. Erweitern Sie zum Erstellen einer vollständigen Sicherung der Datenbank im **Objekt-Explorer** den Eintrag **Datenbanken**, klicken Sie mit der rechten Maustaste auf **MyDB1**, zeigen Sie auf **Aufgaben**, und klicken Sie dann auf **Sichern**.
2. Lassen Sie im Abschnitt **Quelle** die Option **Sicherungstyp** auf **Vollständig** festgelegt. Klicken Sie im Abschnitt **Ziel** auf **Entfernen**, um den Standarddateipfad der Sicherungsdatei zu entfernen.
3. Klicken Sie im Abschnitt **Ziel** auf **Hinzufügen**.
4. Geben Sie in das Textfeld **Dateiname** den Wert **\\ContosoSQL1\backup\MyDB1.bak** ein. Klicken Sie dann auf **OK**, und klicken Sie anschließend wieder auf **OK**, um die Datenbank zu sichern. Wenn der Sicherungsvorgang abgeschlossen ist, klicken Sie wieder auf **OK**, um das Dialogfeld zu schließen.
5. Erweitern Sie zum Erstellen einer Transaktionsprotokollsicherung der Datenbank im **Objekt-Explorer** den Eintrag **Datenbanken**, klicken Sie mit der rechten Maustaste auf **MyDB1**, zeigen Sie auf **Aufgaben**, und klicken Sie dann auf **Sichern**.
6. Wählen Sie unter **Sicherungstyp** die Option **Transaktionsprotokoll** aus. Behalten Sie für den Dateipfad **Ziel** den von Ihnen zuvor angegebenen Pfad bei, und klicken Sie dann auf **OK**. Wenn der Sicherungsvorgang abgeschlossen ist, klicken Sie wieder auf **OK**.
7. Öffnen Sie zum Wiederherstellen der vollständigen sowie der Transaktionsprotokollsicherungen auf **ContosoSQL2** die RDP-Datei für **ContosoSQL2**, und melden Sie sich als **CORP\Install** an. Lassen Sie die Remotedesktopsitzung für **ContosoSQL1** geöffnet.
8. Öffnen Sie **SQL Server Management Studio** über das **Startmenü**, und klicken Sie dann auf **Verbinden**, um eine Verbindung mit der Standardinstanz von SQL Server herzustellen.
9. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf **Datenbanken**, und klicken Sie dann auf **Datenbank wiederherstellen**.
10. Wählen Sie im Abschnitt **Quelle** die Option **Gerät** aus, und klicken Sie dann auf die Schaltfläche mit den Auslassungspunkten (**...**) .
11. Klicken Sie unter **Sicherungsmedien auswählen** auf **Hinzufügen**.
12. Geben Sie in **Speicherort der Sicherungsdatei** den Pfad **\\ContosoSQL1\backup** ein, klicken Sie auf **Aktualisieren**. Wählen Sie dann **MyDB1.bak** aus, klicken Sie auf **OK**, und klicken Sie anschließend erneut auf **OK**. Im Abschnitt **Wiederherzustellende Sicherungssätze** sollten jetzt die vollständige Sicherung und die Protokollsicherung angezeigt werden.
13. Wechseln Sie zur Seite **Optionen**, wählen Sie in **Wiederherstellungsstatus** die Option **MIT NORECOVERY WIEDERHERSTELLEN** aus, und klicken Sie dann auf **OK**, um die Datenbank wiederherzustellen. Wenn die Wiederherstellung abgeschlossen ist, klicken Sie auf **OK**.

### <a name="create-the-availability-group"></a>Erstellen der Verfügbarkeitsgruppe
1. Wechseln Sie zurück zur Remotedesktopsitzung für **ContosoSQL1**. Klicken Sie im **Objekt-Explorer** in SQL Server Management Studio mit der rechten Maustaste auf **Hohe Verfügbarkeit mit Always On**, und klicken Sie dann auf **Assistent für neue Verfügbarkeitsgruppen**, wie auf dem folgenden Bildschirmfoto gezeigt.
   
    ![Starten des Assistenten für neue Verfügbarkeitsgruppen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)
2. Klicken Sie auf der Seite **Einführung** auf **Weiter**. Geben Sie auf der Seite **Namen der Verfügbarkeitsgruppe angeben** den Namen **AG1** in das Feld **Name der Verfügbarkeitsgruppe** ein, und klicken Sie dann erneut auf **Weiter**.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Namen der Verfügbarkeitsgruppe angeben](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)
3. Wählen Sie auf der Seite **Datenbanken auswählen** den Eintrag **MyDB1** aus, und klicken Sie dann auf **Weiter**. Die Datenbank erfüllt die Voraussetzungen für eine Verfügbarkeitsgruppe, weil Sie mindestens eine vollständige Sicherung auf dem vorgesehenen primären Replikat erstellt haben.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Datenbanken auswählen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)
4. Klicken Sie auf der Seite **Replikate angeben** auf **Replikat hinzufügen**.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Replikate angeben](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)
5. Geben Sie im Dialogfeld **Verbindung mit Server herstellen** die Zeichenfolge **ContosoSQL2** in **Servername** ein, und klicken Sie dann auf **Verbinden**.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Verbindung mit Server herstellen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)
6. Auf der Seite **Replikate angeben** sollte jetzt **ContosoSQL2** unter **Verfügbare Replikate** aufgelistet werden. Konfigurieren Sie die Replikate, wie auf dem folgenden Bildschirmfoto gezeigt. Wenn Sie fertig sind, klicken Sie auf **Weiter**.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Replikate angeben (vollständig)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)
7. Wählen Sie auf der Seite **Anfängliche Datensynchronisierung auswählen** die Option **Nur verknüpfen** aus, und klicken Sie dann auf **Weiter**. Sie haben bereits manuell eine Datensynchronisierung ausgeführt, als Sie die vollständige Sicherung und die Transaktionssicherung auf **ContosoSQL1** erstellt und diese auf **ContosoSQL2** wiederhergestellt haben. Alternativ können Sie sich entscheiden, die Sicherungs- und Wiederherstellungsvorgänge nicht mit Ihrer Datenbank durchzuführen, und **Vollständig** auswählen, damit der „Assistent für neue Verfügbarkeitsgruppen“ die Datensynchronisierung für Sie ausführt. Von dieser Option wird jedoch bei sehr großen Datenbanken, wie sie in manchen Unternehmen vorhanden sind, abgeraten.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Anfängliche Datensynchronisierung auswählen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)
8. Klicken Sie auf der Seite **Validierung** auf **Weiter**. Die Seite sollte in etwa dem folgenden Bildschirmfoto entsprechen. Es liegt eine Warnung für die Listenerkonfiguration vor, weil Sie keinen Listener für die Verfügbarkeitsgruppe konfiguriert haben. Diese Warnung können Sie ignorieren, weil in diesem Tutorial kein Listener konfiguriert wird. Informationen zum Konfigurieren des Listeners nach Abschluss dieses Tutorials finden Sie unter [Konfigurieren eines ILB-Listeners für Always On-Verfügbarkeitsgruppen in Azure](../classic/ps-sql-int-listener.md).
   
    ![Assistent für neue Verfügbarkeitsgruppen, Validierung](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)
9. Klicken Sie auf der Seite **Zusammenfassung** auf **Fertig stellen**, und warten Sie dann, während der Assistent die neue Verfügbarkeitsgruppe konfiguriert. Auf der Seite **Status** können Sie auf **Weitere Details** klicken, um den detaillierten Status anzuzeigen. Überprüfen Sie nach Abschluss des Assistenten die Seite **Ergebnisse**, um sich zu vergewissern, dass die Verfügbarkeitsgruppe erfolgreich erstellt wurde (wie auf dem folgenden Bildschirmfoto gezeigt), und klicken Sie dann auf **Schließen**, um den Assistenten zu beenden.
   
    ![Assistent für neue Verfügbarkeitsgruppen, Ergebnisse](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)
10. Erweitern Sie im **Objekt-Explorer** den Eintrag **Hohe Verfügbarkeit mit Always On**, und erweitern Sie dann **Verfügbarkeitsgruppen**. Sie sollten jetzt die neue Verfügbarkeitsgruppe in diesem Container sehen können. Klicken Sie mit der rechten Maustaste auf **AG1 (primär)**, und klicken Sie dann auf **Dashboard anzeigen**.
    
     ![Dashboard „Verfügbarkeitsgruppe anzeigen“](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)
11. Ihr **Always On-Dashboard** sollte in etwa wie auf dem folgenden Bildschirmfoto aussehen. Es werden die Replikate, der Failovermodus jedes Replikats und der Synchronisierungsstatus angezeigt.
    
     ![Dashboard „Verfügbarkeitsgruppe“](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)
12. Kehren Sie zum **Server-Manager** zurück, klicken Sie auf **Tools**, und öffnen Sie dann den **Failovercluster-Manager**.
13. Erweitern Sie **Cluster1.corp.contoso.com**, und erweitern Sie dann **Dienste und Anwendungen**. Wählen Sie **Rollen** aus, und beachten Sie, dass die Verfügbarkeitsgruppenrolle **AG1** erstellt wurde. Beachten Sie, dass AG1 keine IP-Adresse besitzt, über die Datenbankclients eine Verbindung mit der Verfügbarkeitsgruppe herstellen können, weil Sie keinen Listener konfiguriert haben. Sie können direkt mit dem primären Knoten eine Verbindung herstellen, um Lese-/ Schreibvorgänge auszuführen, und mit dem sekundären Knoten für reine Leseabfragen.
    
     ![Verfügbarkeitsgruppe im Failovercluster-Manager](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

> [!WARNING]
> Versuchen Sie nicht, ein Failover der Verfügbarkeitsgruppe aus dem Failovercluster-Manager heraus durchzuführen. Alle Failovervorgänge sollten über das **Always On-Dashboard** in SQL Server Management Studio ausgeführt werden. Weitere Informationen finden Sie unter [Restrictions on Using The Failover Cluster Manager with Availability Groups](https://msdn.microsoft.com/library/ff929171.aspx) (Einschränkungen für die Verwendung des Failovercluster-Managers mit Verfügbarkeitsgruppen).
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun erfolgreich SQL Server AlwaysOn implementiert, indem Sie eine Verfügbarkeitsgruppe in Azure erstellt haben. Informationen zum Konfigurieren eines Listeners für diese Verfügbarkeitsgruppe finden Sie unter [Konfigurieren eines ILB-Listeners für Always On-Verfügbarkeitsgruppen in Azure](../classic/ps-sql-int-listener.md).

Weitere Informationen zur Verwendung von SQL Server in Azure finden Sie unter [SQL Server auf virtuellen Azure-Computern](../sql/virtual-machines-windows-sql-server-iaas-overview.md).


