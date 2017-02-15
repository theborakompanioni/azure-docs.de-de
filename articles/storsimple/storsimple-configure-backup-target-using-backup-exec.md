---
title: Konfigurieren von StorSimple mit Veritas Backup Exec | Microsoft-Dokumentation
description: Beschreibt die Konfiguration des StorSimple-Sicherungsziels mit Veritas Backup Exec.
services: storsimple
documentationcenter: 
author: hkanna
manager: matd
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/05/2016
ms.author: hkanna
translationtype: Human Translation
ms.sourcegitcommit: b7c6a9ffb2c598048bf0547420b1e7afac2c3e74
ms.openlocfilehash: 7a2fa4ccac6b2eda935477034c5f58a0ed90da37

---

# <a name="configure-storsimple-with-veritas-backup-exec8482"></a>Konfigurieren von StorSimple mit Veritas Backup Exec&#8482;

## <a name="overview"></a>Übersicht

Microsoft Azure StorSimple ist eine Hybridcloud-Speicherlösung, mit der sich die Komplexität des exponentiellen Datenwachstums bewältigen lässt. Diese Lösung verwendet ein Azure-Speicherkonto als Erweiterung der lokalen Lösung und speichert die Daten automatisch auf lokale Speicherstufen und Cloudspeicherstufen.

Dieser Artikel beschreibt die StorSimple-Integration in Veritas Backup Exec sowie die bewährten Methoden für die Integration beider Lösungen. Außerdem erhalten Sie Empfehlungen, wie Sie Veritas Backup Exec für eine optimale Integration in StorSimple konfigurieren. Basierend auf bewährten Methoden von Veritas und der Erfahrung von Sicherungsarchitekten und -administratoren wird erläutert, wie Sie Veritas Backup Exec so konfigurieren, dass Ihre Sicherungsanforderungen und SLAs optimal erfüllt werden.

Dieser Artikel veranschaulicht Konfigurationsschritte und wichtige Konzepte, ist jedoch keinesfalls als Schrittanleitung für die Konfiguration oder Installation zu verstehen. In diesem Artikel wird vorausgesetzt, dass die grundlegenden Komponenten und Infrastrukturen funktionieren und die hier beschriebenen Konzepte unterstützen können.

## <a name="why-storsimple-as-a-backup-target"></a>Was spricht für StorSimple als Sicherungsziel?

StorSimple ist aus folgenden Gründen ein hervorragendes Sicherungsziel:

-   Die Lösung stellt standardmäßigen lokalen Speicher für Sicherungsanwendungen bereit, der ohne jede Änderungen verwendet werden kann, und bietet damit ein schnelles Sicherungsziel. Mit StorSimple können auch kürzlich durchgeführte Sicherungen schnell wiederhergestellt werden.

-   Das Cloudtiering ist nahtlos in den kostengünstigen Microsoft Azure-Cloudspeicher integriert.

-   Die Lösung stellt automatisch Offsitespeicher für die Notfallwiederherstellung bereit.


## <a name="target-audience"></a>Zielgruppe

Zur Zielgruppe für diesen Artikel gehören Sicherungsadministratoren, Speicheradministratoren und Speicherarchitekten, die über Kenntnisse zu Speichern, Windows Server 2012 R2, Ethernet, Clouddiensten und Veritas Backup Exec verfügen.

## <a name="supported-versions"></a>Unterstützte Versionen

Eine vollständige Liste der unterstützten Versionen finden Sie hier:

-   [Backup Exec, Version 16 und höher](http://backupexec.com/compatibility).

-   [StorSimple Update 3 und höher](storsimple-overview.md#storsimple-workload-summary).

## <a name="key-concepts"></a>Wichtige Begriffe

Wie bei jeder anderen Speicherlösung auch ist es erfolgsentscheidend, die Speicherleistung, die SLAs, die Änderungsrate und die Anforderungen an das Kapazitätswachstum der Lösung sorgfältig zu bewerten. Der grundlegende Gedanke ist folgender: Aufgrund der Einführung einer Cloudspeicherstufe spielen die Zugriffszeiten und Durchsatzwerte in der Cloud eine wichtige Rolle für die richtige und sinnvolle Funktionsweise von StorSimple.

StorSimple stellt Speicher für Anwendungen bereit, die mit einem klar definierten Satz Arbeitsdaten (heiße Daten) arbeiten. In diesem Modell werden für die Arbeitsdaten lokale Speicherstufen verwendet. Die verbleibenden Daten, mit denen gerade nicht gearbeitet wird (kalte oder archivierte Daten), werden per Tiering in der Cloud gespeichert. Dieses Modell ist in der folgenden Abbildung dargestellt. Die nahezu waagerecht verlaufende grüne Linie repräsentiert die Daten, die auf den lokalen Speicherstufen des StorSimple-Geräts gespeichert sind. Die exponentiell ansteigende rote Linie zeigt die Gesamtmenge an Daten, die auf allen Speicherstufen der StorSimple-Lösung gespeichert sind. Der Abstand zwischen der grünen und der roten Linie stellt die Gesamtmenge der in der Cloud gespeicherten Daten dar.

**StorSimple-Tiering**
![Diagramm zu StorSimple-Tiering](./media/storsimple-configure-backup-target-using-backup-exec/image1.jpg)

Dank dieser Architektur eignet sich StorSimple ideal als Sicherungsziel. StorSimple ermöglicht Folgendes:

-   Ausführen der häufigsten Wiederherstellungen aus dem lokalen Arbeitsdatensatz.

-   Verwenden der Cloud zur externen Notfallwiederherstellung und für ältere Daten, die seltener wiederhergestellt werden müssen.

## <a name="storsimple-benefits"></a>Vorteile von StorSimple

StorSimple bietet eine lokale Lösung, die nahtlos in Microsoft Azure integriert ist, und profitiert vom reibungslosen Zugriff auf lokalen und cloudbasierten Speicher.

StorSimple nutzt ein automatisches Tiering zwischen dem lokalen Gerät, das SSD-Speicher (Solid-State Drive) und SAS-Speicher (Serial-Attached SCSI) umfasst, und Azure Storage. Mit automatischem Tiering verbleiben häufig verwendete Dateien auf den SSD- und SAS-Speicherstufen des lokalen Systems, und seltener verwendete Daten werden nach Azure Storage verschoben.

StorSimple bietet folgende Vorteile:

-   Eindeutige Algorithmen für Deduplizierung und Komprimierung, die dank Cloud ein nie dagewesenes Deduplizierungslevel erreichen

-   Hohe Verfügbarkeit

-   Georeplikation durch Nutzen der Azure-Georeplikation

-   Azure-Integration

-   Datenverschlüsselung in der Cloud

-   Verbesserte Notfallwiederherstellung und Vorschrifteneinhaltung

Obwohl StorSimple zwei wichtige Bereitstellungsszenarien (primäres und sekundäres Sicherungsziel) ermöglicht, denken Sie immer daran, dass es grundsätzlich ein einfaches Blockspeichergerät ist. StorSimple erledigt sämtliche Komprimierungs- und Deduplizierungsaufgaben, sendet Daten an die Cloud und empfängt Daten aus der Cloud – nahtlos sowohl für die Anwendung als auch das Dateisystem.

Weitere Informationen zu StorSimple finden Sie unter [StorSimple 8000-Serie: eine Hybridcloud-Speicherlösung](storsimple-overview.md). Lesen Sie auch die [technischen Spezifikationen des StorSimple 8000-Geräts](storsimple-technical-specifications-and-compliance.md).

> [!IMPORTANT]
> Ein StorSimple-Gerät als Sicherungsziel wird erst ab StorSimple 8000 Update 3 oder höher unterstützt.

## <a name="architecture-overview"></a>Übersicht über die Architektur

Die folgenden Tabellen enthalten wichtige Informationen zu verschiedenen Modellen und entsprechenden Architekturen.

#### <a name="storsimple-capacities-for-local-and-cloud-storage"></a>StorSimple-Kapazitäten für lokalen und cloudbasierten Speicher


| Speicherkapazität       | 8100          | 8600            |
|------------------------|---------------|-----------------|
| Lokale Speicherkapazität | &lt; 10 TiB\*  | &lt; 20 TiB\*  |
| Cloudspeicherkapazität | &gt; 200 TiB\* | &gt; 500 TiB\* |

\* Bei der Speichergröße wird von nicht deduplizierten und nicht komprimierten Daten ausgegangen.

#### <a name="storsimple-capacities-for-primary-and-secondary-backups"></a>StorSimple-Kapazitäten für primäre und sekundäre Sicherungen


| Sicherungsszenario  | Lokale Speicherkapazität                                         | Cloudspeicherkapazität                      |
|------------------|----------------------------------------------------------------|---------------------------------------------|
| Primäre Sicherung   | Aktuelle Sicherungen im lokalen Speicher für eine schnelle Wiederherstellung (RPO) | Sicherungsverlauf (RPO) passt in Cloudkapazität |
| Sekundäre Sicherung | Sekundäre Kopie der Sicherungsdaten kann in Cloudkapazität gespeichert werden  |

## <a name="storsimple-as-a-primary-backup-target"></a>StorSimple als primäres Sicherungsziel

In diesem Szenario werden die StorSimple-Volumes der Sicherungsanwendung als einziges Repository für Sicherungen präsentiert. Die folgende Abbildung veranschaulicht die Lösungsarchitektur, in der alle Sicherungen mehrstufige StorSimple-Volumes sowohl für Sicherungen als auch für Wiederherstellungen verwenden.

![StorSimple als primäres Sicherungsziel – logisches Diagramm](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Logische Schritte bei der Sicherung des Primärziels

1.  Der Sicherungsserver kontaktiert den Sicherungs-Agent auf dem Ziel, und der Sicherungs-Agent überträgt Daten zurück an den Sicherungsserver.

2.  Der Sicherungsserver schreibt Daten auf die mehrstufigen StorSimple-Volumes.

3.  Der Sicherungsserver aktualisiert die Katalogdatenbank und schließt den Sicherungsauftrag ab.

4.  Das Momentaufnahmeskript löst die Verwaltung von StorSimple-Cloudmomentaufnahmen aus (start-delete).

5.  Der Sicherungsserver löscht die abgelaufenen Sicherungen basierend auf einer Aufbewahrungsrichtlinie.

###  <a name="primary-target-restore-logical-steps"></a>Logische Schritte bei der Wiederherstellung des Primärziels

1.  Der Sicherungsserver startet die Wiederherstellung der entsprechenden Daten aus dem Speicherrepository.

2.  Der Sicherungs-Agent empfängt die Daten vom Sicherungsserver.

3.  Der Sicherungsserver schließt den Wiederherstellungsauftrag ab.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple als sekundäres Sicherungsziel

In diesem Szenario werden die StorSimple-Volumes hauptsächlich für die langfristige Aufbewahrung oder Archivierung verwendet.

Die folgende Abbildung veranschaulicht die Architektur, in der die anfänglichen Sicherungen und Wiederherstellungen auf ein Hochleistungsvolume ausgelegt sind. Diese Sicherungen werden kopiert und nach einem angegebenen Zeitplan auf ein mehrstufiges StorSimple-Volume archiviert.

Es ist wichtig, das Hochleistungsvolume mit reichlich Speicherplatz und Leistungsfähigkeit zu bemessen, um die Kapazitäts- und Leistungsanforderungen der Aufbewahrungsrichtlinie zu erfüllen.

![StorSimple als sekundäres Sicherungsziel – logisches Diagramm](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>Logische Schritte bei der Sicherung des Sekundärziels

1.  Der Sicherungsserver kontaktiert den Sicherungs-Agent auf dem Ziel, und der Sicherungs-Agent überträgt Daten zurück an den Sicherungsserver.

2.  Der Sicherungsserver schreibt Daten in den Hochleistungsspeicher.

3.  Der Sicherungsserver aktualisiert die Katalogdatenbank und schließt den Sicherungsauftrag ab.

4.  Der Sicherungsserver kopiert die Sicherungen basierend auf einer Aufbewahrungsrichtlinie nach StorSimple.

5.  Das Momentaufnahmeskript löst die Verwaltung von StorSimple-Cloudmomentaufnahmen aus (start-delete).

6.  Der Sicherungsserver löscht die abgelaufenen Sicherungen basierend auf einer Aufbewahrungsrichtlinie.

### <a name="secondary-target-restore-logical-steps"></a>Logische Schritte bei der Wiederherstellung des Sekundärziels

1.  Der Sicherungsserver startet die Wiederherstellung der entsprechenden Daten aus dem Speicherrepository.

2.  Der Sicherungs-Agent empfängt die Daten vom Sicherungsserver.

3.  Der Sicherungsserver schließt den Wiederherstellungsauftrag ab.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Die Bereitstellung dieser Lösung besteht aus drei Schritten: Vorbereiten der Netzwerkinfrastruktur, Bereitstellen des StorSimple-Geräts als Sicherungsziel und schließlich Bereitstellen von Veritas Backup Exec. Jeder dieser Schritte wird in den folgenden Abschnitten ausführlich erläutert.

### <a name="configure-the-network"></a>Konfigurieren des Netzwerks

StorSimple als in die Azure-Cloud integrierte Lösung erfordert eine aktive und funktionierende Verbindung mit der Azure-Cloud. Dieser Verbindung wird für Vorgänge wie z.B. Cloudmomentaufnahmen, Verwaltung, Metadatenübertragung sowie zum Tiering älterer, seltener genutzter Daten in den Azure-Cloudspeicher verwendet.

Damit die Lösung optimal funktioniert, empfehlen sich die folgenden bewährten Methoden für die Netzwerkeinrichtung:

-   Die für das StorSimple-Tiering erforderliche Verbindung mit Azure muss Ihre Bandbreitenanforderungen erfüllen. Wenden Sie daher die für Ihre RPO/RTO-SLAs geeignete QoS (Quality of Service) auf Ihre Infrastruktur an.

-   Die maximalen Zugriffslatenzen für Azure Blob Storage sollten im 80-Millisekunden-Bereich liegen.

### <a name="deploy-storsimple"></a>Bereitstellen von StorSimple

Eine Schrittanleitung für die Bereitstellung von StorSimple finden Sie unter [Bereitstellen lokaler StorSimple-Geräte](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-veritas-backup-exec"></a>Bereitstellen von Veritas Backup Exec

Bewährte Methoden für die Installation von Veritas Backup Exec finden Sie unter [Best practices for Backup Exec installation](https://www.veritas.com/support/en_US/article.000068207) (Bewährte Methoden für die Installation von Backup Exec).

## <a name="configure-the-solution"></a>Konfigurieren der Lösung

In diesem Abschnitt zeigen wir Ihnen einige Konfigurationsbeispiele. Die folgenden Beispiele bzw. Empfehlungen veranschaulichen die einfachste und grundlegendste Implementierung. Diese Implementierung gilt möglicherweise nicht exakt für Ihre spezifischen Sicherungsanforderungen.

### <a name="configure-storsimple"></a>Konfigurieren von StorSimple

| StorSimple-Bereitstellungsaufgaben                                                                                                                 | Weitere Kommentare                                                                                                                                                                                                                                                                                      |
|---------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Stellen Sie Ihr lokales StorSimple-Gerät bereit.                                                                                                | Unterstützte Version: Update 3 und höher.                                                                                                                                                                                                                                                                 |
| Aktivieren Sie den Sicherungszielmodus.                                                                                                                   | Verwenden Sie folgende Befehle zum Aktivieren und Deaktivieren und zum Abrufen des Status. Weitere Informationen finden Sie unter [Herstellen einer Remoteverbindung mit dem StorSimple-Gerät](storsimple-remote-connect.md).</br> Sicherungsmodus aktivieren:`Set-HCSBackupApplianceMode -enable`</br>  Sicherungsmodus deaktivieren:`Set-HCSBackupApplianceMode -disable`</br> Aktueller Status der Sicherungsmoduseinstellungen:`Get-HCSBackupApplianceMode` |
| Erstellen Sie einen gemeinsamen Volumecontainer für das Volume, auf dem die Sicherungsdaten gespeichert sind.   Alle Daten in einem Volumecontainer sind dedupliziert. | StorSimple-Volumecontainer definieren Deduplizierungsdomänen.                                                                                                                                                                                                                                             |
| Erstellen Sie StorSimple-Volumes.                                                                                                                 | Erstellen Sie Volumes, deren Größen so nah wie möglich an der prognostizierten Nutzung liegen, da sich die Größe eines Volumes auf die Dauer von Cloudmomentaufnahmen auswirkt. Weitere Informationen zum Einrichten eines Volumes in der richtigen Größe finden Sie unter [Aufbewahrungsrichtlinien](#retention-policies).</br> </br> Verwenden Sie mehrstufige StorSimple-Volumes, und aktivieren Sie die Option **Verwenden Sie dieses Volume für Archivdaten, auf die Sie seltener zugreifen**. </br> Die ausschließliche Verwendung von lokalen Volumes wird nicht unterstützt.|
| Erstellen Sie eine eindeutige StorSimple-Sicherungsrichtlinie für alle Sicherungszielvolumes.                                                               | Eine StorSimple-Sicherungsrichtlinie definiert die Volumekonsistenzgruppe.                                                                                                                                                                                                                                       |
| Deaktivieren Sie den Zeitplan, wenn die Momentaufnahmen ablaufen.                                                                                                    | Momentaufnahmen werden als Nachverarbeitungsvorgang ausgelöst.                                                                                                                                                                                                                                                         |
|                                                                                                     |                                             |




### <a name="configure-host-backup-server-storage"></a>Konfigurieren des Speichers auf dem Sicherungshostserver

Stellen Sie sicher, dass der Speicher auf dem Sicherungshostserver gemäß den folgenden Richtlinien konfiguriert ist.  

- Übergreifende Volumes (von der Windows-Datenträgerverwaltung erstellt) werden nicht unterstützt.
- Formatieren Sie die Volumes mit NTFS und einer Zuordnungsgröße von 64 KB.
- Ordnen Sie die StorSimple-Volumes direkt dem Backup Exec-Server zu.
    - Verwenden Sie im Fall von physischen Servern iSCSI.
    - Verwenden Sie Passthrough-Datenträger für virtuelle Server.


## <a name="best-practices-for-storsimple-and-veritas-backup-exec"></a>Bewährte Methoden für StorSimple und Veritas Backup Exec

Konfigurieren Sie Ihre Lösung gemäß den folgenden Richtlinien.


### <a name="operating-system"></a>Betriebssystem

-   Deaktivieren Sie die Windows Server-Verschlüsselung und -Deduplizierung für das NTFS-Dateisystem.

-   Deaktivieren Sie die Windows Server-Defragmentierung auf den StorSimple-Volumes.

-   Deaktivieren Sie die Windows Server-Indizierung auf den StorSimple-Volumes.

-   Führen Sie einen Virenscan auf dem Quellhost durch (nicht für die StorSimple-Volumes).

-   Deaktivieren Sie die standardmäßige [Windows Server-Wartung](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) im Task-Manager.

    - Deaktivieren Sie die Wartungskonfiguration in der Windows-Aufgabenplanung.

        oder

    - Laden Sie [PSEXEC – Microsoft Sysinternals](https://technet.microsoft.com/sysinternals/bb897553.aspx) herunter.

      - Nachdem Sie PSEXEC heruntergeladen haben, führen Sie Windows-PowerShell als Administrator aus, und geben Sie Folgendes ein:

            `psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable`

### <a name="storsimple"></a>StorSimple

-   Stellen Sie sicher, dass das StorSimple-Gerät auf [Update 3 oder höher](storsimple-install-update-3.md) aktualisiert ist.

-   Isolieren Sie iSCSI- und Clouddatenverkehr. Verwenden Sie dedizierte iSCSI-Verbindungen für Datenverkehr zwischen StorSimple und dem Sicherungsserver.

-   Stellen Sie sicher, dass es sich beim StorSimple-Gerät um ein dediziertes Sicherungsziel handelt. Gemischte Workloads werden nicht unterstützt, da diese sich auf Ihre RTO/RPO auswirken.

### <a name="veritas-backup-exec"></a>Veritas Backup Exec

-   Veritas Backup Exec muss auf einem lokalen Laufwerk des Servers installiert werden, nicht auf einem StorSimple-Volume.

-   Legen Sie **concurrent write operations** (gleichzeitige Schreibvorgänge) in Veritas Backup Exec Storage auf den maximal zulässigen Wert fest.

    -   Die **block and buffer size** (Block- und Puffergröße) in Veritas Backup Exec Storage sollte auf 512 KB festgelegt werden.

    -   In Veritas Backup Exec Storage sollten **buffered read and write** (gepufferte Lese- und Schreibvorgänge) aktiviert werden.

-   Vollständige und inkrementelle Sicherungen von Veritas Backup Exec werden unterstützt, synthetische und differenzielle Sicherungen dagegen werden nicht empfohlen.

-   Sicherungsdatendateien sollten nur die Daten für einen bestimmten Auftrag enthalten. Es können beispielsweise keine Medien aus unterschiedlichen Aufträgen angefügt werden.

-   Deaktivieren Sie die Auftragsüberprüfung. Falls erforderlich, sollte die Überprüfung nach dem letzten Sicherungsauftrag geplant werden. Es ist wichtig zu wissen, dass sich dieser Auftrag auf Ihr Sicherungsfenster auswirkt.

-   Deaktivieren Sie unter **Storage > Your disk > Details > Properties** (Speicher > Ihr Datenträger > Details > Eigenschaften) die Option **Pre-allocate disk space** (Speicherplatz auf Datenträger vorab zuweisen).

Neueste Informationen zu Veritas Backup Exec-Einstellungen sowie bewährte Methoden zur Implementierung der Anforderungen finden Sie in der Veritas Backup Exec-Dokumentation unter [www.veritas.com](https://www.veritas.com).

## <a name="retention-policies"></a>Aufbewahrungsrichtlinien

Eine der am häufigsten verwendeten Aufbewahrungsrichtlinien ist die GFS-Richtlinie (Grandfather, Father and Son). Bei dieser Richtlinie wird täglich eine inkrementelle Sicherung ausgeführt. Vollständige Sicherungen erfolgen wöchentlich und monatlich. Diese Richtlinie führt zu sechs mehrstufigen StorSimple-Volumes.

-   Ein Volume enthält die wöchentlichen, monatlichen und jährlichen vollständigen Sicherungen.

-   Auf den anderen fünf Volumes werden die täglichen inkrementellen Sicherungen gespeichert.

Das folgende Beispiel zeigt eine GFS-Rotation. Bei diesem Beispiel wird Folgendes vorausgesetzt:

-   Es werden nicht deduplizierte und nicht komprimierte Daten verwendet.

-   Die vollständigen Sicherungen umfassen jeweils 1 TiB.

-   Die täglichen inkrementellen Sicherungen umfassen jeweils 500 GiB.

-   4 wöchentliche Sicherungen werden einen Monat lang aufbewahrt.

-   12 monatliche Sicherungen werden ein Jahr lang aufbewahrt.

-   1 jährliche Sicherung wird 10 Jahre lang aufbewahrt.

Erstellen Sie basierend auf diesen Annahmen ein mehrstufiges StorSimple-Volume mit 26 TiB für die monatlichen und jährlichen vollständigen Sicherungen. Erstellen Sie ein mehrstufiges StorSimple-Volume mit 5 TiB für jede der täglichen inkrementellen Sicherungen.

| Sicherungstyp | Größe in TiB | GFS-Multiplikator\*                                       | Gesamtkapazität in TiB          |
|-----------------------|----------|--------------------------------------------------------|-----------------------------|
| Wöchentlich vollständig           | 1        | 4                                                      | 4                           |
| Täglich inkrementell     | 0,5      | 20 (Zyklen entsprechen der Anzahl von Wochen pro Monat) | 12 (2 für zusätzliches Kontingent) |
| Monatlich vollständig          | 1        | 12                                                     | 12                          |
| Jährlich vollständig           | 1        | 10                                                     | 10                          |
| GFS-Anforderung       |          |                                                        | 38                          |
| Zusätzliches Kontingent      | 4        |                                                        | 42 (GFS-Anforderung gesamt)   |

\*Der GFS-Multiplikator ist die Anzahl von Kopien, die Sie schützen und beibehalten müssen, um Ihre Sicherungsrichtlinien zu erfüllen.

## <a name="configure-veritas-backup-exec-storage"></a>Konfigurieren von Veritas Backup Exec-Speicher

1.  Wählen Sie in der Veritas Backup Exec-Verwaltungskonsole **Storage  &gt; Configure Storage &gt; Disk-Based Storage** (Speicher > Speicher konfigurieren > Datenträgerbasierter Speicher). Klicken Sie auf **Next &gt;** (Weiter).

    ![Veritas Backup Exec-Verwaltungskonsole, Bildschirm zum Konfigurieren des Speichers](./media/storsimple-configure-backup-target-using-backup-exec/image4.png)

1.  Klicken Sie auf **Disk Storage** (Datenträgerspeicher), und fahren Sie mit der nächsten Seite fort.

    ![Veritas Backup Exec-Verwaltungskonsole, Bildschirm zum Auswählen des Speichers](./media/storsimple-configure-backup-target-using-backup-exec/image5.png)

1.  Geben Sie einen aussagekräftigen Namen wie z.B. „Samstag vollständig“ und eine Beschreibung ein. Fahren Sie mit der nächsten Seite fort.

    ![Veritas Backup Exec-Verwaltungskonsole, Bildschirm zur Eingabe von Name und Beschreibung](./media/storsimple-configure-backup-target-using-backup-exec/image7.png)

1.  Wählen Sie den entsprechenden Datenträger aus, und klicken Sie auf **Next &gt;** (Weiter). Hier wurde z.B. „D:“ ausgewählt.

    ![Veritas Backup Exec-Verwaltungskonsole, Bildschirm zum Auswählen des Speicherdatenträgers](./media/storsimple-configure-backup-target-using-backup-exec/image9.png)

1.  Erhöhen Sie die Anzahl von Schreibvorgängen auf 16, und klicken Sie auch **Next &gt;** (Weiter).

    ![Veritas Backup Exec-Verwaltungskonsole, Bildschirm mit Einstellungen für gleichzeitige Schreibvorgänge ](./media/storsimple-configure-backup-target-using-backup-exec/image10.png)

1.  Überprüfen Sie die Einstellungen, und klicken Sie auf **Finish** (Fertig stellen).

    ![Veritas Backup Exec-Verwaltungskonsole, Bildschirm mit Übersicht über die Speicherkonfiguration](./media/storsimple-configure-backup-target-using-backup-exec/image11.png)

1.  Ändern Sie nach jeder Zuordnung die Einstellungen des Speichergeräts so, dass sie den empfohlenen Einstellungen in der Liste der bewährten Methoden entsprechen.

    ![Veritas Backup Exec-Verwaltungskonsole, Bildschirm mit Einstellungen für das Speichergerät](./media/storsimple-configure-backup-target-using-backup-exec/image12.png)

1.  Wiederholen Sie alle vorherigen Schritte, bis Sie alle StorSimple-Volumes zu Veritas Backup Exec zugewiesen haben.

## <a name="storsimple-as-a-primary-backup-target"></a>StorSimple als primäres Sicherungsziel

> [!NOTE]
> Beachten Sie Folgendes: Wenn Sie Daten aus einer Sicherung wiederherstellen müssen, die per Tiering in der Cloud gespeichert wurde, erfolgt die Wiederherstellung mit der Geschwindigkeit der Cloud.

In der folgenden Abbildung wird die Zuordnung eines typischen Volumes zu einem Sicherungsauftrag veranschaulicht. In diesem Fall werden alle wöchentlichen Sicherungen dem Datenträger mit der vollständigen Sicherung von Samstag und die inkrementellen Sicherungen dem Datenträger mit den inkrementellen Sicherungen von Montag bis Freitag zugeordnet. Alle Sicherungen und Wiederherstellungen erfolgen von einem mehrstufigen StorSimple-Volume.

![Logisches Diagramm der Konfiguration des primären Sicherungsziels ](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetdiagram.png)

#### <a name="storsimple-as-a-primary-backup-target-grandfather-father-and-son-gfs-schedule-example"></a>StorSimple als primäres Sicherungsziel, Beispiel für GFS-Zeitplan

| GFS-Rotationsplan für vier Wochen, monatlich und jährlich |               |             |
|--------------------------------------------------------------------------|---------------|-------------|
| Häufigkeit/Sicherungstyp   | Vollständig          | Inkrementell (Tag 1–5)  |
| Wöchentlich (Woche 1–4)    | Samstag | Montag – Freitag | 
| Monatlich     | Samstag  |             |
| Jährlich      | Samstag  |             |


### <a name="assigning-storsimple-volumes-to-a-veritas-backup-exec-backup-job"></a>Zuweisen von StorSimple-Volumes zu einem Veritas Backup Exec-Sicherungsauftrag.

Bei der folgenden Sequenz wird vorausgesetzt, dass Backup Exec und der Zielhost gemäß den Backup Exec-Agent-Richtlinien konfiguriert sind.

1.  Wechseln Sie in der Veritas Backup Exec-Verwaltungskonsole zu **Host &gt; Backup &gt; Backup to Disk &gt;** (Host > Sicherung > Sicherung auf Datenträger).

    ![Veritas Backup Exec-Verwaltungskonsole, Auswählen von „Host“, „Backup“, „Backup to Disk“ (Host, Sicherung, Sicherung auf Datenträger)](./media/storsimple-configure-backup-target-using-backup-exec/image14.png)

1.  Wählen Sie im Fenster „Backup Definition Properties“ (Eigenschaften der Sicherungsdefinition) unter „Backup“ die Option **Edit** (Bearbeiten).

    ![Fenster „Backup Definition Properties“ (Eigenschaften der Sicherungsdefinition), Auswahl von „Edit“ (Bearbeiten)](./media/storsimple-configure-backup-target-using-backup-exec/image15.png)

1.  Konfigurieren Sie Ihre vollständigen und inkrementellen Sicherungen so, dass sie Ihre RPO/RTO-Anforderungen erfüllen und den bewährten Methoden von Veritas entsprechen.

2.  Wählen Sie im Fenster „Backup Options“ (Sicherungsoptionen) die Option **Storage** (Speicher).

    ![Registerkarte „Storage“ (Speicher) im Fenster „Backup Options“ (Sicherungsoptionen)](./media/storsimple-configure-backup-target-using-backup-exec/image16.png)

1.  Weisen Sie Ihrem Sicherungszeitplan die entsprechenden StorSimple-Volumes zu.

    > [!NOTE]
    > **Compression** (Komprimierung) und **Encryption type** (Verschlüsselungstyp) sind auf **None** (Keine) festgelegt.

2.  Wählen Sie unter **Verify** (Überprüfen) die Option **Do not verify data for this job** (Daten für diesen Auftrag nicht überprüfen) aus, da dies das StorSimple-Tiering beeinträchtigen kann.

    > [!NOTE]
    > Defragmentierung, Indizierung und Hintergrundüberprüfung wirken sich negativ auf das StorSimple-Tiering aus.

    ![Überprüfungseinstellungen für Sicherungsoptionen](./media/storsimple-configure-backup-target-using-backup-exec/image17.png)

1.  Nachdem Sie die verbleibenden Sicherungsoptionen so konfiguriert haben, dass sie Ihre Anforderungen erfüllen, wählen Sie **OK**.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple als sekundäres Sicherungsziel

> [!NOTE]
> Beachten Sie Folgendes: Wenn Sie Daten aus einer Sicherung wiederherstellen müssen, die per Tiering in der Cloud gespeichert wurde, erfolgt die Wiederherstellung mit der Geschwindigkeit der Cloud.

In diesem Modell muss ein Speichermedium (nicht StorSimple) vorhanden sein, das als temporärer Cache dient. Sie können z.B. ein RAID-Volume verwenden, um Speicherplatz, E/A und Bandbreite bereitzustellen. Wir empfehlen RAID 5, 50 und 10.

Die folgende Abbildung veranschaulicht typische Volumes für die kurzfristige lokale Aufbewahrung (auf dem Server) und die langfristige Aufbewahrung im Archiv. In diesem Fall werden alle Sicherungen auf dem lokalen RAID-Volume (auf dem Server) ausgeführt. Diese Sicherungen werden regelmäßig dupliziert und auf einem Archivvolume archiviert. Es ist wichtig, die Größe des lokalen RAID-Volumes (auf dem Server) so einzurichten, dass die Kapazitäts- und Leistungsanforderungen für die kurzfristige Aufbewahrung erfüllt werden.

#### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>StorSimple als sekundäres Sicherungsziel – GFS-Beispiel

![StorSimple als sekundäres Sicherungsziel – logisches Diagramm ](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetdiagram.png)

Die folgende Tabelle zeigt, wie die Sicherungen für die Ausführung auf den lokalen und StorSimple-Datenträgern konfiguriert werden sollten. Die Tabelle enthält auch die Anforderungen an die Einzel- und Gesamtkapazität.

#### <a name="backup-configuration-and-capacity-requirements"></a>Sicherungskonfiguration und Kapazitätsanforderungen

| Sicherungstyp und Aufbewahrung                    |Konfigurierter Speicher| Größe (TiB) | GFS-Multiplikator | Gesamtkapazität (TiB)        |
|----------------------------------------------|-----|----------|----------------|------------------------|
| Woche 1 (vollständig und inkrementell) |Lokaler Datenträger (kurzfristig)| 1        | 1              | 1           |
| StorSimple, Woche 2–4           |StorSimple-Datenträger (langfristig) | 1        | 4              | 4                   |
| Monatlich vollständig                                 |StorSimple-Datenträger (langfristig) | 1        | 12             | 12                   |
| Jährlich vollständig                               |StorSimple-Datenträger (langfristig) | 1        | 1              | 1                   |
|Größenanforderungen für GFS-Volumes | |          |                | 18*|

\* Die Gesamtkapazität umfasst 17 TiB für StorSimple-Datenträger und 1 TiB für lokale RAID-Volumes.


#### <a name="gfs-example-schedule"></a>GFS-Beispielzeitplan

|GFS-Rotation mit wöchentlichem, monatlichem und jährlichem Zeitplan|                    |                   |                   |                   |                   |                   |
|--------------------------------------------------------------------------|--------------------|-------------------|-------------------|-------------------|-------------------|-------------------|
| Frequency                                                                     | Vollständig               | Inkrementell, Tag 1        | Inkrementell, Tag 2        | Inkrementell, Tag 3        | Inkrementell, Tag 4        | Inkrementell, Tag 5        |
| Woche 1                                                                   | Lokales RAID-Volume  | Lokales RAID-Volume | Lokales RAID-Volume | Lokales RAID-Volume | Lokales RAID-Volume | Lokales RAID-Volume |
| Woche 2                                                                   | StorSimple, Woche 2–4 |                   |                   |                   |                   |                   |
| Woche 3                                                                   | StorSimple, Woche 2–4 |                   |                   |                   |                   |                   |
| Woche 4                                                                   | StorSimple, Woche 2–4 |                   |                   |                   |                   |                   |
| Monatlich                                                                  | StorSimple monatlich |                   |                   |                   |                   |                   |
| Jährlich                                                                   | StorSimple jährlich  |                   |                   |                   |                   |                   |


### <a name="assign-storsimple-volumes-to-backup-exec-archivededuplication-job"></a>Zuweisen von StorSimple-Volumes zu Backup Exec-Archivierungs-/Deduplizierungsaufträgen

1.  Sobald der anfängliche Sicherungsauftrag zur Verwendung des RAID-Volumes als primäres Sicherungsziel konfiguriert wurde, wechseln Sie zur Veritas Backup Exec-Verwaltungskonsole. Wählen Sie den Auftrag aus, den Sie auf ein StorSimple-Volume archivieren möchten, klicken Sie mit der rechten Maustaste, und wählen Sie **Backup Definition Properties** (Eigenschaften von Sicherungsdefinitionen) aus. Klicken Sie auf **Bearbeiten**.

    ![Backup Exec-Konsole, Registerkarte mit Eigenschaften von Sicherungsdefinitionen](./media/storsimple-configure-backup-target-using-backup-exec/image19.png)

1.  Wählen Sie **Add Stage** (Auftrag hinzufügen), und wählen Sie dann aus der Dropdownliste die Option **Duplicate to Disk** (Auf Datenträger duplizieren). Klicken Sie auf **Bearbeiten**.

    ![Backup Exec-Konsole, Sicherungsdefinitionen, Speicherstufe hinzufügen](./media/storsimple-configure-backup-target-using-backup-exec/image20.png)

2.  Wählen Sie aus den **Duplicate Options** (Duplikatoptionen) die entsprechende **Source** (Quelle) und den **Schedule** (Zeitplan) aus.

    ![Backup Exec-Konsole, Eigenschaften von Sicherungsdefinitionen, Duplikatoptionen](./media/storsimple-configure-backup-target-using-backup-exec/image21.png)

1.  Wählen Sie aus der Dropdownliste **Storage** (Speicher) das StorSimple-Volume, in dem der Archivierungsauftrag die Daten speichern soll. In diesem Fall wurde die Quelle auf **Full** (Vollständig) und das Ziel auf das vollständige StorSimple-Archivvolume festgelegt.

    ![Backup Exec-Konsole, Eigenschaften von Sicherungsdefinitionen, Duplikatoptionen](./media/storsimple-configure-backup-target-using-backup-exec/image22.png)

1.  Wechseln Sie zu **Verify** (Überprüfen), und wählen Sie **Do not verify data for this job** (Daten für diesen Auftrag nicht überprüfen) aus.

    ![Backup Exec-Konsole, Eigenschaften von Sicherungsdefinitionen, Duplikatoptionen](./media/storsimple-configure-backup-target-using-backup-exec/image23.png)

1.  Klicken Sie auf **OK**.

    ![Backup Exec-Konsole, Eigenschaften von Sicherungsdefinitionen, Duplikatoptionen](./media/storsimple-configure-backup-target-using-backup-exec/image24.png)

1.  Fügen Sie in der Spalte **Backup** (Sicherung) einen neuen Sicherungsauftrag hinzu. Wählen Sie als Quelle eine inkrementelle Sicherung und als Ziel das StorSimple-Volume aus, in dem der inkrementelle Sicherungsauftrag archiviert ist. Wiederholen Sie die vorherigen Schritte.

## <a name="storsimple-cloud-snapshots"></a>StorSimple-Cloudmomentaufnahmen

StorSimple-Cloudmomentaufnahmen schützen die Daten, die sich auf dem StorSimple-Gerät befinden. Dies entspricht dem Senden von Bändern an einen externen Standort bzw. bei Verwendung von georedundantem Speicher in Azure (GRS) dem Senden von Bändern an mehrere Standorte. Wenn im Notfall eine Gerätewiederherstellung erforderlich ist, können Sie ein anderes StorSimple-Gerät online schalten und ein Failover ausführen. Nach dem Failover können Sie aus der jüngsten Cloudmomentaufnahme auf die Daten zugreifen (mit der Geschwindigkeit der Cloud).

Der folgende Abschnitt zeigt, wie Sie ein kurzes Skript schreiben, um StorSimple-Cloudmomentaufnahmen während der Nachverarbeitung von Sicherungen auszulösen und zu löschen.

> [!NOTE]
> Manuell oder programmgesteuert erstellte Momentaufnahmen folgen nicht der Ablaufrichtlinie für StorSimple-Momentaufnahmen. Diese müssen manuell oder programmgesteuert gelöscht werden.

### <a name="start-and-delete-cloud-snapshots-with-a-script"></a>Starten und Löschen von Cloudmomentaufnahmen mit einem Skript

> [!NOTE]
> Bewerten Sie sorgfältig die Auswirkungen auf Compliance und Datenaufbewahrung, bevor Sie eine StorSimple-Momentaufnahme löschen. Weitere Informationen zum Ausführen eines Skripts nach der Sicherung finden Sie in der Veritas Backup Exec-Dokumentation.

#### <a name="backup-lifecycle"></a>Sicherungslebenszyklus

![Diagramm zum Sicherungslebenszyklus](./media/storsimple-configure-backup-target-using-backup-exec/backuplifecycle.png)

#### <a name="requirements"></a>Anforderungen:

-   Der Server, auf dem das Skript ausgeführt wird, muss auf die Azure-Cloud zugreifen können.

-   Das Benutzerkonto muss über die notwendigen Berechtigungen verfügen.

-   Eine StorSimple-Sicherungsrichtlinie mit zugehörigen StorSimple-Volumes muss konfiguriert, aber nicht aktiviert sein.

-   Ressourcenname, Registrierungsschlüssel, Gerätename und Sicherungsrichtlinien-ID von StorSimple.

#### <a name="steps"></a>Schritte:

1.  [Installieren Sie Azure PowerShell](/powershell-install-configure/).

2.  [Laden Sie Veröffentlichungseinstellungen und Abonnementinformationen herunter, und importieren Sie sie](https://msdn.microsoft.com/library/dn385850.aspx).

3.  Rufen Sie im klassischen Azure-Portal den Ressourcennamen und [Registrierungsschlüssel für Ihren StorSimple Manager-Dienst](storsimple-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key) ab.

4.  Führen Sie auf dem Server, auf dem das Skript ausgeführt wird, Windows PowerShell als Administrator aus. Geben Sie Folgendes ein:

    -   `Get-AzureStorSimpleDeviceBackupPolicy –DeviceName <device name>`

    Notieren Sie sich die ID der Sicherungsrichtlinie.

5.  Erstellen Sie im Editor ein neues Windows PowerShell-Skript, und speichern Sie es an dem Speicherort, an dem auch die Azure-Veröffentlichungseinstellungen gespeichert sind. Beispiel: `C:\\CloudSnapshot\\StorSimpleCloudSnapshot.ps1`.

    Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn ein:

    ```powershell
    Import-AzurePublishSettingsFile "c:\\CloudSnapshot Snapshot\\myAzureSettings.publishsettings"
    Disable-AzureDataCollection
    $ApplianceName = <myStorSimpleApplianceName>
    $RetentionInDays = 20
    $RetentionInDays = -$RetentionInDays
    $Today = Get-Date
    $ExpirationDate = $Today.AddDays($RetentionInDays)
    Select-AzureStorSimpleResource -ResourceName "myResource" –RegistrationKey
    Start-AzureStorSimpleDeviceBackupJob –DeviceName $ApplianceName -BackupType CloudSnapshot -BackupPolicyId <BackupId> -Verbose
    $CompletedSnapshots =@()
    $CompletedSnapshots = Get-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName
    Write-Host "The Expiration date is " $ExpirationDate
    Write-Host

    ForEach ($SnapShot in $CompletedSnapshots)
    {
    $SnapshotStartTimeStamp = $Snapshot.CreatedOn
    if ($SnapshotStartTimeStamp -lt $ExpirationDate)

        {
        $SnapShotInstanceID = $SnapShot.InstanceId
        Write-Host "This snapshotdate was created on " $SnapshotStartTimeStamp.Date.ToShortDateString()
        Write-Host "Instance ID " $SnapShotInstanceID
        Write-Host "This snpashotdate is older and needs to be deleted"
        Write-host "\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#"
        Remove-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName -BackupId $SnapShotInstanceID -Force -Verbose
        }
    }   
    ```

1.  Fügen Sie das Skript zu Ihrem Sicherungsauftrag in Veritas Backup Exec hinzu, indem Sie die Befehle für die Vor- und Nachverarbeitung in den Veritas Backup Exec-Auftragsoptionen bearbeiten.

    ![Backup Exec-Konsole, Sicherungsoptionen, Registerkarte mit Befehlen für die Vor- und Nachverarbeitung](./media/storsimple-configure-backup-target-using-backup-exec/image25.png)

> [!NOTE]
> Es empfiehlt sich, die Sicherungsrichtlinie für StorSimple-Cloudmomentaufnahmen als Nachverarbeitungsskript am Ende des täglichen Sicherungsauftrags auszuführen. Weitere Informationen zum Sichern und Wiederherstellen Ihrer Sicherungsanwendungsumgebung, um Ihre RPO/RTO zu erfüllen, erhalten Sie von Ihrem Sicherungsarchitekten.

## <a name="storsimple-as-a-restore-source"></a>StorSimple als Wiederherstellungsquelle

Wiederherstellungen aus StorSimple funktionieren ähnlich wie Wiederherstellungen aus anderen Blockspeichergeräten. Bei Daten, die per Tiering in der Cloud gespeichert wurden, erfolgt die Wiederherstellung mit der Geschwindigkeit der Cloud. Bei lokalen Daten erfolgt die Wiederherstellung mit der Geschwindigkeit des lokalen Datenträgers des Geräts. Informationen zum Ausführen einer Wiederherstellung finden Sie in der Veritas Backup Exec-Dokumentation. Halten Sie sich auch an die bewährten Methoden für die Wiederherstellung mit Veritas Backup Exec.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple-Failover und -Notfallwiederherstellung

> [!NOTE]
> In Sicherungszielszenarien wird die StorSimple Cloud Appliance nicht als Wiederherstellungsziel unterstützt.

Notfälle können durch verschiedene Faktoren eintreten. Die folgende Tabelle führt häufige Notfallwiederherstellungsszenarien auf.

| Szenario                                                                    | Auswirkung                                             | Wiederherstellung                                                                                                                                                                               | Hinweise                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|-----------------------------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ausfall eines StorSimple-Geräts.                                                | Sicherungs- und Wiederherstellungsvorgänge werden unterbrochen. | Ersetzen Sie das ausgefallene Gerät, und führen Sie die [Schritte für StorSimple-Failover und -Notfallwiederherstellung](storsimple-device-failover-disaster-recovery.md) durch. | Wenn die Wiederherstellung der Daten unmittelbar nach der Wiederherstellung des Geräts erfolgt, werden die vollständigen Arbeitsdatensätze aus der Cloud auf das neue Gerät abgerufen. Daher erfolgen alle Vorgänge mit der Geschwindigkeit der Cloud. Darüber hinaus kann das erneute Scannen von Index und Katalog dazu führen, dass alle Sicherungssätze gescannt und aus der Cloudspeicherstufe in die lokale Speicherstufe des Geräts übertragen werden. Damit wird diese Aufgabe noch zeitaufwendiger.                                                               |
| Ausfall des Veritas Backup Exec-Servers.                                       | Sicherungs- und Wiederherstellungsvorgänge werden unterbrochen. | Erstellen Sie den Sicherungsserver neu, und führen Sie eine Datenbankwiederherstellung durch, wie unter [How to do a manual Backup and Restore of Backup Exec (BEDB) database](http://www.veritas.com/docs/000041083) (Manuelles Sichern und Wiederherstellen der Backup Exec-Datenbank [BEDB]) beschrieben.            | Der Veritas Backup Exec-Server muss am Standort der Notfallwiederherstellung neu erstellt oder wiederhergestellt werden. Die Datenbank muss auf den jüngsten Zeitpunkt wiederhergestellt werden. Wenn die wiederhergestellte Veritas Backup Exec-Datenbank nicht mit Ihren jüngsten Sicherungsaufträgen synchron ist, ist eine Indizierung und Katalogisierung erforderlich. Das erneute Scannen von Index und Katalog kann dazu führen, dass alle Sicherungssätze gescannt und aus der Cloudspeicherstufe in die lokale Speicherstufe des Geräts übertragen werden. Damit wird diese Aufgabe noch zeitaufwendiger. |
| Standortausfall, der zum Verlust des Sicherungsservers und von StorSimple führt. | Sicherungs- und Wiederherstellungsvorgänge werden unterbrochen. | Stellen Sie zuerst StorSimple und dann Veritas Backup Exec wieder her.                                                                                                                                   | Stellen Sie zuerst StorSimple und dann Veritas Backup Exec wieder her.                                                                Wenn nach der Wiederherstellung des Geräts eine Datenwiederherstellung erforderlich ist, werden die vollständigen Arbeitsdatensätze aus der Cloud auf das neue Gerät abgerufen. Daher erfolgen alle Vorgänge mit der Geschwindigkeit der Cloud.|


## <a name="references"></a>Referenzen

Folgende Dokumente dienten zur Referenz für diesen Artikel:

- [Konfigurieren von Multipfad-E/A für Ihr StorSimple-Gerät](storsimple-configure-mpio-windows-server.md)

- [Storage scenarios: Thin provisioning](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx) (Speicherszenarien: schlanke Speicherzuweisung)

- [Using GPT drives](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD) (Verwenden von GPT-Laufwerken)

- [Aktivieren und Konfigurieren von Schattenkopien für freigegebene Ordner](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- Erfahren Sie, wie Sie eine [Wiederherstellung aus einem Sicherungssatz durchführen](storsimple-restore-from-backup-set-u2.md).
- Erfahren Sie, wie Sie [ein Gerätefailover und eine Notfallwiederherstellung](storsimple-device-failover-disaster-recovery.md) ausführen.


<!--HONumber=Dec16_HO2-->


