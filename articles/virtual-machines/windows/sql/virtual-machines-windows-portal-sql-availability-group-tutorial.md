---
title: "SQL Server-Verfügbarkeitsgruppen – Azure Virtual Machines – Tutorial | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie, wie Sie eine SQL Server Always On-Verfügbarkeitsgruppe in Azure Virtual Machines erstellen."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/10/2017
ms.author: mikeray
translationtype: Human Translation
ms.sourcegitcommit: 5e41a20f563eab6b236eaa6eaf0ce1d20ebfa493
ms.openlocfilehash: d8982dda38df92c94e7dac4b5a1cf451bab3a5ce


---

# <a name="create-always-on-availability-group-to-improve-availability-and-disaster-recovery"></a>Erstellen einer AlwaysOn-Verfügbarkeitsgruppe zur Verbesserung der Verfügbarkeit und Notfallwiederherstellung

In diesem Tutorial erfahren Sie, wie Sie eine SQL Server Always On-Verfügbarkeitsgruppe in Azure Virtual Machines erstellen. Im Rahmen des vollständigen Tutorials wird eine Verfügbarkeitsgruppe mit einem Datenbankreplikat in zwei SQL Server-Instanzen erstellt.

**Voraussichtliche Dauer:** Etwa 30 Minuten nach Abschluss der erforderlichen Vorbereitungen.

Das Diagramm veranschaulicht, was Sie im Rahmen dieses Tutorials erstellen.

![Verfügbarkeitsgruppe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial werden Grundkenntnisse über SQL Server AlwaysOn-Verfügbarkeitsgruppen vorausgesetzt. Weitere Informationen finden Sie in der [Übersicht über Always On-Verfügbarkeitsgruppen (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).

Die folgende Tabelle gibt Aufschluss über die Voraussetzungen, die erfüllt sein müssen, bevor Sie mit dem Tutorial beginnen:

|  |Anforderung |Beschreibung |
|----- |----- |----- |
|![Quadrat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | Zwei SQL Server-Instanzen | - In einer Azure-Verfügbarkeitsgruppe <br/> - In einer einzelnen Domäne <br/> - Mit installiertem Failoverclustering-Feature |
|![Quadrat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| Windows Server | Dateifreigabe für Clusterzeuge |  
|![Quadrat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server-Dienstkonto | Domänenkonto |
|![Quadrat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server-Agent-Dienstkonto | Domänenkonto |  
|![Quadrat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Geöffnete Firewallports | - SQL Server: **1433** für Standardinstanz <br/> - Datenbankspiegelungsendpunkt: **5022** oder ein anderer verfügbarer Port <br/> - Azure Load Balancer-Test: **59999** oder ein anderer verfügbarer Port |
|![Quadrat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Hinzufügen des Failoverclustering-Features | Dieses Feature wird von beiden SQL Server-Instanzen benötigt. |
|![Quadrat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Domänenkonto für die Installation | - Lokaler Administrator in jeder SQL Server-Instanz <br/> - Mitglied der festen SQL Server-Serverrolle „SysAdmin“ für jede Instanz von SQL Server  |


Vor Beginn des Tutorials müssen die [Schritte zum Erfüllen der Voraussetzungen für die Erstellung von AlwaysOn-Verfügbarkeitsgruppen in Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md) ausgeführt werden. Falls die Voraussetzungen bereits erfüllt werden, können Sie direkt mit dem [Erstellen des Clusters](#CreateCluster) fortfahren.


<!--**Procedure**: *This is the first “step”. Make titles H2’s and short and clear – H2’s appear in the right pane on the web page and are important for navigation.*-->

<a name="CreateCluster"></a>
## Cluster erstellen

Wenn die Voraussetzungen erfüllt sind, müssen Sie zunächst einen Windows Server-Failovercluster mit zwei SQL Server-Instanzen und einem Zeugenserver erstellen.  

1. Stellen Sie eine RDP-Verbindung mit der ersten SQL Server-Instanz her. Verwenden Sie dabei ein Domänenkonto, das in beiden SQL Server-Instanzen sowie auf dem Zeugenserver über Administratorberechtigungen verfügt.

   >[!TIP]
   >Wenn Sie sich an das [Dokument mit den Voraussetzungen](virtual-machines-windows-portal-sql-availability-group-prereq.md) gehalten haben, haben Sie ein Konto namens **CORP\Install** erstellt. Verwenden Sie dieses Konto.

2. Wählen Sie im Dashboard **Server-Manager** die Option **Tools** aus, und klicken Sie dann auf **Failovercluster-Manager**.
3. Klicken Sie im linken Bereich mit der rechten Maustaste auf **Failovercluster-Manager**, und klicken Sie anschließend auf **Cluster erstellen**.
   ![Cluster erstellen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)
4. Erstellen Sie im Clustererstellungs-Assistenten einen Cluster mit einem einzelnen Knoten, indem Sie die Seiten durchlaufen und dabei die Einstellungen aus der folgenden Tabelle verwenden:

   | Seite | Einstellungen |
   | --- | --- |
   | Voraussetzungen |Standardwerte verwenden |
   | Server auswählen |Geben Sie unter **Servernamen eingeben** den Namen der ersten SQL Server-Instanz ein, und klicken Sie auf **Hinzufügen**. |
   | Validierungswarnung |Wählen Sie **Nein. Microsoft-Support für diesen Cluster nicht nötig. Validierungstests nicht durchführen. Klicken Sie auf „Weiter“, um das Erstellen des Clusters fortzusetzen**. |
   | Zugriffspunkt für die Clusterverwaltung |Geben Sie unter **Clustername** einen Clusternamen ein (beispielsweise **SQLAGCluster1**).|
   | Bestätigung |Standardeinstellungen verwenden, es sei denn, Sie verwenden Speicherplätze. Siehe den Hinweis im Anschluss an diese Tabelle. |

### <a name="set-the-cluster-ip-address"></a>Festlegen der Cluster-IP-Adresse

1. Scrollen Sie im **Failovercluster-Manager** nach unten bis zu **Hauptressourcen des Clusters**, und erweitern Sie die Clusterdetails. Die Ressource **Name** und **IP-Adresse** befinden sich im Zustand **Fehler**. Die IP-Adressressource kann nicht online geschaltet werden, da dem Cluster die gleiche IP-Adresse zugewiesen ist wie dem Computer selbst. Es liegt also eine doppelte Adresse vor.

2. Klicken Sie mit der rechten Maustaste auf die fehlerhafte Ressource **IP-Adresse**, und klicken Sie dann auf **Eigenschaften**.

   ![Eigenschaften des Clusters](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. Wählen Sie **Statische IP-Adresse** aus, und geben Sie im Textfeld für die Adresse eine verfügbare Adresse aus dem Subnetz an, in dem sich die SQL Server-Instanz befindet. Klicken Sie dann auf **OK**.
4. Klicken Sie im Abschnitt **Hauptressourcen des Clusters** mit der rechten Maustaste auf den Clusternamen, und klicken Sie anschließend auf **Online schalten**. Warten Sie dann, bis beide Ressourcen online sind. Wenn die Clusternamensressource online ist, aktualisiert sie den DC-Server mit einem neuen AD-Computerkonto. Verwenden Sie dieses AD-Konto zum späteren Ausführen des Clusterdiensts der Verfügbarkeitsgruppe.

### <a name="a-nameaddnodeaadd-the-other-sql-server-to-cluster"></a><a name="addNode"></a>Hinzufügen der anderen SQL Server-Instanz zum Cluster

Fügen Sie die andere SQL Server-Instanz dem Cluster hinzu.

1. Klicken Sie in der Navigationsstruktur mit der rechten Maustaste auf den Cluster, und klicken Sie auf **Knoten hinzufügen**.

    ![Hinzufügen eines Knotens zum Cluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. Klicken Sie im **Assistenten „Knoten hinzufügen“** auf **Weiter**. Fügen Sie auf der Seite **Server auswählen** die zweite SQL Server-Instanz hinzu. Geben Sie unter **Servernamen eingeben** den Namen des Servers ein, und klicken Sie auf **Hinzufügen**. Wenn Sie fertig sind, klicken Sie auf **Weiter**.

1. Klicken Sie auf der Seite **Validierungswarnung** auf **Nein**. (In einem Produktionsszenario sollten Sie die Validierungstests ausführen.) Klicken Sie auf **Weiter**.

8. Deaktivieren Sie bei Verwendung von Speicherplätzen das Kontrollkästchen **Der gesamte geeignete Speicher soll dem Cluster hinzugefügt werden.** auf der Seite **Bestätigung**.

   ![Bestätigung der Knotenhinzufügung](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   >Wenn Sie Speicherplätze verwenden und das Kontrollkästchen **Der gesamte geeignete Speicher soll dem Cluster hinzugefügt werden.** nicht deaktivieren, trennt Windows die virtuellen Datenträger während des Clusterprozesses. Sie werden daher erst im Datenträger-Manager oder -Explorer angezeigt, nachdem die Speicherplätze aus dem Cluster entfernt und mithilfe von PowerShell erneut angefügt wurden. Mit Speicherplätzen werden mehrere Datenträger in Speicherpools gruppiert. Weitere Informationen finden Sie unter [Speicherplätze](https://technet.microsoft.com/library/hh831739).

1. Klicken Sie auf **Weiter**.

1. Klicken Sie auf **Fertig stellen**.

   Im Failovercluster-Manager sehen Sie nun, dass Ihr Cluster über einen neuen Knoten verfügt, und der Knoten wird im Container **Knoten** aufgelistet.

10. Melden Sie sich bei der Remotedesktopsitzung ab.

### <a name="add-a-cluster-quorum-file-share"></a>Hinzufügen einer Clusterquorum-Dateifreigabe

In diesem Beispiel erstellt der Windows-Cluster mithilfe einer Dateifreigabe ein Clusterquorum. In diesem Tutorial wird ein Quorum vom Typ „Knoten- und Dateifreigabemehrheit“ verwendet. Weitere Informationen finden Sie unter [Grundlegendes zu Quorumkonfigurationen in einem Failovercluster](http://technet.microsoft.com/library/cc731739.aspx).

1. Stellen Sie über eine Remotedesktopsitzung eine Verbindung mit dem Dateifreigabenzeuge-Mitgliedsserver her.

1. Klicken Sie im **Server-Manager** auf **Tools**. Öffnen Sie **Computerverwaltung**.

1. Klicken Sie auf **Freigegebene Ordner**.

1. Klicken Sie mit der rechten Maustaste auf **Freigaben**, und klicken Sie anschließend auf **Neue Freigabe...**.

   ![Neue Freigabe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Erstellen Sie mithilfe des Assistenten zum Erstellen von Ordnerfreigaben** **eine Freigabe.

1. Klicken Sie unter **Ordnerpfad** auf **Durchsuchen**, und navigieren Sie zu einem Pfad für den freigegebenen Ordner, oder erstellen Sie einen Pfad. Klicken Sie auf **Weiter**.

1. Überprüfen Sie unter **Name, Beschreibung und Einstellungen** den Freigabenamen und den Pfad. Klicken Sie auf **Weiter**.

1. Legen Sie unter **Berechtigungen für freigegebene Ordner** die Option **Berechtigungen anpassen** fest. Klicken Sie auf **Benutzerdefiniert...**.

1. Klicken Sie unter **Berechtigungen anpassen** auf **Hinzufügen...**.

1. Achten Sie darauf, dass das für die Clustererstellung verwendete Konto über Vollzugriff verfügt.

   ![Neue Freigabe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. Klicken Sie auf **OK**.

1. Klicken Sie unter **Berechtigungen für freigegebene Ordner** auf **Fertig stellen**. Klicken Sie erneut auf **Fertig stellen**.  

1. Melden Sie sich vom Server ab.

### <a name="configure-cluster-quorum"></a>Konfigurieren des Clusterquorums

Legen Sie als Nächstes das Clusterquorum fest.

1. Stellen Sie eine Remotedesktopverbindung mit dem ersten Clusterknoten her.

1. Klicken Sie im **Failovercluster-Manager** mit der rechten Maustaste auf den Cluster, zeigen Sie auf **More Actions** (Weitere Aktionen), und klicken Sie auf **Clusterquorumeinstellungen konfigurieren...**.

   ![Neue Freigabe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. Klicken Sie im Assistenten zum Konfigurieren des Clusterquorums** **auf **Weiter**.

1. Wählen Sie unter **Quorumkonfigurationsoption auswählen** die Option **Quorumzeugen auswählen** aus, und klicken Sie anschließend auf **Weiter**.

1. Klicken Sie unter **Quorumzeuge auswählen** auf **Dateifreigabenzeugen konfigurieren**.

   >[!TIP]
   >Windows Server 2016 unterstützt einen Cloudzeugen. Bei Verwendung dieser Art von Zeuge benötigen Sie keinen Dateifreigabenzeugen. Weitere Informationen finden Sie unter [Deploy a cloud witness for a Failover Cluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness) (Bereitstellen eines Cloudzeugen für einen Failovercluster). In diesem Tutorial wird ein Dateifreigabenzeuge verwendet, der von älteren Betriebssystemen unterstützt wird.

1. Geben Sie unter **Dateifreigabenzeugen konfigurieren** den Pfad für die erstellte Freigabe ein. Klicken Sie auf **Weiter**.

1. Überprüfen Sie unter **Bestätigung** die Einstellungen. Klicken Sie auf **Weiter**.

1. Klicken Sie auf **Fertig stellen**.

Die Hauptressourcen des Clusters werden mit einem Dateifreigabenzeugen konfiguriert.

## <a name="enable-availability-groups"></a>Aktivieren von Verfügbarkeitsgruppen

Aktivieren Sie als Nächstes das Feature **Verfügbarkeitsgruppen**. Führen Sie diese Schritte für beide SQL Server durch.

1. Starten Sie über den **Startbildschirm** den **SQL Server-Konfigurations-Manager**.
2. Klicken Sie in der Browserstruktur auf **SQL Server-Dienste**, klicken Sie dann mit der rechten Maustaste auf den Dienst **SQL Server (MSSQLSERVER)**, und klicken Sie auf **Eigenschaften**.
3. Klicken Sie auf die Registerkarte **Hohe Verfügbarkeit mit AlwaysOn**, und wählen Sie **AlwaysOn-Verfügbarkeitsgruppen aktivieren** aus:

    ![Aktivieren von AlwaysOn-Verfügbarkeitsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. Klicken Sie auf **Übernehmen**. Klicken Sie im Popupdialogfenster auf **OK** .

5. Starten Sie den SQL Server-Dienst neu.

Wiederholen Sie diese Schritte für die andere SQL Server-Instanz.

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for the database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for the instance of SQL Server that is used to synchronize the database replicas in the Availability Groups on that instance.

On both SQL Servers, open the firewall for the TCP port for the database mirroring endpoint.

1. On the first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In the left pane, select **Inbound Rules**. On the right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For the port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In the **Action** page, keep **Allow the connection** selected and click **Next**.
6. In the **Profile** page, accept the default settings and click **Next**.
7. In the **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in the **Name** text box, then click **Finish**.

Repeat these steps on the second SQL Server.
-------------------------->

## <a name="create-a-database-on-the-first-sql-server"></a>Erstellen einer Datenbank in der ersten SQL Server-Instanz

1. Starten Sie die RDP-Datei für die erste SQL Server-Instanz mit einem Domänenkonto, das der festen SysAdmin-Serverrolle angehört.
1. Öffnen Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit der ersten SQL Server-Instanz her.
7. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf **Datenbanken**, und klicken Sie anschließend auf **Neue Datenbank**.
8. Geben Sie unter **Datenbankname** den Namen **MyDB1** ein, und klicken Sie dann auf **OK**.

### <a name="a-namebackupsharea-create-a-backup-share"></a><a name="backupshare"></a> Erstellen einer Sicherungsfreigabe

1. Klicken Sie auf dem ersten SQL Server unter **Server-Manager** auf **Tools**. Öffnen Sie **Computerverwaltung**.

1. Klicken Sie auf **Freigegebene Ordner**.

1. Klicken Sie mit der rechten Maustaste auf **Freigaben**, und klicken Sie anschließend auf **Neue Freigabe...**.

   ![Neue Freigabe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Erstellen Sie mithilfe des Assistenten zum Erstellen von Ordnerfreigaben** **eine Freigabe.

1. Klicken Sie unter **Ordnerpfad** auf **Durchsuchen**, und navigieren Sie zu einem Pfad für den freigegebenen Ordner der Datenbanksicherung, oder erstellen Sie einen Pfad. Klicken Sie auf **Weiter**.

1. Überprüfen Sie unter **Name, Beschreibung und Einstellungen** den Freigabenamen und den Pfad. Klicken Sie auf **Weiter**.

1. Legen Sie unter **Berechtigungen für freigegebene Ordner** die Option **Berechtigungen anpassen** fest. Klicken Sie auf **Benutzerdefiniert...**.

1. Klicken Sie unter **Berechtigungen anpassen** auf **Hinzufügen...**.

1. Vergewissern Sie sich, dass das SQL Server-Dienstkonto und das SQL Server-Agent-Dienstkonto für beide Server über Vollzugriff verfügen.

   ![Neue Freigabe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. Klicken Sie auf **OK**.

1. Klicken Sie unter **Berechtigungen für freigegebene Ordner** auf **Fertig stellen**. Klicken Sie erneut auf **Fertig stellen**.  

### <a name="take-a-full-backup-of-the-database"></a>Erstellen einer vollständigen Sicherung der Datenbank

Die neue Datenbank muss gesichert werden, um die Protokollkette zu initiieren. Wenn Sie die neue Datenbank nicht sichern, kann sie keiner Verfügbarkeitsgruppe hinzugefügt werden.

1. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf die Datenbank, zeigen Sie auf **Aufgaben...**, und klicken Sie anschließend auf **Back Up** (Sichern).

1. Klicken Sie auf **OK**, um eine vollständige Sicherung am Standardspeicherort für Sicherungen zu erstellen.

## <a name="create-the-availability-group"></a>Erstellen der Verfügbarkeitsgruppe
Nun können Sie eine Verfügbarkeitsgruppe konfigurieren. Gehen Sie dazu wie folgt vor:

* Erstellen Sie eine Datenbank in der ersten SQL Server-Instanz.
* Erstellen Sie sowohl eine vollständige Sicherung als auch eine Transaktionsprotokollsicherung der Datenbank.
* Stellen Sie die vollständige Sicherung und die Protokollsicherung mit der **NORECOVERY** in der zweiten SQL Server-Instanz wieder her.
* Erstellen Sie die Verfügbarkeitsgruppe (**AG1**) mit synchronem Commit, automatischem Failover und lesbaren, sekundären Replikaten.

### <a name="create-the-availability-group"></a>Erstellen der Verfügbarkeitsgruppe:

1. Gehen Sie in einer Remotedesktopsitzung mit der ersten SQL Server-Instanz wie folgt vor: Klicken Sie in SSMS im **Objekt-Explorer** mit der rechten Maustaste auf **Hohe Verfügbarkeit mit AlwaysOn**, und klicken Sie anschließend auf **Assistent für neue Verfügbarkeitsgruppen**.

    ![Starten des Assistenten für neue Verfügbarkeitsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. Klicken Sie auf der Seite **Einführung** auf **Weiter**. Geben Sie auf der Seite **Namen der Verfügbarkeitsgruppe angeben** unter **Name der Verfügbarkeitsgruppe** den Namen für die Verfügbarkeitsgruppe ein (beispielsweise **AG1**). Klicken Sie auf **Next**.

    ![Assistent für neue Verfügbarkeitsgruppen, Namen der Verfügbarkeitsgruppe angeben](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. Wählen Sie auf der Seite **Datenbanken auswählen** Ihre Datenbank aus, und klicken Sie auf **Weiter**.

   >[!NOTE]
   >Die Datenbank erfüllt die Voraussetzungen für eine Verfügbarkeitsgruppe, da Sie mindestens eine vollständige Sicherung auf dem vorgesehenen primären Replikat erstellt haben.

   ![Assistent für neue Verfügbarkeitsgruppen, Datenbanken auswählen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. Klicken Sie auf der Seite **Replikate angeben** auf **Replikat hinzufügen**.

   ![Assistent für neue Verfügbarkeitsgruppen, Replikate angeben](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. Das Dialogfeld **Verbindung mit Server herstellen** wird angezeigt. Geben Sie unter **Servername** den Namen des zweiten Servers ein. Klicken Sie auf **Verbinden**.

   Auf der Seite **Replikate angeben** wird nun unter **Verfügbarkeitsreplikate** der zweite Server aufgeführt. Konfigurieren Sie die Replikate wie folgt:

   ![Assistent für neue Verfügbarkeitsgruppen, Replikate angeben (abgeschlossen)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. Klicken Sie auf **Endpunkte**, um den Datenbankspiegelungs-Endpunkt für diese Verfügbarkeitsgruppe anzuzeigen. Verwenden den gleichen Port, den Sie auch beim Festlegen der [Firewallregel für Datenbankspiegelungs-Endpunkte](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall) verwendet haben.

    ![Assistent für neue Verfügbarkeitsgruppen, anfängliche Datensynchronisierung auswählen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. Wählen Sie auf der Seite **Anfängliche Datensynchronisierung auswählen** die Option **Vollständig** aus, und geben Sie einen freigegebenen Netzwerkpfad an. Verwenden Sie für den Speicherort die [zuvor erstellte Sicherungsfreigabe](#backupshare). In diesem Beispiel: **\\\\\<Erste SQL Server-Instanz\>\Backup\**. Klicken Sie auf **Weiter**.

   >[!NOTE]
   >Zur vollständigen Synchronisierung wird die Datenbank der ersten SQL Server-Instanz vollständig gesichert und in der zweiten Instanz wiederhergestellt. Bei umfangreichen Datenbanken wird von einer vollständigen Synchronisierung abgeraten, da sie sehr lange dauern kann. Sie können den Vorgang beschleunigen, indem Sie die Datenbank manuell sichern und mit `NO RECOVERY` wiederherstellen. Falls die Datenbank bereits vor dem Konfigurieren der Verfügbarkeitsgruppe mit `NO RECOVERY` in der zweiten SQL Server-Instanz wiederhergestellt wurde, wählen Sie **Nur verknüpfen** aus. Wenn Sie die Sicherung erst nach dem Konfigurieren der Verfügbarkeitsgruppe erstellen möchten, wählen Sie **Anfängliche Datensynchronisierung überspringen** aus.

    ![Assistent für neue Verfügbarkeitsgruppen, anfängliche Datensynchronisierung auswählen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. Klicken Sie auf der Seite **Validierung** auf **Weiter**. Die Seite sollte in etwa der folgenden Abbildung entsprechen:

    ![Assistent für neue Verfügbarkeitsgruppen, Validierung](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    >Es liegt eine Warnung für die Listenerkonfiguration vor, da Sie keinen Listener für die Verfügbarkeitsgruppe konfiguriert haben. Diese Warnung kann ignoriert werden, da Sie den Listener auf virtuellen Azure-Computern im Anschluss an Azure Load Balancer erstellen.

10. Klicken Sie auf der Seite **Zusammenfassung** auf **Fertig stellen**, und warten Sie, bis der Assistent die neue Verfügbarkeitsgruppe konfiguriert hat. Auf der Seite **Status** können Sie auf **Weitere Details** klicken, um den detaillierten Status anzuzeigen. Vergewissern Sie sich nach Abschluss des Assistenten auf der Seite **Ergebnisse**, dass die Verfügbarkeitsgruppe erfolgreich erstellt wurde.

     ![Assistent für neue Verfügbarkeitsgruppen, Ergebnisse](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. Klicken Sie auf **Schließen**, um den Assistenten zu beenden.

### <a name="check-the-availability-group"></a>Überprüfen der Verfügbarkeitsgruppe

1. Erweitern Sie im **Objekt-Explorer** den Eintrag **Hohe Verfügbarkeit mit AlwaysOn** und anschließend **Verfügbarkeitsgruppen**. In diesem Container sollte nun die neue Verfügbarkeitsgruppe angezeigt werden. Klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, und klicken Sie anschließend auf **Dashboard anzeigen**.

   ![Anzeigen des Verfügbarkeitsgruppen-Dashboards](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   Ihr **AlwaysOn-Dashboard** sollte in etwa wie folgt aussehen:

   ![Verfügbarkeitsgruppen-Dashboard](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   Es werden die Replikate, der Failovermodus jedes Replikats und der Synchronisierungsstatus angezeigt.

2. Klicken Sie im **Failovercluster-Manager** auf Ihren Cluster. Wählen Sie **Rollen** aus. Bei dem verwendeten Verfügbarkeitsgruppennamen handelt es sich um eine Rolle im Cluster. Diese Verfügbarkeitsgruppe besitzt keine IP-Adresse für Clientverbindungen, da Sie keinen Listener konfiguriert haben. Der Listener wird nach der Erstellung einer Azure Load Balancer-Instanz konfiguriert.

   ![Verfügbarkeitsgruppe im Failovercluster-Manager](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > Versuchen Sie nicht, über den Failovercluster-Manager ein Failover für die Verfügbarkeitsgruppe durchzuführen. Alle Failovervorgänge sollten aus dem **AlwaysOn-Dashboard** in SSMS ausgeführt werden. Weitere Informationen finden Sie unter [Einschränkungen für die Verwendung des WSFC-Failovercluster-Managers mit Verfügbarkeitsgruppen](https://msdn.microsoft.com/library/ff929171.aspx).
    >

Sie verfügen nun über eine Verfügbarkeitsgruppe mit Replikaten in zwei Instanzen von SQL Server. Sie können die Verfügbarkeitsgruppe zwischen Instanzen verschieben. Sie können noch keine Verbindung mit der Verfügbarkeitsgruppe herstellen, da Sie über keinen Listener verfügen. Auf virtuellen Azure-Computern wird für den Listener ein Lastenausgleich benötigt. Im nächsten Schritt wird der Lastenausgleich in Azure erstellt.

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a>Erstellen einer Azure Load Balancer-Instanz

Auf virtuellen Azure-Computern benötigt eine SQL Server-Verfügbarkeitsgruppe einen Lastenausgleich. Der Lastenausgleich speichert die IP-Adresse für den Verfügbarkeitsgruppenlistener. In diesem Abschnitt erfahren Sie, wie Sie den Lastenausgleich über das Azure-Portal erstellen.

1. Navigieren Sie im Azure-Portal zu der Ressourcengruppe mit Ihren SQL Server-Instanzen, und klicken Sie auf **+ Hinzufügen**.
2. Suchen Sie nach **Load Balancer**. Wählen Sie den von Microsoft veröffentlichten Lastenausgleich aus.

   ![Verfügbarkeitsgruppe im Failovercluster-Manager](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  Klicken Sie auf **Erstellen**.
3. Konfigurieren Sie den Load Balancer mit folgenden Parametern: 

   | Einstellung | Field |
   | --- | --- |
   | **Name** |Verwenden Sie einen Textnamen für den Lastenausgleich (beispielsweise **sqlLB**). |
   | **Schema** |Intern |
   | **Virtuelles Netzwerk** |Verwenden Sie den Namen des virtuellen Azure-Netzwerks. |
   | **Subnetz** |Verwenden Sie den Namen des Subnetzes, in dem sich der virtuelle Computer befindet.  |
   | **IP-Adresszuweisung** |Statisch |
   | **IP-Adresse** |Verwenden Sie eine verfügbare Adresse aus dem Subnetz. |
   | **Abonnement** |Verwenden Sie das gleiche Abonnement wie der virtuelle Computer. |
   | **Standort** |Verwenden Sie den gleichen Standort wie der virtuelle Computer. |

   Das Blatt im Azure-Portal sollte wie folgt aussehen:

   ![Erstellen oder Aktualisieren eines Lastenausgleichs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. Klicken Sie auf **Erstellen**, um den Lastenausgleich zu generieren.

Zum Konfigurieren des Lastenausgleichs müssen Sie einen Back-End-Pool und einen Test erstellen und die Lastenausgleichsregeln festlegen. Führen Sie diese Schritte im Azure-Portal aus.

### <a name="add-backend-pool"></a>Hinzufügen eines Back-End-Pools

1. Navigieren Sie im Azure-Portal zu Ihrer Verfügbarkeitsgruppe. Unter Umständen müssen Sie die Ansicht aktualisieren, damit der neu erstellte Lastenausgleich angezeigt wird.

   ![Navigieren zum Lastenausgleich in der Ressourcengruppe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. Klicken Sie auf den Lastenausgleich, auf **Back-End-Pools** und anschließend auf **+ Hinzufügen**. Konfigurieren Sie den Back-End-Pool wie folgt:

   | Einstellung | Beschreibung | Beispiel
   | --- | --- |---
   | **Name** | Geben Sie einen Textnamen ein. | SQLLBBE
   | **Verfügbarkeitsgruppe** | Verwenden Sie einen Namen der Verfügbarkeitsgruppe, in der sich Ihre virtuellen SQL Server-Computer befinden. | sqlAvailabilitySet |
   | **Virtuelle Computer** |Die Namen der beiden virtuellen Azure-Computer mit SQL Server | sqlserver-0, sqlserver-1

1. Geben Sie den Namen für den Back-End-Pool ein.

1. Klicken Sie auf **+ Virtuellen Computer hinzufügen**.

1. Wählen Sie als Verfügbarkeitsgruppe die Verfügbarkeitsgruppe mit den SQL Server-Instanzen aus.

1. Schließen Sie bei den virtuellen Computern beide SQL Server-Instanzen ein. Schließen Sie den Dateifreigabenzeugen-Server nicht ein. Ihre Auswahl sollte in etwa wie folgt aussehen:

   ![Navigieren zum Lastenausgleich in der Ressourcengruppe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/88-configurebepool.png)

1. Klicken Sie auf **OK**, um den Back-End-Pool zu erstellen.

### <a name="set-the-probe"></a>Festlegen des Tests

1. Klicken Sie auf den Lastenausgleich, auf **Integritätstests** und anschließend auf **+ Hinzufügen**.

1. Konfigurieren Sie den Integritätstest wie folgt:

   | Einstellung | Beschreibung | Beispiel
   | --- | --- |---
   | **Name** | Text | SQLAlwaysOnEndPointProbe |
   | **Protokoll** | Wählen Sie „TCP“ aus. | TCP |
   | **Port** | Ein beliebiger nicht verwendeter Port. | 59999 |
   | **Intervall**  | Der Zeitraum zwischen Testversuchen in Sekunden. |5 |
   | **Fehlerhafter Schwellenwert** | Die Anzahl aufeinander folgender Testfehler, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.  | 2 |

1. Klicken Sie auf **OK**, um den Integritätstest zu verwenden.

### <a name="set-the-load-balancing-rules"></a>Festlegen der Lastenausgleichsregeln

1. Klicken Sie auf den Lastenausgleich, auf **Load balancing rules** (Lastenausgleichsregeln) und anschließend auf **+Hinzufügen**.

1. Konfigurieren Sie die Lastenausgleichsregeln wie folgt:
   | Einstellung | Beschreibung | Beispiel
   | --- | --- |---
   | **Name** | Text | SQLAlwaysOnEndPointListener |
   | **Frontend IP address** (Front-End-IP-Adresse) | Wählen Sie eine Adresse aus. |Verwenden Sie die Adresse, die Sie beim Erstellen des Lastenausgleichs erstellt haben. |
   | **Protokoll** | Wählen Sie „TCP“ aus. |TCP |
   | **Port** | Verwenden Sie den Port für die SQL Server-Instanz. | 1433 |
   | **Back-End-Port** | Dieses Feld wird nicht verwendet, wenn für Direct Server Return die Option „Floating IP“ festgelegt ist. | 1433 |
   | **Test** |Der Name, den Sie für den Test angegeben haben. | SQLAlwaysOnEndPointProbe |
   | **Session Persistence** (Sitzungspersistenz) | Dropdownliste | **Keine** |
   | **Leerlauftimeout** | Gibt an, wie viele Minuten eine TCP-Verbindung geöffnet bleiben soll. | 4 |
   | **Floating IP (Direct Server Return)** | |Aktiviert |

   > [!WARNING]
   > Direct Server Return wird bei der Erstellung festgelegt. Diese Einstellung kann nicht geändert werden.

1. Klicken Sie auf **OK**, um die Lastenausgleichsregeln festzulegen.

## <a name="a-nameconfigure-listenera-configure-the-listener"></a><a name="configure-listener"></a> Konfigurieren des Listeners

Als Nächstes muss ein Verfügbarkeitsgruppenlistener für den Failovercluster konfiguriert werden.

> [!NOTE]
> In diesem Tutorial erfahren Sie, wie Sie einen einzelnen Listener mit einer einzelnen IP-Adresse eines internen Lastenausgleichs erstellen. Weitere Informationen zum Erstellen einzelner oder mehrerer Listener mit einer oder mehreren IP-Adressen finden Sie unter [Configure one or more Always On Availability Group Listeners - Resource Manager](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern – Resource Manager).
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a>Festlegen des Listenerports

Legen Sie in SQL Server Management Studio den Listenerport fest.

1. Starten Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit dem primären Replikat her.

1. Navigieren Sie zu **Hohe Verfügbarkeit mit Always On** | **Verfügbarkeitsgruppen** | **Verfügbarkeitsgruppenlistener**.

1. Jetzt sollte der Listenername angezeigt werden, den Sie im Failovercluster-Manager erstellt haben. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Eigenschaften**.

1. Geben Sie im Feld **Port** die Portnummer für den Verfügbarkeitsgruppenlistener an. Verwenden Sie dabei den zuvor verwendeten Wert für „$EndpointPort“ (Standardwert:&1433;). Klicken Sie anschließend auf **OK**.

Sie verfügen nun über eine SQL Server-Verfügbarkeitsgruppe auf virtuellen Azure-Computern im Azure Resource Manager-Modus.

## <a name="test-connection-to-listener"></a>Testen der Verbindung mit dem Listener

Gehen Sie wie folgt vor, um die Verbindung zu testen:

1. Stellen Sie eine RDP-Verbindung mit einem SQL Server her, der sich im gleichen virtuellen Netzwerk befindet, aber nicht für das Replikat zuständig ist. Sie können die andere SQL Server-Instanz im Cluster verwenden.

1. Testen Sie die Verbindung mithilfe des **sqlcmd** -Hilfsprogramms. Das folgende Skript stellt beispielsweise über den Listener eine **sqlcmd** -Verbindung mit Windows-Authentifizierung mit dem primären Replikat her:

    ```
    sqlmd -S <listenerName> -E
    ```

    Geben Sie den Port in der Verbindungszeichenfolge an, wenn der Listener einen anderen Port als den Standardport (1433) verwendet. Mit dem folgenden sqlcmd-Befehl wird beispielsweise eine Verbindung mit einem Listener über Port 1435 hergestellt:

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Die sqlcmd-Verbindung wird automatisch mit der SQL Server-Instanz hergestellt, die das primäre Replikat hostet.

> [!TIP]
> Vergewissern Sie sich, dass der angegebene Port in der Firewall beider SQL Server geöffnet ist. Beide Server benötigen eine eingehende Regel für den TCP-Port, den Sie verwenden möchten. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten einer Firewallregel](http://technet.microsoft.com/library/cc753558.aspx).
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by the way” info, an Important is info users need to complete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is the second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note the format for documenting the UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult to maintain. Highlight areas you are referring to in red.*-->

<!--**No. of steps**: *Make sure the number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want to go on.*-->

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen einer IP-Adresse zu einem Lastenausgleich für eine zweite Verfügbarkeitsgruppe](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP)



<!--HONumber=Feb17_HO2-->


