---
title: "Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation"
description: "Sorgen für Sie für hohe Verfügbarkeit, und planen Sie die Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
translationtype: Human Translation
ms.sourcegitcommit: 047eeddecc33b637a4f0dec3aa591c42ada83706
ms.openlocfilehash: 9e988659dac046067c53765fc1ddc90ee152e27f


---
# <a name="high-availability-and-disaster-recovery-of-sap-hana-on-azure-large-instances"></a>Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)

Hohe Verfügbarkeit und Notfallwiederherstellung sind wichtige Aspekte bei der Ausführung Ihrer unternehmenskritischen Server mit SAP HANA in Azure (große Instanzen). Wichtig ist, dass Sie mit SAP, Ihrem Systemintegrator und/oder Microsoft zusammenarbeiten, um die richtige Strategie für hohe Verfügbarkeit/Notfallwiederherstellung zu bestimmen und umzusetzen. Dabei müssen wichtige Aspekte wie Recovery Point Objective (RPO) und Recovery Time Objective (RTO) für Ihre Umgebung berücksichtigt werden.

## <a name="high-availability"></a>Hohe Verfügbarkeit

Microsoft unterstützt &quot;standardmäßig&quot; die SAP HANA-Methoden für hohe Verfügbarkeit wie z.B.:

- **Speicherreplikation:** Die Fähigkeit des Speichersystems selbst, alle Daten an einen anderen Speicherort zu replizieren (entweder innerhalb desselben Rechenzentrums oder außerhalb). SAP HANA wird unabhängig von dieser Methode betrieben.
- **Systemreplikation:** Die Replikation aller Daten in SAP HANA in ein anderes SAP HANA-System. Das RTO wird durch das Replizieren von Daten in regelmäßigen Abständen minimiert. SAP HANA unterstützt asynchrone, synchrone In-Memory- und synchrone Modi (nur empfohlen für SAP HANA-Systeme innerhalb des gleichen Rechenzentrums oder mit einer Entfernung zueinander von weniger als 100 km).
- **Automatisches Hostfailover:** Ein oder mehrere SAP HANA-Standbyknoten sind im Modus &quot;Horizontal skalieren&quot; konfiguriert. SAP HANA führt automatisch ein &quot;Failover&quot; auf einen anderen Knoten durch, sobald der Masterknoten nicht mehr verfügbar ist.

Weitere Informationen zur hohen Verfügbarkeit von SAP HANA finden Sie in den folgenden SAP-Informationen:

- [Whitepaper zur hohen Verfügbarkeit von SAP HANA](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA-Administratorhandbuch](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy-Video zur SAP HANA-Systemreplikation](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP Support Note #1999880 – FAQ on SAP HANA System Replication](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP Support Note #2165547 – SAP HANA Backup and Restore within SAP HANA System Replication Environment](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP Support Note #1984882 – Using SAP HANA System Replication for Hardware Exchange with Minimum/Zero Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="disaster-recovery"></a>Notfallwiederherstellung.

SAP HANA in Azure (große Instanzen) wird in zwei Azure-Regionen in einer geopolitischen Region angeboten. Zwischen den beiden „Große Instanz“-Stapeln zweier verschiedener Regionen gibt es eine direkte Netzwerkverbindung zum Replizieren von Daten während der Notfallwiederherstellung. Die Replikation der Daten basiert auf der Speicherinfrastruktur. Die Replikation der Daten erfolgt nicht standardmäßig, sondern gemäß Kundenkonfigurationen mit angeforderter Notfallwiederherstellung.

Um jedoch die Notfallwiederherstellung nutzen zu können, müssen Sie die Netzwerkverbindung zwischen den beiden Azure-Regionen entsprechend einrichten. Sie benötigen hierfür eine ExpressRoute-Verbindung zwischen Ihrer lokalen Umgebung und Ihrer Azure-Hauptregion und eine weitere ExpressRoute-Verbindung zwischen Ihrer lokalen Umgebung und Ihrer Notfallwiederherstellungsregion. Diese Maßnahme deckt eine Situation ab, in der eine ganze Azure-Region samt MSEE-Standort (Microsoft Enterprise-Edgerouter) einen Ausfall erleidet.

Als zweite Maßnahme verknüpfen Sie idealerweise alle Azure VNets, die mit SAP HANA in Azure (große Instanzen) in einer der Regionen verbunden sind, mit diesen beiden ExpressRoute-Verbindungen. Dies würde den Fall abdecken, bei dem bloß einer der MSEE-Standorte ausfällt, der Ihre lokale Umgebung mit Azure verbindet.

Die folgende Abbildung zeigt die optimale Konfiguration für die Wiederherstellung im Notfall:

![Optimale Konfiguration für die Wiederherstellung im Notfall](./media/sap-hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

Die optimale Konfiguration für die Notfallwiederherstellung des Netzwerks sieht zwei ExpressRoute-Verbindungen der lokalen Umgebung mit zwei verschiedenen Azure-Regionen vor. Eine Verbindung besteht mit Region 1, in der eine Produktionsinstanz ausgeführt wird. Die zweite ExpressRoute-Verbindung besteht mit Region 2, in der einige nicht produktive HANA-Instanzen ausgeführt werden (dies ist wichtig für den Fall, dass eine komplette Azure-Region, einschließlich MSEE und „Großer Instanz“-Stapel, ausfällt).

Als zweite Maßnahme sind alle VNets mit den anderen ExpressRoute-Verbindungen verknüpft, die mit SAP HANA in Azure (große Instanzen) verbunden sind. Sie können den Standort umgehen, an dem ein MSEE ausfällt, oder den RPO-Wert für die Notfallwiederherstellung senken, was weiter unten erklärt wird.

Es folgen die nächsten Anforderungen für die Einrichtung der Notfallwiederherstellung:

- Sie benötigen SAP HANA für Azure (große Instanzen)-SKUs mit derselben Größe wie die Produktions-SKUs, die bestellt und in der Notfallwiederherstellungsregion bereitgestellt wurden. Diese Instanzen können zum Ausführen von HANA-Instanzen für Test-, Sandkasten- oder QS-Zwecke verwendet werden.
- Darüber hinaus müssen Sie bei Bedarf ein Notfallwiederherstellungsprofil für alle SAP HANA für Azure (große Instanzen)-SKUs bestellen, die Sie am Notfallwiederherstellung-Standort wiederherstellen möchten. Dies führt zur Zuordnung von Speichervolumes, die das Ziel der Speicherreplikation aus Ihrer Produktionsregion in die Notfallwiederherstellungsregion sind.

Nach Abschluss der gesamten Einrichtung sind Sie für das Starten der Speicherreplikation zuständig. Bei der für SAP HANA in Azure (große Instanzen) verwendeten Speicherinfrastruktur bilden Speicher-Snapshots die Basis der Speicherreplikation. Zum Starten der Replikation für die Notfallwiederherstellung benötigen Sie Folgendes:

- Einen Snapshot Ihrer Start-LUN (wie zuvor beschrieben).
- Einen Snapshot Ihrer zu HANA gehörigen Volumes (wie zuvor beschrieben).

Nach Erstellung dieser Snapshots wird ein erstes Replikat der Volumes auf den Volumes erstellt, die Ihrem Notfallwiederherstellungsprofil in der Notfallwiederherstellungsregion zugeordnet sind.

Anschließend wird stündlich der neueste Speicher-Snapshot genutzt, um die Unterschiede zu replizieren, die in der Zwischenzeit auf den Speichervolumes angefallen sind.

Der RPO-Wert dieser Konfiguration liegt bei 60 bis 90 Minuten. Um bei einer Notfallwiederherstellung einen besseren RPO-Wert zu erzielen, müssen Sie die Sicherungen des HANA-Transaktionsprotokolls aus SAP HANA in Azure (große Instanzen) in die andere Azure-Region kopieren. Gehen Sie dazu so vor:

- Sichern Sie das HANA-Transaktionsprotokoll so häufig wie möglich in „/hana/log/backup“.
- Kopieren Sie anschließend die fertigen Sicherungen des Transaktionsprotokolls in eine Azure-VM in einem VNet, das mit dem Server mit SAP HANA in Azure (große Instanzen) verbunden ist.
- Kopieren Sie von dieser VM die Sicherung auf eine VM in einem VNet in der Notfallwiederherstellungsregion.
- Belassen Sie die Sicherungen des Transaktionsprotokolls in dieser Region in der VM.

Kopieren Sie bei einem Notfall, nachdem das Notfallwiederherstellungsprofil auf einem tatsächlichen Server bereitgestellt wurde, die Sicherungen des Transaktionsprotokolls von der VM auf den Server mit SAP HANA in Azure (große Instanzen), der nun der primäre Server in der Notfallwiederherstellungsregion ist, und stellen Sie diese Sicherungen wieder her. Dies ist möglich, da der Status von HANA auf den Datenträgern für die Notfallwiederherstellung einem HANA-Snapshot entspricht. Dies ist der Ausgangspunkt für weitere Wiederherstellungen von Transaktionsprotokollsicherungen.

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Einer der wichtigsten Aspekte beim Betreiben von Datenbanken ist das Sicherstellen, dass die Datenbank gegen verschiedene Arten von katastrophalen Ereignissen geschützt ist. Diese Ereignisse können sowohl durch Naturkatastrophen als auch einfache Benutzerfehler verursacht werden.

Das Sichern einer Datenbank mit der Möglichkeit einer beliebigen Zeitpunktwiederherstellung (z.B. vor dem Zeitpunkt des Löschens kritischer Daten) lässt das Wiederherstellen eines Status zu, der so nah wie möglich demjenigen entspricht, der vor der Störung vorlag.

Für optimale Ergebnisse müssen zwei Arten von Sicherungen erfolgen:

- Datenbanksicherungen
- Transaktionsprotokollsicherungen

Zusätzlich zu vollständigen Datenbanksicherungen auf Anwendungsebene können Sie Sicherungen mit Speicher-Snapshots erstellen. Das Erstellen von Protokollsicherungen ist auch wichtig für das Wiederherstellen der Datenbank (und auch das Leeren der Protokolle von Transaktionen mit erfolgtem Commit).

Es gibt zwei Sicherungs- und Wiederherstellungsoptionen für SAP HANA in Azure (große Instanzen):

- Eigenständig. Führen Sie, nachdem genügend vorhandener Speicherplatz berechnet wurde, vollständige Datenbank- und Protokollsicherungen mithilfe von Datenträger-Sicherungsmethoden (auf diese Datenträger) durch. Im Laufe der Zeit werden die Sicherungen in Azure Storage kopiert (nachdem ein auf Azure basierender Dateiserver mit praktisch unbegrenztem Standardspeicher eingerichtet wurde). Sie können auch Azure Backup Vault oder Azure Cold Storage verwenden. Eine andere Möglichkeit ist die Nutzung eines Datenschutztools eines anderen Anbieters, z.B. Commvault, um die Sicherungen zu speichern, nachdem sie in Azure-Standardspeicher kopiert wurden. Die eigenständige Sicherungsoption kann auch für Daten erforderlich sein, die aufgrund von Compliance- oder zu Überwachungszwecken über längere Zeiträume gespeichert werden müssen.
- Verwenden Sie die Sicherungs- und Wiederherstellungsfunktionalität, die die zugrunde liegende Infrastruktur von SAP HANA in Azure (große Instanzen) bereitstellt. Diese Option erfüllt den Bedarf an Sicherungen und macht manuelle Sicherungen nahezu überflüssig (außer in Fällen, in denen Datensicherungen für Compliancezwecke benötigt werden). Im restlichen Teil dieses Abschnitts werden die Sicherungs- und Wiederherstellungsfunktionen behandelt, die von HANA (große Instanzen) geboten werden.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Verwenden von Speicher-Snapshots von SAP HANA in Azure (große Instanzen)

Die zugrunde liegende Speicherinfrastruktur von SAP HANA in Azure (große Instanzen) unterstützt das Konzept eines Speicher-Snapshots von Volumes. Sowohl die Sicherung als auch die Wiederherstellung eines bestimmten Volumes wird unter Berücksichtigung der folgenden Aspekte unterstützt:

- Anstelle von Datenbanksicherungen erfolgen Snapshots von Speichervolumes in regelmäßigen Abständen.
- Der Speicher-Snapshot löst einen SAP HANA-Snapshot vor der Erstellung des Speicher-Snapshots aus. Dieser HANA-Snapshot ist der Punkt, an dem nach der Wiederherstellung des Speicher-Snapshots etwaige Protokollwiederherstellungen erfolgen.
- Sobald der Speicher-Snapshot erfolgreich erstellt wurde, wird der SAP HANA-Snapshot gelöscht.
- Protokollsicherungen erfolgen in regelmäßigen Abständen und werden auf dem Protokollsicherungsvolume oder in Azure gespeichert.
- Wenn die Datenbank bezogen auf einen bestimmten Zeitpunkt wiederhergestellt werden muss, wird eine Anforderung an den Microsoft Azure-Support (Produktionsausfall) oder das SAP HANA in Azure-Dienstverwaltungsteam gerichtet, eine Wiederherstellung bis zu einem bestimmten Speicher-Snapshot durchzuführen (z.B. eine geplante Wiederherstellung eines Sandkastensystems in seinem ursprünglichen Zustand).
- Der im Speicher-Snapshot enthaltene SAP HANA-Snapshot ist ein Ausgangspunkt zum Anwenden von Protokollsicherungen, die ausgeführt und gespeichert wurden, nachdem der Speicher-Snapshot erstellt wurde.
- Diese Protokollsicherungen werden erstellt, um die Datenbank an einem bestimmten Punkt wiederherzustellen.

Bei Angabe des \_Sicherungsnamens wird ein Snapshot der folgenden Volumes erstellt:

- hana/data
- hana/log
- hana/log\_backup (als Sicherung in „hana/log“ eingebunden)
- hana/shared

### <a name="storage-snapshot-considerations"></a>Aspekte von Speicher-Snapshots

>[!NOTE] 
>Speicher-Snapshots werden _nicht_ kostenlos zur Verfügung gestellt, da zusätzlicher Speicherplatz zugeordnet werden muss.

Besondere Funktionsweise von Speicher-Snapshots von SAP HANA in Azure (große Instanzen):

- Ein bestimmter Speicher-Snapshot (zum Zeitpunkt seiner Erstellung) belegt nur sehr wenig Speicherplatz.
- Wenn sich Dateninhalte und der Inhalt in SAP HANA-Datendateien auf dem Speichervolume ändern, muss die Momentaufnahme den ursprünglichen Blockinhalt speichern.
- Die Speichermomentaufnahme vergrößert sich. Je länger der Snapshot vorhanden ist, desto größer wird der Speicher-Snapshot.
- Je mehr Änderungen am SAP HANA-Datenbankvolume während der Lebensdauer eines Speicher-Snapshots erfolgen, desto mehr Speicherplatz belegt der Speicher-Snapshot.

SAP HANA in Azure (große Instanzen) bietet standardmäßig feste Volumegrößen für das SAP HANA- Daten- und Protokollvolume. Das Erstellen von Snapshots dieser Volumes belegt Speicherplatz auf Ihren Volumes, weshalb Sie für das Planen von Speicher-Snapshots (innerhalb des SAP HANA in Azure [große Instanzen]-Prozesses) zuständig sind.

Die folgenden Abschnitte enthalten Informationen zum Erstellen dieser Snapshots sowie allgemeine Empfehlungen:

- Obwohl die Hardware 255 Snapshots pro Volume unterstützen kann, wird dringend empfohlen, unter diesem Wert zu bleiben.
- Freier Speicherplatz muss überwacht und seine Größe bekannt sein, ehe Speicher-Snapshots erstellt werden.
- Verringern Sie die Anzahl von Speicher-Snapshots basierend auf dem freien Speicherplatz. Die beibehaltene Anzahl von Snapshots muss ggf. reduziert werden, oder die Volumes müssen womöglich erweitert werden (zusätzlicher Speicher kann in 1-TB-Einheiten bestellt werden).
- Bei Aktivitäten wie dem Verschieben von Daten in SAP HANA mit Systemmigrationstools (mit R3Load oder Wiederherstellen von SAP HANA-Datenbanken aus Sicherungen) wird dringend empfohlen, keine Speicher-Snapshots zu erstellen. (Normalerweise erfolgt eine Systemmigration auf einem neuen SAP HANA-System, sodass Speicher-Snapshots in diesem Fall nicht ausgeführt werden müssen.)
- Bei größeren Reorganisationen von SAP HANA-Tabellen sollten Speicher-Snapshots nach Möglichkeit vermieden werden.
- Darüber hinaus sind Speicher-Snapshots eine Voraussetzung für die Notfallwiederherstellungsfunktionen von SAP HANA in Azure (große Instanzen).

### <a name="setting-up-storage-snapshots"></a>Einrichten von Speicher-Snapshots

1. Stellen Sie sicher, dass Perl im Linux-Betriebssystem auf dem Server mit HANA (große Instanzen) installiert ist.

2. Fügen Sie „/etc/ssh/ssh\_config“ die Zeile _MACs hmac-sha1_ hinzu.

3. Erstellen Sie ein SAP HANA-Sicherungsbenutzerkonto auf dem Masterknoten für jede SAP HANA-Instanz, die Sie ausführen (falls zutreffend).

4. Der SAP HANA HDB-Client muss auf allen Servern mit HANA (große Instanzen) installiert werden.

5. Auf dem ersten Server mit HANA (große Instanzen) in jeder Region muss ein öffentlicher Schlüssel erstellt werden, um auf die zugrunde liegende Speicherinfrastruktur zuzugreifen, die die Erstellung von Snapshots steuert.

6. Kopieren Sie das Skript „azure\_hana\_backup.pl“ aus „/scripts“ an den Speicherort von **hdbsql** in der SAP HANA-Installation.

7. Kopieren Sie die Datei „HANABackupDetails.txt“ aus „/scripts“ an denselben Speicherort wie das Perl-Skript.

8. Ändern Sie bei Bedarf die Datei „HANABackupDetails.txt“ entsprechend den jeweiligen Kundenvorgaben.

### <a name="detailed-storage-snapshot-steps"></a>Detaillierte Schritte zum Erstellen von Speicher-Snapshots

**Schritt 1: Installieren des SAP HANA HDB-Clients**

Das für SAP HANA in Azure (große Instanzen) installierte Linux enthält die Ordner und Skripts, die benötigt werden, um die SAP HANA-Speicher-Snapshots für Sicherungs- und Notfallwiederherstellungszwecke zu erstellen. Es ist jedoch Ihre Aufgabe, den SAP HANA HDB-Client während der Installation von SAP HANA zu installieren.  (Microsoft installiert weder den HDB-Client noch SAP HANA.)

**Schritt 2: Ändern von „/etc/ssh/ssh\_config**“

Ändern Sie „/etc/ssh/ssh\_config“ durch Hinzufügen der Zeile _MACs hmac-sha1_ (siehe unten).
Bearbeiten Sie den folgenden Abschnitt durch Hinzufügen von **MACs hmac-sha1**:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

**Schritt 3: Erstellen eines öffentlichen Schlüssels**

Erstellen Sie auf dem ersten Server mit SAP HANA in Azure (große Instanzen) in jeder Azure-Region einen öffentlichen Schlüssel, der für den Zugriff auf die Speicherinfrastruktur verwendet wird, die die Erstellung von Snapshots ermöglicht. Der öffentliche Schlüssel stellt sicher, dass für die Anmeldung beim Speicher kein Kennwort erforderlich ist und dass die Kennwortanmeldeinformationen nicht beibehalten werden. Führen Sie unter Linux auf dem Server mit HANA (große Instanzen) den folgenden Befehl aus, um den öffentlichen Schlüssel zu generieren:
```
  ssh-keygen –t dsa –b 1024
```
Der neue Speicherort ist „_/root/.ssh/id\_dsa.pub“. Geben Sie _keine_ Passphrase ein, denn andernfalls werden Sie bei jeder Anmeldung aufgefordert, die Passphrase einzugeben, ehe Sie fortfahren können. Drücken Sie stattdessen zweimal die **EINGABETASTE**, um die Anforderung zur Eingabe der Passphrase nach der Anmeldung zu entfernen.

Vergewissern Sie sich, dass der öffentliche Schlüssel wie erwartet korrigiert wurde, indem Sie zu „/root/.ssh/“ navigieren und dann den Befehl **ls** ausführen. Sofern der Schlüssel vorhanden ist, wird der öffentliche Schlüssel mit dem folgenden Befehl kopiert. Anschließend wird die gesamte Zeichenfolge kopiert:

![Der öffentliche Schlüssel wird durch Ausführen dieses Befehls kopiert](./media/sap-hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Wenden Sie sich an diesem Punkt an das SAP HANA in Azure-Dienstverwaltungsteam, und geben Sie den Schlüssel an. Das Team nutzt den öffentlichen Schlüssel, um ihn in der zugrunde liegenden Infrastruktur zu registrieren.

**Schritt 4: Erstellen eines SAP HANA-Benutzerkontos**

Erstellen Sie für Sicherungszwecke in SAP HANA Studio ein SAP HANA-Benutzerkonto. Dieses Konto benötigt die folgenden Berechtigungen: BACKUP ADMIN und CATALOG READ. In diesem Beispiel wird das Benutzerkonto SCADMIN erstellt.

![Erstellen eines Benutzers in HANA Studio](./media/sap-hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

**Schritt 5: Autorisieren des SAPA HANA-Benutzerkontos**

Autorisieren Sie das SAPA HANA-Benutzerkonto (das von den Skripts verwendet wird, ohne bei jeder Skriptausführung eine Autorisierung anzufordern). Der SAP HANA-Befehl **hdbuserstore** ermöglicht die Erstellung eines SAP HANA-Benutzerschlüssels, der auf einem oder mehreren SAP HANA-Knoten gespeichert wird, und ermöglicht dem Benutzer den Zugriff auf SAP HANA, ohne Kennwörter innerhalb des Skriptprozesses (der nachstehend erörtert wird) verwalten zu müssen.

>[!IMPORTANT]
>Der nachfolgende Befehl muss als _root_ ausgeführt werden. Andernfalls funktioniert das Skript nicht ordnungsgemäß.

Der Befehl **hdbuserstore** wird wie folgt eingegeben:

![Der Befehl „hdbuserstore“ wird eingegeben ](./media/sap-hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

Im folgenden Beispiel, bei dem der Benutzer SCADMIN01 und Hostname „lhanad01“ lautet, ist der Befehl wie folgt:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Wählen Sie für horizontal skalierte HANA-Instanzen das Verwalten aller Skripts über einen einzelnen Server aus. In diesem Beispiel muss der SAP HANA-Schlüssel von SCADMIN01 für jeden Host auf eine Weise geändert werden, die den Host wiedergibt, der in Beziehung zum Schlüssel steht (d.h. dass das SAP HANA-Sicherungskonto durch die Instanznummer von HANA DB **lhanad** ergänzt wird). Der Schlüssel benötigt Administratorrechte auf dem Host, dem er zugewiesen ist. Der Sicherungsbenutzer (für horizontale Skalierung) benötigt Zugriffsrechte für alle SAP HANA-Instanzen.
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

**Schritt 6: Kopieren von Elementen aus dem Ordner „/scripts“**

Kopieren Sie entweder die folgenden Elemente aus dem Ordner „/scripts“, die im Gold-Image der Installation enthalten sind, in das Arbeitsverzeichnis für **hdbsql**. Für aktuelle HANA-Installationen lautet es „/hana/shared/D01/exe/linuxx86\_64/hdb“.
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
Oder kopieren Sie folgende Elemente, wenn sie die horizontale Skalierung oder OLAP ausführen:
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
Die Datei „HANABackupCustomerDetails.txt“ kann für eine Bereitstellung mit horizontaler Skalierung wie folgt geändert werden. Es handelt sich im Wesentlichen um die Steuerungs- und Konfigurationsdatei für das Skript, mit dem die Speicher-Snapshots erstellt werden. Die Angaben für _Storage Backup Name_ und _Storage IP Address_ sollten Sie vom SAP HANA in Azure-Dienstverwaltungsteam erhalten haben, als Ihre Instanzen bereitgestellt wurden. Reihenfolge, Sortierung und Abstände dieser Variablen _können_ nicht geändert werden, da sonst das Skript nicht ordnungsgemäß ausgeführt werden kann.

Für eine Bereitstellung mit vertikaler Skalierung sieht die Konfigurationsdatei so aus:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Created by customer using hdbuserstore
HANA Backup Name: SCADMIND01
```
Bei einer Konfiguration mit horizontaler Skalierung sieht die Datei „HANABackupCustomerDetailsBW.txt“ so aus:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 10.254.15.21
Node 1 HANA instance number: 01
Node 1 HANA Backup Name: SCADMIN01
Node 2 IP Address: 10.254.15.22
Node 2 HANA instance number: 02
Node 2 HANA Backup Name: SCADMIN02
Node 3 IP Address: 10.254.15.23
Node 3 HANA instance number: 03
Node 3 HANA Backup Name: SCADMIN03
Node 4 IP Address: 10.254.15.24
Node 4 HANA instance number: 04
Node 4 HANA Backup Name: SCADMIN04
Node 5 IP Address: 10.254.15.25
Node 5 HANA instance number: 05
Node 5 HANA Backup Name: SCADMIN05
Node 6 IP Address: 10.254.15.26
Node 6 HANA instance number: 06
Node 6 HANA Backup Name: SCADMIN06
Node 7 IP Address: 10.254.15.27
Node 7 HANA instance number: 07
Node 7 HANA Backup Name: SCADMIN07
Node 8 IP Address: 10.254.15.28
Node 8 HANA instance number: 08
Node 8 HANA Backup Name: SCADMIN08
```
Wichtig ist der Hinweis, dass derzeit nur Details zu „Node 1“ im tatsächlichen HANA-Speicher-Snapshots-Skript verwendet werden. Doch es wird empfohlen, den Zugriff auf und von allen HANA-Knoten so zu testen, dass wenn sich der Mastersicherungsknoten jemals ändern muss, Sie bereits dafür gesorgt haben, dass jeder andere Knoten seine Stelle einnehmen kann, indem Sie die Details in „Node 1“ bearbeiten.

Um die Konfigurationen in der Konfigurationsdatei oder die Verbindung mit den HANA-Instanzen auf Richtigkeit zu prüfen, müssen Sie (unabhängig von der SAP-Workload) das nachstehende erste Skript für eine Konfiguration mit horizontaler Skalierung und anschließend das zweite Skript in der Liste für eine Konfiguration mit vertikaler Skalierung ausführen.
```
testHANAConnection.pl
```
Oder führen Sie für Konfigurationen für horizontale Skalierung Folgendes aus:
```
testHANAConnectionBW.pl
```
Stellen Sie sicher, dass die HANA-Masterinstanz Zugriff auf alle benötigten HANA-Server hat. Das Skript enthält keine Parameter, doch die entsprechende Datei „HANABackupCustomerDetails“/„HANABackupCustomerDetailsBW“ muss ausgefüllt sein, damit das Skript ordnungsgemäß ausgeführt wird. Wenngleich es dem Skript nicht möglich ist, jede bestimmte Instanz auf Fehler zu überprüfen, da nur die Fehlercodes von Shellbefehlen zurückgegeben werden, liefert das Skript dennoch einige hilfreiche Kommentare, die Sie überprüfen können.

So führen Sie das Skript aus
```
 ./testHANAConnection.pl
```
Das Skript gibt die Meldung aus, dass die HANA-Verbindung erfolgreich war, wenn das Skript den Status der HANA-Instanz erfolgreich abrufen kann.

Darüber hinaus gibt es einen zweiten Typ von Skript, das die Fähigkeit des HANA-Masterinstanzservers prüft, sich beim Speicher anzumelden. Sie müssen das folgende Skript ausführen, ehe Sie das Skript „azure\_hana\_backup(\_bw).pl“ ausführen. Wenn auf einem Volume keine Snapshots vorhanden sind, ist es unmöglich, den Unterschied zwischen einem Volume ohne Snapshots und einem SSH-Fehler beim Abrufen der Snapshotdetails zu bestimmen.  Aus diesem Grund erfolgt die Ausführung des folgenden Skripts in zwei Schritten:

- Es wird überprüft, ob auf die Speicherkonsole zugegriffen werden kann.
- Nach HANA-Instanz wird für jedes Volume ein Test, Dummy oder Snapshot erstellt.

Aus diesem Grund ist die HANA-Instanz als ein Argument enthalten. Wiederum ist keine Fehlerüberprüfung möglich, da sich das Skript auf die Verbindung mit dem Speicher bezieht, doch es bietet hilfreiche Hinweise, wenn die Ausführung misslingt.

Das Skript wird ausgeführt als:
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
oder
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
Das Skript gibt auch die Meldung aus, dass Sie sich bei Ihrem bereitgestellten Speichermandanten entsprechend anmelden können, der gemäß Ihren LUNs organisiert ist, die von Ihren Serverinstanzen verwendet werden.

Bevor Sie die ersten auf Speicher-Snapshots basierenden Sicherungen erstellen, müssen Sie diese Skripts ausführen, um sicherzustellen, dass die Konfiguration ordnungsgemäß ist.

Nach Ausführung dieser Skripts müssen Sie die Snapshots durch Ausführen eines der folgenden Befehle löschen:
```
./removeTestStorageSnapshot.pl <hana instance>
```
oder
```
./removeTestStorageSnapshot.pl <hana instance>
```

**Schritt 7: Bedarfsabhängiges Erstellen von Snapshots**

Erstellen Sie (wie nachstehende beschrieben) Snapshots bedarfsabhängig (und planen Sie regelmäßige Snapshots mithilfe von Cron), indem Sie für Konfigurationen mit vertikaler Skalierung das folgende Skript ausführen:
```
./azure_hana_backup.pl lhanad01 customer 20
```
Oder führen Sie es wie folgt für Konfigurationen mit horizontaler Skalierung aus:
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
Das Skript für horizontale Skalierung führt einige zusätzliche Prüfungen durch, um den Zugriff auf alle HANA-Server sicherzustellen. Alle HANA-Instanzen geben den entsprechenden Status der Instanz zurück, ehe mit dem Erstellen von SAP HANA- oder Speicher-Snapshots fortgefahren wird.

Die folgenden Argumente sind erforderlich:

- Die HANA-Instanz, die eine Sicherung erfordert.
- Das Momentaufnahmenpräfix für die Speichermomentaufnahme.
- Die Anzahl der Momentaufnahmen, die für das angegebene Präfix beibehalten werden.
```
./azure_hana_backup.pl lhanad01 customer 20
```
Bei Ausführung des Skripts wird die Speichermomentaufnahme in diesen drei Phasen erstellt:

- Erstellen einer HANA-Momentaufnahme
- Erstellen einer Speicher-Momentaufnahme
- Entfernen des HANA-Snapshots

Führen Sie das Skript aus, indem Sie es im ausführbaren HDB-Ordner aufrufen, in den es kopiert wurde. Es sichert mindestens die folgenden Volumes, aber auch Volumes, die den expliziten SAP HANA-Instanznamen im Volumenamen enthalten.
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
Die Beibehaltungsdauer entspricht streng der Anzahl von Snapshots, die bei Ausführung des Skripts als Parameter übergeben wird (z.B. 20 im obigen Beispiel). Die Dauer hängt vom Ausführungszeitraum und der Anzahl der Snapshots im Aufruf des Skripts ab. Wenn die Anzahl der beibehaltenen Snapshots die Anzahl in einem Parameter im Aufruf des Skripts überschreitet, wird der älteste Speicher-Snapshot mit dieser Bezeichnung (in unseren Fall _custom_) gelöscht, ehe ein neuer Snapshot erstellt wird. Dies bedeutet, dass die Anzahl, die Sie als letzten Parameter des Aufrufs angeben, dazu dient, die Anzahl der Snapshots zu steuern.

>[!NOTE] 
>Sobald Sie die Bezeichnung ändern, beginnt der Zählvorgang erneut.

Sie müssen den HANA-Instanznamen, den Sie vom SAP HANA in Azure-Dienstverwaltungsteam erhalten haben, als Argument hinzufügen, wenn eine Erstellung von Snapshots in Umgebungen mit mehreren Knoten erfolgt. In Umgebungen mit einzelnem Knoten ist der Name der SAP HANA in Azure (große Instanz)-Einheit ausreichend. Der HANA-Instanznamen wird jedoch weiterhin empfohlen.

Darüber hinaus besteht die Möglichkeit, Startvolumes/LUNs mithilfe desselben Skripts zu sichern. Sie müssen Ihr Startvolume mindestens einmal sichern, wenn HANA erstmals den Betrieb aufnimmt, wenngleich ein wöchentlicher oder nächtlicher Sicherungszeitplan für das Startvolume in Cron empfohlen wird. Anstatt einen SAP HANA-Instanznamen hinzuzufügen, fügen Sie einfach _boot_ als Argument in das Skript wie folgt ein:
```
./azure_hana_backup boot customer 20
```
Dieselbe Aufbewahrungsrichtlinie wird auch für das Startvolume erzeugt. Bedarfsabhängig erstellte Snapshots sollten nur in bestimmten Fällen verwendet werden (z.B. während eines SAP EHP-Upgrades oder wenn Sie einen besonderen Speicher-Snapshot erstellen müssen).

Es wird dringend empfohlen, geplante Speicher-Snapshots mithilfe von Cron zu erstellen. Außerdem wird empfohlen, das gleiche Skript für alle Sicherungs- und Notfallwiederherstellungsanforderungen zu verwenden (wobei die Eingaben des Skripts an die verschiedenen gewünschten Sicherungszeiten angepasst werden). Diese werden in Cron alle abhängig von ihrer Ausführungszeit unterschiedlich geplant: stündlich, alle 12 Stunden, täglich oder wöchentlich (je nach Umständen). Der Cron-Zeitplan dient zum Speichern von Speicher-Snapshots, die der Aufbewahrungsbezeichnung entsprechen, die zuvor für die langfristige standortexterne Sicherung erörtert wurde. Das Skript enthält Befehle zum Sichern aller Produktionsvolumes in Abhängigkeit von der angeforderten Häufigkeit (Daten- und Protokolldateien werden stündlich gesichert, während das Startvolume nur täglich gesichert wird).

Die Einträge im folgenden Cron-Skript werden stündlich in der zehnten Minute, alle 12 Stunden in der zehnten Minute und täglich in der zehnten Minute ausgeführt. Die Cron-Aufträge sind so eingerichtet, dass nur ein SAP HANA-Speicher-Snapshot stündlich erstellt wird, damit stündliche und tägliche Sicherungen nicht zeitgleich (12:10 Uhr) erstellt werden. Das SAP HANA in Azure-Dienstverwaltungsteam schlägt Ihnen die empfohlene Uhrzeit zum Erstellen Ihrer Sicherungen vor, um die Erstellung und Replikation von Snapshots zu optimieren.

Die standardmäßige Cron-Zeitplanung in „/etc/crontab“ ist wie folgt:
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
In den obigen Cron-Anweisungen wird für die HANA-Volumes (ohne das Startvolume) ein Snapshot mit der Bezeichnung „hourly“ erstellt. Von diesen Snapshots werden 66 beibehalten. Darüber hinaus werden 14 Snapshots mit der Bezeichnung „12hour“ beibehalten. Sie haben so im Wesentlichen die Möglichkeit von Snapshots des Typs „hourly“ von drei Tagen sowie von Snapshots des Typs „12hour“ für weitere vier Tage, sodass Sie mit diesen Snapshots die gesamte Woche abdecken können. (Die stündlichen Snapshots werden für die vollständigen alle 12 Stunden erfolgenden Snapshots nicht eingeplant. Anstatt sowohl eines Snapshots „hourly“ als auch eines Snapshots „12hour“ wird nur der „12hour“-Snapshot erstellt.)

Die Zeitplanung mit Cron kann knifflig sein, da zu einem beliebigen Zeitpunkt immer nur ein Skript ausgeführt werden kann, es sei denn, dass Skript ist um mehrere Minuten zeitversetzt. Wenn tägliche Sicherungen langfristig aufbewahrt werden sollen, wird eine tägliche Sicherung zusammen mit einer alle 12 Stunden erfolgenden Sicherung aufgehoben (mit einer Aufbewahrungsanzahl von jeweils sieben). Oder der stündliche Snapshot ist um 10 Minuten nach hinten zeitversetzt, wobei aber nur ein täglicher Snapshot auf dem Produktionsvolume aufbewahrt wird.
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7 
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
Die nachstehend aufgeführten Häufigkeiten sind nur Beispiele. Sie müssen die optimale Anzahl von Snapshots basierend auf den folgenden Kriterien bestimmen:

- RTO-Anforderungen für Zeitpunktwiederherstellung.
- Speicherplatzbelegung.
- RPO- und RTO-Anforderungen für eventuelle Notfallwiederherstellung.
- Tatsächliche Erstellung vollständiger HANA-Datenbanksicherungen für Datenträger. Immer wenn eine vollständige Datenbanksicherung für Datenträger (Schnittstelle _backint_) erfolgt, misslingt die Erstellung von Speicher-Snapshots. Wenn Sie zusätzlich zu Speicher-Snapshots vollständige Datenbanksicherungen ausführen möchten, müssen Sie sicherstellen, dass die Erstellung von Speicher-Snapshots während dieses Zeitraums deaktiviert ist.

>[!IMPORTANT]
> Die Verwendung von Speicher-Snapshots für SAP HANA-Sicherungen ist nur möglich, wenn diese in Verbindung mit SAP HANA-Protokollsicherungen erfolgt. Diese Protokollsicherungen müssen die Zeiträume zwischen den Speicher-Snapshots abdecken. Wenn Benutzern eine Zeitpunktwiederherstellung von 30 Tagen zugesagt wurde, wird Folgendes benötigt:

- Die Fähigkeit des Zugriffs auf einen Speicher-Snapshot, der 30 Tage alt ist.
- Zusammenhängende Protokollsicherungen der letzten 30 Tage.

Erstellen Sie im Bereich der Protokollsicherungen auch einen Snapshot des Sicherungsprotokollvolumes. Sie müssen jedoch sicherstellen, dass regelmäßige Protokollsicherungen für folgende Zwecke erfolgen:

- Verfügen über zusammenhängende Protokollsicherungen, die für Zeitpunktwiederherstellungen erforderlich sind
- Verhindern, dass der Speicherplatz des SAP HANA-Protokollvolumes zur Neige geht

Einer der letzten Schritte ist das Planen von SAP HANA-Sicherungsprotokollen in SAP HANA Studio. Das Ziel des SAP HANA-Sicherungsprotokolls ist das spezifisch erstellte Volume „hana/log\_backups“ mit dem Einbindungspunkt „/hana/log/backups“.

![Planen von SAP HANA-Sicherungsprotokollen in SAP HANA Studio](./media/sap-hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Sicherungen, die häufiger als alle 15 Minuten erfolgen, können gewählt werden. Einige Benutzer erstellen sogar minütliche Protokollsicherungen, aber ein _höherer Wert_ als 15 Minuten wird nicht empfohlen.

Der letzte Schritt besteht im Erstellen einer dateibasierten Sicherung (nach der Erstinstallation von SAP HANA) zum Hinzufügen eines einzelnen Sicherungseintrags, der im Sicherungskatalog vorhanden sein muss. Andernfalls löst SAP HANA nicht die zuvor festgelegten Protokollsicherungen aus.

![Erstellen einer dateibasierten Sicherung zum Hinzufügen eines einzelnen Sicherungseintrags](./media/sap-hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-number-and-size-of-snapshots-on-disk-volume"></a>Überwachen der Anzahl und Größe von Snapshots auf dem Datenträgervolume

Auf einem bestimmten Speichervolume können Sie die Anzahl der Snapshots und deren Speicherbelegung überwachen. Der Befehl **ls** zeigt nicht das Snapshot-Verzeichnis und auch nicht die zugehörigen Dateien. Verwenden Sie hierfür den Linux-Betriebssystembefehl **du** mit den folgenden Befehlen:
```
- du –sh .snapshot will provide a total of all snapshots within the snapshot directory.
- du –sh --max-depth=1 will list all snapshots saved in the .snapshot folder and the size of each snapshot.
- du –hc will just provide the total size used by all snapshots.
```
Verwenden Sie diese Befehle, um sicherzustellen, dass die erstellten und gespeicherten Snapshots nicht den gesamten Speicher auf den Volumes belegen.

### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Verringern der Anzahl von Snapshots auf einem Server

Wie bereits erwähnt, können Sie die Anzahl gespeicherter Snapshots mit einer bestimmten Bezeichnung verringern. Die beiden letzten Parameter des Befehls zum Erstellen eines Snapshots sind eine Bezeichnung und die Anzahl von Snapshots, die Sie beibehalten möchten.
```
./azure_hana_backup.pl lhanad01 customer 20
```
Im Beispiel oben lautet die Snapshot-Bezeichnung _customer_, und die Anzahl von Snapshots mit dieser Bezeichnung, die beibehalten werden soll, ist _20_. Als Reaktion auf die Speicherplatzbelegung können Sie die Anzahl gespeicherter Snapshots reduzieren. Eine einfache Methode zum Verringern der Snapshots ist das Ausführen des Skripts und das Festlegen des letzten Parameters auf 5:
```
./azure_hana_backup.pl lhanad01 customer 5
```
Als Folge der Ausführung des Skripts mit der obigen Einstellung wird die Anzahl der Snapshots einschließlich des neuen auf fünf reduziert. Beachten Sie Folgendes:

- Eine Reduzierung kann nur erreicht werden, wenn der letzte Snapshot, der vor der Ausführung des Befehls mit der geringeren Anzahl erstellt wurde, älter als&1; Stunde ist.
- Das Skript löscht außerdem keine Snapshots, die weniger als eine Stunde alt sind.

Diese Einschränkungen beziehen sich auf die optional gebotenen Funktionen für die Notfallwiederherstellung.

Angenommen, Sie möchten einen Satz von Snapshots mit einem bestimmten Präfix nicht mehr beibehalten. Sie können dann das Skript mit _0_ als beizubehaltender Anzahl angeben, woraufhin das Skript alle dem Präfix entsprechenden Snapshots entfernt und anschließend beendet wird. Allerdings wirkt sich dies auf die Fähigkeit zur Notfallwiederherstellung aus.

### <a name="recovering-to-the-latest-hana-snapshot"></a>Wiederherstellen des letzten HANA-Snapshots

Wenn die Produktionsumgebung ausfällt, kann die Wiederherstellung aus einem Speicher-Snapshot über das Melden eines Kundenvorfalls beim SAP HANA in Azure-Dienstverwaltungsteam ausgelöst werden. Dieser Vorfall kann eine hohe Dringlichkeit haben, wenn Daten aus einem Produktionssystem gelöscht wurden, sodass die einzige Möglichkeit, sie zurückzuerhalten, eine Wiederherstellung der Produktionsdatenbank ist.

Eine Zeitpunktwiederherstellung kann dagegen eine niedrigere Dringlichkeit haben und Tage im Voraus geplant werden. Dies können Sie mit dem SAP HANA in Azure-Dienstverwaltungsteam planen, anstatt ein Problem mit hoher Priorität zu melden. Dies wäre der Fall, wenn Sie ein Upgrade der SAP-Software planen, indem Sie ein neues Enhancement Package einspielen und anschließend zu einem Snapshot zurückkehren müssen, der den Zustand vor dem Enhancement Package-Upgrade darstellt.

Bevor Sie die Anforderung stellen, müssen einige Vorbereitungen erfolgen (siehe unten). Das SAP HANA in Azure-Dienstverwaltungsteam wird dann in der Lage sein, die Anforderung so zu bearbeiten, dass die wiederhergestellten Volumes bereitgestellt werden. Es ist Ihre Aufgabe, die HANA-Datenbank basierend auf den Snapshots wiederherzustellen.

>[!NOTE]  
>Die nachfolgenden Screenshots können je nach der von Ihnen verwendeten SAP HANA-Version variieren.

1. Entscheiden Sie, welcher Snapshot wiederhergestellt werden soll. Falls nicht anders angegeben, wird nur das Volume „hana/data“ wiederhergestellt.

2. Fahren Sie die HANA-Instanz herunter.
![Fahren Sie die HANA-Instanz herunter](./media/sap-hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Heben Sie die Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank auf. Die Wiederherstellung des Snapshots misslingt, wenn die Bereitstellung der Datenvolumes nicht aufgehoben wurde.
![Heben Sie die Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank auf](./media/sap-hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Erstellen Sie eine Azure-Supportanforderung, in der die Wiederherstellung eines bestimmten Snapshots angefordert wird.

    **Während der Wiederherstellung der Speicher-Snapshots** werden Sie ggf. vom SAP HANA in Azure-Dienstverwaltungsteam gebeten, an einer Telefonkonferenz teilzunehmen, um sicherzustellen, dass keine Daten verloren gehen.

    **Nach der Wiederherstellung des Speicher-Snapshots** werden Sie vom SAP HANA in Azure-Dienstverwaltungsteam benachrichtigt, sobald der Speicher-Snapshot wiederhergestellt wurde. 

5. Stellen Sie nach Abschluss des Wiederherstellungsvorgangs alle Datenvolumes wieder bereit.
![Stellen Sie alle Datenvolumes erneut bereit](./media/sap-hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Wählen Sie Wiederherstellungsoptionen in SAP HANA Studio aus, sofern diese nicht automatisch beim Wiederherstellen der Verbindung mit der HANA-Datenbank über HANA Studio angezeigt werden. Das folgende Beispiel zeigt eine Wiederherstellung des letzten HANA-Snapshots. Ein HANA-Snapshot ist in einen Speicher-Snapshot eingebettet. Wenn Sie den letzten Speicher-Snapshot wiederherstellen, sollte dieser den neuesten HANA-Snapshot enthalten. (Wenn Sie ältere Speicher-Snapshots wiederherstellen, müssen Sie den HANA-Snapshot basierend auf dem Zeitpunkt der Erstellung des Speicher-Snapshots bestimmen.) ![Wählen Sie in SAP HANA Studio Wiederherstellungsoptionen aus](./media/sap-hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Wählen Sie den Wiederherstellungstyp **Recover the database to a specific data backup or storage snapshot** aus.
![Wählen Sie den Wiederherstellungstyp „Recover the database to a specific data backup or storage snapshot“](./media/sap-hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Wählen Sie die Option **Specify backup without catalog**.
![Wählen Sie die Option „Specify backup without catalog“](./media/sap-hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. Wählen Sie **Destination Type: Snapshot**.
![Wählen Sie „Destination Type: Snapshot“](./media/sap-hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Klicken Sie auf **Finish**, um den Wiederherstellungsvorgang zu starten.
![Klicken Sie auf „Finish“, um den Wiederherstellungsvorgang zu starten](./media/sap-hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. Die HANA-Datenbank wird mithilfe des HANA-Snapshots wiederhergestellt, der im Speicher-Snapshot enthalten ist.
![Die HANA-Datenbank wird mithilfe des HANA-Snapshots wiederhergestellt, der im Speicher-Snapshot enthalten ist](./media/sap-hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-most-recent-state"></a>Wiederherstellen des letzten Status

Dieser Prozess dient zum Wiederherstellen eines HANA-Snapshots, der im Speicher-Snapshot enthalten ist, und anschließend zum Wiederherstellen der Transaktionsprotokollsicherungen mit dem letzten Status der Datenbank, ehe der Speicher-Snapshot wiederhergestellt wird.

>[!Important] 
>Stellen Sie sicher, dass Sie über eine vollständige und zusammenhängende Kette von Transaktionsprotokollsicherungen verfügen, bevor Sie fortfahren. Ohne diese können Sie nicht zurück um aktuellen Zustand der Datenbank gelangen.

Führen Sie die Schritte 1-6 des Verfahrens zur Wiederherstellung bis zum Moment der Erstellung des letzten HANA-Snapshots durch.

1. Wählen Sie **Recover the database to its most recent state**.
![Wählen Sie „Recover the database to its most recent state“](./media/sap-hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

2. Geben Sie den Speicherort der letzten HANA-Protokollsicherungen an. Der angegebene Speicherort muss alle HANA-Transaktionsprotokollsicherungen aus dem HANA-Snapshot bis zum letzten Zustand enthalten.
![Geben Sie den Speicherort der letzten HANA-Protokollsicherungen an](./media/sap-hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

3. Wählen Sie eine Sicherung als Grundlage zum Wiederherstellen der Datenbank aus. In unserem Beispiel ist dies der HANA-Snapshot, der im Speicher-Snapshot enthalten war. (Im folgenden Screenshot wurde nur einer dieser Snapshots erstellt.) ![Wählen Sie eine Sicherung als Grundlage zum Wiederherstellen der Datenbank aus](./media/sap-hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

4. Deaktivieren Sie **Use Delta Backups**, sofern diese nicht zwischen dem Zeitpunkt des HANA-Snapshots und dem letzten Zustand vorhanden waren.
![Deaktivieren Sie „Use Delta Backups“, sofern diese nicht zwischen dem Zeitpunkt des HANA-Snapshots und dem letzten Zustand vorhanden waren](./media/sap-hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

5. Klicken Sie auf dem Bildschirm mit der Zusammenfassung auf **Finish**, um den Wiederherstellungsvorgang zu starten.
![Klicken Sie auf dem Bildschirm mit der Zusammenfassung auf „Finish“, um den Wiederherstellungsvorgang zu starten](./media/sap-hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

Für eine Zeitpunktwiederherstellung zwischen dem HANA-Snapshot (der im Speicher-Snapshot enthalten ist) und einem Snapshot, der später als der HANA-Snapshot ist, gehen Sie so vor:

- Stellen Sie sicher, dass Sie über alle Transaktionsprotokollsicherungen im HANA-Snapshot bis zum Zeitpunkt verfügen, den Sie wiederherstellen möchten.
- Wählen Sie **Recover the database to the following point in time** (siehe Schritt 1 _Recover the database to its most recent state_).
- Die verbleibenden Schritte sind wie oben beschrieben.

## <a name="monitoring-snapshot-execution"></a>Überwachen der Ausführung des Snapshots

Die Ausführung von Storage-Snapshots muss von Ihnen überwacht werden. Das Skript, das einen Speicher-Snapshot ausführt, schreibt die Ausgabe in eine Datei, die am selben Speicherort wie die Perl-Skripts gespeichert wird. Für jeden Snapshot wird eine separate Datei geschrieben. Die Ausgabe dieser Datei zeigt eindeutig die verschiedenen Phasen, die das Snapshot-Skript durchläuft, nämlich:

- Suchen der Volumes, für die ein Snapshot erstellt werden muss
- Suchen der Snapshots, die von diesen Volumes erstellt wurden
- Löschen von eventuell vorhandenen Snapshots übereinstimmend mit der Anzahl von Snapshots, die Sie angegeben haben
- Erstellen eines HANA-Snapshots
- Erstellen des Speicher-Snapshots aller Volumes
- Löschen des HANA-Snapshots
- Umbenennen des neuesten Snapshots in **.0**

Der wichtigste Teil des Skripts ist dieser:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Sie können anhand dieses Beispiels erkennen, wie das Skript die Erstellung des HANA-Snapshots aufzeichnet. Bei horizontaler Skalierung wird dies auf dem Masterknoten ausgelöst. Der Masterknoten löst die synchrone Erstellung der Snapshots auf allen Workerknoten aus.

Dann wird der Speicher-Snapshot erstellt. Nach erfolgreicher Erstellung der Speichermomentaufnahmen wird die HANA-Momentaufnahme gelöscht.



<!--HONumber=Dec16_HO2-->


