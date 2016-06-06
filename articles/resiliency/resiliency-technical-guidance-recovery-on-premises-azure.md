<properties
   pageTitle="Technischer Leitfaden zur Resilienz in Azure – Wiederherstellung eines lokalen Rechenzentrums in Azure | Microsoft Azure"
   description="Artikel über die Grundlagen und den Entwurf von Systemen zur Wiederherstellung der lokalen Infrastruktur in Azure"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="hongfeig"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/13/2016"
   ms.author="patw;jroth;aglick"/>

#Technischer Leitfaden zur Resilienz in Azure – Wiederherstellung eines lokalen Rechenzentrums in Azure

Azure bietet eine umfassende Reihe von Diensten zur Erweiterung eines lokalen Rechenzentrums in Azure, um hohe Verfügbarkeit und Notfallwiederherstellung zu ermöglichen:

  * __Netzwerk__: Mit einem virtuellen privaten Netzwerk können Sie Ihr lokales Netzwerk sicher in die Cloud erweitern.
  * __Compute__: Kunden, die Hyper-V lokal verwenden, können vorhandene virtuelle Computer ganz einfach per „Lift&Shift“ nach Azure verlagern.
  * __Speicher__: StorSimple erweitert Ihr Dateisystem in Azure Storage. Der Azure Backup-Dienst ermöglicht die Sicherung von Dateien und Azure SQL-Datenbank-Instanzen in Azure Storage.
  * __Datenbankreplikation__: Mit Verfügbarkeitsgruppen in SQL 2014 (oder höher) können Sie eine hohe Verfügbarkeit und Notfallwiederherstellung für Ihre lokalen Daten implementieren.

##Netzwerk
Mit Azure Virtual Network können Sie einen logisch isolierten Bereich in Azure erstellen, den Sie über eine IPsec-Verbindung sicher mit Ihrem lokalen Rechenzentrum oder einem einzelnen Clientcomputer verbinden können. Mit Virtual Network können Sie die Vorteile der skalierbaren bedarfsgesteuerten Infrastruktur von Azure einfacher nutzen und gleichzeitig eine Verbindung mit lokalen Daten und Anwendungen bereitstellen, einschließlich Systemen, die unter Windows Server, Mainframes und UNIX ausgeführt werden. Weitere Informationen finden Sie in der [Azure-Netzwerkdokumentation](../virtual-network/virtual-networks-overview.md).

##Compute
Wenn Sie Hyper-V lokal verwenden, können Sie vorhandene virtuelle Computer per „Lift&Shift“ nach Azure sowie an Dienstanbieter unter Windows Server 2012 (oder höher) verlagern, ohne Änderungen am virtuellen Computer vornehmen oder VM-Formate konvertieren zu müssen. Weitere Informationen finden Sie unter [Informationen zu Festplatten und VHDs für virtuelle Azure-Computer](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##Azure Site Recovery
Wenn Sie DRaaS (Disaster Recovery as a Service) einrichten möchten, nutzen Sie [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/). Azure Site Recovery bietet umfassenden Schutz für VMware-Server, Hyper-V-Server und physische Server. Mit Azure Site Recovery können Sie einen anderen lokalen Server oder Azure als Wiederherstellungsstandort nutzen. Weitere Informationen zu Azure Site Recovery finden Sie in der [Dokumentation zu Azure Site Recovery](https://azure.microsoft.com/documentation/services/site-recovery/).

##Speicher
Es gibt verschiedene Möglichkeiten, Azure als Sicherungsstandort für lokale Daten zu nutzen.

###StorSimple
StorSimple integriert Cloudspeicher für lokale Anwendungen sicher und transparent in ein einziges Gerät, das hochleistungsfähige abgestufte Speicherung im lokalen System und in der Cloud, Livearchivierung sowie cloudbasierten Datenschutz und cloudbasierte Notfallwiederherstellung bietet. Weitere Informationen finden Sie auf der [StorSimple-Produktseite](https://azure.microsoft.com/services/storsimple/).

###Azure Backup
Azure Backup ermöglicht Cloudsicherungen mithilfe der vertrauten Tools in Windows Server 2012 (oder höher), Windows Server 2012 Essentials (oder höher) und System Center 2012 Data Protection Manager (oder höher). Diese Tools stellen einen Workflow für die Sicherungsverwaltung bereit, der unabhängig vom Speicherort der Sicherungen ist – egal, ob es sich um einen lokalen Datenträger oder Azure Storage handelt. Nachdem die Daten in der Cloud gesichert wurden, können autorisierte Benutzer die Sicherungen ohne weiteres auf jedem beliebigen Server wiederherstellen.

Bei inkrementellen Sicherungen werden nur Änderungen an Dateien in die Cloud übertragen. Dies trägt zu einer effizienten Speichernutzung bei, reduziert die Bandbreitenauslastung und unterstützt die Point-in-Time-Wiederherstellung mehrerer Datenversionen. Sie können auch zusätzliche Funktionen nutzen, wie etwa Datenaufbewahrungsrichtlinien, Datenkomprimierung und Drosselung von Datenübertragungen. Die Verwendung von Azure als Speicherort für Sicherungen bietet den offensichtlichen Vorteil, dass die Sicherungen automatisch außerhalb des Standorts gespeichert werden. Damit entfallen zusätzliche Anforderungen zum Sichern und Schützen der lokalen Sicherungsmedien. Weitere Informationen finden Sie unter [Was ist Azure Backup](../backup/backup-introduction-to-azure-backup.md) und [Konfigurieren von Azure Backup für DPM-Daten](https://technet.microsoft.com/library/jj728752.aspx).

##Datenbank
Sie können eine Notfallwiederherstellungslösung für Ihre SQL Server-Datenbanken in Hybridlösungen mit AlwaysOn-Verfügbarkeitsgruppen, Datenbankspiegelung, Protokollversand und Sicherung und Wiederherstellung mit Azure-Blob-Speicher umsetzen. All diese Lösungen verwenden SQL Server auf Azure Virtual Machines. AlwaysOn-Verfügbarkeitsgruppen können in hybriden IT-Umgebungen verwenden werden, in denen Datenbankreplikate sowohl lokal als auch in der Cloud existieren. Dies wird im folgenden Diagramm veranschaulicht, das aus dem detaillierten Artikel [Hohe Verfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) stammt.

![SQL AlwaysOn-Verfügbarkeitsgruppen in einer hybriden Cloudarchitektur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png "SQL AlwaysOn-Verfügbarkeitsgruppen in einer hybriden Cloudarchitektur")

Die Datenbankspiegelung kann in einer zertifikatbasierten Konfiguration auch lokale Server und die Cloud umfassen. Das folgende Diagramm veranschaulicht dieses Konzept.

![SQL-Datenbankspiegelung in einer hybriden Cloudarchitektur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png "SQL-Datenbankspiegelung in einer hybriden Cloudarchitektur")

Der Protokollversand kann verwendet werden, um eine lokale Datenbank mit einer SQL-Server-Datenbank auf einem virtuellen Azure-Computer zu synchronisieren.

![SQL-Protokollversand in einer hybriden Cloudarchitektur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png "SQL-Protokollversand in einer hybriden Cloudarchitektur")

Und schließlich können Sie eine lokale Datenbank direkt in einem Azure Blobspeicher sichern.

![Sichern von SQL Server in einen Azure-Speicherblob in einer hybriden Cloudarchitektur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png "Sichern von SQL Server in einen Azure-Speicherblob in einer hybriden Cloudarchitektur")

Weitere Informationen finden Sie unter [Hohe Verfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) und [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##Prüflisten für die lokale Wiederherstellung in Microsoft Azure

##Prüfliste – Netzwerk
  1. Lesen Sie den Abschnitt [Netzwerk](#networking) in diesem Dokument.
  2. Verwenden Sie Virtual Network, um eine sichere Verbindung zwischen lokalem System und der Cloud herzustellen.

##Prüfliste – Compute
  1. Lesen Sie den Abschnitt [Compute](#compute) in diesem Dokument.
  2. Verschieben Sie virtuelle Computer zwischen Hyper-V und Azure.

##Prüfliste – Speicher
  1. Lesen Sie den Abschnitt [Speicher](#storage) in diesem Dokument.
  2. Nutzen Sie StorSimple-Dienste für Cloudspeicher.
  3. Verwenden Sie Azure Backup Services.

##Prüfliste – Datenbank
  1. Lesen Sie den Abschnitt [Datenbank](#database) in diesem Dokument.
  2. Erwägen Sie, SQL Server auf virtuellen Azure-Computern als Sicherung zu verwenden.
  3. Richten Sie AlwaysOn-Verfügbarkeitsgruppen ein.
  4. Konfigurieren Sie die zertifikatbasierte Datenbankspiegelung.
  5. Verwenden Sie den Protokollversand.
  6. Sichern Sie die lokale Datenbank in einem Azure-Blobspeicher.

##Nächste Schritte
Dieser Artikel gehört zu einer Reihe von Artikeln, die als [Technischer Leitfaden zur Resilienz in Azure](./resiliency-technical-guidance.md) dienen. Im nächsten Artikel dieser Reihe geht es um die [Wiederherstellung nach Datenbeschädigung](./resiliency-technical-guidance-recovery-data-corruption.md).

<!---HONumber=AcomDC_0525_2016-->