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
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: fa842efd99718be7fa9eaf8aac8030c32cbceeec
ms.openlocfilehash: 661733edbabd61b42bfb44b4ed107b1e757c2e28
ms.lasthandoff: 03/01/2017


---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) 

Hohe Verfügbarkeit und Notfallwiederherstellung sind wichtige Aspekte bei der Ausführung Ihrer unternehmenskritischen Server mit SAP HANA in Azure (große Instanzen). Es ist wichtig, dass Sie mit SAP, Ihrem Systemintegrator oder Microsoft zusammenarbeiten, um die richtige Strategie für hohe Verfügbarkeit bzw. die Notfallwiederherstellung zu implementieren. Außerdem ist es wichtig, die Werte für RPO (Recovery Point Objective) und die RTO (Recovery Time Objective) zu berücksichtigen, die für Ihre Umgebung gelten.

## <a name="high-availability"></a>Hohe Verfügbarkeit

Microsoft unterstützt standardmäßig die SAP HANA-Methoden für hohe Verfügbarkeit, z.B.:

- **Speicherreplikation:** Die Fähigkeit des Speichersystems, alle Daten an einen anderen Speicherort zu replizieren (entweder innerhalb oder außerhalb desselben Datencenters). SAP HANA wird unabhängig von dieser Methode betrieben.
- **HANA-Systemreplikation:** Die Replikation aller Daten in SAP HANA in ein anderes SAP HANA-System. RTO (Recovery Time Objective) wird durch die Datenreplikation in regelmäßigen Abständen verringert. SAP HANA unterstützt asynchrone, synchrone In-Memory- und synchrone Modi (nur empfohlen für SAP HANA-Systeme innerhalb des gleichen Datencenters oder mit einer Entfernung von weniger als 100 km zueinander). Beim aktuellen Entwurf mit HANA-„Stapeln“ mit großen Instanzen kann die HANA-Systemreplikation nur für hohe Verfügbarkeit verwendet werden.
- **Automatisches Hostfailover:** Eine lokale Lösung zur Wiederherstellung nach Fehlern, die als Alternative zur Systemreplikation verwendet werden kann. Wenn der Masterknoten nicht mehr verfügbar ist, wird mindestens ein SAP HANA-Standbyknoten im Modus „Horizontal hochskalieren“ konfiguriert, und SAP HANA führt ein automatisches Failover auf einen anderen Knoten durch.

Weitere Informationen zur hohen Verfügbarkeit von SAP HANA finden Sie in den folgenden SAP-Informationen:

- [Whitepaper zur hohen Verfügbarkeit von SAP HANA](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA-Administratorhandbuch](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy-Video zur SAP HANA-Systemreplikation](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP Support Note #1999880 – FAQ on SAP HANA System Replication](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP Support Note #2165547 – SAP HANA Backup and Restore within SAP HANA System Replication Environment](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP Support Note #1984882 – Using SAP HANA System Replication for Hardware Exchange with Minimum/Zero Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="disaster-recovery"></a>Notfallwiederherstellung

SAP HANA in Azure (große Instanzen) wird in zwei Azure-Regionen in einer geopolitischen Region angeboten. Zwischen den beiden „Große Instanz“-Stapeln zweier verschiedener Regionen gibt es eine direkte Netzwerkverbindung zum Replizieren von Daten während der Notfallwiederherstellung. Die Replikation der Daten basiert auf der Speicherinfrastruktur. Standardmäßig wird die Replikation nicht durchgeführt. Sie wird für die Kundenkonfigurationen durchgeführt, von denen die Notfallwiederherstellung angefordert wurde. Im aktuellen Entwurf kann die HANA-Systemreplikation nicht für die Notfallwiederherstellung verwendet werden.

Um aber die Notfallwiederherstellung nutzen zu können, müssen Sie die Netzwerkverbindung zwischen den beiden Azure-Regionen einrichten. Hierzu benötigen Sie eine Azure ExpressRoute-Verbindung von Ihrem lokalen Standort in Ihrer Azure-Hauptregion und eine andere Verbindung von Ihrem lokalen Standort mit der Region für die Notfallwiederherstellung. Diese Maßnahme deckt eine Situation ab, in der eine ganze Azure-Region samt MSEE-Standort (Microsoft Enterprise-Edgerouter) einen Ausfall erleidet.

Als zweite Maßnahme können Sie für alle virtuellen Azure-Netzwerke, die mit SAP HANA in Azure (große Instanzen) in einer der Regionen verbunden sind, Verbindungen mit diesen beiden ExpressRoute-Verbindungen herstellen. Die ist eine Maßnahme für den Fall, in dem nur einer der MSEE-Standorte, der mit Ihrem lokalen Standort über Azure verbunden ist, ausfällt.

Die folgende Abbildung zeigt die optimale Konfiguration für die Notfallwiederherstellung:

![Optimale Konfiguration für die Notfallwiederherstellung](./media/sap-hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

Die optimale Konfiguration für die Notfallwiederherstellung des Netzwerks sieht zwei ExpressRoute-Verbindungen der lokalen Umgebung mit zwei verschiedenen Azure-Regionen vor. Eine Verbindung besteht mit Region 1, in der eine Produktionsinstanz ausgeführt wird. Die zweite ExpressRoute-Verbindung besteht mit Region 2, in der einige andere HANA-Instanzen ausgeführt werden, die nicht für die Produktion bestimmt sind. (Dies ist wichtig, wenn eine gesamte Azure-Region, einschließlich MSEE und „Große Instanzen“-Stapel, ausfällt.)

Die zweite Maßnahme besteht darin, die verschiedenen virtuellen Netzwerke mit den einzelnen ExpressRoute-Verbindungen zu verbinden, die mit SAP HANA in Azure (große Instanzen) verknüpft sind. Sie können den Standort umgehen, an dem ein MSEE ausfällt, oder Sie können die RPO (Recovery Point Objective) für die Notfallwiederherstellung reduzieren. Dies wird weiter unten beschrieben.

Die nächsten Anforderungen für die Einrichtung der Notfallwiederherstellung sind:

- Es ist erforderlich, SKUs vom Typ „SAP HANA für Azure (große Instanzen)“ mit derselben Größe wie die Produktions-SKUs anzufordern und in der Region für die Notfallwiederherstellung bereitzustellen. Diese Instanzen können zum Ausführen von HANA-Instanzen für Test-, Sandbox- oder QS-Zwecke verwendet werden.
- Sie müssen ein Notfallwiederherstellungsprofil für jede SKU vom Typ „SAP HANA in Azure (große Instanzen)“ anfordern, die Sie, falls erforderlich, am Standort für die Notfallwiederherstellung wiederherstellen möchten. Diese Aktion führt zur Zuordnung von Speichervolumes, die das Ziel der Speicherreplikation aus Ihrer Produktionsregion in die Notfallwiederherstellungsregion sind.

Nachdem Sie die obigen Anforderungen erfüllt haben, sind Sie dafür verantwortlich, die Speicherreplikation zu starten. Bei der für SAP HANA in Azure (große Instanzen) verwendeten Speicherinfrastruktur bilden Speichermomentaufnahmen die Basis der Speicherreplikation. Zum Starten der Replikation für die Notfallwiederherstellung müssen Sie Folgendes durchführen:

- Erstellen einer Momentaufnahme Ihrer Start-LUN (wie zuvor beschrieben)
- Erstellen einer Momentaufnahme Ihrer zu HANA gehörigen Volumes (wie zuvor beschrieben)

Nachdem Sie diese Momentaufnahmen erstellt haben, wird ein Anfangsreplikat der Volumes auf den Volumes platziert, die Ihrem Notfallwiederherstellungsprofil in der Region für die Notfallwiederherstellung zugeordnet sind.

Anschließend wird jede Stunde die letzte Speichermomentaufnahme verwendet, um die Deltas zu replizieren, die sich auf den Speichervolumes entwickeln.

Die RPO (Recovery Point Objective), die mit dieser Konfiguration erzielt wird, beträgt 60 bis 90 Minuten. Kopieren Sie die HANA-Transaktionsprotokollsicherungen aus SAP HANA in Azure (große Instanzen) in die andere Azure-Region, um bei einer Notfallwiederherstellung eine bessere RPO (Recovery Point Objective) zu erreichen. Gehen Sie wie folgt vor, um diese RPO (Recovery Point Objective) zu erreichen:

1. Sichern Sie das HANA-Transaktionsprotokoll so häufig wie möglich unter „/hana/log/backup“.
2. Kopieren Sie die fertigen Sicherungen des Transaktionsprotokolls auf einen virtuellen Azure-Computer in einem virtuellen Netzwerk, das mit dem Server mit SAP HANA in Azure (große Instanzen) verbunden ist.
3. Kopieren Sie die Sicherung von dieser VM auf eine VM, die sich in einem virtuellen Netzwerk in der Region für die Notfallwiederherstellung befindet.
4. Belassen Sie die Sicherungen des Transaktionsprotokolls in dieser Region in der VM.

Kopieren Sie bei einem Notfall, nachdem das Notfallwiederherstellungsprofil auf einem tatsächlichen Server bereitgestellt wurde, die Sicherungen des Transaktionsprotokolls von der VM auf den Server mit SAP HANA in Azure (große Instanzen), der nun der primäre Server in der Notfallwiederherstellungsregion ist, und stellen Sie diese Sicherungen wieder her. Diese Wiederherstellung ist möglich, da der Zustand von HANA auf den Datenträgern für die Notfallwiederherstellung der Zustand einer HANA-Momentaufnahme ist. Dies ist der Ausgangspunkt für weitere Wiederherstellungen von Transaktionsprotokollsicherungen.

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Einer der wichtigsten Aspekte beim Betreiben von Datenbanken ist das Sicherstellen, dass die Datenbank vor verschiedenen Katastrophenfällen geschützt ist. Diese Ereignisse können sowohl durch Naturkatastrophen als auch einfache Benutzerfehler verursacht werden.

Das Sichern einer Datenbank mit der Möglichkeit einer beliebigen Zeitpunktwiederherstellung (z.B. vor dem Zeitpunkt des Löschens kritischer Daten) lässt das Wiederherstellen eines Status zu, der möglichst genau dem Status vor der Störung entspricht.

Für optimale Ergebnisse müssen zwei Arten von Sicherungen erfolgen:

- Datenbanksicherungen
- Transaktionsprotokollsicherungen

Zusätzlich zu vollständigen Datenbanksicherungen auf Anwendungsebene können Sie Sicherungen mit Speichermomentaufnahmen erstellen. Das Erstellen von Protokollsicherungen ist auch wichtig für das Wiederherstellen der Datenbank (und auch das Leeren der Protokolle von Transaktionen mit erfolgtem Commit).

Es gibt zwei Sicherungs- und Wiederherstellungsoptionen für SAP HANA in Azure (große Instanzen):

- Eigenständig. Führen Sie, nachdem genügend vorhandener Speicherplatz berechnet wurde, vollständige Datenbank- und Protokollsicherungen mithilfe von Datenträger-Sicherungsmethoden (auf diese Datenträger) durch. Im Laufe der Zeit werden die Sicherungen in ein Azure-Speicherkonto kopiert (nachdem ein auf Azure basierender Dateiserver mit praktisch unbegrenztem Speicher eingerichtet wurde). Sie können auch einen Azure-Sicherungstresor oder Azure Cold Storage verwenden. Eine andere Möglichkeit ist die Nutzung eines Datenschutztools eines Drittanbieters, z.B. Commvault, um die Sicherungen zu speichern, nachdem sie in ein Speicherkonto kopiert wurden. Die eigenständige Sicherungsoption kann auch für Daten erforderlich sein, die aufgrund von Konformitäts- oder Überwachungszwecken über längere Zeiträume gespeichert werden müssen.
- Verwenden Sie die Sicherungs- und Wiederherstellungsfunktionalität, die die zugrunde liegende Infrastruktur von SAP HANA in Azure (große Instanzen) bereitstellt. Diese Option erfüllt den Bedarf an Sicherungen und macht manuelle Sicherungen nahezu überflüssig (außer in Fällen, in denen Datensicherungen für Konformitätszwecke benötigt werden). Im restlichen Teil dieses Abschnitts werden die Sicherungs- und Wiederherstellungsfunktionen behandelt, die von HANA (große Instanzen) bereitgestellt werden.

> [!NOTE]
> Die Technologie für Momentaufnahmen, die von der zugrunde liegenden Infrastruktur von HANA (große Instanzen) verwendet wird, verfügt über eine Abhängigkeit von SAP HANA-Momentaufnahmen. SAP HANA-Momentaufnahmen funktionieren nicht in Verbindung mit SAP HANA-Datenbankcontainern mit mehreren Mandanten. Daher kann diese Sicherungsmethode nicht zum Bereitstellen von SAP HANA-Datenbankcontainern mit mehreren Mandanten verwendet werden.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Verwenden von Speichermomentaufnahmen von SAP HANA in Azure (große Instanzen)

Die zugrunde liegende Speicherinfrastruktur von SAP HANA in Azure (große Instanzen) unterstützt das Konzept einer Speichermomentaufnahme von Volumes. Sowohl die Sicherung als auch die Wiederherstellung eines bestimmten Volumes wird unter Berücksichtigung der folgenden Aspekte unterstützt:

- Anstelle von Datenbanksicherungen erfolgen Momentaufnahmen von Speichervolumes in regelmäßigen Abständen.
- Die Speichermomentaufnahme löst eine SAP HANA-Momentaufnahme vor der Erstellung der Speichermomentaufnahme aus. Diese SAP HANA-Momentaufnahme ist der Einrichtungspunkt für nachfolgende Wiederherstellungen von Protokollen, nachdem die Speichermomentaufnahme wiederhergestellt wurde.
- Sobald die Speichermomentaufnahme erfolgreich erstellt wurde, wird die SAP HANA-Momentaufnahme gelöscht.
- Protokollsicherungen erfolgen in regelmäßigen Abständen und werden auf dem Protokollsicherungsvolume oder in Azure gespeichert.
- Wenn die Datenbank bezogen auf einen bestimmten Zeitpunkt wiederhergestellt werden muss, wird eine Anforderung an den Microsoft Azure-Support (Produktionsausfall) oder das SAP HANA in Azure-Dienstverwaltungsteam gesendet, um eine Wiederherstellung bis zu einer bestimmten Speichermomentaufnahme durchführen zu lassen (z.B. eine geplante Wiederherstellung eines Sandboxsystems in seinem ursprünglichen Zustand).
- Die in der Speichermomentaufnahme enthaltene SAP HANA-Momentaufnahme ist ein Ausgangspunkt zum Anwenden von Protokollsicherungen, die ausgeführt und gespeichert wurden, nachdem die Speichermomentaufnahme erstellt wurde.
- Diese Protokollsicherungen werden erstellt, um die Datenbank an einem bestimmten Punkt wiederherzustellen.

Wenn Sie den „backup\_name“ angeben, wird eine Momentaufnahme der folgenden Volumes erstellt:

- hana/data
- hana/log
- hana/log\_backup (als Sicherung in „hana/log“ eingebunden)
- hana/shared

### <a name="storage-snapshot-considerations"></a>Aspekte von Speichermomentaufnahmen

>[!NOTE]
>Speichermomentaufnahmen werden _nicht_ kostenlos zur Verfügung gestellt, da zusätzlicher Speicherplatz zugeordnet werden muss.

Zur besonderen Funktionsweise von Speichermomentaufnahmen von SAP HANA in Azure (große Instanzen) gehört Folgendes:

- Eine bestimmte Speichermomentaufnahme (zum Zeitpunkt der Erstellung) belegt nur sehr wenig Speicherplatz.
- Wenn sich Dateninhalte und der Inhalt in SAP HANA-Datendateien auf dem Speichervolume ändern, muss die Momentaufnahme den ursprünglichen Blockinhalt speichern.
- Die Speichermomentaufnahme vergrößert sich. Je länger die Momentaufnahme vorhanden ist, desto größer wird die Speichermomentaufnahme.
- Je mehr Änderungen am SAP HANA-Datenbankvolume während der Lebensdauer einer Speichermomentaufnahme erfolgen, desto mehr Speicherplatz belegt die Speichermomentaufnahme.

SAP HANA in Azure (große Instanzen) bietet standardmäßig feste Volumegrößen für das SAP HANA-Daten- und Protokollvolume. Das Erstellen von Momentaufnahmen dieser Volumes belegt Speicherplatz auf Ihren Volumes, weshalb Sie für das Planen von Speichermomentaufnahmen (innerhalb des SAP HANA in Azure (große Instanzen)-Prozesses) zuständig sind.

Die folgenden Abschnitte enthalten Informationen zum Erstellen dieser Momentaufnahmen sowie allgemeine Empfehlungen:

- Obwohl die Hardware 255 Momentaufnahmen pro Volume unterstützen kann, wird dringend empfohlen, deutlich unter diesem Wert zu bleiben.
- Überwachen und verfolgen Sie den freien Speicherplatz, bevor Sie Speichermomentaufnahmen erstellen.
- Verringern Sie die Anzahl von Speichermomentaufnahmen basierend auf dem freien Speicherplatz. Unter Umständen müssen Sie die Anzahl von erstellten Momentaufnahmen verringern, oder Sie müssen die Volumes erweitern. (Sie können zusätzlichen Speicher in 1-TB-Einheiten anfordern.)
- Bei Aktivitäten wie dem Verschieben von Daten in SAP HANA mit Systemmigrationstools (mit R3load oder per Wiederherstellung von SAP HANA-Datenbanken aus Sicherungen) wird dringend empfohlen, keine Speichermomentaufnahmen zu erstellen. (Eine Systemmigration erfolgt auf einem neuen SAP HANA-System, sodass Speichermomentaufnahmen nicht ausgeführt werden müssen.)
- Bei größeren Reorganisationen von SAP HANA-Tabellen sollten Speichermomentaufnahmen nach Möglichkeit vermieden werden.
- Speichermomentaufnahmen sind eine Voraussetzung für die Nutzung der Funktionen für die Notfallwiederherstellung von SAP HANA in Azure (große Instanzen).

### <a name="setting-up-storage-snapshots"></a>Einrichten von Speichermomentaufnahmen

1. Stellen Sie sicher, dass Perl im Linux-Betriebssystem auf dem Server mit HANA (große Instanzen) installiert ist.
2. Fügen Sie „/etc/ssh/ssh\_config“ die Zeile _MACs hmac-sha1_ hinzu.
3. Erstellen Sie ein SAP HANA-Sicherungsbenutzerkonto auf dem Masterknoten für jede SAP HANA-Instanz, die Sie ausführen (falls zutreffend).
4. Der SAP HANA HDB-Client muss auf allen Servern mit SAP HANA (große Instanzen) installiert werden.
5. Auf dem ersten Server mit SAP HANA (große Instanzen) in jeder Region muss ein öffentlicher Schlüssel erstellt werden, um auf die zugrunde liegende Speicherinfrastruktur zuzugreifen, die die Erstellung von Momentaufnahmen steuert.
6. Kopieren Sie das Skript „azure\_hana\_backup.pl“ aus „/scripts“ an den Speicherort von **hdbsql** in der SAP HANA-Installation.
7. Kopieren Sie die Datei „HANABackupDetails.txt“ aus „/scripts“ an denselben Speicherort wie das Perl-Skript.
8. Ändern Sie bei Bedarf die Datei „HANABackupDetails.txt“ gemäß den jeweiligen Kundenvorgaben.

### <a name="step-1-install-sap-hana-hdbclient"></a>Schritt 1: Installieren des SAP HANA HDB-Clients

Das für SAP HANA in Azure (große Instanzen) installierte Linux-Betriebssystem enthält die Ordner und Skripts, die benötigt werden, um die SAP HANA-Speichermomentaufnahmen für Sicherungs- und Notfallwiederherstellungszwecke zu erstellen. Es ist aber Ihre Aufgabe, den SAP HANA HDB-Client während der Installation von SAP HANA zu installieren. (Microsoft installiert weder den HDB-Client noch SAP HANA.)

### <a name="step-2-change-etcsshsshconfig"></a>Schritt 2: Ändern von „/etc/ssh/ssh\_config“

Ändern Sie „/etc/ssh/ssh\_config“ wie folgt durch das Hinzufügen der Zeile _MACs hmac-sha1_:
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

### <a name="step-3-create-a-public-key"></a>Schritt 3: Erstellen eines öffentlichen Schlüssels

Erstellen Sie auf dem ersten Server mit SAP HANA in Azure (große Instanzen) in jeder Azure-Region einen öffentlichen Schlüssel, der für den Zugriff auf die Speicherinfrastruktur verwendet wird, um die Erstellung von Momentaufnahmen zu ermöglichen. Der öffentliche Schlüssel stellt sicher, dass für die Anmeldung beim Speicher kein Kennwort erforderlich ist und dass die Kennwortanmeldeinformationen nicht beibehalten werden. Führen Sie unter Linux auf dem Server mit SAP HANA (große Instanzen) den folgenden Befehl aus, um den öffentlichen Schlüssel zu generieren:
```
  ssh-keygen –t dsa –b 1024
```
Der neue Speicherort ist „_/root/.ssh/id\_dsa.pub“. Geben Sie keine Passphrase ein, das dies sonst bei jedem Anmeldevorgang erforderlich ist. Drücken Sie stattdessen zweimal die **EINGABETASTE**, um die Anforderung zur Eingabe der Passphrase bei der Anmeldung zu entfernen.

Vergewissern Sie sich, dass der öffentliche Schlüssel wie erwartet korrigiert wurde, indem Sie zu „/root/.ssh/“ navigieren und dann den Befehl **ls** ausführen. Wenn der Schlüssel vorhanden ist, können Sie ihn mit dem folgenden Befehl kopieren:

![Der öffentliche Schlüssel wird durch Ausführen dieses Befehls kopiert](./media/sap-hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Wenden Sie sich an diesem Punkt an das SAP HANA in Azure-Dienstverwaltungsteam, und geben Sie den Schlüssel an. Die Mitarbeiterin bzw. der Mitarbeiter verwendet den öffentlichen Schlüssel für die Registrierung in der zugrunde liegenden Speicherinfrastruktur.

### <a name="step-4-create-an-sap-hana-user-account"></a>Schritt 4: Erstellen eines SAP HANA-Benutzerkontos

Erstellen Sie für Sicherungszwecke in SAP HANA Studio ein SAP HANA-Benutzerkonto. Dieses Konto benötigt die folgenden Berechtigungen: _Backup Admin_ und _Catalog Read_. In diesem Beispiel wird der Benutzername SCADMIN erstellt.

![Erstellen eines Benutzers in HANA Studio](./media/sap-hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Schritt 5: Autorisieren des SAP HANA-Benutzerkontos

Autorisieren Sie das SAP HANA-Benutzerkonto (das von den Skripts verwendet wird, ohne bei jeder Skriptausführung eine Autorisierung anzufordern). Mit dem SAP HANA-Befehl `hdbuserstore` können Sie einen SAP HANA-Benutzerschlüssel erstellen, der auf mindestens einem SAP HANA-Knoten gespeichert wird. Außerdem ermöglicht der Benutzerschlüssel für Benutzer den Zugriff auf SAP HANA, ohne dass über den Skripterstellungsprozess Kennwörter verwaltet werden müssen. Dies wird weiter unten beschrieben.

>[!IMPORTANT]
>Führen Sie den folgenden Befehl als `_root_` aus. Andernfalls funktioniert das Skript nicht richtig.

Geben Sie den Befehl `hdbuserstore` wie folgt ein:

![Eingeben des Befehls „hdbuserstore“](./media/sap-hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

Im folgenden Beispiel, bei dem der Benutzer SCADMIN01 und Hostname „lhanad01“ lautet, ist der Befehl wie folgt:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Verwalten Sie für horizontal hochskalierte HANA-Instanzen alle Skripts über einen einzelnen Server. In diesem Beispiel muss der SAP HANA-Schlüssel SCADMIN01 für jeden Host so angepasst werden, dass der zum Schlüssel passende Host widergespiegelt wird. Hierzu wird das SAP HANA-Sicherungskonto mit der Instanznummer der HANA DB, **lhanad**, angepasst. Der Schlüssel benötigt Administratorrechte auf dem Host, dem er zugewiesen ist. Der Sicherungsbenutzer (für horizontale Skalierung) benötigt Zugriffsrechte für alle SAP HANA-Instanzen.
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

### <a name="step-6-copy-items-from-the-scripts-folder"></a>Schritt 6: Kopieren von Elementen aus dem Ordner „/scripts“

Kopieren Sie die folgenden Elemente aus dem Ordner „/scripts“, die im Gold-Image der Installation enthalten sind, in das Arbeitsverzeichnis für **hdbsql**. Für aktuelle HANA-Installationen lautet dieses Verzeichnis „/hana/shared/D01/exe/linuxx86\_64/hdb“.
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
Kopieren Sie folgende Elemente, wenn die horizontale Skalierung oder OLAP ausgeführt wird:
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
Die Datei „HANABackupCustomerDetails.txt“ kann für eine Bereitstellung mit horizontaler Skalierung wie folgt geändert werden. Es handelt sich um die Steuerungs- und Konfigurationsdatei für das Skript, mit dem die Speichermomentaufnahmen erstellt werden. Die Angaben für _Storage Backup Name_ und _Storage IP Address_ sollten Sie vom SAP HANA in Azure-Dienstverwaltungsteam erhalten haben, als Ihre Instanzen bereitgestellt wurden. Reihenfolge, Sortierung und Abstände dieser Variablen können nicht geändert werden, da sonst das Skript nicht richtig ausgeführt wird.

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
>[!NOTE]
>Derzeit werden nur die Details von Knoten 1 im eigentlichen Skript der HANA-Speichermomentaufnahme genutzt. Wir empfehlen Ihnen, den Zugriff auf bzw. von allen HANA-Knoten zu testen. Sollte sich der Masterknoten für die Sicherungen dann einmal ändern, haben Sie bereits sichergestellt, dass ein anderer Knoten seinen Platz einnehmen kann, indem die Details auf Knoten 1 geändert werden.

Führen Sie eines der folgenden Skripts aus, um eine Prüfung auf die richtigen Konfigurationen in der Konfigurationsdatei oder die richtige Verbindung mit den HANA-Instanzen durchzuführen:
- Für eine Konfiguration für das zentrale Hochskalieren (unabhängig von der SAP-Workload):

 ```
testHANAConnection.pl
```
- Für eine Konfiguration für das horizontale Hochskalieren:

 ```
testHANAConnectionBW.pl
```

Stellen Sie sicher, dass die HANA-Masterinstanz Zugriff auf alle benötigten HANA-Server hat. Das Skript enthält keine Parameter, aber Sie müssen die entsprechende Datei „HANABackupCustomerDetails“/„HANABackupCustomerDetailsBW“ ausfüllen, damit das Skript richtig ausgeführt wird. Da nur die Fehlercodes des Shell-Befehls zurückgegeben werden, ist es für das Skript nicht möglich, jede Instanz auf Fehler zu prüfen. Mit dem Skript werden trotzdem einige hilfreiche Kommentare bereitgestellt, die Sie überprüfen können.

So führen Sie das Skript aus
```
 ./testHANAConnection.pl
```
 Wenn das Skript den Status der HANA-Instanz erfolgreich abruft, wird eine Nachricht mit dem Hinweis angezeigt, dass die HANA-Verbindung hergestellt wurde.

Darüber hinaus gibt es einen zweiten Typ von Skript, mit dem Sie überprüfen können, ob sich der HANA-Masterinstanzserver beim Speicher anmelden kann. Bevor Sie das Skript „azure\_hana\_backup(\_bw).pl“ ausführen, müssen Sie das nächste Skript ausführen. Wenn ein Volume keine Momentaufnahmen enthält, ist es unmöglich zu bestimmen, ob das Volume einfach leer ist oder ob ein SSH-Fehler vorliegt und keine Details zu Momentaufnahmen beschafft werden können. Aus diesem Grund werden mit dem Skript zwei Schritte ausgeführt:

- Es wird überprüft, ob auf die Speicherkonsole zugegriffen werden kann.
- Anhand der HANA-Instanz wird für jedes Volume ein Test, ein Dummy oder eine Momentaufnahme erstellt.

Aus diesem Grund ist die HANA-Instanz als ein Argument enthalten. Wiederum ist keine Fehlerüberprüfung für die Speicherverbindung möglich, aber das Skript enthält hilfreiche Hinweise, wenn die Ausführung fehlschlägt.

Das Skript wird ausgeführt als:
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
Oder es wird ausgeführt als:
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
Das Skript zeigt auch eine Meldung an, dass Sie sich richtig an Ihrem bereitgestellten Speichermandanten anmelden können, der basierend auf den logischen Gerätenummern (LUNs) organisiert ist, die von Ihren eigenen Serverinstanzen verwendet werden.

Bevor Sie die ersten auf Speichermomentaufnahmen basierenden Sicherungen erstellen, führen Sie die nächsten Skripts aus, um sicherzustellen, dass die Konfiguration korrekt ist.

Nach Ausführung dieser Skripts können Sie die Momentaufnahmen durch Ausführen eines der folgenden Befehle löschen:
```
./removeTestStorageSnapshot.pl <hana instance>
```
oder
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="step-7-perform-on-demand-snapshots"></a>Schritt 7: Bedarfsabhängiges Erstellen von Momentaufnahmen

Erstellen Sie wie hier beschrieben bedarfsabhängige Momentaufnahmen (und planen Sie mit Cron regelmäßige Momentaufnahmen).

Führen Sie für Konfigurationen für das zentrale Hochskalieren das folgende Skript aus:
```
./azure_hana_backup.pl lhanad01 customer 20
```
Führen Sie für Konfigurationen für das horizontale Hochskalieren das folgende Skript aus:
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
Das Skript für horizontale Skalierung führt einige zusätzliche Prüfungen durch, um den Zugriff auf alle HANA-Server sicherzustellen. Alle HANA-Instanzen geben den entsprechenden Status der Instanz zurück, ehe mit dem Erstellen von SAP HANA- oder Speichermomentaufnahmen fortgefahren wird.

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
- Entfernen der HANA-Momentaufnahme

Führen Sie das Skript aus, indem Sie es im ausführbaren HDB-Ordner aufrufen, in den es kopiert wurde. Es sichert mindestens die folgenden Volumes, aber auch Volumes, die den expliziten SAP HANA-Instanznamen im Volumenamen enthalten.
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
Die Beibehaltungsdauer entspricht streng der Anzahl von Momentaufnahmen, die bei Ausführung des Skripts als Parameter übergeben wird (z.B. 20 im obigen Beispiel). Die Dauer hängt vom Ausführungszeitraum und der Anzahl der Momentaufnahmen im Aufruf des Skripts ab. Wenn die Anzahl von beibehaltenen Momentaufnahmen die Anzahl in einem Parameter im Skriptaufruf überschreitet, wird die älteste Speichermomentaufnahme mit dieser Bezeichnung (im obigen Fall _custom_) gelöscht, bevor eine neue Momentaufnahme ausgeführt wird. Dies bedeutet, dass die Anzahl, die Sie als letzten Parameter des Aufrufs angeben, dazu dient, die Anzahl der Momentaufnahmen zu steuern.

>[!NOTE]
>Sobald Sie die Bezeichnung ändern, beginnt der Zählvorgang erneut.

Sie müssen den HANA-Instanznamen, den Sie vom SAP HANA in Azure-Dienstverwaltungsteam erhalten haben, als Argument hinzufügen, wenn eine Erstellung von Momentaufnahmen in Umgebungen mit mehreren Knoten erfolgt. In Umgebungen mit einzelnem Knoten ist der Name der SAP HANA in Azure (große Instanzen)-Einheit ausreichend. Der HANA-Instanzname wird aber weiterhin empfohlen.

Darüber hinaus können Sie Startvolumes\LUNs mit demselben Skript sichern. Sie müssen Ihr Startvolume mindestens einmal sichern, wenn Sie HANA zum ersten Mal ausführen. Wir empfehlen Ihnen aber einen Zeitplan mit wöchentlicher oder nächtlicher Sicherung für das Startvolume in Cron. Anstatt einen SAP HANA-Instanznamen hinzuzufügen, fügen Sie wie folgt _boot_ als Argument in das Skript ein:
```
./azure_hana_backup boot customer 20
```
Dieselbe Aufbewahrungsrichtlinie wird auch für das Startvolume erzeugt. Verwenden Sie bedarfsabhängig erstellte Momentaufnahmen nur in bestimmten Fällen, z.B. während eines SAP EHP-Upgrades (SAP Enhancement Package) oder wenn Sie eine besondere Speichermomentaufnahme erstellen müssen.

Wir raten Ihnen, mit Cron geplante Speichermomentaufnahmen durchzuführen. Außerdem wird empfohlen, für alle Sicherungs- und Notfallwiederherstellungsaufgaben dasselbe Skript zu verwenden (indem Sie die Skripteingaben ändern, um es an die unterschiedlichen Zeiten für angeforderte Sicherungen anzupassen). Diese werden in Cron alle abhängig von ihrer Ausführungszeit unterschiedlich geplant: stündlich, alle 12 Stunden, täglich oder wöchentlich. Der Cron-Zeitplan dient zum Erstellen von Speichermomentaufnahmen, die der weiter oben beschriebenen Aufbewahrungsbezeichnung für die langfristige standortexterne Sicherung entsprechen. Das Skript enthält Befehle zum Sichern aller Produktionsvolumes in Abhängigkeit von der angeforderten Häufigkeit (Daten- und Protokolldateien werden stündlich gesichert, während das Startvolume täglich gesichert wird).

Die Einträge im folgenden Cron-Skript werden stündlich in der zehnten Minute, alle 12 Stunden in der zehnten Minute und täglich in der zehnten Minute ausgeführt. Die Cron-Aufträge sind so eingerichtet, dass nur eine SAP HANA-Speichermomentaufnahme stündlich erstellt wird, damit die stündlichen und täglichen Sicherungen nicht zeitgleich (00:10 Uhr) erstellt werden. Das SAP HANA in Azure-Dienstverwaltungsteam schlägt Ihnen die empfohlene Uhrzeit zum Erstellen Ihrer Sicherungen vor, um die Erstellung und Replikation von Momentaufnahmen zu optimieren.

Die standardmäßige Cron-Zeitplanung in „/etc/crontab“ ist wie folgt:
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
In den obigen Cron-Anweisungen wird für die HANA-Volumes (ohne das Startvolume) eine stündliche Momentaufnahme mit der entsprechenden Bezeichnung erstellt. Von diesen Momentaufnahmen werden 66 beibehalten. Außerdem werden 14 Momentaufnahmen mit 12-Stunden-Bezeichnung beibehalten. Sie müssten also stündliche Momentaufnahmen für drei Tage sowie 12-Stunden-Momentaufnahmen für weitere vier Tage erhalten. Sie verfügen somit über Momentaufnahmen für eine gesamte Woche.

Die Zeitplanung mit Cron kann knifflig sein, da zu einem beliebigen Zeitpunkt immer nur ein Skript ausgeführt werden sollte, sofern die Skripts nicht um mehrere Minuten zeitversetzt sind. Wenn tägliche Sicherungen langfristig aufbewahrt werden sollen, wird eine tägliche Momentaufnahme zusammen mit einer 12-Stunden-Momentaufnahme beibehalten (mit einer Aufbewahrungsanzahl von jeweils sieben). Oder die stündliche Momentaufnahme ist um 10 Minuten nach hinten zeitversetzt. Auf dem Produktionsvolume wird nur eine tägliche Momentaufnahme aufbewahrt.
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
Die hier aufgeführten Häufigkeiten sind lediglich Beispiele. Verwenden Sie die folgenden Kriterien, um die optimale Anzahl von Momentaufnahmen zu ermitteln:

- Anforderungen in Bezug auf die RTO (Recovery Time Objective) für die Zeitpunktwiederherstellung.
- Speicherplatzbelegung.
- Anforderungen in Bezug auf die RPO (Recovery Point Objective) und RTO (Recovery Time Objective) für die potenzielle Notfallwiederherstellung.
- Tatsächliche Erstellung vollständiger HANA-Datenbanksicherungen für Datenträger. Immer wenn eine vollständige Datenbanksicherung für Datenträger (Schnittstelle _backint_) erfolgt, schlägt die Erstellung von Speichermomentaufnahmen fehl. Wenn Sie zusätzlich zu Speichermomentaufnahmen vollständige Datenbanksicherungen ausführen möchten, sollten Sie sicherstellen, dass die Erstellung von Speichermomentaufnahmen während dieses Zeitraums deaktiviert ist.

>[!IMPORTANT]
> Die Verwendung von Speichermomentaufnahmen für SAP HANA-Sicherungen ist nur gültig, wenn diese in Verbindung mit SAP HANA-Protokollsicherungen erstellt werden. Diese Protokollsicherungen müssen die Zeiträume zwischen den Speichermomentaufnahmen abdecken. Sie benötigen Folgendes, wenn Sie für Benutzer eine Zeitpunktwiederherstellung für einen Zeitraum von 30 Tagen zugesagt haben:

- Die Fähigkeit des Zugriffs auf eine Speichermomentaufnahme, die 30 Tage alt ist.
- Zusammenhängende Protokollsicherungen der letzten 30 Tage.

Erstellen Sie im Bereich der Protokollsicherungen auch eine Momentaufnahme des Sicherungsprotokollvolumes. Achten Sie aber darauf, regelmäßige Protokollsicherungen durchzuführen, damit Folgendes erfüllt ist:

- Sicherstellen des Vorhandenseins von zusammenhängenden Protokollsicherungen, die für Zeitpunktwiederherstellungen erforderlich sind
- Verhindern, dass der Speicherplatz des SAP HANA-Protokollvolumes zur Neige geht

Einer der letzten Schritte ist das Planen von SAP HANA-Sicherungsprotokollen in SAP HANA Studio. Das Ziel des SAP HANA-Sicherungsprotokolls ist das spezifisch erstellte Volume „hana/log\_backups“ mit dem Einbindungspunkt „/hana/log/backups“.

![Planen von SAP HANA-Sicherungsprotokollen in SAP HANA Studio](./media/sap-hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Sie können Sicherungen wählen, die häufiger als alle 15 Minuten erstellt werden. Einige Benutzer erstellen sogar minütliche Protokollsicherungen, aber ein _höherer Wert_ als 15 Minuten wird nicht empfohlen.

Der letzte Schritt besteht im Durchführen einer dateibasierten Sicherung (nach der Erstinstallation von SAP HANA) zum Hinzufügen eines einzelnen Sicherungseintrags, der im Sicherungskatalog vorhanden sein muss. Andernfalls kann SAP HANA Ihre angegebenen Protokollsicherungen nicht initiieren.

![Erstellen einer dateibasierten Sicherung zum Hinzufügen eines einzelnen Sicherungseintrags](./media/sap-hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Überwachen der Anzahl und Größe von Momentaufnahmen auf dem Datenträgervolume

Auf einem bestimmten Speichervolume können Sie die Anzahl der Momentaufnahmen und deren Speicherbelegung überwachen. Mit dem Befehl `ls` werden keine Momentaufnahmenverzeichnisse oder -dateien angezeigt. Verwenden Sie hierfür den Linux-Betriebssystembefehl `du` mit den folgenden Befehlen:

- `du –sh .snapshot`: Gibt die Summe aller Momentaufnahmen im Verzeichnis für die Momentaufnahmen an.
- `du –sh --max-depth=1`: Listet alle Momentaufnahmen, die im Ordner „.snapshot“ gespeichert sind, und die Größe der einzelnen Momentaufnahmen auf.
- `du –hc`: Gibt die Gesamtgröße des Speicherplatzes an, der von allen Momentaufnahmen genutzt wird.

Verwenden Sie diese Befehle, um sicherzustellen, dass die erstellten und gespeicherten Momentaufnahmen nicht den gesamten Speicher auf den Volumes belegen.

### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Verringern der Anzahl von Momentaufnahmen auf einem Server

Wie bereits beschrieben, können Sie die Anzahl von bestimmten Bezeichnungen von Momentaufnahmen reduzieren, die Sie speichern. Die beiden letzten Parameter des Befehls zum Erstellen einer Momentaufnahme sind eine Bezeichnung und die Anzahl von Momentaufnahmen, die Sie beibehalten möchten.
```
./azure_hana_backup.pl lhanad01 customer 20
```
Im obigen Beispiel lautet die Bezeichnung der Momentaufnahme _customer_, und die Anzahl von Momentaufnahmen mit dieser Bezeichnung, die beibehalten werden soll, beträgt _20_. Als Reaktion auf die Speicherplatzbelegung können Sie die Anzahl von gespeicherten Momentaufnahmen reduzieren. Eine einfache Methode zum Verringern der Anzahl von Momentaufnahmen ist das Ausführen des Skripts und das Festlegen des letzten Parameters auf 5:
```
./azure_hana_backup.pl lhanad01 customer 5
```
Nach der Ausführung des Skripts mit dieser Einstellung beträgt die Anzahl von Momentaufnahmen, einschließlich der neuen Speichermomentaufnahme, _5_.

 >[!NOTE]
 > Mit diesem Skript wird die Anzahl von Momentaufnahmen nur reduziert, wenn die letzte vorherige Momentaufnahme älter als eine Stunde ist. Mit dem Skript werden Momentaufnahmen, die weniger als eine Stunde alt sind, also nicht gelöscht.

Diese Einschränkungen beziehen sich auf die bereitgestellten optionalen Funktionen für die Notfallwiederherstellung.

Falls Sie eine Gruppe von Momentaufnahmen mit diesem Präfix nicht mehr beibehalten möchten, können Sie das Skript mit _0_ als beizubehaltende Anzahl ausführen, um alle Momentaufnahmen mit diesem Präfix zu entfernen. Das Entfernen aller Momentaufnahmen kann sich aber auf die Funktionen für die Notfallwiederherstellung auswirken.

### <a name="recovering-to-the-most-recent-hana-snapshot"></a>Wiederherstellen der letzten HANA-Momentaufnahme

Wenn die Produktionsumgebung ausfällt, kann die Wiederherstellung aus einer Speichermomentaufnahme über das Melden eines Kundenvorfalls beim SAP HANA in Azure-Dienstverwaltungsteam ausgelöst werden. Ein unerwartetes Szenario dieser Art kann zu einer hohen Dringlichkeit führen, wenn Daten in einem Produktionssystem gelöscht wurden und die einzige Möglichkeit zum Wiedererlangen der Daten die Wiederherstellung der Produktionsdatenbank ist.

Eine Zeitpunktwiederherstellung kann dagegen eine niedrigere Dringlichkeit haben und Tage im Voraus geplant werden. Diese Wiederherstellung können Sie mit dem SAP HANA in Azure-Dienstverwaltungsteam planen, anstatt ein Problem mit hoher Priorität zu melden. Es kann beispielsweise sein, dass Sie ein Upgrade der SAP-Software planen, indem Sie ein neues Enhancement Package einspielen und anschließend zu einer Momentaufnahme zurückkehren müssen, die den Zustand vor dem Enhancement Package-Upgrade darstellt.

Vor dem Ausführen der Anforderung sind einige vorbereitende Schritte erforderlich. Das SAP HANA in Azure-Dienstverwaltungsteam kann die Anforderung dann bearbeiten und die wiederhergestellten Volumes bereitstellen. Anschließend ist es Ihre Aufgabe, die HANA-Datenbank basierend auf den Momentaufnahmen wiederherzustellen. Sie bereiten sich wie folgt auf die Anforderung vor:

>[!NOTE]
>Ihre Benutzeroberfläche kann von den folgenden Screenshots abweichen. Dies richtet sich nach der von Ihnen verwendeten SAP HANA-Version.

1. Entscheiden Sie, welche Momentaufnahme wiederhergestellt werden soll. Falls nicht anders angegeben, wird nur das Volume „hana/data“ wiederhergestellt.

2. Fahren Sie die HANA-Instanz herunter.

 ![Herunterfahren der HANA-Instanz](./media/sap-hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Heben Sie die Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank auf. Die Wiederherstellung der Momentaufnahme schlägt fehl, wenn die Bereitstellung der Datenvolumes nicht aufgehoben wurde.

 ![Aufheben der Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank](./media/sap-hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Erstellen Sie eine Azure-Supportanforderung, in der die Wiederherstellung einer bestimmten Momentaufnahme angefordert wird.

 - Während der Wiederherstellung: Unter Umständen werden Sie von SAP HANA unter der Azure-Dienstverwaltung gebeten, an einer Telefonkonferenz teilzunehmen, um sicherzustellen, dass die Daten nicht verloren gehen.

 - Nach der Wiederherstellung: Sie werden von SAP HANA unter der Azure-Dienstverwaltung benachrichtigt, wenn die Speichermomentaufnahme wiederhergestellt wurde.

5. Stellen Sie nach Abschluss des Wiederherstellungsvorgangs alle Datenvolumes wieder bereit.

 ![Erneutes Bereitstellen aller Datenvolumes](./media/sap-hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Wählen Sie Wiederherstellungsoptionen in SAP HANA Studio aus, sofern diese nicht automatisch beim Wiederherstellen der Verbindung mit der HANA-Datenbank über SAP HANA Studio angezeigt werden. Das folgende Beispiel zeigt eine Wiederherstellung der letzten HANA-Momentaufnahme. Eine HANA-Momentaufnahme ist in eine Speichermomentaufnahme eingebettet. Wenn Sie die letzte Speichermomentaufnahme wiederherstellen, sollte dies die neueste HANA-Momentaufnahme sein. (Wenn Sie ältere Speichermomentaufnahmen wiederherstellen, müssen Sie die HANA-Momentaufnahme basierend auf dem Zeitpunkt der Erstellung der Speichermomentaufnahme bestimmen.)

 ![Auswählen von Wiederherstellungsoptionen in SAP HANA Studio](./media/sap-hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Wählen Sie **Recover the database to a specific data backup or storage snapshot** (Datenbank in eine bestimmte Datensicherung oder Speichermomentaufnahme wiederherstellen) aus.

 ![Fenster „Specify Recovery Type“ (Wiederherstellungstyp angeben)](./media/sap-hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Wählen Sie **Specify backup without catalog** (Sicherung ohne Katalog angeben).

 ![Fenster „Specify Backup Location“ (Sicherungsspeicherort angeben)](./media/sap-hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. Wählen Sie in der Liste **Destination Type** (Zieltyp) die Option **Snapshot** (Momentaufnahme).

 ![Fenster „Specify the Backup to Recover“ (Wiederherzustellende Sicherung angeben)](./media/sap-hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Klicken Sie auf **Finish** (Fertig stellen), um den Wiederherstellungsprozess zu starten.

 ![Klicken auf „Finish“ (Fertig stellen), um den Wiederherstellungsprozess zu starten](./media/sap-hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. Die HANA-Datenbank wird mithilfe der HANA-Momentaufnahme wiederhergestellt, die in der Speichermomentaufnahme enthalten ist.

 ![HANA-Datenbank wird über die HANA-Momentaufnahme wiederhergestellt](./media/sap-hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a>Wiederherstellen des letzten Zustands

Mit dem folgenden Prozess wird die HANA-Momentaufnahme wiederhergestellt, die in der Speichermomentaufnahme enthalten ist. Anschließend wird für die Transaktionsprotokollsicherungen der letzte Zustand der Datenbank wiederhergestellt, bevor die Speichermomentaufnahme wiederhergestellt wird.

>[!IMPORTANT]
>Stellen Sie sicher, dass Sie über eine vollständige und zusammenhängende Kette von Transaktionsprotokollsicherungen verfügen, bevor Sie fortfahren. Ohne diese Sicherungen können Sie den aktuellen Zustand der Datenbank nicht wiederherstellen.

1. Führen Sie die Schritte 1 bis 6 des obigen Verfahrens unter „Wiederherstellen der letzten HANA-Momentaufnahme“ aus.

2. Wählen Sie die Option **Recover the database to its most recent state** (Letzten Zustand der Datenbank wiederherstellen).

 ![Auswählen von „Recover the database to its most recent state“ (Letzten Zustand der Datenbank wiederherstellen)](./media/sap-hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Geben Sie den Speicherort der letzten HANA-Protokollsicherungen an. Der Speicherort muss alle HANA-Transaktionsprotokollsicherungen aus der HANA-Momentaufnahme bis zum letzten Zustand enthalten.

 ![Angeben des Speicherorts der letzten HANA-Protokollsicherungen](./media/sap-hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Wählen Sie eine Sicherung als Grundlage für die Wiederherstellung der Datenbank aus. In unserem Beispiel ist dies die HANA-Momentaufnahme, die in der Speichermomentaufnahme enthalten war. (Im folgenden Screenshot ist nur eine Momentaufnahme aufgeführt.)

 ![Auswählen einer Sicherung als Grundlage zum Wiederherstellen der Datenbank](./media/sap-hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Deaktivieren Sie das Kontrollkästchen **Use Delta Backups** (Deltasicherungen verwenden), sofern diese nicht zwischen dem Zeitpunkt der HANA-Momentaufnahme und dem letzten Zustand vorhanden waren.

 ![Deaktivieren des Kontrollkästchens „Use Delta Backups“ (Deltasicherungen verwenden), falls keine Deltas vorhanden sind](./media/sap-hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Klicken Sie auf dem Bildschirm mit der Zusammenfassung auf **Finish** (Fertig stellen), um den Wiederherstellungsvorgang zu starten.

 ![Klicken auf „Finish“ (Fertig stellen) auf dem Bildschirm mit der Zusammenfassung](./media/sap-hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a>Wiederherstellen eines anderen Zeitpunkts
Gehen Sie zum Durchführen einer Zeitpunktwiederherstellung zwischen der HANA-Momentaufnahme (die in der Speichermomentaufnahme enthalten ist) und einer späteren Zeitpunktwiederherstellung der HANA-Momentaufnahme wie folgt vor:

1. Stellen Sie sicher, dass Sie über alle Transaktionsprotokollsicherungen in der HANA-Momentaufnahme bis zum Zeitpunkt verfügen, den Sie wiederherstellen möchten.
2. Beginnen Sie mit dem Verfahren unter „Wiederherstellen des letzten Zustands“.
3. Wählen Sie in Schritt 2 des Verfahrens im Fenster **Specify Recovery Type** (Wiederherstellungstyp angeben) die Option **Recover the database to the following point in time** (Datenbank für den folgenden Zeitpunkt wiederherstellen), geben Sie den Zeitpunkt an, und führen Sie die Schritte 3 bis 6 aus.

## <a name="monitoring-the-execution-of-snapshots"></a>Überwachen der Ausführung von Momentaufnahmen

Sie müssen die Überwachung der Ausführung von Speichermomentaufnahmen überwachen. Das Skript, das eine Speichermomentaufnahme ausführt, schreibt die Ausgabe in eine Datei und speichert sie an demselben Speicherort wie die Perl-Skripts. Für jede Momentaufnahme wird eine separate Datei geschrieben. Die Ausgabe jeder Datei zeigt eindeutig die verschiedenen Phasen, die vom Momentaufnahmenskript ausgeführt werden:

- Suchen der Volumes, für die eine Momentaufnahme erstellt werden muss
- Suchen der Momentaufnahmen, die von diesen Volumes erstellt wurden
- Löschen von eventuell vorhandenen Momentaufnahmen übereinstimmend mit der Anzahl von Momentaufnahmen, die Sie angegeben haben
- Erstellen einer HANA-Momentaufnahme
- Erstellen der Speichermomentaufnahme über alle Volumes hinweg
- Löschen der HANA-Momentaufnahme
- Umbenennen der neuesten Momentaufnahme in **.0**

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
Sie können anhand dieses Beispiels erkennen, wie das Skript die Erstellung der HANA-Momentaufnahme aufzeichnet. Bei horizontaler Skalierung wird dieser Prozess auf dem Masterknoten ausgelöst. Der Masterknoten löst die synchrone Erstellung der Momentaufnahmen auf allen Workerknoten aus. Dann wird die Speichermomentaufnahme erstellt. Nach erfolgreicher Erstellung der Speichermomentaufnahmen wird die HANA-Momentaufnahme gelöscht.

