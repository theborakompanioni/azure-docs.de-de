---
title: "Ausführen des Hyper-V Capacity Planner-Tools für Site Recovery | Microsoft Docs"
description: "In diesem Artikel wird beschrieben, wie Sie das Hyper-V Capacity Planner-Tool für Azure Site Recovery ausführen."
services: site-recovery
documentationcenter: na
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 2bc3832f-4d6e-458d-bf0c-f00567200ca0
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/06/2017
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: 79adce1f3fb9a33e60159af72e83118dd88e0946
ms.openlocfilehash: ecddf255f4b4750bf67f2689b0274c04f8ec54b7


---
# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>Ausführen des Hyper-V Capacity Planner-Tools für Site Recovery

Als Teil Ihrer Azure Site Recovery-Bereitstellung müssen Sie Ihre Replikations- und Bandbreitenanforderungen ermitteln. Das Hyper-V Capacity Planner-Tool für Site Recovery hilft Ihnen, dies für die Replikation virtueller Hyper-V-Computer durchzuführen.

In diesem Artikel wird beschrieben, wie Sie das Hyper-V Capacity Planner-Tool ausführen. Dieses Tool sollte zusammen mit den unter [Kapazitätsplanung für Site Recovery](site-recovery-capacity-planner.md) beschriebenen Informationen verwendet werden.

## <a name="before-you-start"></a>Vorbereitung
Sie führen das Tool auf einem Hyper-V-Server- oder Clusterknoten an Ihrem primären Standort aus. Der Hyper-V-Hostserver benötigt zum Ausführen des Tools Folgendes:

* Betriebssystem: Windows Server 2012 oder 2012 R2
* Arbeitsspeicher: 20 MB (Minimum)
* CPU: 5 % freie Auslastung (Minimum)
* Festplattenspeicher: 5 MB (Minimum)

Vor dem Ausführen des Tools müssen Sie den primären Standort vorbereiten. Wenn Sie die Replikation zwischen zwei lokalen Standorten durchführen und die Bandbreite überprüfen möchten, müssen Sie zusätzlich einen Replikatserver vorbereiten.

## <a name="step-1-prepare-the-primary-site"></a>Schritt 1: Vorbereiten des primären Standorts

1. Erstellen Sie am primären Standort eine Liste mit allen Hyper-V-VMs, die Sie replizieren möchten, und den Hyper-V-Hosts/-Clustern, auf denen sie sich befinden. Das Tool kann jeweils für mehrere eigenständige Hosts oder für einen einzelnen Cluster ausgeführt werden, aber nicht für beides zusammen. Außerdem muss es für jedes Betriebssystem separat ausgeführt werden. Erfassen und notieren Sie Informationen über Hyper-V-Server wie folgt:

   * Eigenständige Windows Server 2012-Server
   * Windows Server 2012-Cluster
   * Eigenständige Windows Server 2012 R2-Server
   * Windows Server 2012 R2-Cluster
2. Aktivieren Sie den Remotezugriff auf WMI auf allen Hyper-V-Hosts und -Clustern. Führen Sie diesen Befehl auf allen Servern/Clustern aus, um sicherzustellen, dass die Firewallregeln und Benutzerberechtigungen festgelegt sind:

        netsh firewall set service RemoteAdmin enable
3. Aktivieren Sie die Leistungsüberwachung auf Servern und Clustern wie folgt:

   * Öffnen Sie die Windows-Firewall mit dem Snap-In **Erweiterte Sicherheit** und aktivieren Sie dann die folgenden eingehenden Regeln: **COM+-Netzwerkzugriff (DCOM-In)** und alle Regeln in der Gruppe **Remote-Ereignisprotokollverwaltung**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Schritt 2: Vorbereiten eines Replikatservers (Replikation zwischen zwei lokalen Standorten)
Sie müssen diesen Schritt nicht ausführen, wenn Sie zu Azure replizieren.

Wir empfehlen Ihnen, einen einzelnen Hyper-V-Host als Wiederherstellungsserver einzurichten, damit eine Dummy-VM auf diesen Server repliziert werden kann, um die Bandbreite zu überprüfen.  Sie können diesen Schritt überspringen, allerdings können Sie die Bandbreite nur messen, wenn Sie ihn ausführen.

1. Wenn Sie einen Clusterknoten als Replikat verwenden möchten, müssen Sie den Hyper-V-Replikatbroker konfigurieren:

   * Öffnen Sie im **Server-Manager** den **Failovercluster-Manager**.
   * Stellen Sie eine Verbindung mit dem Cluster her, markieren Sie den Clusternamen und klicken Sie auf **Actions** (Aktionen) > **Configure Role** (Rolle konfigurieren), um den Assistenten für hohe Verfügbarkeit zu öffnen.
   * Klicken Sie unter **Select Role** (Rolle auswählen) auf **Hyper-V Replica Broker** (Hyper-V-Replikatbroker). Geben Sie im Assistenten einen **NetBIOS-Namen** und eine **IP-Adresse** als Verbindungspunkt mit dem Cluster (einen sogenannten Clientzugriffspunkt) an. Der **Hyper-V-Replikatbroker** wird konfiguriert. Notieren Sie sich den Namen des Clientzugriffspunkts, der erstellt wird.
   * Vergewissern Sie sich, dass die Rolle „Hyper-V Replica Broker“ erfolgreich online geschaltet und ein Failover zwischen allen Knoten des Clusters ausgeführt wird. Klicken Sie hierzu mit der rechten Maustaste auf die Rolle, zeigen Sie auf **Move** (Verschieben), und klicken Sie dann auf **Select Node** (Knoten auswählen). Wählen Sie einen Knoten aus > **OK**.
   * Stellen Sie bei Verwendung der zertifikatbasierten Authentifizierung sicher, dass für jeden Clusterknoten und den Clientzugriffspunkt das Zertifikat installiert ist.
2. Aktivieren eines Replikatservers:

   * Öffnen Sie den Failovercluster-Manager für ein Cluster, stellen Sie eine Verbindung mit dem Cluster her, und klicken Sie auf **Roles** (Rollen) > Select Role > **Replication Settings** (Replikationseinstellungen) > **Enable this cluster as a Replica server** (Dieses Cluster als Replikatserver aktivieren). Wenn Sie ein Cluster als Replikat verwenden, muss auch im Cluster am primären Standort die Rolle „Hyper-V Replica Broker“ vorhanden sein.
   * Öffnen Sie für einen eigenständigen Server den Hyper-V-Manager. Klicken Sie im Bereich **Actions** für den Server, den Sie aktivieren möchten, auf **Hyper-V Settings** (Hyper-V-Einstellungen). Klicken Sie dann unter **Replication Configuration** (Replikationskonfiguration) auf **Enable this computer as a Replica server** (Diesen Computer als Replikatserver aktivieren).
3. Einrichten der Authentifizierung:

   * Wählen Sie unter **Authentifizierung und Ports** die Authentifizierungsart für den primären Server und die Authentifizierungsports aus. Klicken Sie bei Verwendung eines Zertifikats auf **Zertifikat auswählen**, um ein Zertifikat auszuwählen. Verwenden Sie Kerberos, wenn sich der primäre Hyper-V-Host und der Hyper-V-Wiederherstellungshost in derselben Domäne oder in vertrauenswürdigen Domänen befinden. Verwenden Sie Zertifikate für unterschiedliche Domänen oder für eine Arbeitsgruppenbereitstellung.
   * Lassen Sie im Bereich **Autorisierung und Speicherung** für **alle** authentifizierten (primären) Server das Senden von Replikationsdaten an diesen Replikatserver zu. 

     ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)
   * Führen Sie **netsh http show servicestate** aus, um zu überprüfen, ob der Listener für das von Ihnen angegebene Protokoll bzw. den Port ausgeführt wird:  
4. Einrichten von Firewalls: Bei der Hyper-V-Installation werden Firewallregeln erstellt, um den Datenverkehr über die Standardports (443 für HTTPS, 80 für Kerberos) zuzulassen. Aktivieren Sie diese Regeln wie folgt:
  - Zertifikatauthentifizierung auf dem Cluster (443): ``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
  - Kerberosauthentifizierung auf dem Cluster (80): ``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
  - Zertifikatauthentifizierung auf einem eigenständigen Server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
  - Kerberosauthentifizierung auf einem eigenständigen Server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planner-tool"></a>Schritt 3: Ausführen des Tools Capacity Planner
Nachdem Sie Ihren primären Standort vorbereitet und einen Wiederherstellungsserver eingerichtet haben, können Sie das Tool ausführen.

1. [Laden Sie das Tool aus dem Microsoft Download Center herunter](https://www.microsoft.com/download/details.aspx?id=39057) .
2. Führen Sie das Tool auf einem der primären Server (oder in einem der Knoten des primären Clusters) aus. Klicken Sie mit der rechten Maustaste auf die EXE-Datei, und wählen Sie dann **Als Administrator ausführen**aus.
3. Geben Sie unter **Before you begin** (Vor dem Start) an, wie lange Sie Daten sammeln möchten. Es wird empfohlen, das Tool während des Produktionszeitraums auszuführen, um sicherzustellen, dass die Daten repräsentativ sind. Wenn Sie nur die Netzwerkverbindung überprüfen möchten, reicht für das Sammeln eine Minute aus.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)
4. Geben Sie unter **Primary site details** (Details zum primären Standort) den Servernamen oder FQDN für einen eigenständigen Host an, oder geben Sie für ein Cluster den FQDN des Clientzugriffspunkts, den Clusternamen oder einen beliebigen Knoten im Cluster an, und klicken Sie dann auf **Next** (Weiter). Das Tool erkennt automatisch den Namen des Servers, auf dem es ausgeführt wird. Mit dem Tool werden die VMs ausgewählt, die für die angegebenen Server überwacht werden können.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)
5. Wählen Sie unter **Replica Site Details** (Details zum Replikatstandort) die Option **Skip tests involving replica site** (Tests für Replikatstandort überspringen), wenn Sie zu Azure replizieren oder wenn Sie die Replikation in ein sekundäres Rechenzentrum durchführen und keinen Replikatserver eingerichtet haben. Wenn Sie in ein sekundäres Rechenzentrum replizieren und einen Replikattyp eingerichtet haben, geben Sie den FQDN des eigenständigen Servers oder den Clientzugriffspunkt für das Cluster unter **Server name (or) Hyper-V Replica Broker CAP** (Servername (oder) Hyper-V-Replikatbroker-CAP) ein.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)
6. Aktivieren Sie unter **Extended Replica Details** (Erweiterte Replikatdetails) die Option **Erweiterte Replikatdetails** (Tests für Standorte mit erweitertem Replikat überspringen). Diese Tests werden von Site Recovery nicht unterstützt.
7. Unter **Choose VMs to Replicate** (VMs für Replikation auswählen) stellt das Tool eine Verbindung mit dem Server oder Cluster her und zeigt die VMs und Datenträger an, die auf dem primären Server ausgeführt werden. Dies erfolgt gemäß den Einstellungen, die Sie auf der Seite **Primary Site Details** angegeben haben. VMs, die bereits für die Replikation aktiviert sind oder die nicht ausgeführt werden, werden nicht angezeigt. Wählen Sie die VMs aus, für die Sie Metriken sammeln möchten. Beim Auswählen der VHDs werden auch automatisch Daten für die VMs erfasst.
8. Wenn Sie einen Replikatserver oder -cluster konfiguriert haben, geben Sie unter **Network information** (Netzwerkinformationen) die ungefähre WAN-Bandbreite an, die voraussichtlich zwischen dem primären und Replikatstandort verwendet wird, und wählen Sie die Zertifikate aus, falls Sie die Zertifikatauthentifizierung konfiguriert haben.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)
9. Überprüfen Sie unter **Summary** (Zusammenfassung) die Einstellungen, und klicken Sie auf **Next**, um mit dem Erfassen von Metriken zu beginnen. Der Fortschritt und Status des Tools wird auf der Seite **Kapazität berechnen** angezeigt. Klicken Sie nach Abschluss der Ausführung des Tools auf **View Report** (Bericht anzeigen), um sich die Ausgabe anzusehen. Berichte und Protokolle werden standardmäßig unter **%systemdrive%\Users\Public\Documents\Capacity Planner** gespeichert.

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Schritt 4: Interpretieren der Ergebnisse

Hier werden die wichtigen Ergebnisse angegeben. Sie können Metriken ignorieren, die hier nicht aufgelistet sind. Sie sind für Site Recovery nicht relevant.

### <a name="on-premises-to-on-premises-replication"></a>Replikation zwischen zwei lokalen Standorten

* Auswirkung der Replikation auf die Rechenkapazität und den Speicher des primären Hosts
* Auswirkung der Replikation auf den Festplattenspeicherplatz des primären und Wiederherstellungshosts (IOPS)
* Erforderliche Gesamtbandbreite für die Deltareplikation (MBit/s)
* Aufgezeichnete Netzwerkbandbreite zwischen dem primären Host und dem Wiederherstellungshost (MBit/s)
* Vorschlag für die ideale Anzahl der aktiven parallelen Übertragungen zwischen den beiden Hosts oder Clustern

### <a name="on-premises-to-azure-replication"></a>Replikation zwischen einem lokalen Standort und Azure

* Auswirkung der Replikation auf die Rechenkapazität und den Speicher des primären Hosts
* Auswirkung der Replikation auf den Speicherplatz des primären Hosts (IOPS)
* Erforderliche Gesamtbandbreite für die Deltareplikation (MBit/s)

## <a name="more-resources"></a>Weitere Ressourcen
* Ausführliche Informationen zu diesem Tool finden Sie im Dokument, das mit dem Tool heruntergeladen wird.
* Sehen Sie sich eine exemplarische Vorgehensweise zum Tool im [TechNet-Blog](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx)von Keith Mayer an.
* [Ergebnisse](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) unserer Leistungstests für die Hyper-V-Replikation zwischen zwei lokalen Standorten an.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Planung abgeschlossen haben, können Sie mit der Bereitstellung von Site Recovery beginnen:

* [Replizieren Sie Hyper-V-VMs in VMM-Clouds zu Azure.](site-recovery-vmm-to-azure.md)
* [Replizieren von Hyper-V-VMs (ohne VMM) in Azure](site-recovery-hyper-v-site-to-azure.md)
* [Replizieren von Hyper-V-VMs zwischen VMM-Standorten](site-recovery-vmm-to-vmm.md)
* [Replizieren von Hyper-V-VMs zwischen VMM-Standorten mit SAN](site-recovery-vmm-san.md)
* [Replizieren von Hyper-V-VMs auf einem einzelnen VMM-Server](site-recovery-single-vmm.md)



<!--HONumber=Nov16_HO3-->


