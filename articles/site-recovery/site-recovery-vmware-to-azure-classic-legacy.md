---
title: "Replizieren von virtuellen VMware-Computern und physischen Servern in Azure mit Azure Site Recovery (älter) | Microsoft-Dokumentation"
description: Beschreibt, wie Sie lokale virtuelle Computer und physische Windows-/Linux-Server mithilfe von Azure Site Recovery in einer Legacybereitstellung im klassischen Portal nach Azure replizieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: f980fb52-8ec2-4dd9-85e2-8e52e449f1ba
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 5614c39d914d5ae6fde2de9c0d9941e7b93fc10f
ms.openlocfilehash: c8daa813758f2b0b420bc146d942d56f90f0d526


---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>Replizieren von virtuellen VMware-Computern und physischen Servern nach Azure mithilfe von Azure Site Recovery und dem klassischen Portal (Legacy)
> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-vmware-to-azure.md)
> * [Klassisches Portal](site-recovery-vmware-to-azure-classic.md)
> * [Klassisches Portal (Vorgängerversion)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

Willkommen bei Azure Site Recovery! Dieser Artikel beschreibt eine Legacybereitstellung für die Replizierung lokaler virtueller VMware-Computer oder physischer Windows-/Linux-Server nach Azure mithilfe von Azure Site Recovery und dem klassischen Portal.

## <a name="overview"></a>Übersicht
Organisationen benötigen eine Strategie für die Geschäftskontinuität und Notfallwiederherstellung (BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Bei Ihrer BCDR-Strategie sollten Sie darauf achten, dass Unternehmensdaten geschützt sind und wiederhergestellt werden können. Stellen Sie außerdem sicher, dass Workloads auch nach dem Eintreten eines Notfalls ständig verfügbar sind.

Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Maschinen in die Cloud (Azure) oder in ein sekundäres Rechenzentrum orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md)

> [!WARNING]
> Dieser Artikel enthält **Legacyanweisungen**. Verwenden Sie die Anweisungen nicht für neue Bereitstellungen. Gehen Sie stattdessen wie [hier](site-recovery-vmware-to-azure.md) beschrieben vor, um Site Recovery im Azure-Portal bereitzustellen, oder [konfigurieren Sie die erweiterte Bereitstellung im klassischen Portal](site-recovery-vmware-to-azure-classic.md). Wenn Sie die Bereitstellung bereits mit der in diesem Artikel beschriebenen Methode durchgeführt haben, empfiehlt sich eine Migration zur erweiterten Bereitstellung im klassischen Portal.
>
>

## <a name="migrate-to-the-enhanced-deployment"></a>Migrieren zur erweiterten Bereitstellung
Dieser Abschnitt ist nur relevant, wenn Sie Site Recovery bereits gemäß den in diesem Artikel beschriebenen Anweisungen bereitgestellt haben.

Um Ihre bestehende Bereitstellung zu migrieren müssen Sie die folgenden Schritte durchführen:

1. Stellen Sie die neuen Site Recovery-Komponenten lokal bereit.
2. Konfigurieren Sie Anmeldeinformationen für die automatische Ermittlung virtueller VMware-Computer auf dem neuen Konfigurationsserver.
3. Ermitteln Sie die VMware-Server mit dem neuen Konfigurationsserver.
4. Erstellen Sie eine neue Schutzgruppe mit dem neuen Konfigurationsserver.

Vorbereitung:

* Für die Migration empfiehlt sich die Einrichtung eines Wartungsfensters.
* Die Option **Computer migrieren** ist nur verfügbar, wenn Schutzgruppen vorhanden sind, die im Rahmen einer Legacybereitstellung erstellt wurden.
* Nach Abschluss der Migrationsschritte kann es 15 Minuten oder länger dauern, bis die Anmeldeinformationen aktualisiert und die virtuellen Computer ermittelt und aktualisiert wurden, um sie einer Schutzgruppe hinzufügen zu können. Wenn Sie nicht so lange warten möchten, können Sie die Aktualisierung auch manuell durchführen.

Migrieren Sie wie folgt:

1. Informieren Sie sich über die [erweiterte Bereitstellung im klassischen Portal](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Machen Sie sich mit der erweiterten [Architektur](site-recovery-vmware-to-azure-classic.md#scenario-architecture) und den [Voraussetzungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) vertraut.
2. Deinstallieren Sie auf Computern, die derzeit repliziert werden, den Mobilitätsdienst. Auf den Computern wird eine neue Version des Diensts installiert, wenn Sie sie der neuen Schutzgruppe hinzufügen.
3. Laden Sie einen [Tresorregistrierungsschlüssel](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) herunter, und [führen Sie den Assistenten für ein einheitliches Setup aus](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server), um die Komponenten des Konfigurationsservers, des Prozessservers und des Masterzielservers zu installieren. Erfahren Sie mehr über die [Kapazitätsplanung](site-recovery-vmware-to-azure-classic.md#capacity-planning).
4. [Richten Sie Anmeldeinformationen ein](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) , mit denen Site Recovery auf den VMware-Server zugreifen und automatisch virtuelle VMware-Computer ermitteln kann. Informieren Sie sich über die [erforderlichen Berechtigungen](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. Fügen Sie [vCenter-Server oder vSphere-Hosts](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts)hinzu. Es kann 15 Minuten oder länger dauern, bis Server im Site Recovery-Portal angezeigt werden.
6. Erstellen Sie eine [neue Schutzgruppe](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group). Es dauert bis zu 15 Minuten, bis das Portal aktualisiert ist und die virtuellen Computer ermittelt und angezeigt werden. Wenn Sie nicht warten möchten, können Sie den Namen des Verwaltungsservers markieren (nicht darauf klicken) und auf **Aktualisieren** klicken.
7. Klicken Sie in der neuen Schutzgruppe auf **Computer migrieren**.

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)
8. Wählen Sie unter **Virtuellen Computer auswählen** die Schutzgruppe, aus der Sie migrieren möchten, sowie die zu migrierenden virtuellen Computer aus.

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)
9. Geben Sie bei **Zieleinstellungen konfigurieren** an, ob Sie für alle Computer die gleichen Einstellungen verwenden möchten, und wählen Sie den Prozessserver und das Azure-Speicherkonto aus. Falls Sie über keinen separaten Prozessserver verfügen, handelt es sich hierbei um die IP-Adresse des Konfigurationsserver.

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Die Migration von Speicherkonten](../resource-group-move-resources.md) zwischen Ressourcengruppen im gleichen Abonnement oder zwischen verschiedenen Abonnements wird für Speicherkonten nicht unterstützt, die für die Site Recovery-Bereitstellung verwendet werden.

1. Wählen Sie unter **Konten angeben**das Konto aus, das Sie erstellt haben, damit der Prozessserver auf den Computer zugreifen und mittels Push die neue Version des Mobilitätsdiensts übertragen kann.

   ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)
2. Site Recovery migriert Ihre replizierten Daten zu dem von Ihnen angegebenen Azure-Speicherkonto. Optional können Sie das Speicherkonto wiederverwenden, das Sie bereits bei der Legacy-Bereitstellung verwendet haben.
3. Nach Fertigstellung dieser Aufgabe synchronisieren sich die virtuellen Computer automatisch. Nach Abschluss der Synchronisierung können Sie die virtuellen Computer aus der alten Schutzgruppe löschen.
4. Nachdem alle Computer migriert wurden, können Sie die alte Schutzgruppe löschen.
5. Denken Sie daran, die Failover-Eigenschaften für virtuelle Computer und die Azure-Netzwerkeinstellungen nach dem Ende der Synchronisation festzulegen.
6. Wenn Sie bereits Wiederherstellungspläne haben, können Sie diese mit der Option **Wiederherstellungsplan migrieren** zur neuen Bereitstellung migrieren. Sie sollten dies nur tun, nachdem alle geschützten virtuellen Computer migriert wurden.

   ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

> [!NOTE]
> Fahren Sie nach Abschluss der Migration mit dem [erweiterten Artikel](site-recovery-vmware-to-azure-classic.md)fort. Der Rest dieses Legacyartikels ist nicht mehr relevant, und es müssen keine der darin beschriebenen Schritte mehr ausgeführt werden.**
>
>

## <a name="what-do-i-need"></a>Was benötige ich?
Dieses Diagramm zeigt die Bereitstellungskomponenten.

![Neuer Tresor](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Sie benötigen Folgendes:

| **Komponente** | **Bereitstellung** | **Details** |
| --- | --- | --- |
| **Konfigurationsserver** |Ein virtueller Standard-A3-Azure-Computer im gleichen Abonnement wie Site Recovery |Der Konfigurationsserver koordiniert die Kommunikation zwischen geschützten Computern, dem Prozessserver und den Masterzielservern in Azure. Bei einem Failover richtet er die Replikation ein und koordiniert die Wiederherstellung in Azure. |
| **Masterzielserver** |Ein virtueller Azure-Computer – entweder als Windows-Server, der auf einem Windows Server 2012 R2-Katalogimage basiert (zum Schutz von Windows-Computern), oder als Linux-Server, der auf einem OpenLogic CentOS 6.6-Katalogimage basiert (zum Schutz von Linux-Computern)<br/><br/> Es stehen drei Größenoptionen zur Verfügung: Standard A4, Standard D14 und Standard DS4.<br/><br/> Der Server ist mit demselben Azure-Netzwerk verbunden wie der Konfigurationsserver.<br/><br/>  Die Einrichtung erfolgt über das Site Recovery-Portal. |Er empfängt replizierte Daten von Ihren geschützten Computern und speichert sie mithilfe der angefügten virtuellen Festplatten, die im Blobspeicher in Ihrem Azure-Speicherkonto erstellt wurden.<br/><br/>  „Standard DS4“ ist speziell für den Schutz von Workloads vorgesehen, die konsistent hohe Leistung und niedrige Latenz mit Storage Premium erfordern. |
| **Prozessserver** |Hierbei handelt es sich um einen lokalen virtuellen oder physischen Server unter Windows Server 2012 R2.<br/><br/> Es wird empfohlen, diesen in demselben Netzwerk und LAN-Segment zu platzieren, in dem sich auch die zu schützenden Computer befinden. Er kann jedoch auch in einem anderen Netzwerk ausgeführt werden, solange die geschützten Computer über L3-Netzwerksichtbarkeit verfügen.<br/><br/> Einrichtung und Registrierung beim Konfigurationsserver werden über das Site Recovery-Portal vorgenommen. |Geschützte Computer senden Replikationsdaten an den lokalen Prozessserver. Der Server verfügt über einen datenträgerbasierten Cache zum Zwischenspeichern der empfangenen Replikationsdaten. Er führt eine Reihe von Aktionen für diese Daten aus.<br/><br/> Daten werden zur Optimierung vor dem Senden an den Masterzielserver zwischengespeichert, komprimiert und verschlüsselt.<br/><br/> Der Server wickelt die Pushinstallation des Mobility Service ab.<br/><br/>  Und er führt die automatische Ermittlung virtueller VMware-Computer durch. |
| **Lokale Computer** |Lokale virtuelle VMware-Computer oder physische Server unter Windows oder Linux |Sie konfigurieren Replikationseinstellungen, die für einen oder mehrere Computer gelten. Ein Failover kann für einen einzelnen Computer durchgeführt werden, gängiger sind jedoch Failover für mehrere Computer im Rahmen eines Wiederherstellungsplans. |
| **Mobilitätsdienst** |Wird auf jedem virtuellen Computer oder physischen Server installiert, den Sie schützen möchten.<br/><br/>  Kann manuell oder vom Prozessserver automatisch mittels Push installiert werden, wenn Sie die Replikation für einen Computer aktivieren. |Der Mobilitätsdienst sendet im Rahmen der ersten Replikation (Resync) Daten an den Prozessserver. Wenn der Computer nach Abschluss von Resync geschützt ist, erfasst der Mobilitätsdienst Schreibvorgänge auf den Datenträger im Arbeitsspeicher und sendet sie an den Prozessserver. Anwendungskonsistenz für Windows-Server wird mithilfe von VSS erzielt. |
| **Azure Site Recovery-Tresor** |Sie erstellen einen Site Recovery-Tresor mit einem Azure-Abonnement und registrieren Server im Tresor. |Der Tresor koordiniert und orchestriert Datenreplikation, Failover und Wiederherstellung zwischen dem lokalen Standort und Azure. |
| **Replikationsmechanismus** |**Über das Internet**: Nutzt einen sicheren SSL/TLS-Kanals über das Internet zum Kommunizieren und zum Replizieren von Daten von lokalen geschützten Servern zu Azure. Dies ist die Standardoption.<br/><br/> **VPN/ExpressRoute**: Nutzt eine VPN-Verbindung zum Kommunizieren und zum Replizieren von Daten zwischen lokalen Servern und Azure. Sie müssen eine Standort-zu-Standort-VPN-Verbindung oder eine ExpressRoute-Verbindung zwischen Ihrem lokalen Standort und dem Azure-Netzwerk einrichten.<br/><br/>  Die gewünschte Replizierung wird während der Bereitstellung von Site Recovery ausgewählt. Wenn der Mechanismus einmal konfiguriert wurde, kann er nicht mehr geändert werden, ohne die Replikation vorhandener Computer zu beeinträchtigen. |Bei keiner der Optionen müssen auf geschützten Computern eingehende Netzwerkports geöffnet werden. Die gesamte Netzwerkkommunikation wird vom lokalen Standort aus initiiert. |

## <a name="capacity-planning"></a>Kapazitätsplanung
Berücksichtigen Sie folgende Hauptaspekte:

* **Quellumgebung**– Die VMware-Infrastruktur sowie Einstellungen und Anforderungen des Quellcomputers.
* **Komponentenserver**– Der Prozessserver, Konfigurationsserver und Masterzielserver.

### <a name="considerations-for-the-source-environment"></a>Überlegungen zur Quellumgebung
* **Maximale Datenträgergröße**– Derzeit beträgt die maximale Größe eines Datenträgers, der an einen virtuellen Computer angefügt werden kann, 1 TB. Deshalb ist die maximale Größe eines Quelldatenträgers, der repliziert werden kann, ebenfalls auf 1 TB beschränkt.
* **Maximale Größe pro Quellcomputer**– Die maximale Größe eines einzelnen Quellcomputers mit einer für den Masterzielserver bereitgestellten D14-Instanz beträgt 31 TB (mit 31 Datenträgern).
* **Anzahl von Quellen pro Masterzielserver**– Mit einem einzelnen Masterzielserver können mehrere Quellcomputer geschützt werden. Ein einzelner Quellcomputer kann jedoch nicht über mehrere Masterzielserver geschützt werden, da bei der Datenträgerreplikation eine virtuelle Festplatte, die die Größe des Datenträgers widerspiegelt, in Azure-BLOB-Speicher erstellt und als Datenträger mit dem Masterzielserver verbunden wird.  
* **Maximale tägliche Änderungsrate pro Quellcomputer**– Bei der empfohlenen Änderungsrate pro Quellcomputer müssen drei Faktoren berücksichtigt werden. Im Hinblick auf den Zieldatenträger sind für jeden Vorgang auf dem Quellcomputer zwei IOPS auf dem Zieldatenträger erforderlich. Der Grund dafür ist, dass auf dem Zieldatenträger ein Lesevorgang für alte Daten und ein Schreibvorgang für neue Daten erfolgen. 
  * **Vom Prozessserver unterstützte tägliche Änderungsrate**– Ein Quellcomputer kann nicht mehrere Prozessserver umfassen. Ein einzelner Prozessserver unterstützt eine tägliche Änderungsrate von bis zu 1 TB. Daher beträgt die maximale Datenänderungsrate pro Tag, die für einen Quellcomputer unterstützt wird, 1 TB.
  * **Maximaler vom Zieldatenträger unterstützter Durchsatz**– Die maximale Codeänderung pro Quelldatenträger darf nicht mehr als 144 GB/Tag (mit einer Schreibgröße von 8 K) betragen. Informationen zum Durchsatz und den IOPS des Ziels für verschiedene Schreibgrößen finden Sie in der Tabelle im Abschnitt zum Masterzielserver. Diese Zahl muss durch zwei dividiert werden, da jede Quell-IOPS 2 IOPS auf dem Zieldatenträger generiert. Informieren Sie sich über [Azure-Skalierbarkeit und -Leistungsziele](../storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) beim Konfigurieren des Ziels für Storage Premium-Konten.
  * **Maximaler vom Speicherkonto unterstützter Durchsatz**– Ein Quellcomputer kann nicht mehrere Speicherkonten umfassen. Da ein Speicherkonto bis zu 20.000 Anforderungen pro Sekunde annimmt und jede Quell-IOPS 2 IOPS auf dem Masterzielserver generiert, wird empfohlen, die Anzahl der IOPS für den Quellcomputer auf 10.000 zu beschränken. Informieren Sie sich über [Azure-Skalierbarkeit und -Leistungsziele](../storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) beim Konfigurieren der Quelle für Storage Premium-Konten.

### <a name="considerations-for-component-servers"></a>Überlegungen zu Komponentenservern
In Tabelle 1 sind die Größen der virtuellen Computer für den Konfigurationsserver und Masterzielserver zusammengefasst.

| **Komponente** | **Bereitgestellte Azure-Instanzen** | **Kerne** | **Arbeitsspeicher** | **Max. Anzahl Datenträger** | **Datenträgergröße** |
| --- | --- | --- | --- | --- | --- |
| Konfigurationsserver |Standard A3 |4 |7 GB |8 |1023 GB |
| Masterzielserver |Standard A4 |8 |14 GB |16 |1023 GB |
| Standard D14 |16 |112 GB |32 |1023 GB | |
| Standard DS4 |8 |28 GB |16 |1023 GB | |

**Tabelle 1**

#### <a name="process-server-considerations"></a>Überlegungen zu Prozessservern
Die Größe von Prozessservern hängt im Allgemeinen von der täglichen Änderungsrate aller geschützten Workloads ab.

* Sie benötigen ausreichend Rechenleistung zum Ausführen von Aufgaben wie z. B. Inlinekomprimierung und Verschlüsselung.
* Der Prozessserver verwendet einen datenträgerbasierten Cache. Stellen Sie sicher, dass der empfohlene Cachespeicherplatz und Datenträgerdurchsatz verfügbar sind, um bei einem Netzwerkengpass oder -ausfall das Speichern geänderter Daten zu erleichtern.
* Stellen Sie ausreichend Bandbreite sicher, damit der Prozessserver die Daten auf den Masterzielserver hochladen kann, um kontinuierlichen Datenschutz zu bieten.

Tabelle 2 enthält eine Zusammenfassung der Richtlinien für den Prozessserver.

| **Datenänderungsrate** | **CPU** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Durchsatz des Cachedatenträgers** | **Eingangs-/Ausgangsbandbreite** |
| --- | --- | --- | --- | --- | --- |
| < 300 GB |4 vCPUs (2 Sockets * 2 Kerne mit @2,5 GHz) |4 GB |600 GB |7 bis 10 MB pro Sekunde |30 Mbit/s bzw. 21 Mbit/s |
| 300 bis 600 GB |8 vCPUs (2 Sockets * 4 Kerne @ 2,5 GHz) |6 GB |600 GB |11 bis 15 MB pro Sekunde |60 Mbit/s bzw. 42 Mbit/s |
| 600 GB bis 1 TB |12 vCPUs (2 Sockets * 6 Kerne @ 2,5 GHz) |8 GB |600 GB |16 bis 20 MB pro Sekunde |100 Mbit/s bzw. 70 Mbit/s |
| > 1 TB |Bereitstellen eines weiteren Prozessservers | | | | |

**Tabelle 2**

Hinweis:

* Die Eingangsbandbreite ist die Downloadbandbreite (Intranet zwischen Quellcomputer und Prozessserver).
* Die Ausgangsbandbreite ist die Uploadbandbreite (Internet zwischen dem Prozessserver und dem Masterzielserver). Bei den Werten für die Ausgangsbandbreite wird eine durchschnittliche Prozessserverkomprimierung von 30 % angenommen.
* Für alle Prozessserver wird als Cachedatenträger ein eigener Betriebssystemdatenträger von mindestens 128 GB empfohlen.
* Als Benchmark für den Durchsatz des Cachedatenträgers wurde folgender Speicher verwendet: 8 SAS-Laufwerke mit 10.000 U/min und RAID 10-Konfiguration.

#### <a name="configuration-server-considerations"></a>Überlegungen zu Konfigurationsservern
Jeder Konfigurationsserver unterstützt bis zu 100 Quellcomputer mit 3-4 Volumes. Bei größeren Bereitstellungen empfiehlt sich die Bereitstellung eines weiteren Konfigurationsservers. Die Standardeigenschaften der virtuellen Computer des Konfigurationsservers sind in Tabelle 1 angegeben.

#### <a name="master-target-server-and-storage-account-considerations"></a>Überlegungen zu Masterzielservern und Speicherkonten
Der Speicher für jeden Masterzielserver setzt sich aus einem Betriebssystemdatenträger, einem Aufbewahrungsvolume und Datenträgern zusammen. Das Aufbewahrungslaufwerk verwaltet das Journal der Datenträgeränderungen für die im Site Recovery-Portal definierte Aufbewahrungsdauer.  Die Eigenschaften virtueller Computer des Masterzielservers sind in Tabelle 1 angegeben. In Tabelle 3 wird die Verwendung der Datenträger von A4 gezeigt.

| **Instanz** | **Betriebssystemdatenträger** | **Aufbewahrung** | **Datenträger** |
| --- | --- | --- | --- |
| **Aufbewahrung** |**Datenträger** | | |
| Standard A4 |1 Datenträger (1 * 1023 GB) |1 Datenträger (1 * 1023 GB) |15 Datenträger (15 * 1023 GB) |
| Standard D14 |1 Datenträger (1 * 1023 GB) |1 Datenträger (1 * 1023 GB) |31 Datenträger (15 * 1023 GB) |
| Standard DS4 |1 Datenträger (1 * 1023 GB) |1 Datenträger (1 * 1023 GB) |15 Datenträger (15 * 1023 GB) |

**Tabelle 3**

Die Kapazitätsplanung für den Masterzielserver hängt von folgenden Faktoren ab:

* Leistung und Einschränkungen des Azure-Speichers
  * Die maximale Anzahl der Datenträger mit hoher Auslastung für einen virtuellen Computer im Standard-Tarif liegt bei etwa 40 Datenträgern (20.000/500 IOPS pro Datenträger) in einem einzigen Speicherkonto. Informieren Sie sich über [Skalierbarkeitsziele für Standardspeicherkonten](../storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) und für [Storage Premium-Konten](../storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
* Tägliche Änderungsrate
* Speicher des Aufbewahrungsvolumes

Beachten Sie Folgendes:

* Ein Quellcomputer kann nicht mehrere Speicherkonten umfassen. Dies gilt für die Datenträger, die für die ausgewählten Speicherkonten verwendet werden, wenn Sie den Schutz konfigurieren. Der Betriebssystem- und Aufbewahrungsdatenträger werden in der Regel automatisch für das bereitgestellte Speicherkonto verwendet.
* Das erforderliche Aufbewahrungsspeichervolume hängt von der täglichen Änderungsrate und der Anzahl der Aufbewahrungstage ab. Erforderlicher Aufbewahrungsspeicher pro Masterzielserver = gesamte Codeänderung von Quellcomputer pro Tag * Anzahl Aufbewahrungstage.
* Jeder Masterzielserver verfügt über nur ein Aufbewahrungsvolume. Das Datenträgervolume wird von den an den Masterzielserver angefügten Datenträgern gemeinsam verwendet. Beispiel:
  * Wenn ein Quellcomputer über 5 Datenträger verfügt und jeder Datenträger auf dem Quellcomputer 120 IOPS (mit einer Größe von 8K) generiert, ergibt dies 240 IOPS pro Datenträger (2 Vorgänge auf dem Zieldatenträger pro Quell-E/A). Der Wert von 240 IOPS liegt innerhalb der Azure-Beschränkung von 500 IOPS pro Datenträger.
  * Auf dem Datenträger ergeben sich daraus 120 * 5 = 600 IOPS, und dies kann zu einem Engpass führen. In diesem Fall empfiehlt es sich, dem Aufbewahrungsvolume weitere Datenträger hinzuzufügen und dieses als RAID-Stripekonfiguration auf mehrere Laufwerke zu verteilen. Dadurch wird die Leistung verbessert, da die IOPS auf mehrere Laufwerke verteilt sind. Dem Aufbewahrungsvolume sollte die folgende Anzahl von Laufwerken hinzugefügt werden:
    * IOPS gesamt aus der Quellumgebung / 500.
    * Codeänderung gesamt pro Tag aus der Quellumgebung (unkomprimiert) / 287 GB. 287 GB beträgt der maximale Durchsatz, der von einem Zieldatenträger pro Tag unterstützt wird. Dieses Maß variiert basierend auf der Schreibgröße mit einem Faktor von 8K, da in diesem Fall als Schreibgröße 8K angenommen wird. Wenn z. B. die Schreibgröße 4K beträgt, ist der Durchsatz 287/2. Und wenn die Schreibgröße 16K beträgt, ist der Durchsatz 287*2.
* Erforderliche Anzahl von Speicherkonten = Quell-IOPS insgesamt /10.000.

## <a name="before-you-start"></a>Vorbereitung
| **Komponente** | **Anforderungen** | **Details** |
| --- | --- | --- |
| **Azure-Konto** |Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. | |
| **Azure-Speicher** |Sie benötigen ein Azure-Speicherkonto, um replizierte Daten zu speichern.<br/><br/> Bei dem Konto muss es sich um ein [georedundantes Standardspeicherkonto](../storage/storage-redundancy.md#geo-redundant-storage) oder um ein [Storage Premium-Konto](../storage/storage-premium-storage.md) handeln.<br/><br/> Es muss sich in der gleichen Region wie der Azure Site Recovery-Dienst befinden und dem gleichen Abonnement zugeordnet sein. Eine Verschiebung von mit dem [neuen Azure-Portal](../storage/storage-create-storage-account.md) erstellten Speicherkonten über Ressourcengruppen hinweg wird nicht unterstützt.<br/><br/> Weitere Informationen finden Sie unter [Einführung in Microsoft Azure Storage](../storage/storage-introduction.md). | |
| **Azure Virtual Network** |Sie benötigen ein virtuelles Azure-Netzwerk, in dem der Konfigurationsserver und der Masterzielserver bereitgestellt werden. Es muss in demselben Abonnement und in derselben Region enthalten sein wie der Azure Site Recovery-Tresor. Wenn Sie Daten über eine ExpressRoute- oder VPN-Verbindung replizieren möchten, muss das virtuelle Azure-Netzwerk mit Ihrem lokalen Netzwerk per ExpressRoute-Verbindung oder Standort-zu-Standort-VPN verbunden sein. | |
| **Azure-Ressourcen** |Stellen Sie sicher, dass Sie genügend Azure-Ressourcen für die Bereitstellung aller Komponenten besitzen. Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements](../azure-subscription-service-limits.md). | |
| **Azure Virtual Machines** |Die zu schützenden virtuellen Computer müssen die [Voraussetzungen für Azure](site-recovery-best-practices.md) erfüllen.<br/><br/> **Anzahl der Datenträger** – Bis zu 31 Datenträger können auf einem einzelnen geschützten Server unterstützt werden.<br/><br/> **Datenträgergrößen** – Die Kapazität der einzelnen Datenträger sollte 1.023 GB nicht überschreiten.<br/><br/> **Clustering** – Gruppierte Server werden nicht unterstützt.<br/><br/> **Start** – Das Starten über UEFI (Unified Extensible Firmware Interface)/EFI (Extensible Firmware Interface) wird nicht unterstützt.<br/><br/> **Volumes** – Mit Bitlocker verschlüsselte Volumes werden nicht unterstützt.<br/><br/> **Servernamen**: Namen müssen zwischen einem und 63 Zeichen (Buchstaben, Zahlen und Bindestriche) enthalten. Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. Nachdem ein Computer geschützt wurde, können Sie den Azure-Namen ändern. | |
| **Konfigurationsserver** |In Ihrem Abonnement wird für den Konfigurationsserver auf der Grundlage eines Azure Site Recovery Windows Server 2012 R2-Katalogimages ein virtueller Standard-A3-Computer erstellt. Er wird als erste Instanz in einem neuen Clouddienst erstellt. Wenn Sie als Verbindungstyp für den Konfigurationsserver die Option „Öffentliches Internet“ auswählen, wird der Clouddienst mit einer reservierten öffentlichen IP-Adresse erstellt.<br/><br/>  Der Installationspfad darf nur Zeichen der englischen Sprache enthalten. | |
| **Masterzielserver** |Virtueller Azure-Computer (Standard A4, D14 oder DS4).<br/><br/> Der Installationspfad darf nur Zeichen der englischen Sprache enthalten. Für einen Masterzielserver unter Linux sollte der Pfad z.B. **/usr/local/ASR** lauten. | |
| **Prozessserver** |Sie können den Prozessserver auf einem physischen oder virtuellen Computer mit Windows Server 2012 R2 mit den neuesten Updates bereitstellen. Führen Sie die Installation auf „C:/“ durch.<br/><br/> Es wird empfohlen, dass Sie den Server im gleichen Netzwerk und Subnetz wie die Computer platzieren, die Sie schützen möchten.<br/><br/>  Installieren Sie VMware vSphere CLI 5.5.0 auf dem Prozessserver. Die Komponente VMware vSphere CLI ist auf dem Prozessserver erforderlich, um virtuelle Computer, die von einem vCenter-Server verwaltet werden, oder virtuelle Computer, die auf einem ESXi-Host ausgeführt werden, zu ermitteln.<br/><br/>  Der Installationspfad darf nur Zeichen der englischen Sprache enthalten.<br/><br/>  Das ReFS-Dateisystem wird nicht unterstützt. | |
| **VMware** |Ein VMware vCenter-Server zur Verwaltung Ihrer VMware vSphere-Hypervisoren. Auf diesem Server muss vCenter 5.1 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>  Mindestens ein vSphere-Hypervisor mit zu schützenden virtuellen VMware-Computern. Auf dem Hypervisor muss ESX/ESXi 5.1 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>  Auf virtuellen VMware-Computern müssen VMware-Tools installiert sein und ausgeführt werden. | |
| **Windows-Computer** |Geschützte physische Server oder virtuelle VMware-Computer unter Windows müssen eine Reihe von Voraussetzungen erfüllen.<br/><br/> Ein unterstütztes 64-Bit-Betriebssystem: **Windows Server 2012 R2**, **Windows Server 2012** oder **Windows Server 2008 R2 mit SP1 oder höher**.<br/><br/> Hostname, Bereitstellungspunkte, Gerätenamen, Windows-Systempfad (etwa „C:\Windows“) dürfen nur Zeichen der englischen Sprache enthalten.<br/><br/> Das Betriebssystem sollte auf Laufwerk C:\ installiert werden.<br/><br/>  Nur Basisdatenträger werden unterstützt. Dynamische Datenträger werden nicht unterstützt.<br/><br/> Die Firewallregeln auf geschützten Computern sollten diesen den Zugriff auf den Konfigurations- und den Masterzielserver in Azure ermöglichen.p><p>Für die Pushinstallation des Mobilitätsdiensts auf Windows-Servern muss ein Administratorkonto (lokaler Administrator auf dem Windows-Computer) angegeben werden. Wenn das bereitgestellte Konto kein Domänenkonto ist, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Fügen Sie hierzu unter "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" den Registrierungseintrag "LocalAccountTokenFilterPolicy DWORD" mit dem Wert 1 hinzu. Um den Registrierungseintrag über eine Befehlszeilenschnittstelle hinzuzufügen, öffnen Sie eine CMD- oder PowerShell-Befehlszeilenschnittstelle, und geben Sie **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** ein. [Erfahren Sie mehr über die Zugriffssteuerung](https://msdn.microsoft.com/library/aa826699.aspx).<br/><br/>  Wenn virtuelle Windows-Computer in Azure nach einem Failover mit Remotedesktop eine Verbindung herstellen sollen, stellen Sie sicher, dass Remotedesktop für den lokalen Computer aktiviert ist. Wenn Sie die Verbindung nicht über ein VPN herstellen, müssen die Firewallregeln Remotedesktopverbindungen über das Internet ermöglichen. | |
| **Linux-Computer** |Ein unterstütztes 64-Bit-Betriebssystem: **Centos 6.4, 6.5, 6.6**, **Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Die Firewallregeln auf geschützten Computern sollten diesen den Zugriff auf den Konfigurations- und den Masterzielserver in Azure ermöglichen.<br/><br/> Dateien vom Typ „/etc/hosts“ auf geschützten Computern sollten Einträge enthalten, die den Namen des lokalen Hosts IP-Adressen zuordnen, die allen NICs zugeordnet sind. <br/><br/> Wenn Sie nach einem Failover eine Verbindung mit virtuellen Azure-Computern unter Linux über einen Secure Shell-Client (SSH) herstellen möchten, stellen Sie sicher, dass der Secure Shell-Dienst auf dem geschützten Computer beim Systemstart automatisch gestartet wird und dass die Firewallregeln eine SSH-Verbindung zulassen.<br/><br/> Der Hostname, Bereitstellungspunkte, Gerätenamen und Linux-Systempfade und -Dateinamen (z. B./etc/, /usr) sollten nur englische Zeichen umfassen.<br/><br/> Der Schutz kann für lokale Computer mit dem folgenden Speicher aktiviert werden:<br>Dateisystem: EXT3, ETX4, ReiserFS, XFS<br>Multipfad-Software-Gerätemapper (Multipfad)<br>Volume-Manager: LVM2<br>Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. | |
| **Drittanbieter** |Einige Bereitstellungskomponenten in diesem Szenario sind von Drittanbietersoftware abhängig, um ordnungsgemäß zu funktionieren. Eine vollständige Liste finden Sie unter [Hinweise und Informationen zu Drittanbietersoftware](#third-party) | |

### <a name="network-connectivity"></a>Netzwerkverbindung
Beim Konfigurieren der Netzwerkkonnektivität zwischen Ihrem lokalen Standort und dem virtuellen Azure-Netzwerk, auf dem die Infrastrukturkomponenten (Konfigurationsserver, Masterzielserver) bereitgestellt werden, haben Sie zwei Möglichkeiten. Sie müssen sich für eine Netzwerkverbindungsoption entscheiden, bevor Sie den Konfigurationsserver bereitstellen können. Sie müssen diese Einstellung zum Zeitpunkt der Bereitstellung auswählen. Bei Bedarf können Sie sie später wieder ändern.

**Internet:** Die Kommunikation und Replikation von Daten zwischen den lokalen Servern (Prozessserver, geschützte Computer) und den Komponentenservern der Azure-Infrastruktur (Konfigurationsserver, Masterzielserver) erfolgt über eine sichere SSL/TLS-Verbindung. Sie verläuft vom lokalen Server zu den öffentlichen Endpunkten auf dem Konfigurationsserver und dem Masterzielserver. (Die einzige Ausnahme ist die Verbindung zwischen dem Prozessserver und dem Masterzielserver auf TCP-Port 9080, die unverschlüsselt ist. Über diese Verbindung werden nur Steuerinformationen ausgetauscht, die sich auf das Replikationsprotokoll für die Einrichtung der Replikation beziehen.)

![Bereitstellungsdiagramm: Internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN:**Die Kommunikation und Replikation von Daten zwischen den lokalen Servern (Prozessserver, geschützte Computer) und den Komponentenservern der Azure-Infrastruktur (Konfigurationsserver, Masterzielserver) erfolgt über eine VPN-Verbindung zwischen Ihrem lokalen Netzwerk und dem virtuellen Azure-Netzwerk, in dem der Konfigurationsserver und die Masterzielserver bereitgestellt werden. Stellen Sie sicher, dass Ihr lokales Netzwerk mit dem virtuellen Azure-Netzwerk über eine ExpressRoute-Verbindung oder eine Standort-zu-Standort-VPN-Verbindung verbunden ist.

![Bereitstellungsdiagramm: VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)

## <a name="step-1-create-a-vault"></a>Schritt 1: Erstellen eines Tresors
1. Melden Sie sich im [Verwaltungsportal](https://portal.azure.com)an.
2. Erweitern Sie **Data Services** > **Recovery Services**, und klicken Sie auf **Site Recovery-Tresor**.
3. Klicken Sie auf **Neu erstellen** > **Schnellerfassung**.
4. Geben Sie unter **Name**einen Anzeigenamen ein, über den der Tresor identifiziert wird.
5. Wählen Sie unter **Region**die geografische Region für den Tresor aus. Sie finden eine Liste der unterstützten Regionen unter Geografische Verfügbarkeit auf der Seite [Azure Site Recovery – Preisübersicht](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Klicken Sie auf **Tresor erstellen**.

    ![Neuer Tresor](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Überprüfen Sie auf der Statusleiste, ob der Tresor erfolgreich erstellt wurde. Der Tresor wird auf der Hauptseite von **Recovery Services** als **aktiv** aufgelistet.

## <a name="step-2-deploy-a-configuration-server"></a>Schritt 2: Bereitstellen eines Konfigurationsservers
### <a name="configure-server-settings"></a>Konfigurieren der Servereinstellungen
1. Klicken Sie auf der Seite **Recovery Services** auf den Tresor, um die Seite „Schnellstart“ zu öffnen. Schnellstart kann auch jederzeit über das Symbol geöffnet werden.

    ![Schnellstart-Symbol](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)
2. Wählen Sie in der Dropdownliste den Eintrag **Zwischen einem lokalen Standort mit VMware-/physischen Servern und Azure**.
3. Klicken Sie unter **Zielressourcen (Azure-Ressourcen) vorbereiten** auf **Konfigurationsserver bereitstellen**.

    ![Konfigurationsserver bereitstellen](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)
4. Geben Sie unter **Details des neuen Konfigurationsservers** Folgendes an:

   * Einen Namen für den Konfigurationsserver und die Anmeldeinformationen für die Verbindung mit dem Server.
   * Wählen Sie im Dropdownmenü als Netzwerkkonnektivitätstyp die Option **Öffentliches Internet** oder **VPN**. Diese Einstellung kann später nicht mehr geändert werden.
   * Wählen Sie das Azure-Netzwerk aus, in dem sich der Server befinden soll. Vergewissern Sie sich bei Verwendung von VPN, dass das Azure-Netzwerk ordnungsgemäß mit Ihrem lokalen Netzwerk verbunden ist.
   * Geben Sie die interne IP-Adresse und das Subnetz an, die dem Server zugewiesen werden sollen. Beachten Sie, dass in jedem Subnetz die ersten vier IP-Adressen für die interne Verwendung durch Azure reserviert sind. Verwenden Sie eine beliebige andere verfügbare IP-Adresse.

     ![Konfigurationsserver bereitstellen](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)
5. Wenn Sie auf **OK** klicken, wird ein virtueller Standard-A3-Computer basierend auf einem Azure Site Recovery Windows Server 2012 R2-Katalogimage in Ihrem Abonnement für den Konfigurationsserver erstellt. Er wird als erste Instanz in einem neuen Clouddienst erstellt. Wenn Sie sich für die Internetoption entschieden haben, wird der Clouddienst mit einer reservierten öffentlichen IP-Adresse erstellt. Sie können den Fortschritt auf der Registerkarte **Aufträge** überwachen.

    ![Fortschritt überwachen](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)
6. Notieren Sie sich bei Verwendung einer Internetverbindung nach dem Bereitstellen des Konfigurationsservers die ihm zugewiesene öffentliche IP-Adresse (zu finden auf der Seite **Virtuelle Computer** im Azure-Portal). Notieren Sie dann auf der Registerkarte **Endpunkte** den öffentlichen HTTPS-Port, der dem privaten Port 443 zugeordnet ist. Diese Informationen benötigen Sie später, wenn Sie den Masterzielserver und den Prozessserver beim Konfigurationsserver registrieren. Der Konfigurationsserver wird mit den folgenden Endpunkten bereitgestellt:

   * HTTPS: Die über das Internet abgewickelte Kommunikation zwischen Komponentenservern und Azure wird mithilfe eines öffentlichen Ports verwaltet. Der private Port 443 wird zum Koordinieren der Kommunikation zwischen Komponentenservern und Azure über ein VPN verwendet.
   * Benutzerdefiniert: Für die über das Internet abgewickelte Kommunikation mit dem Failbacktool wird ein öffentlicher Port verwendet. Der private Port 9443 wird für die Kommunikation mit dem Failbacktool über ein VPN verwendet.
   * PowerShell: privater Port 5986
   * Remotedesktop: privater Port 3389

   ![VM-Endpunkte](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

   > [!WARNING]
   > Die öffentliche oder private Portnummer von Endpunkten, die im Zuge der Bereitstellung des Konfigurationsservers erstellt wurden, darf nicht geändert oder gelöscht werden.
   >
   >

Der Konfigurationsserver wird in einem automatisch erstellten Azure-Clouddienst mit einer reservierten IP-Adresse bereitgestellt. Durch die reservierte Adresse wird sichergestellt, dass die IP-Adresse des Konfigurationsserver-Clouddiensts nach einem Neustart der virtuellen Computer (einschließlich des Konfigurationsservers) im Clouddienst unverändert bleibt. Wenn der Konfigurationsserver außer Betrieb gesetzt wird und die Reservierung der öffentlichen IP-Adresse nicht manuell rückgängig gemacht wird, bleibt die öffentliche IP-Adresse reserviert. Standardmäßig sind bis zu 20 reservierte öffentliche IP-Adressen pro Abonnement zulässig. [hier](../virtual-network/virtual-networks-reserved-private-ip.md) .

### <a name="register-the-configuration-server-in-the-vault"></a>Registrieren des Konfigurationsservers im Tresor
1. Klicken Sie auf der Seite **Schnellstart** auf **Zielressourcen vorbereiten** > **Registrierungsschlüssel herunterladen**. Die Schlüsseldatei wird automatisch generiert. Sie ist nach der Erstellung 5 Tage lang gültig. Kopieren Sie sie auf den Konfigurationsserver.
2. Wählen Sie unter **Virtuelle Computer** den Konfigurationsserver aus der Liste der virtuellen Computer aus. Öffnen Sie die Registerkarte **Dashboard**, und klicken Sie auf **Verbinden**. **Öffnen** Sie die heruntergeladene RDP-Datei, um sich über Remotedesktop beim Konfigurationsserver anzumelden. Verwenden Sie bei Verwendung von VPN für eine Remotedesktopverbindung über den lokalen Standort die interne IP-Adresse (also die Adresse, die Sie beim Bereitstellen des Konfigurationsservers angegeben haben). Wenn Sie sich zum ersten Mal anmelden, wird der Azure Site Recovery-Assistent zum Einrichten des Konfigurationsservers automatisch ausgeführt.

    ![Registrierung](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)
3. Klicken Sie unter **Installation von Drittanbietersoftware** auf **Ich stimme zu**, um MySQL herunterzuladen und zu installieren.

    ![MySQL-Installation](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)
4. Erstellen Sie unter **MySQL-Server-Details** Anmeldeinformationen, um sich bei MySQL Server anzumelden.

    ![MySQL-Anmeldeinformationen](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)
5. Geben Sie unter **Interneteinstellungen** an, wie der Konfigurationsserver eine Verbindung mit dem Internet herstellt. Beachten Sie Folgendes:

   * Wenn Sie einen benutzerdefinierten Proxy verwenden möchten, sollten Sie diesen vor der Installation des Anbieters einrichten.
   * Wenn Sie auf **Weiter** klicken, wird ein Test ausgeführt, um die Proxyverbindung zu überprüfen.
   * Wenn Sie einen benutzerdefinierten Proxy verwenden oder wenn der Standardproxy Authentifizierung erfordert, müssen Sie die Proxydetails eingeben, einschließlich Adresse, Port und Anmeldeinformationen.
   * Über den Proxy sollte Zugriff auf die folgenden URLs möglich sein:
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Wenn Sie auf IP-Adressen basierende Firewallregeln verwenden, sollten Sie sicherstellen, dass die Regeln die Kommunikation zwischen dem Konfigurationsserver und den unter [IP-Bereiche des Azure-Rechenzentrums](https://msdn.microsoft.com/library/azure/dn175718.aspx) beschriebenen IP-Adressen sowie das HTTPS (443)-Protokoll zulassen. Fügen Sie die IP-Adressbereiche der zu verwendenden Azure-Region sowie die IP-Adressbereiche der westlichen USA einer Positivliste hinzu.

     ![Proxyregistrierung](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)
6. Geben Sie unter **Einstellungen für die Lokalisierung von Anbieterfehlermeldungen** an, in welcher Sprache Fehlermeldungen angezeigt werden sollen.

    ![Fehlermeldungsregistrierung](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)
7. Suchen Sie auf der Seite **Azure Site Recovery-Registrierung** die Schlüsseldatei, die Sie auf den Server kopiert haben, und wählen Sie sie aus.

    ![Schlüsseldateiregistrierung](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)
8. Wählen Sie auf der Abschlussseite des Assistenten die folgenden Optionen aus:

   * Wählen Sie **Dialogfeld für die Kontoverwaltung öffnen** aus, um anzugeben, dass nach dem Beenden des Assistenten das Dialogfeld „Konten verwalten“ geöffnet werden soll.
   * Wählen Sie **Create a desktop icon for Cspsconfigtool** (Desktopsymbol für Cspsconfigtool erstellen) aus, um auf dem Konfigurationsserver eine Desktopverknüpfung hinzuzufügen, damit Sie jederzeit das Dialogfeld **Konten verwalten** öffnen können, ohne den Assistenten erneut auszuführen.

     ![Registrierung abschließen](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)
9. Klicken Sie auf **Fertig stellen**, um den Assistenten abzuschließen. Eine Passphrase wird generiert. Kopieren Sie sie an einen sicheren Ort. Sie benötigen diese Informationen, um den Prozess- und den Masterzielserver beim Konfigurationsserver zu authentifizieren und zu registrieren. Außerdem dienen sie zur Sicherstellung der Kanalintegrität bei der Kommunikation mit dem Konfigurationsserver. Sie können die Passphrase neu generieren. In diesem Fall müssen Sie jedoch den Masterzielserver und den Prozessserver anhand der neuen Passphrase neu registrieren.

    ![Passphrase](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Nach der Registrierung wird der Konfigurationsserver auf der Seite **Konfigurationsserver** im Tresor aufgeführt.

### <a name="set-up-and-manage-accounts"></a>Einrichten und Verwalten von Konten
Site Recovery fordert während der Bereitstellung Anmeldeinformationen für die folgenden Aktionen an:

* Ein VMware-Konto, damit Site Recovery automatisch virtuelle Computer auf vCenter-Servern oder vSphere-Hosts ermitteln kann.
* Wenn Sie zu schützende Computer hinzufügen, damit Site Recovery den Mobilitätsdienst auf ihnen installieren kann.

Nachdem Sie den Konfigurationsserver registriert haben, können Sie das Dialogfeld **Konten verwalten** öffnen, um Konten hinzuzufügen und zu verwalten, die für diese Aktionen verwendet werden sollen. Es gibt mehrere Möglichkeiten, dies zu tun:

* Öffnen Sie die Verknüpfung, die Sie auf der letzten Seite des Setups des Konfigurationsservers für das Dialogfeld erstellt haben (cspsconfigtool).
* Öffnen Sie das Dialogfeld beim Beenden der Konfigurationsserversetups.

1. Wählen Sie unter **Konten verwalten** click **Konto hinzufügen**. Sie können auch vorhandene Konten ändern und löschen.

    ![Konten verwalten](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)
2. Geben Sie unter **Kontodetails** einen in Azure zu verwendenden Kontonamen und Anmeldeinformationen (Domäne/Benutzername) an.

    ![Konten verwalten](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Herstellen der Verbindung mit dem Konfigurationsserver
Es gibt zwei Möglichkeiten zum Herstellen der Verbindung mit dem Konfigurationsserver:

* Über eine Site-to-Site-VPN-Verbindung oder ExpressRoute-Verbindung
* Über das Internet

Beachten Sie Folgendes:

* Eine Internetverbindung verwendet die Endpunkte des virtuellen Computers in Verbindung mit der öffentlichen virtuellen IP-Adresse des Servers.
* Bei einer VPN-Verbindung wird die interne IP-Adresse des Servers zusammen mit den Ports des privaten Endpunkts verwendet.
* Es kann nur einmal entschieden werden, ob die Verbindung (zum Senden von Steuerungs- und Replikationsdaten) der lokalen Server mit den verschiedenen unter Azure ausgeführten Komponentenservern (Konfigurationsserver, Masterzielserver) über ein VPN oder über das Internet erfolgt. Sie können diese Einstellung später nicht ändern. Wenn Sie die Einstellung ändern, müssen Sie das Szenario erneut bereitstellen und die Computer erneut schützen.  

## <a name="step-3-deploy-the-master-target-server"></a>Schritt 3: Bereitstellen des Masterzielservers
1. Klicken Sie auf **Zielressourcen (Azure-Ressourcen) vorbereiten** > **Masterzielserver bereitstellen**.
2. Geben Sie die Details und Anmeldeinformationen für den Masterzielserver an. Der Server wird im gleichen Azure-Netzwerk bereitgestellt wie der Konfigurationsserver. Wenn Sie zum Abschluss klicken, wird ein virtueller Azure-Computer mit einem Windows- oder Linux-Galerie-Image erstellt.

    ![Zielservereinstellungen](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Beachten Sie, dass in jedem Subnetz die ersten vier IP-Adressen für die interne Verwendung durch Azure reserviert sind. Geben Sie eine beliebige andere verfügbare IP-Adresse an.

> [!NOTE]
> Wählen Sie bei der Konfiguration des Schutzes für Workloads, die konsistent hohe E/A-Leistung und niedrige Latenz erfordern, „Standard DS4“ aus, um E/A-intensive Workloads mit einem [Storage Premium-Konto](../storage/storage-premium-storage.md)zu hosten.
>
>

1. Ein virtueller Windows-Masterzielserver wird mit den folgenden Endpunkten erstellt. Beachten Sie, dass öffentliche Endpunkte nur erstellt werden, wenn die Verbindung über das Internet hergestellt wird.

   * Benutzerdefiniert: Der öffentliche Port wird vom Prozessserver zum Senden von Replikationsdaten über das Internet verwendet. Der private Port 9443 wird vom Prozessserver zum Senden von Replikationsdaten an den Masterzielserver über ein VPN verwendet.
   * Benutzerdefiniert1: Der öffentliche Port wird vom Prozessserver zum Senden von Metadaten über das Internet verwendet. Der private Port 9080 wird vom Prozessserver zum Senden von Metadaten an den Masterzielserver über VPN verwendet.
   * PowerShell: privater Port 5986
   * Remotedesktop: privater Port 3389
2. Ein virtueller Linux-Masterzielserver wird mit den folgenden Endpunkten erstellt. Beachten Sie, dass öffentliche Endpunkte nur erstellt werden, wenn die Verbindung über das Internet hergestellt wird.

   * Benutzerdefiniert: Der öffentliche Port wird vom Prozessserver zum Senden von Replikationsdaten über das Internet verwendet. Der private Port 9443 wird vom Prozessserver zum Senden von Replikationsdaten an den Masterzielserver über ein VPN verwendet.
   * Benutzerdefiniert1: Der öffentliche Port wird vom Prozessserver zum Senden von Metadaten über das Internet verwendet. Der private Port 9080 wird vom Prozessserver zum Senden von Metadaten an den Masterzielserver über VPN verwendet.
   * SSH: Privater Port 22

     > [!WARNING]
     > Sie dürfen die öffentliche oder private Portnummer der Endpunkte, die während der Bereitstellung des Masterzielservers erstellt wurden, nicht ändern oder löschen.
     >
     >
3. Warten Sie unter **Virtuelle Computer** , bis der virtuelle Computer gestartet wird.

   * Notieren Sie sich bei einem Windows-Server die Remotedesktopdetails.
   * Wenn es sich um einen Linux-Server handelt und eine VPN-Verbindung hergestellt wird, notieren Sie sich die interne IP-Adresse des virtuellen Computers. Notieren Sie die öffentliche IP-Adresse, wenn Sie eine Verbindung über das Internet herstellen.
4. Melden Sie sich beim Server an, um die Installation abzuschließen und den Server beim Konfigurationsserver zu registrieren.
5. Wenn Sie Windows verwenden:

   1. Initiieren Sie eine Remotedesktopverbindung mit dem virtuellen Computer. Beim ersten Anmelden wird ein Skript in einem PowerShell-Fenster ausgeführt. Schließen Sie es nicht. Bei Fertigstellung wird das Host-Agent-Konfigurationstool zum Registrieren des Servers automatisch geöffnet.
   2. Geben Sie unter **Host-Agent-Konfiguration** die interne IP-Adresse des Konfigurationsservers und Port 443 an. Sie können die interne Adresse und den privaten Port 443 auch dann verwenden, wenn Sie keine Verbindung per VPN herstellen, da der virtuelle Computer demselben Azure-Netzwerk zugeordnet ist wie der Konfigurationsserver. Lassen Sie **HTTPS verwenden** aktiviert. Geben Sie die Passphrase für den Konfigurationsserver ein, die Sie zuvor notiert haben. Klicken Sie auf **OK** , um den Server zu registrieren. Die NAT-Optionen können ignoriert werden. Sie werden nicht verwendet.
   3. Wenn das Aufbewahrungslaufwerk schätzungsweise mehr als 1 TB erfordert, können Sie das Aufbewahrungsvolume (R:) mit einem virtuellen Datenträger und [Speicherplätzen](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx)

   ![Windows-Masterzielserver](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)
6. Wenn Sie Linux verwenden:

   1. Vergewissern Sie sich vor der Installation des Masterzielservers, dass die neuesten Linux Integration Services (LIS) installiert sind. Die neueste Version von LIS sowie eine Anleitung zur Installation finden Sie [hier](https://www.microsoft.com/download/details.aspx?id=46842). Starten Sie den Computer nach der LIS-Installation neu.
   2. Klicken Sie unter **Zielressourcen (Azure-Ressourcen) vorbereiten** auf **Weitere Software herunterladen und installieren (nur für Linux-Masterzielserver)**. Kopieren Sie die heruntergeladene TAR-Datei mit einem SFTP-Client auf den virtuellen Computer. Alternativ können Sie sich bei dem bereitgestellten Linux-Masterzielserver anmelden und die Datei mithilfe von *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* herunterladen.
   3. Melden Sie sich über einen Secure Shell-Client beim Server an. Wenn Sie über VPN mit dem Azure-Netzwerk verbunden sind, verwenden Sie die interne IP-Adresse. Verwenden Sie andernfalls die externe IP-Adresse und den öffentlichen SSH-Endpunkt.
   4. Extrahieren Sie die Dateien aus dem GZIP-Installationsprogramm, indem Sie folgenden Befehl ausführen: **tar –xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
      ![Linux-Masterzielserver](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
   5. Stellen Sie sicher, dass Sie sich in dem Verzeichnis befinden, in das Sie den Inhalt der TAR-Datei extrahiert haben.
   6. Kopieren Sie die Passphrase für den Konfigurationsserver mit dem Befehl **echo *`<passphrase>`* >passphrase.txt** in eine lokale Datei.
   7. Führen Sie den Befehl **sudo ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i *`<Configuration server internal IP address>`* -p 443 -s y -c https -P passphrase.txt** aus.

      ![Zielserver registrieren](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)
7. Warten Sie etwa zehn bis 15 Minuten, und vergewissern Sie sich dann auf der Seite **Server** > **Konfigurationsserver**, dass der Masterzielserver auf der Registerkarte **Serverdetails** als registriert aufgeführt ist. Wenn Sie Linux verwenden und der Server nicht registriert wurde, führen Sie das Hostkonfigurationstool über „/usr/local/ASR/Vx/bin/hostconfigcli“ erneut aus. Sie müssen die Zugriffsberechtigungen festlegen, indem Sie chmod als root ausführen.

    ![Zielserver überprüfen](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

> [!NOTE]
> Nach Abschluss der Registrierung kann es bis zu 15 Minuten dauern, bis der Masterzielserver im Portal aufgeführt wird. Klicken Sie zur sofortigen Aktualisierung auf der Seite **Aktualisieren** on the **Konfigurationsserver** angegeben.
>
>

## <a name="step-4-deploy-the-on-premises-process-server"></a>Schritt 4: Bereitstellen eines lokalen Prozessservers
Es empfiehlt sich, zunächst eine statische IP-Adresse für den Prozessserver zu konfigurieren, um sicherzustellen, dass diese nach einem Neustart beibehalten wird.

1. Klicken Sie auf „Schnellstart“ und dann auf **Prozessserver lokal installieren** > **Prozessserver herunterladen und installieren**.

    ![Prozessserver installieren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)
2. Kopieren Sie die heruntergeladene ZIP-Datei auf den Server, auf dem Sie den Prozessserver installieren möchten. Die ZIP-Datei enthält zwei Installationsdateien:

   * Microsoft-ASR_CX_TP_8.4.0.0_Windows*
   * Microsoft-ASR_CX_8.4.0.0_Windows*
3. Entpacken Sie das Archiv, und kopieren Sie die Installationsdateien in einen Speicherort auf dem Server.
4. Führen Sie die Installationsdatei **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** aus, und folgen Sie den Anweisungen. Dadurch werden die Drittanbieterkomponenten installiert, die für die Bereitstellung erforderlich sind.
5. Führen Sie dann **Microsoft-ASR_CX_8.4.0.0_Windows*** aus.
6. Wählen Sie auf der Seite **Servermodus** die Option **Prozessserver** aus.
7. Gehen Sie auf der Seite **Umgebungsdetails** folgendermaßen vor:

    - Wenn Sie virtuelle VMware-Computer schützen möchten, klicken Sie auf **Ja**
    - Wenn Sie nur physische Server schützen möchten und folglich VMware vCLI nicht auf dem Prozessserver installiert werden muss, klicken Sie auf **Nein**, und fahren Sie fort.

1. Beachten Sie beim Installieren von VMware vCLI Folgendes:

   * **Es wird nur VMware vSphere CLI 5.5.0 unterstützt**. Der Prozessserver wird mit anderen Versionen oder Updates von vSphere CLI nicht ausgeführt.
   * Laden Sie vSphere CLI 5.5.0 [hier](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
   * Wenn Sie vSphere CLI direkt vor dem Installieren des Prozessservers installiert haben und vSphere CLI von Setup nicht erkannt wird, warten Sie bis zu fünf Minuten lang, bevor Sie Setup erneut starten. Dadurch wird sichergestellt, dass alle für die Ermittlung von vSphere CLI erforderlichen Umgebungsvariablen richtig initialisiert wurden.
2. Wählen Sie unter **NIC-Auswahl für den Prozessserver** den Netzwerkadapter aus, der vom Prozessserver verwendet werden soll.

   ![Adapter auswählen](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)
3. Unter **Konfigurationsserverdetails**:

   * Wenn Sie eine Verbindung über ein VPN herstellen, geben Sie als IP-Adresse die interne IP-Adresse des Konfigurationsservers und 443 für den Port an. Geben Sie andernfalls die öffentliche virtuelle IP-Adresse und den zugeordneten öffentlichen HTTP-Endpunkt.
   * Geben Sie die Passphrase des Konfigurationsservers ein.
   * Deaktivieren Sie **Softwaresignatur des Mobilitätsdiensts** prüfen, wenn Sie zum Installieren des Diensts die automatische Pushfunktion verwenden und die Überprüfung deaktivieren möchten. Die Signaturüberprüfung benötigt eine Internetverbindung vom Prozessserver aus.
   * Klicken Sie auf **Next**.

   ![Konfigurationsserver registrieren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)
4. Wählen Sie unter **Installationslaufwerk auswählen** ein Cachelaufwerk aus. Der Prozessserver benötigt eine Cachelaufwerk mit mindestens 600 GB freiem Speicherplatz. Klicken Sie dann auf **Weiter**.

   ![Konfigurationsserver registrieren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)
5. Beachten Sie, dass Sie möglicherweise den Server neu starten müssen, um die Installation abzuschließen. Wählen Sie unter **Configuration Server** > **Serverdetails** , ob der Prozessserver angezeigt wird und erfolgreich im Tresor registriert wurde.

> [!NOTE]
> Es kann bis zu 15 Minuten nach Abschluss der Registrierung dauern, bis der Prozessserver unter dem Konfigurationsserver aufgeführt wird. Wenn die Ansicht sofort aktualisiert werden soll, aktualisieren Sie den Konfigurationsserver, indem Sie unten auf der Seite "Konfigurationsserver" auf die Schaltfläche "Aktualisieren" klicken.
>
>

![Prozessserver überprüfen](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Wenn Sie beim Registrieren des Prozessservers die Signaturüberprüfung für den Mobilitätsdienst nicht deaktiviert haben, können Sie dies später nachholen:

1. Melden Sie sich beim Prozessserver als Administrator an, und öffnen Sie die Datei "C:\pushinstallsvc\pushinstaller.conf", um sie zu bearbeiten. Fügen Sie im Abschnitt **[PushInstaller.transport]** die folgende Zeile hinzu: **SignatureVerificationChecks=”0”**. Speichern und schließen Sie die Datei.
2. Starten Sie den InMage PushInstall-Dienst neu.

## <a name="step-5-update-site-recovery-components"></a>Schritt 5: Aktualisieren der Site Recovery-Komponenten
Site Recovery-Komponenten werden von Zeit zu Zeit aktualisiert. Neu verfügbare Updates sollten in der folgenden Reihenfolge installiert werden:

1. Konfigurationsserver
2. Prozessserver
3. Masterzielserver
4. Failbacktool (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Abrufen und Installieren der Updates
1. Updates für Konfigurations-, Prozess- und Masterzielserver können über das **Dashboard**von Site Recovery abgerufen werden. Extrahieren Sie für eine Linux-Installation die Dateien aus dem GZIP-Installationsprogramm, und führen Sie zum Installieren des Updates den Befehl „sudo ./install“ aus.
2. [hier](http://go.microsoft.com/fwlink/?LinkID=533813) das neueste Update für das Failbacktool (vContinuum) herunter.
3. Wenn Sie virtuelle Computer oder physische Server ausführen, auf denen der Mobilitätsdienst bereits installiert ist, können Sie die Updates für den Dienst wie folgt abrufen:

   * **Option 1**: Laden Sie die Updates herunter:
     * [Windows Server (nur 64 Bit)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
     * [CentOS 6.4, 6.5, 6.6 (nur 64 Bit)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
     * [Oracle Enterprise Linux 6.4, 6.5 (nur 64 Bit)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
     * [SUSE Linux Enterprise Server SP3 (nur 64 Bit)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
     * Nach Aktualisierung des Prozessservers steht die aktualisierte Version des Mobilitätsdiensts auf dem Prozessserver im Ordner „C:\pushinstallsvc\repository“ zur Verfügung.
   * **Option 2**: Wenn auf einem Computer eine ältere Version des Mobilitätsdiensts installiert ist, können Sie über das Verwaltungsportal ein automatisches Upgrade des Mobilitätsdiensts auf dem Computer ausführen.

     1. Stellen Sie sicher, dass der Prozessserver aktualisiert wird.
     2. Vergewissern Sie sich, dass der geschützte Computer die [Voraussetzungen](#install-the-mobility-service-automatically) für eine automatische Pushinstallation des Mobilitätsdiensts erfüllt, damit das Update ordnungsgemäß funktioniert.
     3. Wählen Sie die Schutzgruppe aus, markieren Sie den geschützten Computer, und klicken Sie auf **Mobility Service aktualisieren**. Diese Schaltfläche ist nur verfügbar, wenn eine neuere Version des Mobilitätsdiensts verfügbar ist.

         ![vCenter-Server auswählen](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Geben Sie unter „Konten auswählen“ das Administratorkonto an, das zum Aktualisieren des Mobilitätsdiensts auf dem geschützten Server verwendet werden soll. Klicken Sie auf „OK“, und warten Sie, bis der ausgelöste Auftrag abgeschlossen ist.

## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Schritt 6: Hinzufügen von vCenter-Servern oder vSphere-Hosts
1. Klicken Sie unter **Server** > **Konfigurationsserver** auf den Konfigurationsserver und anschließend auf **vCenter-Server hinzufügen**, um einen vCenter-Server oder einen vSphere-Host hinzuzufügen.

    ![vCenter-Server auswählen](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)
2. Geben Sie Details für den Server oder Host an, und wählen Sie den Prozessserver aus, der zu dessen Ermittlung verwendet werden soll.

   * Wenn der vCenter-Server nicht über den Standardport 443 ausgeführt wird, geben Sie die Nummer des Ports an, über den der vCenter-Server ausgeführt wird.
   * Der Prozessserver muss sich im gleichen Netzwerk befinden wie der vCenter-Server/vSphere-Host und über VMware vSphere CLI 5.5.0 verfügen.

     ![Einstellungen des vCenter-Servers](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)
3. Nach Abschluss der Erkennung wird der vCenter-Server unter den Serverkonfigurationsdetails aufgeführt.

    ![Einstellungen des vCenter-Servers](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)
4. Falls Sie beim Hinzufügen des Servers oder Hosts kein Administratorkonto verwenden, vergewissern Sie sich, dass das Konto über die folgenden Berechtigungen verfügt:

   * Für vCenter-Konten müssen die Rechte "Datencenter", "Datenspeicher", "Ordner", "Host", "Netzwerk", "Ressourcen", "Speicheransichten", "Virtuelle Maschine" und "vSphere Distributed Switch" aktiviert sein.
   * Für vSphere-Hostkonten müssen die Rechte „Datencenter“, „Datenspeicher“, „Ordner“, „Host“, „Netzwerk“, „Ressourcen“, „Virtueller Computer“ und „vSphere Distributed Switch“ aktiviert sein.

## <a name="step-7-create-a-protection-group"></a>Schritt 7: Erstellen einer Schutzgruppe
1. Öffnen Sie **Geschützte Elemente** > **Schutzgruppe** > **Schutzgruppe erstellen**.

    ![Schutzgruppe erstellen](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)
2. Geben Sie auf der Seite **Schutzgruppeneinstellungen festlegen** einen Namen für die Gruppe an, und wählen Sie den Konfigurationsserver aus, auf dem Sie die Gruppe erstellen möchten.

    ![Schutzgruppeneinstellungen](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)
3. Konfigurieren Sie auf der Seite **Replikationseinstellungen festlegen** die Replikationseinstellungen, die für alle Computer in der Gruppe verwendet werden.

    ![Schutzgruppenreplikation](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)
4. Einstellungen:

   * **Multi-VM-Konsistenz:**Wenn Sie diese Option aktivieren, werden freigegebene anwendungskonsistente Wiederherstellungspunkte auf den Computern in der Schutzgruppe erstellt. Diese Einstellung ist besonders wichtig, wenn auf allen Computern in der Schutzgruppe dieselbe Arbeitsauslastung ausgeführt wird. Alle Computer werden an demselben Datenpunkt wiederhergestellt. Steht nur bei Windows-Servern zur Verfügung.
   * **RPO-Schwellenwert:**Warnungen werden generiert, wenn die Replikations-RPO für den kontinuierlichen Datenschutz den konfigurierten RPO-Schwellenwert überschreitet.
   * **Aufbewahrungszeitraum des Wiederherstellungspunkts:**Gibt die Aufbewahrungsdauer an. Geschützte Computer können innerhalb dieses Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
   * **Anwendungskonsistente Momentaufnahmehäufigkeit:**Gibt an, wie oft Wiederherstellungspunkte erstellt werden, die anwendungskonsistente Momentaufnahmen enthalten.

Auf der Seite **Geschützte Elemente** können Sie die Erstellung der Schutzgruppen überwachen.

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Schritt 8: Einrichten von Computern, die Sie schützen möchten
Sie müssen den Mobilitätsdienst auf virtuellen Computern und physischen Servern installieren, die Sie schützen möchten. Hierzu stehen zwei Möglichkeiten zur Verfügung:

* Automatische Pushinstallation des Diensts auf jedem Computer über den Prozessserver
* Manuelle Installation des Diensts

### <a name="install-the-mobility-service-automatically"></a>Automatisches Installieren des Mobilitätsdiensts
Beim Hinzufügen von Computern zu einer Schutzgruppe wird der Mobilitätsdienst vom Prozessserver automatisch per Pushfunktion auf jedem Computer installiert.

**Automatische Pushinstallation des Mobilitätsdiensts auf Windows-Servern:**

1. Installieren Sie die neuesten Updates für den Prozessserver wie in [Schritt 5: Installieren der neuesten Updates](#step-5-install-latest-updates)beschrieben, und stellen Sie sicher, dass der Prozessserver verfügbar ist.
2. Stellen Sie sicher, dass eine Netzwerkverbindung zwischen dem Quellcomputer und dem Prozessserver besteht und dass der Prozessserver Zugriff auf den Quellcomputer hat.  
3. Konfigurieren Sie die Windows-Firewall so, dass die **Datei- und Druckerfreigabe** und die **Windows-Verwaltungsinstrumentation** zulässig sind. Wählen Sie in den Einstellungen der Windows-Firewall die Option "Eine App oder ein Feature durch die Windows-Firewall zulassen" aus, und wählen Sie die in der folgenden Abbildung dargestellten Anwendungen aus. Für Computer, die einer Domäne angehören, können Sie die Firewallrichtlinie mit einem Gruppenrichtlinienobjekt konfigurieren.

    ![Firewalleinstellungen](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)
4. Das zur Durchführung der Pushinstallation verwendete Konto muss in der Gruppe "Administratoren" auf dem Computer enthalten sein, den Sie schützen möchten. Diese Anmeldeinformationen werden nur für die Pushinstallation des Mobilitätsdiensts verwendet, und Sie geben sie beim Hinzufügen eines Computers zu einer Schutzgruppe an.
5. Wenn es sich bei dem bereitgestellten Konto nicht um ein Domänenkonto handelt, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Fügen Sie hierzu unter "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" den Registrierungseintrag "LocalAccountTokenFilterPolicy DWORD" mit dem Wert 1 hinzu. Um den Registrierungseintrag über eine Befehlszeilenschnittstelle hinzuzufügen, öffnen Sie eine CMD- oder PowerShell-Befehlszeilenschnittstelle, und geben Sie **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**ein.

**Automatische Pushinstallation des Mobilitätsdiensts auf Linux-Servern:**

1. Installieren Sie die neuesten Updates für den Prozessserver wie in [Schritt 5: Installieren der neuesten Updates](#step-5-install-latest-updates)beschrieben, und stellen Sie sicher, dass der Prozessserver verfügbar ist.
2. Stellen Sie sicher, dass eine Netzwerkverbindung zwischen dem Quellcomputer und dem Prozessserver besteht und dass der Prozessserver Zugriff auf den Quellcomputer hat.  
3. Stellen Sie sicher, dass das Konto einem Root-Benutzer auf dem Linux-Quellserver entspricht.
4. Stellen Sie sicher, dass die Datei "/etc/hosts" auf dem Linux-Quellserver Einträge enthält, die den Namen des lokalen Hosts IP-Adressen zuordnen, die allen NICs zugeordnet sind.
5. Installieren Sie die neuesten openssh-, openssh-server- und openssl-Pakete auf dem Computer, den Sie schützen möchten.
6. Stellen Sie sicher, dass SSH auf Port 22 aktiviert ist und ausgeführt wird.
7. Aktivieren Sie das SFTP-Subsystem und die Kennwortauthentifizierung in der Datei "sshd_config" wie folgt:

   * a) Melden Sie sich als root an.
   * b) Suchen Sie in der Datei „/etc/ssh/sshd_config“ die Zeile, die mit **PasswordAuthentication** beginnt.
   * c) Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert von "no" in "yes".

       ![Linux-Mobilität](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)
   * d) Suchen Sie die Zeile, die mit "Subsystem" beginnt, und heben Sie die Auskommentierung der Zeile auf.

       ![Linux-Pushmobilität](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    
8. Stellen Sie sicher, dass die Linux-Variante des Quellcomputers unterstützt wird.

### <a name="install-the-mobility-service-manually"></a>Manuelles Installieren des Mobilitätsdiensts
Die Softwarepakete zum Installieren des Mobilitätsdiensts befinden sich auf dem Prozessserver in "C:\pushinstallsvc\repository". Melden Sie sich beim Prozessserver an, und kopieren Sie das entsprechende Installationspaket anhand der folgenden Tabelle auf den Quellcomputer:

| Quellbetriebssystem | Mobilitätsdienstpaket auf dem Prozessserver |
| --- | --- |
| Windows Server (nur 64 Bit) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe` |
| CentOS 6.4, 6.5, 6.6 (nur 64 Bit) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz` |
| SUSE Linux Enterprise Server 11 SP3 (nur 64 Bit) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz` |
| Oracle Enterprise Linux 6.4, 6.5 (nur 64 Bit) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz` |

**Um den Mobilitätsdienst manuell auf einem Windows Server zu installieren**, gehen Sie folgendermaßen vor:

1. Kopieren Sie das Paket **Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** aus dem in der obigen Tabelle aufgeführten Verzeichnispfad des Prozessservers auf den Quellcomputer.
2. Installieren Sie den Mobilitätsdienst, indem Sie die ausführbare Datei auf dem Quellcomputer ausführen.
3. Folgen Sie den Anweisungen des Installationsprogramms.
4. Wählen Sie als Rolle **Mobilitätsdienst** aus, und klicken Sie auf **Weiter**.

    ![Mobilitätsdienst installieren](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)
5. Belassen Sie das Installationsverzeichnis als Standardinstallationspfad, und klicken Sie auf **Installieren**.
6. Geben Sie unter **Host-Agent-Konfiguration** die IP-Adresse und den HTTPS-Port des Konfigurationsservers an.

   * Wenn Sie die Verbindung über das Internet herstellen, geben Sie die öffentliche virtuelle IP-Adresse und als Port den öffentlichen HTTPS-Endpunkt an.
   * Wenn Sie die Verbindung über ein VPN herstellen, geben Sie die interne IP-Adresse und den Port 443 an. Lassen Sie **HTTPS verwenden** aktiviert.

     ![Mobilitätsdienst installieren](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)
7. Geben Sie die Passphrase für den Konfigurationsserver an, und klicken Sie auf **OK** , um den Mobilitätsdienst beim Konfigurationsserver zu registrieren.

**So führen Sie dies über die Befehlszeile aus:**

1. Kopieren Sie die Passphrase vom CX-Server in die Datei "C:\connection.passphrase" auf dem Server, und führen Sie diesen Befehl aus. In unserem Beispiel lautet die IP-Adresse des CX-Servers 104.40.75.37, und der HTTPS-Port lautet 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Manuelles Installieren des Mobilitätsdiensts auf einem Linux-Server:**

1. Kopieren Sie anhand der obigen Tabelle das entsprechende TAR-Archiv vom Prozessserver auf den Quellcomputer.
2. Öffnen Sie ein Shellprogramm, und extrahieren Sie das gezippte TAR-Archiv in einen lokalen Pfad, indem Sie `tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Erstellen Sie in dem lokalen Verzeichnis, in das Sie den Inhalt des TAR-Archivs extrahiert haben, die Datei „passphrase.txt“, indem Sie über die Shell *`echo <passphrase> >passphrase.txt`* eingeben.
4. Installieren Sie den Mobilitätsdienst, indem Sie *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*eingeben.
5. Geben Sie die IP-Adresse und den Port an:

   * Wenn Sie die Verbindung mit dem Konfigurationsserver über das Internet herstellen, geben Sie in `<IP address>` und `<port>` die virtuelle öffentliche IP-Adresse und den öffentlichen HTTPS-Endpunkt des Konfigurationsservers an.
   * Wenn Sie die Verbindung über ein VPN herstellen, geben Sie die interne IP-Adresse und den Port 443 an.

**So führen Sie dies über die Befehlszeile aus:**

1. Kopieren Sie die Passphrase vom CX-Server in die Datei "passphrase.txt" auf dem Server, und führen Sie diesen Befehl aus. In unserem Beispiel lautet die IP-Adresse des CX-Servers 104.40.75.37, und der HTTPS-Port lautet 62519:

So installieren Sie den Dienst auf einem Produktionsserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

So installieren Sie den Dienst auf dem Zielserver:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

> [!NOTE]
> Wenn Sie einer Schutzgruppe Computer hinzufügen, auf denen bereits eine geeignete Version des Mobilitätsdiensts ausgeführt wird, wird die Pushinstallation übersprungen.
>
>

## <a name="step-9-enable-protection"></a>Schritt 9: Aktivieren des Schutzes
Zum Aktivieren des Schutzes fügen Sie einer Schutzgruppe virtuelle Computer und physische Server hinzu. Bevor Sie beginnen, beachten Sie Folgendes:

* Virtuelle Computer werden alle 15 Minuten ermittelt, und es kann bis zu 15 Minuten dauern, bis sie nach der Ermittlung in Azure Site Recovery angezeigt werden.
* Es kann auch bis zu 15 Minuten dauern, bis Umgebungsänderungen auf dem virtuellen Computer (z. B. Installation von VMware-Tools) in Site Recovery aktualisiert werden.
* Sie können auf der Seite **Konfigurationsserver** im Feld **LETZTER KONTAKT UM** für den vCenter-Server/ESXi-Host den Zeitpunkt der letzten Ermittlung überprüfen.
* Wenn Sie bereits eine Schutzgruppe erstellt haben und anschließend einen vCenter-Server oder ESXi-Host hinzufügen, dauert es 15 Minuten, bis das Azure Site Recovery-Portal aktualisiert wird und virtuelle Computer im Dialogfeld **Hinzufügen von Computern zu einer Schutzgruppe** angezeigt werden.
* Wenn Sie einer Schutzgruppe sofort Computer hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, markieren Sie den Konfigurationsserver (klicken Sie nicht darauf), und klicken Sie auf die Schaltfläche **Aktualisieren** .
* Wenn Sie einer Schutzgruppe virtuelle oder physische Computer hinzufügen, wird der Mobilitätsdienst automatisch vom Prozessserver auf den Quellserver gepusht und auf diesem installiert, sofern der Dienst nicht bereits installiert ist.
* Damit der automatische Pushmechanismus funktioniert, stellen Sie sicher, dass Sie die geschützten Computer so eingerichtet haben, wie im vorherigen Schritt beschrieben wurde.

Fügen Sie Computer wie folgt hinzu:

1. Klicken Sie auf **Geschützte Elemente** > **Schutzgruppe** > **Computer** > **Computer hinzufügen**. Als bewährte Methode wird empfohlen, dass Schutzgruppen ihre Workloads spiegeln, damit Sie Computer, auf denen eine bestimmte Anwendung ausgeführt wird, derselben Gruppe hinzufügen.
2. Wenn Sie physische Server schützen, geben Sie im Assistenten **Physische Computer hinzufügen** unter **Virtuelle Computer auswählen** die IP-Adresse und den Anzeigenamen an. Wählen Sie dann die Betriebssystemfamilie aus.

    ![vCenter-Server hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)
3. Wenn Sie virtuelle VMware-Computer schützen, wählen Sie unter **Virtuelle Computer auswählen** einen vCenter-Server aus, der die virtuellen Computer verwaltet (oder den EXSi-Host, auf dem sie ausgeführt werden), und wählen Sie dann die Computer aus.

    ![vCenter-Server hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png)    
4. Wählen Sie unter **Zielressourcen angeben** die Masterzielserver und den Speicher aus, die für die Replikation verwendet werden sollen, und wählen Sie aus, ob die Einstellungen für alle Workloads verwendet werden sollen. Wählen Sie bei der Konfiguration des Schutzes für Workloads, die konsistent hohe E/A-Leistung und niedrige Latenz erfordern, [Premium-Speicherkonto](../storage/storage-premium-storage.md) aus, um E/A-intensive Workloads zu hosten. Wenn Sie ein Premium-Speicherkonto für Ihre Workloaddatenträger verwenden möchten, müssen Sie das Masterziel der DS-Serie verwenden. Sie können keine Datenträger des Premium-Speichers mit einem Masterziel verwenden, das nicht zur DS-Serie gehört.

   > [!NOTE]
   > Eine Verschiebung von mit dem [neuen Azure-Portal](../storage/storage-create-storage-account.md) erstellten Speicherkonten über Ressourcengruppen hinweg wird nicht unterstützt.
   >
   >

    ![vCenter-Server](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)
5. Wählen Sie unter **Konten angeben** das Konto aus, das Sie zum Installieren des Mobilitätsdiensts auf geschützten Computern verwenden möchten. Die Kontoanmeldeinformationen werden für die automatische Installation des Mobilitätsdiensts benötigt. Wenn Sie kein Konto auswählen können, müssen Sie ein Konto einrichten, wie in Schritt 2 beschrieben. Beachten Sie, dass Azure keinen Zugriff auf dieses Konto hat. Für Windows-Server sollte das Konto über Administratorrechte auf dem Quellserver verfügen. Für Linux muss das Konto "root" lauten.

    ![Linux-Anmeldeinformationen](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)
6. Klicken Sie auf das Häkchen, um das Hinzufügen von Computern zur Schutzgruppe zu beenden und die erste Replikation für jeden Computer zu starten. Sie können den Status auf der Seite **Aufträge** überwachen.

    ![vCenter-Server hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)
7. Darüber hinaus können Sie den Schutzstatus überwachen, indem Sie auf **Geschützte Elemente** > Name der Schutzgruppe > **Virtuelle Computer** klicken. Nachdem die erste Replikation abgeschlossen ist und die Computer Daten synchronisieren, wird der Status **Geschützt** angezeigt.

    ![Aufträge virtueller Computer](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)

### <a name="set-protected-machine-properties"></a>Festlegen der Eigenschaften geschützter Computer
1. Sobald ein Computer den Status **Geschützt** aufweist, können Sie die Failovereigenschaften konfigurieren. Wählen Sie in den Schutzgruppendetails den Computer aus, und öffnen Sie die Registerkarte **Konfigurieren** .
2. Sie können den Namen, der dem Computer in Azure nach dem Failover zugewiesen wird, und die Größe des virtuellen Azure-Computers ändern. Außerdem können Sie das Azure-Netzwerk auswählen, mit dem der Computer nach einem Failover verbunden werden soll.

   > [!NOTE]
   > Für Netzwerke, die für die Site Recovery-Bereitstellung verwendet werden, wird die [Migration von Netzwerken](../resource-group-move-resources.md) zwischen Ressourcengruppen im selben Abonnement oder zwischen verschiedenen Abonnements nicht unterstützt.
   >
   >

    ![Eigenschaften für virtuelle Computer festlegen](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Beachten Sie Folgendes:

* Der Name des Azure-Computers muss den Azure-Anforderungen entsprechen.
* Standardmäßig werden replizierte virtuelle Computer in Azure nicht mit einem Azure-Netzwerk verbunden. Wenn repliziere virtuelle Computer kommunizieren sollen, stellen Sie sicher, dass für beide dasselbe Azure-Netzwerk festgelegt wird.
* Wenn Sie die Größe ein Volumes auf einem virtuellen VMware-Computer oder einem physischen Server ändern, wechselt der virtuelle Computer bzw. der Server in einen kritischen Zustand. Wenn Sie die Größe ändern müssen, führen Sie folgende Schritte aus:

  * a) Ändern Sie die Größeneinstellung.
  * b) Wählen Sie auf der Registerkarte **Virtuelle Computer** den virtuellen Computer aus, und klicken Sie auf **Entfernen**.
  * c) Wählen Sie unter **Virtuellen Computer entfernen** die Option **Schutz deaktivieren (Für Wiederherstellungsdrilloption und Volumegrößenänderung verwenden)** aus. Mit dieser Option wird der Schutz deaktiviert, die Wiederherstellungspunkte in Azure bleiben jedoch erhalten.

      ![Eigenschaften für virtuelle Computer festlegen](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)
  * d) Aktivieren Sie erneut den Schutz für den virtuellen Computer. Wenn Sie den Schutz erneut aktivieren, werden die Daten für das angepasste Volume an Azure übertragen.

## <a name="step-10-run-a-failover"></a>Schritt 10: Ausführen eines Failovers
Derzeit können nur nicht geplante Failover für geschützte virtuelle VMware-Computer und physische Server ausgeführt werden. Beachten Sie Folgendes:

* Stellen Sie vor dem Initiieren eines Failovers sicher, dass der Konfigurationsserver und Masterzielserver ausgeführt werden und fehlerfrei sind. Andernfalls schlägt das Failover fehl.
* Quellcomputer werden bei einem nicht geplanten Failover nicht heruntergefahren. Bei einem nicht geplanten Failover wird die Datenreplikation für die geschützten Server beendet. Sie müssen die Computer aus der Schutzgruppe löschen und erneut hinzufügen, um nach Abschluss des nicht geplanten Failovers die Computer erneut zu schützen.
* Wenn das Failover ohne Datenverlust erfolgen soll, stellen Sie sicher, dass die virtuellen Computer des primären Standorts deaktiviert sind, bevor Sie das Failover initiieren.

1. Fügen Sie auf der Seite **Wiederherstellungspläne** einen Wiederherstellungsplan hinzu. Geben Sie Details für den Plan an, und wählen Sie **Azure** als Ziel aus.

    ![Wiederherstellungsplan konfigurieren](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)
2. Wählen Sie unter **Virtuellen Computer auswählen** eine Schutzgruppe aus, und wählen Sie dann Computer in der Gruppe aus, die dem Wiederherstellungsplan hinzugefügt werden sollen. [hier](site-recovery-create-recovery-plans.md) .

    ![Virtuelle Computer hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)
3. Bei Bedarf können Sie den Plan anpassen und Gruppen erstellen sowie die Reihenfolge anpassen, in der für die Computer im Wiederherstellungsplan ein Failover durchgeführt wird. Sie können auch Aufforderungen für manuelle Aktionen und Skripts hinzufügen. Die Skripts beim Wiederherstellen in Azure können mithilfe von [Azure Automation-Runbooks](site-recovery-runbook-automation.md)hinzugefügt werden.
4. Wählen Sie den Plan auf der Seite **Wiederherstellungspläne** aus, und klicken Sie auf **Nicht geplantes Failover**.
5. Überprüfen Sie unter **Failover bestätigen** die Failoverrichtung (auf Azure), und wählen Sie den Wiederherstellungspunkt für das Failover aus.
6. Warten Sie, bis der Failoverauftrag abgeschlossen ist, und prüfen Sie dann, ob das Failover wie erwartet durchgeführt wurde und die replizierten virtuellen Computer in Azure erfolgreich gestartet wurden.

## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Schritt 11: Ausführen eines Failbacks von Computern aus Azure
[Weitere Informationen](site-recovery-failback-azure-to-vmware-classic-legacy.md) über das Failback von unter Azure ausgeführten Computern zur lokalen Umgebung.

## <a name="manage-your-process-servers"></a>Verwalten der Prozessserver
Der Prozessserver sendet Replikationsdaten an den Masterzielserver in Azure und ermittelt neue virtuelle VMware-Computer, die einem vCenter-Server hinzugefügt wurden. In den folgenden Situationen empfiehlt es sich, den Prozessserver in der Bereitstellung zu ändern:

* Wenn der aktuelle Prozessserver ausfällt
* Wenn die RPO (Recovery Point Objective) einen für Ihre Organisation nicht akzeptablen Wert erreicht.

Bei Bedarf können Sie die Replikation einiger oder aller lokalen virtuellen VMware-Computer und physischen Server auf einen anderen Prozess-Server verschieben. Beispiel:

* **Fehler**– Wenn ein Prozessserver ausfällt oder nicht verfügbar ist, können Sie die Replikation geschützter Computer auf einen anderen Prozessserver verschieben. Die Metadaten des Quellcomputers und Replikatcomputers werden auf den neuen Prozessserver verschoben, und die Daten werden synchronisiert. Der neue Prozessserver stellt automatisch eine Verbindung mit dem vCenter-Server her, um die automatische Ermittlung auszuführen. Im Site Recovery-Dashboard können Sie den Zustand der Prozessserver überwachen.
* **Lastenausgleich zum Anpassen der RPO**– Zur Erzielung eines verbesserten Lastenausgleichs können Sie im Site Recovery-Portal einen anderen Prozessserver auswählen und die Replikation eines oder mehrerer Computer dorthin verschieben, um einen manuellen Lastenausgleich durchzuführen. In diesem Fall werden die Metadaten des ausgewählten Quell- und Replikatcomputers auf den neuen Prozessserver verschoben. Der ursprüngliche Prozessserver bleibt mit dem vCenter-Server verbunden.

### <a name="monitor-the-process-server"></a>Überwachen des Prozessservers
Wenn ein Prozessserver in einem kritischen Zustand ist, wird im Site Recovery-Dashboard eine Statuswarnung angezeigt. Sie können auf den Status klicken, um die Registerkarte "Ereignisse" zu öffnen, und dann auf der Registerkarte "Aufträge" Detailinformationen zu bestimmten Aufträgen anzeigen.

### <a name="modify-the-process-server-used-for-replication"></a>Ändern des für die Replikation verwendeten Prozessservers
1. Klicken Sie unter **Server** > **Konfigurationsserver** auf den Konfigurationsserver und anschließend auf **Serverdetails**.
2. Klicken Sie neben dem Server, den Sie ändern möchten, auf **Prozessserver** > **Prozessserver ändern**.

    ![Prozessserver ändern 1](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)
3. Wählen Sie unter **Prozessserver ändern** > **Zielprozessserver** den neuen Server aus, den Sie verwenden möchten, und wählen Sie anschließend die virtuellen Computer aus, die auf den neuen Server repliziert werden sollen. Klicken Sie auf das Informationssymbol neben dem Servernamen, um Details zum verfügbaren Speicherplatz und zum verwendeten Arbeitsspeicher anzuzeigen. Um Sie bei Entscheidungen zur Last zu unterstützen, wird der durchschnittliche Speicherplatz angezeigt, der zum Replizieren jedes ausgewählten virtuellen Computers auf den neuen Prozessserver benötigt wird.

    ![Prozessserver ändern 2](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)
4. Klicken Sie auf das Häkchen, um mit dem Replizieren auf den neuen Prozessserver zu beginnen. Wenn Sie alle virtuellen Computer von einem Prozessserver mit kritischem Zustand entfernen, wird im Dashboard keine Warnung mehr für einen kritischen Zustand angezeigt.

## <a name="third-party-software-notices-and-information"></a>Hinweise und Informationen zu Drittanbietersoftware
Do Not Translate or Localize

The software and firmware running in the Microsoft product or service is based on or incorporates material from the projects listed below (collectively, “Third Party Code”).  Microsoft is the not original author of the Third Party Code.  The original copyright notice and license, under which Microsoft received such Third Party Code, are set forth below.

The information in Section A is regarding Third Party Code components from the projects listed below. Such licenses and information are provided for informational purposes only.  This Third Party Code is being relicensed to you by Microsoft under Microsoft's software licensing terms for the Microsoft product or service.  

The information in Section B is regarding Third Party Code components that are being made available to you by Microsoft under the original licensing terms.

The complete file may be found on the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft reserves all rights not expressly granted herein, whether by implication, estoppel or otherwise.



<!--HONumber=Nov16_HO3-->


