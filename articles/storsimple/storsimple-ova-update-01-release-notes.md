<properties 
   pageTitle="Versionshinweise zu Update 0.1 für StorSimple Virtual Array| Microsoft Azure"
   description="Beschreibt wichtige offene Probleme und Lösungen für StorSimple Virtual Array mit Update 0.1."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/24/2016"
   ms.author="alkohli" />

# Versionshinweise zu Update 0.1 für StorSimple Virtual Array

## Übersicht

In den folgenden Versionshinweisen werden die wichtigsten offenen und gelösten Probleme der Updateversion 0.1 für Microsoft Azure StorSimple Virtual Array behandelt. (Microsoft Azure StorSimple Virtual Array wird auch als lokales virtuelles StorSimple-Gerät oder als virtuelles StorSimple-Gerät bezeichnet.) Diese Veröffentlichung entspricht der Softwareversion **10.0.10279.0**.

Die Versionshinweise werden fortlaufend aktualisiert, und wenn schwerwiegende Probleme festgestellt werden, die eine Problemumgehung erfordern, werden sie hinzugefügt. Lesen Sie vor der Bereitstellung Ihres virtuellen StorSimple-Geräts die Informationen in den Versionsanmerkungen sorgfältig durch.

> [AZURE.NOTE] Das Update 0.1 führt zu einer Unterbrechung und startet Ihr Gerät neu. Falls gerade E/A-Vorgänge ausgeführt werden, kommt es auf dem Gerät zu Ausfällen.

## Neuerungen in Update 0.1

Update 0.1 enthält folgende Fehlerbehebungen und Verbesserungen:

- **Resilienz bei Cloudausfällen**: Diese Version enthält mehrere Fehlerbehebungen für Notfallwiederherstellung, Sicherung, Wiederherstellung und Tiering im Falle einer Unterbrechung der Cloudkonnektivität. 

- **Schnellere Wiederherstellung**: Diese Version enthält Fehlerbehebungen, die die Ausführungsdauer der Wiederherstellungsaufträge erheblich verringern.

- **Optimierte automatisierte Speicherplatzrückgewinnung **: Wenn Daten auf spärlich bereitgestellten Volumes gelöscht werden, müssen die nicht verwendeten Speicherblöcke wieder freigegeben werden. In dieser Version wurde der Prozess zur Speicherplatzrückgewinnung in der Cloud verbessert, was dazu führt, dass nicht genutzter Speicherplatz im Vergleich mit den vorherigen Versionen schneller wieder verfügbar ist.

- **Neue VHD-, VHDX- und VMDK-Dateien**: Im klassischen Azure-Portal sind neue VHD-, VHDX- und VMDK-Dateien verfügbar. Diese Images können zur Bereitstellung neuer Update 0.1-Geräte heruntergeladen werden.

- **Verbesserte Präzision des Auftragsstatus im Portal**: In der früheren Version der Software wurde der Auftragsstatus im Portal nicht präzise angegeben. Dieses Problem wurde in dieser Version behoben.

- **Fehlerbehebungen im Zusammenhang mit Domänenbeitritt und Umbenennung** des Geräts.


## In Update 0.1 behobene Probleme

Die folgende Tabelle enthält eine Zusammenfassung der Probleme, die in dieser Version behoben wurden:

| Nr. | Funktion | Problem |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 | VMDK | In bestimmten VMware-Versionen wurde der Betriebssystemdatenträger als spärlich betrachtet, was Warnungen sowie die Störung des regulären Betriebs zur Folge hatte. Dies wurde in dieser Version behoben. |
| 2 | iSCSI-Server | In der letzten Version musste der Benutzer für jede aktivierte Netzwerkschnittstelle des virtuellen StorSimple-Geräts ein Gateway angeben. Dieses Verhalten wurde in dieser Version geändert, sodass der Benutzer nun mindestens ein Gateway für alle aktivierten Netzwerkschnittstellen konfigurieren muss. |
| 3 | Supportpaket | In der früheren Version der Software trat bei der Supportpaketsammlung ein Fehler auf, wenn die Pakete größer als 1 GB waren. Dieses Problem wurde in dieser Version behoben. |
| 4 | Cloudzugriff | In der letzten Version lag folgendes Problem vor: Wenn StorSimple Virtual Array über keine Netzwerkkonnektivität verfügte und neu gestartet wurde, traten auf der lokalen Benutzeroberfläche Verbindungsprobleme auf. Dieses Problem wurde in dieser Version behoben. |
| 5 | Überwachungsdiagramme | In der vorherigen Version enthielten Cloudkapazitätsauslastungs-Diagramme im klassischen Azure-Portal nach einem Gerätefailover falsche Werte. Dies wurde in der aktuellen Version behoben. |



## Bekannte Probleme in Update 0.1

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme für StorSimple Virtual Array sowie die Probleme aus früheren Versionsinformationen. **Die Probleme aus den aktuellen Versionshinweisen sind mit einem Sternchen gekennzeichnet. Nahezu alle Probleme in dieser Liste stammen aus der GA-Version von StorSimple Virtual Array.**


| Nr. | Funktion | Problem | Problemumgehung/Kommentare |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Aktualisierungen | In der Vorabversion erstellte virtuellen Geräte können nicht auf eine unterstützte Version für allgemeine Verfügbarkeit aktualisiert werden. | Für diese virtuellen Geräte muss mithilfe eines Notfallwiederherstellungs-Workflows ein Failover für die allgemein verfügbare Version ausgeführt werden. |
| **2.** | Bereitgestellter Datenträger | Nachdem Sie einen Datenträger mit einer bestimmten angegebenen Größe bereitgestellt und das entsprechende virtuelle StorSimple-Gerät erstellt haben, darf der Datenträger nicht erweitert oder verkleinert werden. Ein entsprechender Versuch führt zum Verlust aller Daten auf den lokalen Ebenen des Geräts. | |
| **3.** | Gruppenrichtlinie | Wenn ein Gerät Mitglied einer Domäne ist, kann das Anwenden einer Gruppenrichtlinie den Gerätebetrieb beeinträchtigen. | Stellen Sie sicher, dass sich Ihr virtuelles Array in einer eigenen Organisationseinheit (OU) für Active Directory befindet und keine Gruppenrichtlinienobjekte (GPO) darauf angewendet werden.|
| **4.** | Lokale Web-UI | Wenn in Internet Explorer (IE ESC) erweiterten Sicherheitsfeatures aktiviert sind, funktionieren einige lokale Web-UI-Seiten wie "Problembehandlung" oder "Wartung" möglicherweise nicht ordnungsgemäß. Außerdem funktionieren Schaltflächen auf diesen Seiten möglicherweise nicht. | Deaktivieren Sie erweiterte Sicherheitsfeatures in Internet Explorer.|
| **5.** | Lokale Web-UI | Auf einer virtuellen Hyper-V-Maschine werden die Netzwerkschnittstellen in der Web-UI als 10-Gbit/s-Schnittstellen angezeigt. | Dieses Verhalten ist eine Spiegelung von Hyper-V. Hyper-V zeigt immer 10 Gbit/s für virtuelle Netzwerkadapter an. |
| **6.** | Mehrstufige Volumes oder Freigaben | Bytebereichsperren für Anwendungen, die mit den mehrstufigen StorSimple-Volumes arbeiten, werden nicht unterstützt. Wenn Bytebereichsperren aktiviert sind, funktioniert die StorSimple-Staffelung nicht. | Empfohlene Maßnahmen: <br></br>Deaktivieren Sie Bytebereichsperren in der Anwendungslogik.<br></br>Wählen Sie zum Einfügen von Daten für diese Anwendung lokal angeheftete Volumes anstatt mehrstufige Volumes.<br></br>*Warnung*: Wenn lokal angeheftete Volumes verwendet werden und Bytebereichsperren aktiviert sind, kann das lokal angeheftete Volume online sein, bevor die Wiederherstellung abgeschlossen ist. Wenn in solchen Fällen eine Wiederherstellung ausgeführt wird, müssen Sie auf den Abschluss des Wiederherstellungsvorgangs warten. |
| **7.** | Mehrstufige Freigaben | Das Arbeiten mit großen Dateien kann zu einer langsamen Abstufung führen. | Bei der Arbeit mit großen Dateien sollte die größte Datei nach Möglichkeit kleiner als 3 % der Größe der Dateifreigabe sein. |
| **8.** | Für Freigaben genutzte Kapazität | Möglicherweise wird eine Nutzung durch die Freigabe angezeigt, auch wenn diese keine Daten enthält. Dies liegt daran, dass die für Freigaben genutzte Kapazität Metadaten umfasst. | |
| **9.** | Notfallwiederherstellung | Sie können die Notfallwiederherstellung eines Dateiservers nur in der Domäne des Quellgeräts ausführen. Die Notfallwiederherstellung auf einem Zielgerät in einer anderen Domäne wird in dieser Version nicht unterstützt. | Dies wird in einer späteren Version implementiert werden. |
| **10.** | Azure PowerShell | Die virtuellen StorSimple-Geräte können in dieser Version nicht mithilfe von Azure PowerShell verwaltet werden. | Die gesamte Verwaltung der virtuellen Geräte sollte über das klassische Azure-Portal und die lokale Web-UI erfolgen. |
| **11.** | Kennwortänderung | Die Virtual Array-Gerätekonsole akzeptiert Eingaben nur über eine de-DE-Tastatur. | |
| **12.** | CHAP | Einmal erstellte CHAP-Anmeldeinformationen können nicht wieder entfernt werden. Wenn Sie die CHAP-Anmeldeinformationen ändern, müssen Sie außerdem die Volumes offline und erneut online schalten, damit die Änderungen wirksam werden. | Dies wird in einer zukünftigen Version behoben. |
| **13.** | iSCSI-Server | Der für ein iSCSI-Volume „verwendete Speicher“ kann im StorSimple Manager-Dienst und im iSCSI-Host unterschiedlich angezeigt werden. | Der iSCSI-Host verwendet die Dateisystemansicht.<br></br>Dem Gerät werden die Blöcke angegeben, die dem Volume bei maximaler Größe zugeordnet waren.|
| **14.** | Dateiserver* | Wenn einer Datei in einem Ordner ein alternativer Datenstrom (Alternate Data Stream, ADS) zugeordnet ist, wird dieser nicht gesichert oder mittels Notfallwiederherstellung, Klonen oder Wiederherstellung auf Elementebene wiederhergestellt.| |


## Nächster Schritt

[Installieren Sie Update 0.1](storsimple-ova-install-update-01.md) für StorSimple Virtual Array.

<!---HONumber=AcomDC_0525_2016-->