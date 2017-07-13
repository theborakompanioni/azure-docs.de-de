---
title: 'Azure AD Connect Sync: Scheduler | Microsoft Docs'
description: Dieses Thema beschreibt das integrierte Schedulerfeature in Azure AD Connect Sync.
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b1a598f-89c0-4244-9b20-f4aaad5233cf
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: fe7f508ed1c4eb57663f7e252d286719af03dbb1
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017

---
<a id="azure-ad-connect-sync-scheduler" class="xliff"></a>

# Azure AD Connect Sync: Scheduler
Dieses Thema beschreibt den integrierten Scheduler in Azure AD Connect Sync (auch bekannt als Synchronisierungsmodul).

Diese Funktion wurde mit Build 1.1.105.0 eingeführt (veröffentlicht im Februar 2016).

<a id="overview" class="xliff"></a>

## Übersicht
Azure AD Connect Sync synchronisiert Änderungen in Ihrem lokalen Verzeichnis mithilfe eines Schedulers. Es gibt zwei Schedulerprozesse: einen für die Kennwortsynchronisierung und einen anderen für die Objekt-/Attributsynchronisierung sowie für Wartungstasks. In diesem Thema wird letzterer behandelt.

In früheren Versionen war der Scheduler für Objekte und Attribute nicht in das Synchronisierungsmodul integriert. Das Modul verwendete die Windows-Aufgabenplanung oder einen separaten Windows-Dienst, um den Synchronisierungsprozess auslösen. Ab den 1.1-Versionen ist der Scheduler in das Synchronisierungsmodul integriert und kann angepasst werden. Die neue Standardeinstellung für die Synchronisierungsfrequenz ist 30 Minuten.

Der Scheduler ist für zwei Tasks zuständig:

* **Synchronisierungszyklus**. Der Prozess zum Importieren, Synchronisieren und Exportieren von Änderungen.
* **Wartungstasks**. Erneuern von Schlüsseln und Zertifikaten für das Zurücksetzen von Kennwörtern und für den Geräteregistrierungsdienst (Device Registration Service, DRS). Löschen alter Einträge im Vorgangsprotokoll.

Der Scheduler selbst wird immer ausgeführt, kann jedoch so konfiguriert werden, dass er nur einen oder keinen dieser Tasks ausführt. Wenn Sie beispielsweise einen eigenen Prozess für den Synchronisierungszyklus verwenden, können Sie diesen Task im Scheduler deaktivieren, den Wartungstask aber weiterhin ausführen.

<a id="scheduler-configuration" class="xliff"></a>

## Schedulerkonfiguration
Um die aktuellen Konfigurationseinstellungen anzuzeigen, wechseln Sie zu PowerShell, und führen Sie `Get-ADSyncScheduler`aus. Die Ausgabe ähnelt der folgenden Abbildung:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings2016.png)

Wenn beim Ausführen dieses Cmdlets die Meldung angezeigt wird, dass der **** Befehl oder das Cmdlet zur Synchronisierung nicht verfügbar ist, ist das PowerShell-Modul nicht geladen. Dieses Problem kann auftreten, wenn Sie Azure AD Connect auf einem Domänencontroller oder einem Server mit höherer PowerShell-Einschränkungsebene ausführen als in den Standardeinstellungen festgelegt. Wenn dieser Fehler angezeigt wird, führen Sie `Import-Module ADSync` aus, um das Cmdlet verfügbar zu machen.

* **AllowedSyncCycleInterval**. Das kürzeste in Azure AD zulässige Zeitintervall zwischen Synchronisierungszyklen. Eine höhere Synchronisierungsfrequenz als diese Einstellung wird nicht unterstützt.
* **CurrentlyEffectiveSyncCycleInterval**. Der momentan verwendete Zeitplan. Er weist den gleichen Wert auf wie CustomizedSyncInterval (falls festgelegt), falls er keine höhere Häufigkeit als AllowedSyncInterval aufweist. Wenn Sie einen älteren Build als 1.1.281 verwenden und CustomizedSyncCycleInterval ändern, tritt diese Änderung nach dem nächsten Synchronisierungszyklus in Kraft. Ab Build 1.1.281 wird die Änderung sofort wirksam.
* **CustomizedSyncCycleInterval**. Wenn der Scheduler mit einer beliebigen anderen Häufigkeit als der Standardfrequenz von 30 Minuten ausgeführt werden soll, konfigurieren Sie diese Einstellung. In der Abbildung oben wurde der Scheduler stattdessen auf einmal pro Stunde festgelegt. Wenn Sie diese Einstellung auf einen niedrigeren Wert als „AllowedSyncInterval“ festlegen, wird stattdessen „AllowedSyncInterval“ verwendet.
* **NextSyncCyclePolicyType**. Entweder „Delta“ oder „Initial“. Legt fest, ob bei der nächsten Ausführung nur Deltaänderungen verarbeitet werden sollen oder ob bei der nächsten Ausführung ein vollständiger Import mit Synchronisierung stattfinden soll. Mit der zweiten Option werden auch neue oder geänderte Regeln erneut verarbeitet.
* **NextSyncCycleStartTimeInUTC**. Der nächste Zeitpunkt, zu dem der Scheduler den nächsten Synchronisierungszyklus startet.
* **PurgeRunHistoryInterval**. Der Zeitraum, für den die Vorgangsprotokolle aufbewahrt werden sollen. Diese Protokolle können im Synchronization Service Manager überprüft werden. Standardmäßig werden diese Protokolle sieben Tage lang gespeichert.
* **SyncCycleEnabled**. Gibt an, ob der Scheduler die Import-, Synchronisierungs- und Exportprozesse als Teil seiner Operation ausführt.
* **MaintenanceEnabled**. Zeigt an, ob der Wartungsprozess aktiviert ist. Dabei werden die Zertifikate/Schlüssel aktualisiert und das Vorgangsprotokoll bereinigt.
* **StagingModeEnabled**. Zeigt an, ob der [Stagingmodus](active-directory-aadconnectsync-operations.md#staging-mode) aktiviert ist. Wenn diese Einstellung aktiviert ist, unterdrückt sie die Ausführung der Exporte. Der Import und die Synchronisierung werden jedoch weiterhin ausgeführt.
* **SchedulerSuspended**. Während eines Upgrades von Connect festgelegt, um den Scheduler vorübergehend am Ausführen zu hindern.

Sie können einige dieser Einstellungen mit `Set-ADSyncScheduler`ändern. Folgende Parameter können geändert werden:

* CustomizedSyncCycleInterval
* NextSyncCyclePolicyType
* PurgeRunHistoryInterval
* SyncCycleEnabled
* MaintenanceEnabled

In früheren Builds von Azure AD Connect **isStagingModeEnabled** wurde im Set-ADSyncScheduler verfügbar gemacht. Es wird zum Festlegen dieser Eigenschaft **nicht unterstützt**. Die Eigenschaft **SchedulerSuspended** sollte nur von Connect geändert werden. Es wird **nicht unterstützt** dies direkt mit PowerShell festzulegen.

Die Konfiguration des Schedulers wird in Azure AD gespeichert. Wenn Sie über einen Stagingserver verfügen, wirken sich alle Änderungen am primären Server auch auf den Stagingserver aus (mit Ausnahme von „IsStagingModeEnabled“).

<a id="customizedsynccycleinterval" class="xliff"></a>

### CustomizedSyncCycleInterval
Syntax: `Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
T - Tage, HH - Stunden, mm - Minuten, ss - Sekunden

Beispiel: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Ändert den Scheduler so, dass er alle drei Stunden ausgeführt wird.

Beispiel: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Ändert den Scheduler so, dass er täglich ausgeführt wird.

<a id="disable-the-scheduler" class="xliff"></a>

### Deaktivieren des Schedulers  
Wenn Sie Änderungen an der Konfiguration vornehmen müssen, sollten Sie den Scheduler deaktivieren, z.B. wenn Sie [die Filterung konfigurieren](active-directory-aadconnectsync-configure-filtering.md) oder [Änderungen an Synchronisierungsregeln vornehmen](active-directory-aadconnectsync-change-the-configuration.md).

Führen Sie `Set-ADSyncScheduler -SyncCycleEnabled $false` aus, um den Scheduler zu deaktivieren.

![Deaktivieren des Schedulers](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)

Wenn Sie die Änderungen vorgenommen haben, denken Sie daran, den Scheduler mit `Set-ADSyncScheduler -SyncCycleEnabled $true` erneut zu aktivieren.

<a id="start-the-scheduler" class="xliff"></a>

## Starten des Schedulers
Der Scheduler wird standardmäßig alle 30 Minuten ausgeführt. In einigen Fällen möchten Sie vielleicht einen Synchronisierungszyklus zwischen den geplanten Zyklen ausführen, oder Sie müssen einen anderen Typ ausführen.

**Deltasynchronisierungszyklus**  
: Ein Deltasynchronisierungszyklus umfasst die folgenden Schritte:

* Deltaimport auf allen Connectors
* Deltasynchronisierung auf allen Connectors
* Export auf allen Connectors

Es kann vorkommen, dass eine dringende Änderung sofort synchronisiert werden muss. In diesem Fall müssen Sie einen Zyklus manuell ausführen. Wenn Sie einen Zyklus manuell ausführen müssen, führen Sie in PowerShell den Befehl `Start-ADSyncSyncCycle -PolicyType Delta` aus.

**Vollständiger Synchronisierungszyklus**  
Wenn Sie eine der folgenden Konfigurationsänderungen vorgenommen haben, müssen Sie einen vollständigen Synchronisierungszyklus ausführen (auch als Erstsynchronisierung bezeichnet):

* Sie haben weitere Objekte oder Attribute hinzugefügt, die aus einem Quellverzeichnis importiert werden müssen.
* Sie haben Änderungen an den Synchronisierungsregeln vorgenommen.
* Sie haben die [Filterung](active-directory-aadconnectsync-configure-filtering.md) geändert, sodass eine andere Anzahl von Objekten einbezogen werden soll.

Wenn Sie eine dieser Änderungen vorgenommen haben, müssen Sie einen vollständigen Synchronisierungszyklus ausführen, damit das Synchronisierungsmodul die Möglichkeit hat, die Connectorbereiche neu zu konsolidieren. Ein vollständiger Synchronisierungszyklus umfasst die folgenden Schritte:

* Vollständiger Import auf allen Connectors
* Vollständige Synchronisierung auf allen Connectors
* Export auf allen Connectors

Um einen vollständigen Synchronisierungszyklus zu initiieren, führen Sie an einer PowerShell-Eingabeaufforderung den Befehl `Start-ADSyncSyncCycle -PolicyType Initial` aus. Mit diesem Befehl wird ein vollständiger Synchronisierungszyklus gestartet.

<a id="stop-the-scheduler" class="xliff"></a>

## Beenden des Schedulers
Wenn der Scheduler gerade einen Synchronisierungszyklus ausführt, müssen Sie diesen beenden. Dies ist zum Beispiel der Fall, wenn Sie beim Starten des Installations-Assistenten den folgenden Fehler erhalten:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Während ein Synchronisierungszyklus ausgeführt wird, sind Änderungen an der Konfiguration nicht möglich. Sie können warten, bis der Scheduler den Prozess beendet hat. Sie können den Prozess aber auch beenden, um Ihre Änderungen sofort vorzunehmen. Das Beenden des aktuellen Zyklus ist ungefährlich, und ausstehende Änderungen werden bei der nächsten Ausführung verarbeitet.

1. Weisen Sie den Scheduler zunächst mit dem PowerShell-Cmdlet `Stop-ADSyncSyncCycle` an, den aktuellen Zyklus zu beenden.
2. Wenn Sie einen älteren Build als 1.1.281 verwenden, wird der aktuelle Task des aktuellen Connectors durch das Beenden des Schedulers nicht unterbrochen. Um ein Beenden des Connectors zu erzwingen, führen Sie folgende Aktionen aus: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png).
   * Starten Sie den **Synchronisierungsdienst** über das Startmenü. Wechseln Sie zu **Connectors**, markieren Sie den Connector mit dem Status **Wird ausgeführt**, und wählen Sie unter „Aktionen“ die Option **Beenden** aus.

Der Scheduler ist noch immer aktiv und wird bei der nächsten Gelegenheit wieder gestartet.

<a id="custom-scheduler" class="xliff"></a>

## Benutzerdefinierter Scheduler
Die in diesem Abschnitt dokumentierten Cmdlets stehen nur in Build [1.1.130.0](active-directory-aadconnect-version-history.md#111300) und höher zur Verfügung.

Wenn der integrierte Scheduler Ihre Anforderungen nicht erfüllt, können Sie die Connectors mithilfe von PowerShell planen.

<a id="invoke-adsyncrunprofile" class="xliff"></a>

### Invoke-ADSyncRunProfile
Sie können ein Profil für einen Connector auf folgende Weise starten:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Die verwendbaren [Connectornamen](active-directory-aadconnectsync-service-manager-ui-connectors.md) und [Ausführungsprofilnamen](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) finden Sie auf der [Synchronization Service Manager-Benutzeroberfläche](active-directory-aadconnectsync-service-manager-ui.md).

![Aufrufen des Ausführungsprofils](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

Das `Invoke-ADSyncRunProfile`-Cmdlet ist synchron, d.h., es gibt die Steuerung erst zurück, wenn der Connector den Vorgang erfolgreich oder mit Fehler abgeschlossen hat.

Es empfiehlt sich, die Connectors in der folgenden Reihenfolge zu planen:

1. Importieren aus lokalen Verzeichnissen wie beispielsweise Active Directory (vollständig oder als Delta)
2. Importieren aus Azure AD (vollständig oder als Delta)
3. Synchronisieren aus lokalen Verzeichnissen wie beispielsweise Active Directory (vollständig oder als Delta)
4. Synchronisieren aus Azure AD (vollständig oder als Delta)
5. Exportieren in Azure AD
6. Exportieren in lokale Verzeichnisse wie beispielsweise Active Directory

Der integrierte Scheduler führt die Connectors in dieser Reihenfolge aus.

<a id="get-adsyncconnectorrunstatus" class="xliff"></a>

### Get-ADSyncConnectorRunStatus
Sie können das Synchronisierungsmodul auch überwachen, um zu ermitteln, ob es beschäftigt ist oder im Leerlauf befindet. Das Cmdlet gibt ein leeres Ergebnis zurück, wenn sich das Synchronisierungsmodul im Leerlauf befindet und keinen Connector ausführt. Wenn ein Connector ausgeführt wird, gibt das Cmdlet den Namen des Connectors zurück.

```
Get-ADSyncConnectorRunStatus
```

![Status der Connectorausführung](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
In der obigen Abbildung weist die erste Zeile darauf hin, dass sich das Synchronisierungsmodul im Leerlauf befindet. Die zweite Zeile bedeutet, dass der Azure AD-Connector ausgeführt wird.

<a id="scheduler-and-installation-wizard" class="xliff"></a>

## Scheduler und Installations-Assistent
Wenn Sie den Installations-Assistenten starten, wird der Scheduler vorübergehend unterbrochen. Der Grund für dieses Verhalten ist die Annahme, dass Sie Konfigurationsänderungen vornehmen möchten, und diese Einstellungen können nicht angewendet werden, solange das Synchronisierungsmodul aktiv ausgeführt wird. Lassen Sie daher den Installations-Assistenten nicht geöffnet, da er alle Synchronisierungsaktionen auf dem Synchronisierungsmodul verhindert.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md) .

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

