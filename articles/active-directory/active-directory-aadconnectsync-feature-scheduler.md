<properties
   pageTitle="Azure AD Connect Sync: Scheduler | Microsoft Azure"
   description="Dieses Thema beschreibt das integrierte Schedulerfeature in Azure AD Connect Sync."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-sync:-scheduler"></a>Azure AD Connect Sync: Scheduler
Dieses Thema beschreibt den integrierten Scheduler in Azure AD Connect Sync (auch bekannt als  Synchronisierungsmodul).

Diese Funktion wurde mit Build 1.1.105.0 eingeführt (veröffentlicht im Februar 2016).

## <a name="overview"></a>Übersicht
Azure AD Connect Sync synchronisiert Änderungen in Ihrem lokalen Verzeichnis mithilfe eines Schedulers. Es gibt zwei Schedulerprozesse: einen für die Kennwortsynchronisierung und einen anderen für die Objekt-/Attributsynchronisierung und für Wartungstasks. In diesem Thema wird letzterer behandelt.

In früheren Versionen befand sich der Scheduler für Objekte und Attribute außerhalb des Synchronisierungsmoduls und des Windows-Taskplaners, oder es wurde ein separater Windows-Dienst zum Auslösen des Synchronisierungsprozesses verwendet. Ab den 1.1-Versionen ist der Scheduler in das Synchronisierungsmodul integriert und kann angepasst werden. Die neue Standardeinstellung für die Synchronisierungsfrequenz ist 30 Minuten.

Der Scheduler ist für zwei Tasks zuständig:

- **Synchronisierungszyklus**. Der Prozess zum Importieren, Synchronisieren und Exportieren von Änderungen.
- **Wartungstasks**. Erneuern von Schlüsseln und Zertifikaten für das Zurücksetzen von Kennwörtern und für den Geräteregistrierungsdienst (Device Registration Service, DRS). Löschen alter Einträge im Vorgangsprotokoll.

Der Scheduler selbst wird immer ausgeführt, kann jedoch so konfiguriert werden, dass er nur einen oder keinen dieser Tasks ausführt. Wenn Sie beispielsweise einen eigenen Prozess für den Synchronisierungszyklus verwenden, können Sie diesen Task im Scheduler deaktivieren, den Wartungstask aber weiterhin ausführen.

## <a name="scheduler-configuration"></a>Schedulerkonfiguration
Um die aktuellen Konfigurationseinstellungen anzuzeigen, wechseln Sie zu PowerShell, und führen Sie `Get-ADSyncScheduler`aus. Folgendes wird angezeigt:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Wenn beim Ausführen dieses Cmdlets die Meldung angezeigt wird, dass der **** Befehl oder das Cmdlet zur Synchronisierung nicht verfügbar ist, ist das PowerShell-Modul nicht geladen. Dies kann passieren, wenn Sie Azure AD Connect auf einem Domänencontroller oder einem Server mit höherer PowerShell-Einschränkungsebene ausführen als in den Standardeinstellungen festgelegt. Wenn dieser Fehler angezeigt wird, führen Sie `Import-Module ADSync` aus, um das Cmdlet verfügbar zu machen.

- **AllowedSyncCycleInterval**. Die höchste von Azure AD zugelassene Synchronisierungshäufigkeit. Eine höhere Synchronisierungsfrequenz als hier angegeben wird nicht unterstützt.
- **CurrentlyEffectiveSyncCycleInterval**. Der momentan verwendete Zeitplan. Er besitzt den gleichen Wert wie CustomizedSyncInterval (falls festgelegt), falls er keine höhere Häufigkeit als AllowedSyncInterval aufweist. Wenn Sie CustomizedSyncCycleInterval ändern, tritt diese Änderung nach dem nächsten Synchronisierungszyklus in Kraft.
- **CustomizedSyncCycleInterval**. Wenn der Scheduler mit einer beliebigen anderen Häufigkeit als der Standardfrequenz von 30 Minuten ausgeführt werden soll, konfigurieren Sie diese Einstellung. In der oben gezeigten Abbildung wurde der Scheduler stattdessen auf einmal pro Stunde festgelegt. Wenn Sie diese Einstellung auf einen niedrigeren Wert als AllowedSyncInterval festlegen, wird stattdessen AllowedSyncInterval verwendet.
- **NextSyncCyclePolicyType**. Entweder „Delta“ oder „Initial“. Legt fest, ob bei der nächsten Ausführung nur Deltaänderungen verarbeitet werden sollen oder ob bei der nächsten Ausführung ein vollständiger Import mit Synchronisierung stattfinden soll. Dabei werden auch neue oder geänderte Regeln erneut verarbeitet.
- **NextSyncCycleStartTimeInUTC**. Der nächste Zeitpunkt, zu dem der Scheduler den nächsten Synchronisierungszyklus startet.
- **PurgeRunHistoryInterval**. Der Zeitraum, für den die Vorgangsprotokolle aufbewahrt werden sollen. Diese können im Synchronization Service Manager überprüft werden. Standardmäßig werden sie sieben Tage lang gespeichert.
- **SyncCycleEnabled**. Gibt an, ob der Scheduler die Import-, Synchronisierungs- und Exportprozesse als Teil seiner Operation ausführt.
- **MaintenanceEnabled**. Zeigt an, ob der Wartungsprozess aktiviert ist. Dabei werden die Zertifikate/Schlüssel aktualisiert und das Vorgangsprotokoll bereinigt.
- **IsStagingModeEnabled**. Zeigt an, ob der [Stagingmodus](active-directory-aadconnectsync-operations.md#staging-mode) aktiviert ist.

Sie können einige dieser Einstellungen mit `Set-ADSyncScheduler`ändern. Folgende Parameter können geändert werden:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

Die Konfiguration des Schedulers wird in Azure AD gespeichert. Wenn Sie über einen Stagingserver verfügen, wirken sich alle Änderungen am primären Server auch auf den Stagingserver aus (mit Ausnahme von „IsStagingModeEnabled“).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntax: `Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
 T - Tage, HH - Stunden, mm - Minuten, ss - Sekunden

Beispiel: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
 Ändert den Scheduler so, dass er alle drei Stunden ausgeführt wird.

Beispiel: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
 Ändert den Scheduler so, dass er täglich ausgeführt wird.

## <a name="start-the-scheduler"></a>Starten des Schedulers
Der Scheduler wird standardmäßig alle 30 Minuten ausgeführt. In einigen Fällen möchten Sie vielleicht einen Synchronisierungszyklus zwischen den geplanten Zyklen ausführen, oder Sie müssen einen anderen Typ ausführen.

**Deltasynchronisierungszyklus**  
: Ein Deltasynchronisierungszyklus umfasst die folgenden Schritte:

- Deltaimport auf allen Connectors
- Deltasynchronisierung auf allen Connectors
- Export auf allen Connectors

Es kann vorkommen, dass eine dringende Änderung sofort synchronisiert werden muss. In diesem Fall müssen Sie einen Zyklus manuell ausführen. Wenn Sie einen Zyklus manuell ausführen müssen, führen Sie in PowerShell den Befehl `Start-ADSyncSyncCycle -PolicyType Delta` aus.

**Vollständiger Synchronisierungszyklus**  
Wenn Sie eine der folgenden Konfigurationsänderungen vorgenommen haben, müssen Sie einen vollständigen Synchronisierungszyklus ausführen (auch als Erstsynchronisierung bezeichnet):

- Sie haben weitere Objekte oder Attribute hinzugefügt, die aus einem Quellverzeichnis importiert werden müssen.
- Sie haben Änderungen an den Synchronisierungsregeln vorgenommen.
- Sie haben die [Filterung](active-directory-aadconnectsync-configure-filtering.md) geändert, sodass eine andere Anzahl von Objekten einbezogen werden soll.

Wenn Sie eine dieser Änderungen vorgenommen haben, müssen Sie einen vollständigen Synchronisierungszyklus ausführen, damit das Synchronisierungsmodul die Möglichkeit hat, die Connectorbereiche neu zu konsolidieren. Ein vollständiger Synchronisierungszyklus umfasst die folgenden Schritte:

- Vollständiger Import auf allen Connectors
- Vollständige Synchronisierung auf allen Connectors
- Export auf allen Connectors

Um einen vollständigen Synchronisierungszyklus zu initiieren, führen Sie an einer PowerShell-Eingabeaufforderung den Befehl `Start-ADSyncSyncCycle -PolicyType Initial` aus. Dadurch wird ein vollständiger Synchronisierungszyklus gestartet.

## <a name="stop-the-scheduler"></a>Beenden des Schedulers
Wenn der Scheduler gerade einen Synchronisierungszyklus ausführt, müssen Sie diesen beenden. Dies ist zum Beispiel der Fall, wenn Sie beim Starten des Installations-Assistenten den folgenden Fehler erhalten:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Während ein Synchronisierungszyklus ausgeführt wird, sind Änderungen an der Konfiguration nicht möglich. Sie können warten, bis der Scheduler den Prozess beendet hat. Sie können den Prozess aber auch beenden, um Ihre Änderungen sofort vorzunehmen. Das Beenden des aktuellen Zyklus ist ungefährlich, und noch nicht verarbeitete Änderungen werden bei der nächsten Ausführung verarbeitet.

1. Weisen Sie den Scheduler zunächst mit dem PowerShell-Cmdlet `Stop-ADSyncSyncCycle` an, den aktuellen Zyklus zu beenden.
2. Durch das Beenden des Schedulers wird der aktuelle Task des aktuellen Connectors nicht unterbrochen. Um ein Beenden des Connectors zu erzwingen, führen Sie folgende Aktionen aus: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png).
    - Starten Sie den **Synchronisierungsdienst** über das Startmenü. Wechseln Sie zu **Connectors**, markieren Sie den Connector mit dem Status **Wird ausgeführt**, und wählen Sie unter „Aktionen“ die Option **Beenden** aus.

Der Scheduler ist noch immer aktiv und wird bei der nächsten Gelegenheit wieder gestartet.

## <a name="custom-scheduler"></a>Benutzerdefinierter Scheduler
Die in diesem Abschnitt dokumentierten Cmdlets stehen nur in Build [1.1.130.0](active-directory-aadconnect-version-history.md#111300) und höher zur Verfügung.

Wenn der integrierte Scheduler Ihre Anforderungen nicht erfüllt, können Sie die Connectors mithilfe von PowerShell planen.

### <a name="invoke-adsyncrunprofile"></a>Invoke-ADSyncRunProfile
Sie können ein Profil für einen Connector auf folgende Weise starten:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Die verwendbaren [Connectornamen](active-directory-aadconnectsync-service-manager-ui-connectors.md) und [Ausführungsprofilnamen](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) finden Sie auf der [Synchronization Service Manager-Benutzeroberfläche](active-directory-aadconnectsync-service-manager-ui.md).

![Aufrufen des Ausführungsprofils](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

Das `Invoke-ADSyncRunProfile` -Cmdlet ist synchron, d.h., es gibt die Steuerung erst zurück, wenn der Connector den Vorgang erfolgreich oder mit Fehler abgeschlossen hat.

Es empfiehlt sich, die Connectors in der folgenden Reihenfolge zu planen:

1. Importieren aus lokalen Verzeichnissen wie beispielsweise Active Directory (vollständig oder als Delta)
2. Importieren aus Azure AD (vollständig oder als Delta)
3. Synchronisieren aus lokalen Verzeichnissen wie beispielsweise Active Directory (vollständig oder als Delta)
4. Synchronisieren aus Azure AD (vollständig oder als Delta)
5. Exportieren in Azure AD
6. Exportieren in lokale Verzeichnisse wie beispielsweise Active Directory

Wenn Sie den integrierten Scheduler betrachten, in dies die Reihenfolge, in der die Connectors ausgeführt werden.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Sie können das Synchronisierungsmodul auch überwachen, um zu ermitteln, ob es beschäftigt ist oder im Leerlauf befindet. Das Cmdlet gibt ein leeres Ergebnis zurück, wenn sich das Synchronisierungsmodul im Leerlauf befindet und keinen Connector ausführt. Wenn ein Connector ausgeführt wird, gibt das Cmdlet den Namen des Connectors zurück.

```
Get-ADSyncConnectorRunStatus
```

![Status der Connectorausführung](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
 In der obigen Abbildung weist die erste Zeile darauf hin, dass sich das Synchronisierungsmodul im Leerlauf befindet. Die zweite Zeile bedeutet, dass der Azure AD-Connector ausgeführt wird.

## <a name="scheduler-and-installation-wizard"></a>Scheduler und Installations-Assistent
Wenn Sie den Installations-Assistenten starten, wird der Scheduler vorübergehend unterbrochen. Der Grund dafür ist die Annahme, dass Sie Konfigurationsänderungen vornehmen möchten, und diese können nicht angewendet werden, solange das Synchronisierungsmodul aktiv ausgeführt wird. Lassen Sie daher den Installations-Assistenten nicht geöffnet, da er alle Synchronisierungsaktionen auf dem Synchronisierungsmodul verhindert.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md) .

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).



<!--HONumber=Oct16_HO2-->


