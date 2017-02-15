---
title: StorSimple Manager-Dienstverwaltung | Microsoft Docs
description: "Erfahren Sie, wie Sie Ihr StorSimple-Gerät mithilfe des StorSimple Manager-Diensts im klassischen Azure-Portal verwalten."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2586582e-d85c-42e1-afb3-be734c1c0461
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: d5d5aa67d14d2344c58e67ee78ea3f2b5d8fd415


---
# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>Verwalten Ihres StorSimple-Geräts mithilfe des StorSimple Manager-Diensts
## <a name="overview"></a>Übersicht
Dieser Artikel beschreibt die Benutzeroberfläche des StorSimple Manager-Diensts und erläutert, wie Sie eine Verbindung herstellen und welche Optionen verfügbar sind. Darüber hinaus enthält der Artikel Links zu den Workflows, die über diese Benutzeroberfläche ausgeführt werden können. Die Anleitung gilt sowohl für das physische als auch für das virtuelle StorSimple-Gerät.

In diesem Artikel lernen Sie Folgendes:

* Herstellen einer Verbindung mit dem StorSimple Manager-Dienst
* Navigieren auf der Benutzeroberfläche von StorSimple Manager
* Verwalten Ihres StorSimple-Geräts über den StorSimple Manager-Dienst

## <a name="connect-to-storsimple-manager-service"></a>Herstellen einer Verbindung mit dem StorSimple Manager-Dienst
Der StorSimple Manager-Dienst wird in Microsoft Azure ausgeführt. Er stellt Verbindungen mit mehreren StorSimple-Geräten her. Diese Geräte werden über ein zentrales browserbasiertes klassisches Microsoft Azure-Portal verwaltet. Gehen Sie wie folgt vor, um eine Verbindung mit dem StorSimple Manager-Dienst herzustellen:

#### <a name="to-connect-to-the-service"></a>So stellen Sie eine Verbindung mit dem Dienst her
1. Rufen Sie [https://manage.windowsazure.com/](https://manage.windowsazure.com/)auf.
2. Melden Sie sich mit den Anmeldeinformationen Ihres Microsoft-Kontos beim klassischen Microsoft Azure-Portal (rechts oben) an.
3. Führen Sie im linken Navigationsbereich einen Bildlauf nach unten durch, um auf den StorSimple Manager-Dienst zuzugreifen.

## <a name="navigate-storsimple-manager-service-ui"></a>Navigieren auf der Benutzeroberfläche des StorSimple Manager-Diensts
Die folgende Tabelle gibt Aufschluss über die Navigationshierarchie der Benutzeroberfläche des StorSimple Manager-Diensts.

* **StorSimple Manager** gelangen Sie zu den Seiten auf Dienstebene, die alle Geräte in einem Dienst betreffen.
* **Geräte** gelangen zu den Seiten auf Geräteebene, die ein bestimmtes Gerät betreffen.
* **Volumecontainer** gelangen Sie zur Volumeseite mit allen dem Gerät zugeordneten Volumes.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Navigationshierarchie des StorSimple Manager-Diensts
| Zielseite | Seiten auf Dienstebene | Seiten auf Geräteebene | Seiten auf Geräteebene |
| --- | --- | --- | --- |
| StorSimple Manager-Dienst |Dienstdashboard |Gerätedashboard | |
| Geräte → |Überwachen | | |
| Sicherungskatalog |Volumecontainer→ |Volumes | |
| Konfigurieren (Dienst) |Sicherungsrichtlinien | | |
| Aufträge |Konfigurieren (Gerät) | | |
| Warnungen |Wartung | | |

![Video verfügbar](./media/storsimple-manager-service-administration/Video_icon.png) **Video verfügbar**

Um sich ein Video anzusehen, das Sie durch die Benutzeroberfläche des StorSimple Manager-Diensts führt, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Verwalten des StorSimple-Geräts mithilfe des StorSimple Manager-Diensts
Die folgende Tabelle zeigt eine Zusammenfassung der allgemeinen Verwaltungsaufgaben und komplexen Workflows, die auf der Benutzeroberfläche des StorSimple Manager-Diensts ausgeführt werden können. Diese Aufgaben sind nach der Benutzeroberflächenseite strukturiert, auf der sie jeweils initiiert werden.

Klicken Sie auf die Verfahren in der Tabelle, um weitere Informationen zu den einzelnen Workflows zu erhalten.

#### <a name="storsimple-manager-workflows"></a>StorSimple Manager-Workflows
| Gewünschte Aktion | Seite auf der Benutzeroberfläche | Verfahren |
| --- | --- | --- |
| Dienst erstellen</br>Löschen von Diensten</br>Dienstregistrierungsschlüssel abrufen</br>Dienstregistrierungsschlüssel erneut generieren |StorSimple Manager-Dienst |[Bereitstellen eines StorSimple Manager-Diensts](storsimple-manage-service.md) |
| Dienstdaten-Verschlüsselungsschlüssel ändern</br>Vorgangsprotokolle anzeigen |StorSimple Manager-Dienst → Dashboard |[Verwenden des Dashboards des StorSimple Manager-Diensts](storsimple-service-dashboard.md) |
| Gerät deaktivieren</br>Gerät löschen |StorSimple Manager-Dienst → Geräte |[Deaktivieren oder Löschen eines Geräts](storsimple-deactivate-and-delete-device.md) |
| Über Notfallwiederherstellung und Gerätefailover informieren</br>Durchführen eines Failovers auf ein physisches Gerät</br>Durchführen eines Failovers auf ein virtuelles Gerät</br>Business Continuity Disaster Recovery (BCDR) |StorSimple Manager-Dienst → Geräte |[Ausführen eines Failovers und einer Notfallwiederherstellung für das StorSimple-Gerät](storsimple-device-failover-disaster-recovery.md) |
| Liste mit Sicherungen für ein Volume anzeigen</br>Wählen Sie einen Sicherungssatz aus</br>Sicherungssatz löschen |StorSimple Manager-Dienst → Sicherungskatalog |[Verwalten von Sicherungen](storsimple-manage-backup-catalog.md) |
| Volume klonen |StorSimple Manager-Dienst → Sicherungskatalog |[Klonen eines Volumes](storsimple-clone-volume.md) |
| Sicherungssatz wiederherstellen |StorSimple Manager-Dienst → Sicherungskatalog |[Wiederherstellen eines Sicherungssatzes](storsimple-restore-from-backup-set.md) |
| Informationen zu Speicherkonten</br>Hinzufügen von Speicherkonten</br>Bearbeiten eines Speicherkontos</br>Löschen eines Speicherkontos</br>Schlüsselrotation für Speicherkonten ausführen |StorSimple Manager-Dienst → Konfigurieren |[Verwalten von Speicherkonten](storsimple-manage-storage-accounts.md) |
| Über Bandbreitenvorlagen informieren</br>Hinzufügen einer Bandbreitenvorlage</br>Bearbeiten einer Bandbreitenvorlage</br>Löschen einer Bandbreitenvorlage</br>Verwenden einer Standardbandbreitenvorlage</br>Ganztägige Bandbreitenvorlage mit definiertem Startzeitpunkt erstellen |StorSimple Manager-Dienst → Konfigurieren |[Verwalten von Bandbreitenvorlagen](storsimple-manage-bandwidth-templates.md) |
| Informationen zu Access Control-Datensätzen</br>Erstellen eines Zugriffssteuerungsdatensatzes</br>Bearbeiten von Zugriffssteuerungsdatensätzen</br>Löschen von Access Control-Datensätzen |StorSimple Manager-Dienst → Konfigurieren |[Verwalten von Zugriffssteuerungsdatensätzen](storsimple-manage-acrs.md) |
| Auftragsdetails anzeigen</br>Auftrag abbrechen |StorSimple Manager-Dienst → Aufträge |[Verwalten von Aufträgen](storsimple-manage-jobs.md) |
| Warnungsbenachrichtigungen empfangen</br>Warnungen verwalten</br>Überprüfen von Warnungen |StorSimple Manager-Dienst → Warnungen |[Anzeigen und Verwalten von StorSimple-Warnungen](storsimple-manage-alerts.md) |
| Verbundene Initiatoren anzeigen</br>Suchen nach der Seriennummer des Geräts</br>Ziel-IQN ermitteln |StorSimple Manager-Dienst → Geräte → Dashboard |[Verwenden des StorSimple-Gerätedashboards](storsimple-device-dashboard.md) |
| Überwachungsdiagramme erstellen |StorSimple Manager-Dienst → Geräte → Überwachen |[Überwachen von StorSimple-Geräten](storsimple-monitor-device.md) |
| Volumecontainer hinzufügen</br>Ändern eines Volumecontainers</br>Volumecontainer löschen |StorSimple Manager-Dienst → Geräte → Volumecontainer |[Verwalten von Volumecontainern](storsimple-manage-volume-containers.md) |
| Hinzufügen eines Volumes</br>Ändern von Volumes</br>Offlineschalten von Volumes</br>Löschen von Volumes</br>Überwachen von Volumes |StorSimple Manager-Dienst → Geräte → Volumecontainer → Volumes |[Verwalten von Volumes](storsimple-manage-volumes.md) |
| Ändern von Geräteeinstellungen</br>Ändern von Zeiteinstellungen</br>Ändern von DNS.md-Einstellungen</br>Netzwerkschnittstellen konfigurieren |StorSimple Manager-Dienst → Geräte → Konfigurieren |[Ändern der Gerätekonfiguration für Ihr StorSimple-Gerät](storsimple-modify-device-config.md) |
| Webproxyeinstellungen anzeigen |StorSimple Manager-Dienst → Geräte → Konfigurieren |[Konfigurieren des Webproxys für Ihr Gerät](storsimple-configure-web-proxy.md) |
| Geräteadministratorkennwort ändern</br>Kennwort für StorSimple Snapshot Manager ändern |StorSimple Manager-Dienst → Geräte → Konfigurieren |[Ändern von StorSimple-Kennwörtern](storsimple-change-passwords.md) |
| Remoteverwaltung konfigurieren |StorSimple Manager-Dienst → Geräte → Konfigurieren |[Herstellen einer Remoteverbindung mit dem StorSimple-Gerät](storsimple-remote-connect.md) |
| Warnungseinstellungen konfigurieren |StorSimple Manager-Dienst → Geräte → Konfigurieren |[Anzeigen und Verwalten von StorSimple-Warnungen](storsimple-manage-alerts.md) |
| CHAP für Ihr StorSimple-Gerät konfigurieren |StorSimple Manager-Dienst → Geräte → Konfigurieren |[Konfigurieren von CHAP für Ihr StorSimple-Gerät](storsimple-configure-chap.md) |
| Sicherungsrichtlinie hinzufügen</br>Hinzufügen oder Ändern eines Zeitplans</br>Löschen einer Sicherungsrichtlinie</br>Erstellen einer manuellen Sicherung</br>Benutzerdefinierte Sicherungsrichtlinie mit mehreren Volumes und Zeitplänen erstellen |StorSimple Manager-Dienst → Geräte → Sicherungsrichtlinien |[Verwalten von Sicherungsrichtlinien](storsimple-manage-backup-policies.md) |
| Gerätecontroller beenden</br>Gerätecontroller neu starten</br>Gerätecontroller herunterfahren</br>Gerät auf die Werkseinstellungen zurücksetzen</br>(nur bei lokalen Geräten) |StorSimple Manager-Dienst → Geräte → Wartung |[Verwalten des StorSimple-Gerätecontrollers](storsimple-manage-device-controller.md) |
| Über StorSimple-Hardwarekomponenten informieren</br>Hardwarestatus überwachen</br>(nur bei lokalen Geräten) |StorSimple Manager-Dienst → Geräte → Wartung |[Überwachen der Hardwarekomponenten](storsimple-monitor-hardware-status.md) |
| Unterstützungspaket erstellen |StorSimple Manager-Dienst → Geräte → Wartung |[Erstellen und Verwalten eines Unterstützungspakets](storsimple-create-manage-support-package.md) |
| Softwareupdates installieren |StorSimple Manager-Dienst → Geräte → Wartung |[Aktualisieren Ihres Geräts](storsimple-update-device.md) |

## <a name="next-steps"></a>Nächste Schritte
Informationen zu Problemen beim alltäglichen Betrieb Ihres StorSimple-Geräts oder zu Problemen mit dessen Hardwarekomponenten finden Sie hier:

* [Problembehandlung bei einem betriebsbereiten Gerät](storsimple-troubleshoot-operational-device.md)
* [StorSimple-Überwachungsindikatoren](storsimple-monitoring-indicators.md)

Falls sich die Probleme nicht beheben lassen und Sie eine Serviceanfrage erstellen müssen, finden Sie unter [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md)weitere Informationen.




<!--HONumber=Nov16_HO3-->


