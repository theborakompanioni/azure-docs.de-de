---
title: "Azure AD Connect: Versionsveröffentlichungsverlauf | Microsoft Docs"
description: "In diesem Thema werden alle Versionen von Azure AD Connect und Azure AD Sync aufgeführt."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: ef2797d7-d440-4a9a-a648-db32ad137494
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 60f41890912eee3be364f5b3a47ded71991ee342
ms.openlocfilehash: 780fb6d69dab0b27e13f1e358be048f834dfc185


---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Versionsveröffentlichungsverlauf
Das Azure Active Directory-Team aktualisiert Azure AD Connect regelmäßig mit neuen Features und Funktionen. Nicht alle Erweiterungen gelten für alle Benutzergruppen.

Dieser Artikel soll Ihnen helfen, die Versionen zu verfolgen, die veröffentlicht wurden, und zu wissen, ob Sie auf die neueste Version aktualisieren müssen oder nicht.

Liste der verwandten Themen:


Thema |  
--------- | --------- |
Schritte zum Upgrade von Azure AD Connect | Verschiedene Methoden zum [Aktualisieren von einer früheren Version auf die aktuelle Version](active-directory-aadconnect-upgrade-previous-version.md) von Azure AD Connect.
Erforderliche Berechtigungen | Informationen zu den zum Anwenden eines Updates erforderlichen Berechtigungen finden Sie unter [Konten und Berechtigungen](./active-directory-aadconnect-accounts-permissions.md#upgrade).
Download| [Azure AD Connect herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="113800"></a>1.1.380.0
Veröffentlicht im Dezember 2016

**Behobenes Problem:**

* Das Problem der fehlenden Anspruchsregel issuerid für AD FS in diesem Build wurde behoben.

>[!NOTE]
>Dieser Build steht den Kunden nicht über das Feature zum automatischen Upgrade von Azure AD Connect zur Verfügung.

## <a name="113710"></a>1.1.371.0
Veröffentlicht im Dezember 2016

**Bekanntes Problem:**

* Die Anspruchsregel issuerid für AD FS fehlt in diesem Build. Die Anspruchsregel issuerid ist erforderlich, wenn Sie mehrere Domänen mit Azure AD in einem Verbund zusammenfassen. Wenn Sie Azure AD Connect zum Verwalten Ihrer lokalen AD FS-Bereitstellung verwenden, wird durch ein Upgrade auf diesen Build die vorhandene Anspruchsregel issuerid aus Ihrer AD FS-Konfiguration entfernt. Sie können dieses Problem umgehen, indem Sie die Anspruchsregel issuerid nach der Installation bzw. dem Upgrade hinzufügen. Informationen zum Hinzufügen der Anspruchsregel issuerid finden Sie in diesem Artikel unter [Unterstützung mehrerer Domänen für den Verbund mit Azure AD](active-directory-aadconnect-multiple-domains.md).

**Behobenes Problem:**

* Bei der Installation oder Aktualisierung von Azure AD Connect tritt ein Fehler auf, wenn Port 9090 nicht für ausgehende Verbindungen geöffnet ist.

>[!NOTE]
>Dieser Build steht den Kunden nicht über das Feature zum automatischen Upgrade von Azure AD Connect zur Verfügung.

## <a name="113700"></a>1.1.370.0
Veröffentlicht im Dezember 2016

**Bekannte Probleme:**

* Die Anspruchsregel issuerid für AD FS fehlt in diesem Build. Die Anspruchsregel issuerid ist erforderlich, wenn Sie mehrere Domänen mit Azure AD in einem Verbund zusammenfassen. Wenn Sie Azure AD Connect zum Verwalten Ihrer lokalen AD FS-Bereitstellung verwenden, wird durch ein Upgrade auf diesen Build die vorhandene Anspruchsregel issuerid aus Ihrer AD FS-Konfiguration entfernt. Sie können dieses Problem umgehen, indem Sie die Anspruchsregel issuerid nach der Installation bzw. dem Upgrade hinzufügen. Informationen zum Hinzufügen der Anspruchsregel issuerid finden Sie in diesem Artikel unter [Unterstützung mehrerer Domänen für den Verbund mit Azure AD](active-directory-aadconnect-multiple-domains.md).
* Port 9090 muss für ausgehende Verbindungen geöffnet werden, um die Installation abzuschließen.

**Neue Features:**

* Passthrough-Authentifizierung (Vorschau)

>[!NOTE]
>Dieser Build steht den Kunden nicht über das Feature zum automatischen Upgrade von Azure AD Connect zur Verfügung.

## <a name="113430"></a>1.1.343.0
Veröffentlicht im November 2016

**Bekanntes Problem:**

* Die Anspruchsregel issuerid für AD FS fehlt in diesem Build. Die Anspruchsregel issuerid ist erforderlich, wenn Sie mehrere Domänen mit Azure AD in einem Verbund zusammenfassen. Wenn Sie Azure AD Connect zum Verwalten Ihrer lokalen AD FS-Bereitstellung verwenden, wird durch ein Upgrade auf diesen Build die vorhandene Anspruchsregel issuerid aus Ihrer AD FS-Konfiguration entfernt. Sie können dieses Problem umgehen, indem Sie die Anspruchsregel issuerid nach der Installation bzw. dem Upgrade hinzufügen. Informationen zum Hinzufügen der Anspruchsregel issuerid finden Sie in diesem Artikel unter [Unterstützung mehrerer Domänen für den Verbund mit Azure AD](active-directory-aadconnect-multiple-domains.md).

**Behobene Probleme:**

* Manchmal tritt beim Installieren von Azure AD Connect ein Fehler auf, da kein lokales Dienstkonto erstellt werden kann, dessen Kennwort die von der Kennwortrichtlinie der Organisation angegebene Komplexitätsanforderungen erfüllt.
* Behebung eines Fehlers, bei dem Verknüpfungsregeln nicht neu ausgewertet werden, wenn sich ein Objekt im Connectorbereich gleichzeitig für eine Verknüpfungsregel im Bereich und für eine andere Regel außerhalb des Bereichs befindet. Dies kann geschehen, wenn Sie über zwei oder mehr Verknüpfungsregeln verfügen, deren Verknüpfungsbedingungen sich gegenseitig ausschließen.
* Behebung eines Problems, bei dem Synchronisierungsregeln für eingehenden Datenverkehr (von Azure AD), die keine Verknüpfungsregeln enthalten, nicht verarbeitet werden, wenn sie niedrigere Rangfolgenwerte aufweisen als Regeln mit Verknüpfungsregeln.

**Verbesserungen:**

* Unterstützung für die Installation von Azure AD Connect unter Windows Server 2016 Standard oder höher wurde hinzugefügt.
* Unterstützung für die Verwendung von SQL Server 2016 als Remotedatenbank für Azure AD Connect wurde hinzugefügt.

## <a name="112810"></a>1.1.281.0
Veröffentlicht im August 2016

**Behobene Probleme:**

* Änderungen am Synchronisierungsintervall werden erst nach Abschluss des nächsten Synchronisierungszyklus wirksam.
* Der Azure AD Connect-Assistent akzeptiert keine Azure AD-Konten, deren Benutzernamen mit einem Unterstrich (\_) beginnen.
* Der Azure AD Connect-Assistent kann das angegebene Azure AD-Konto nicht authentifizieren, wenn das Kontokennwort zu viele Sonderzeichen enthält. Die Fehlermeldung „Die Anmeldeinformationen konnten nicht gefunden werden. Ein unerwarteter Fehler ist aufgetreten.“ wird zurückgegeben.
* Durch Deinstallieren des Stagingservers wird die Kennwortsynchronisierung im Azure AD-Mandanten deaktiviert, sodass bei der Kennwortsynchronisierung mit dem aktiven Server ein Fehler auftritt.
* Die Kennwortsynchronisierung ist in seltenen Fällen nicht erfolgreich, wenn für den Benutzer kein Kennworthash gespeichert ist.
* Wenn der Azure AD Connect-Server für den Stagingmodus aktiviert wird, wird das Kennwortrückschreiben nicht vorübergehend deaktiviert.
* Wenn sich der Server im Stagingmodus befindet, zeigt der Azure AD Connect-Assistent nicht die tatsächliche Konfiguration von Kennwortsynchronisierung und Kennwortrückschreiben. Die Konfiguration wird immer als deaktiviert angezeigt.
* Wenn sich der Server im Stagingmodus befindet, werden Änderungen an der Konfiguration von Kennwortsynchronisierung und Kennwortrückschreiben vom Azure AD Connect-Assistenten nicht beibehalten.

**Verbesserungen:**

* Das Cmdlet „Start-ADSyncSyncCycle“ wurde aktualisiert und gibt nun an, ob ein neuer Synchronisierungszyklus gestartet werden kann.
* Mit dem hinzugefügten Cmdlet „Stop-ADSyncSyncCycle“ können der derzeit ausgeführte Synchronisierungszyklus und -vorgang beendet werden.
* Mit dem hinzugefügten Cmdlet „Stop-ADSyncScheduler“ können der derzeit ausgeführte Synchronisierungszyklus und -vorgang beendet werden.
* Im Azure AD Connect-Assistenten kann nun beim Konfigurieren von [Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md) das AD-Attribut vom Typ „Teletex-Zeichenfolge“ ausgewählt werden.

## <a name="111890"></a>1.1.189.0
Veröffentlicht im Juni 2016

**Behobene Probleme und Verbesserungen:**

* Azure AD Connect kann jetzt auf einem FIPS-kompatiblen Server installiert werden.
  * Informationen zur Kennwortsynchronisierung finden Sie unter [Kennwortsynchronisierung und FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
* Ein Problem, bei dem ein NetBIOS-Name im FQDN im Active Directory Connector nicht aufgelöst werden konnte, wurde behoben.

## <a name="111800"></a>1.1.180.0
Veröffentlicht im Mai 2016

**Neue Features:**

* Warnungen und Unterstützung beim Überprüfen von Domänen, wenn Sie in Azure AD Connect noch keine Überprüfung durchgeführt haben.
* Unterstützung für [Microsoft Cloud Deutschland](active-directory-aadconnect-instances.md#microsoft-cloud-germany)wurde hinzugefügt.
* Unterstützung für die neueste Infrastruktur der [Microsoft Azure Government-Cloud](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) mit neuen URL-Anforderungen wurde hinzugefügt.

**Behobene Probleme und Verbesserungen:**

* Eine neue Filterfunktion im Editor für Synchronisierungsregeln erleichtert das Auffinden von Synchronisierungsregeln.
* Verbesserte Leistung beim Löschen eines Connectorbereichs.
* Beseitigung eines Problems, bei dem in einem Vorgang dasselbe Objekt sowohl gelöscht als auch hinzugefügt wurde (Löschen/Hinzufügen).
* Eine deaktivierte Synchronisierungsregel führt bei einem Upgrade oder einer Aktualisierung des Verzeichnisschemas nicht mehr dazu, dass eingeschlossene Objekte und Attribute erneut aktiviert werden.

## <a name="111300"></a>1.1.130.0
Veröffentlicht im April 2016

**Neue Features:**

* Unterstützung für mehrwertige Attribute in [Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md)wurde hinzugefügt.
* Unterstützung für weitere Konfigurationsvarianten für das [automatische Upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) wurde hinzugefügt, damit diese für ein Upgrade infrage kommen.
* Einige Cmdlets für den [benutzerdefinierten Scheduler](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)wurden hinzugefügt.

## <a name="111190"></a>1.1.119.0
Veröffentlicht März 2016

**Behobene Probleme:**

* Es wurde sichergestellt, dass die Express-Installation unter Windows Server 2008 (vor R2) nicht verwendet werden kann, da die Kennwortsynchronisierung unter diesem Betriebssystem nicht unterstützt wird.
* Upgrade von DirSync mit einer benutzerdefinierten Filterkonfiguration funktionierte nicht wie erwartet.
* Beim Upgrade auf eine neuere Version ohne Änderungen an der Konfiguration sollte kein vollständiger Import-/Synchronisierungsvorgang geplant werden.

## <a name="111100"></a>1.1.110.0
Veröffentlicht im Februar 2016

**Behobene Probleme:**

* Ein Upgrade von früheren Versionen kann nicht ausgeführt werden, wenn sich die Installation nicht im Standardordner **C:\Programme** befindet.
* Wenn Sie bei der Installation am Ende des Installations-Assistenten die Option zum **Starten des Synchronisierungsvorgangs** deaktivieren, wird der Scheduler durch erneutes Ausführen des Installations-Assistenten nicht aktiviert.
* Auf Servern, auf denen das Datums-/Uhrzeitformat nicht dem US-englischen Format entspricht, wird der Planer nicht wie erwartet ausgeführt. Außerdem wird verhindert, dass `Get-ADSyncScheduler` korrekte Zeitangaben zurückgibt.
* Wenn Sie eine frühere Version von Azure AD Connect mit AD FS als Anmeldeoption und Upgrade installiert haben, können Sie den Installations-Assistenten nicht erneut ausführen.

## <a name="111050"></a>1.1.105.0
Veröffentlicht im Februar 2016

**Neue Features:**

* [Automatic upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) für Kunden mit Expresseinstellungen.
* Unterstützung für den globalen Administrator mit MFA und PIM im Installations-Assistenten.
  * Bei Verwendung von MFA muss Ihr Proxy Datenverkehr über „https://secure.aadcdn.microsoftonline-p.com“ zulassen.
  * Außerdem müssen Sie „https://secure.aadcdn.microsoftonline-p.com“ in Ihre Liste vertrauenswürdiger Websites aufnehmen, um eine einwandfreie Funktionsweise von MFA sicherzustellen.
* Das Ändern der Anmeldemethode des Benutzers nach der Erstinstallation ist zulässig.
* Lassen Sie im Installations-Assistenten das [Filtern von Domänen und Organisationseinheiten](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) zu. Dadurch wird außerdem das Herstellen einer Verbindung mit Gesamtstrukturen ermöglicht, in denen nicht alle Domänen verfügbar sind.
* [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) ist in das Synchronisierungsmodul integriert.

**Features, die von der Vorschau auf die allgemeine Verfügbarkeit hochgestuft wurden:**

* [Geräterückschreiben](active-directory-aadconnect-feature-device-writeback.md).
* [Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md).

**Neue Vorschaufeatures:**

* Das neue Standardintervall für den Synchronisierungszyklus beträgt 30 Minuten. In allen früheren Versionen wurde ein Intervall von 3 Stunden verwendet. Unterstützung zum Ändern des [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) -Verhaltens wurde hinzugefügt.

**Behobene Probleme:**

* Von der Seite zum Überprüfen von DNS-Domänen wurden die Domänen nicht immer erkannt.
* Domänen-Admin-Anmeldeinformationen werden beim Konfigurieren von AD FS abgefragt.
* Die lokalen AD-Konten werden vom Installations-Assistenten nicht erkannt, wenn sie sich in einer Domäne befinden, die eine andere DNS-Struktur als die Stammdomäne aufweist.

## <a name="1091310"></a>1.0.9131.0
Veröffentlicht im Dezember 2015

**Behobene Probleme:**

* Möglicherweise funktioniert die Kennwortsynchronisierung in AD DS nicht beim Ändern von Kennwörtern, jedoch beim Festlegen eines Kennworts.
* Mit einem Proxy-Server schlägt die Azure AD-Authentifizierung während der Installation möglicherweise fehl oder die Aktualisierung der Konfigurationsseite wird aufgehoben.
* Das Aktualisieren einer vorherigen Version von Azure AD Connect mit einem vollständigen SQL Server schlägt fehl, wenn Sie in SQL nicht SA sind.
* Beim Aktualisieren einer vorherigen Version von Azure AD Connect über einen Remotecomputer mit SQL Server wird die Fehlermeldung "Kein Zugriff auf die ADSync SQL-Datenbank." angezeigt.

## <a name="1091250"></a>1.0.9125.0
Veröffentlicht im November 2015

**Neue Features:**

* Es ist möglich, die Vertrauensstellung zwischen AD FS und Azure AD neu zu konfigurieren.
* Das Active Directory-Schema kann aktualisiert und die Synchronisierungsregeln können neu generiert werden.
* Synchronisierungsregeln können deaktiviert werden.
* In einer Synchronisierungsregel kann „AuthoritativeNull“ als neues Literal definiert werden.

**Neue Vorschaufeatures:**

* [Azure AD Connect Health für die Synchronisierung](../connect-health/active-directory-aadconnect-health-sync.md)
* Unterstützung für die Kennwortsynchronisierung mit [Azure AD-Domänendiensten](../active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)

**Neues unterstütztes Szenario:**

* Es werden mehrere lokale Exchange-Organisationen unterstützt. Weitere Informationen finden Sie unter [Hybrid-Bereitstellungen mit mehreren Active Directory-Gesamtstrukturen](https://technet.microsoft.com/library/jj873754.aspx) .

**Behobene Probleme:**

* Probleme bei der Kennwortsynchronisierung:
  * Für ein Objekt, das sich bisher außerhalb des Bereichs befand und das dann in den Bereich verschoben wurde, wird das Kennwort nicht synchronisiert. Dies schließt die Organisationseinheit und die Attributfilterung ein.
  * Wenn eine neue Organisationseinheit in die Synchronisierung einbezogen wird, ist keine vollständige Kennwortsynchronisierung erforderlich.
  * Nach dem Aktivieren eines bisher deaktivierten Benutzers wird das Kennwort nicht synchronisiert.
  * Für die Warteschlange mit Kennwortwiederholungsversuchen gilt kein Limit, der bisherige Grenzwert von 5.000 Objekten bis zum Außerkraftsetzen wurde entfernt.
  * [Verbesserte Problembehandlung](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshooting-password-synchronization).
* Mit der Gesamtstrukturfunktionsebene von Windows Server 2016 kann keine Verbindung mit Active Directory hergestellt werden.
* Nach der Erstinstallation kann die für das gruppenspezifische Filtern verwendete Gruppe nicht geändert werden.
* Wenn Benutzer, für die das Kennwortrückschreiben aktiviert ist, ihr Kennwort ändern, wird auf dem Azure AD Connect-Server kein neues Benutzerprofil mehr angelegt.
* In den Geltungsbereichen der Synchronisierungsregeln können keine langen Ganzzahlen als Werte verwendet werden.
* Das Kontrollkästchen „Geräterückschreiben“ bleibt deaktiviert, wenn nicht erreichbare Domänencontroller vorhanden sind.

## <a name="1086670"></a>1.0.8667.0
Veröffentlicht im August 2015

**Neue Features:**

* Der Azure AD Connect-Installations-Assistent ist jetzt für alle Windows Server-Sprachen lokalisiert.
* Erweiterte Unterstützung für die Kontoentsperrung bei Verwendung der Azure AD-Kennwortverwaltung.

**Behobene Probleme:**

* Azure AD-Connect-Installations-Assistent stürzt ab, wenn ein anderer Benutzer als die Person, die die Installation gestartet hat, die Installation fortsetzt.
* Wenn eine frühere Deinstallation von Azure AD Connect die Azure AD Connect-Synchronisierung nicht sauber deinstalliert, ist keine Neuinstallation möglich.
* Azure AD Connect kann nicht per Expressinstallation installiert werden, wenn der Benutzer nicht zur Stammdomäne der Gesamtstruktur gehört, oder wenn eine nichtenglische Version von Active Directory verwendet wird.
* Wenn der FQDN des Active Directory-Benutzerkontos nicht aufgelöst werden kann, wird eine irreführende Fehlermeldung "Schreiben des Schemas fehlgeschlagen" angezeigt.
* Wenn das auf dem Active Directory Connector verwendete Konto außerhalb des Assistenten geändert wird, schlägt die Ausführung des Assistenten bei nachfolgenden Versuchen fehl.
* Die Installation von Azure AD Connect auf einem Domänencontroller schlägt manchmal fehl.
* Aktivieren und Deaktivieren des "Stagingmodus" ist nicht möglich, wenn Erweiterungsattribute hinzugefügt wurden.
* Das Rückschreiben von Kennwörtern schlägt in einigen Konfigurationen aufgrund eines falschen Kennworts für den Active Directory Connector fehl.
* DirSync kann nicht aktualisiert werden, wenn DN bei der Attributfilterung verwendet wird.
* Beim Verwenden der Kennwortzurücksetzung wird die CPU übermäßig ausgelastet.

**Entfernte Vorschaufunktionen:**

* Die Vorschaufunktion [Benutzerrückschreiben](active-directory-aadconnect-feature-preview.md#user-writeback) wurde aufgrund von Rückmeldungen unserer Vorschaukunden vorübergehend entfernt. Sie wird später erneut hinzugefügt werden, wenn das bereitgestellte Feedback umgesetzt wurde.

## <a name="1086410"></a>1.0.8641.0
Veröffentlicht im Juni 2015

**Erste Version von Azure AD Connect.**

Änderung des Namens von Azure AD Sync zu Azure AD Connect.

**Neue Features:**

* Installation mit [Express-Einstellungen](active-directory-aadconnect-get-started-express.md)
* [ADFS konfigurieren](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) möglich
* [Upgrade von DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) möglich
* [Verhindern von versehentlichen Löschungen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
* Neuer [Stagingmodus](active-directory-aadconnectsync-operations.md#staging-mode)

**Neue Vorschaufeatures:**

* [Rückschreiben von Benutzern](active-directory-aadconnect-feature-preview.md#user-writeback)
* [Gruppenrückschreiben](active-directory-aadconnect-feature-preview.md#group-writeback)
* [Geräterückschreiben](active-directory-aadconnect-feature-device-writeback.md)
* [Verzeichniserweiterungen](active-directory-aadconnect-feature-preview.md)

## <a name="104940501"></a>1.0.494.0501
Veröffentlicht im Mai 2015

**Neue Voraussetzung:**

* Azure AD Sync erfordert jetzt die Installation von .Net Framework-Version 4.5.1.

**Behobene Probleme:**

* Das Rückschreiben von Kennwörtern von Azure AD schlägt mit einem Servicebus-Konnektivitätsfehler fehl.

## <a name="104910413"></a>1.0.491.0413
Veröffentlicht im April 2015

**Behobene Probleme und Verbesserungen:**

* Der Active Directory Connector verarbeitet Löschvorgänge nicht ordnungsgemäß, wenn der Papierkorb aktiviert ist, und mehrere Domänen in der Gesamtstruktur vorhanden sind.
* Die Leistung bei Importvorgängen wurde für den Azure Active Directory Connector verbessert.
* Wenn eine Gruppe die Mitgliedergrenze überschritten hatte (standardmäßig ist der Grenzwert auf 50.000 Objekte festgelegt), wurde die Gruppe in Azure Active Directory gelöscht. Jetzt bleibt die Gruppe erhalten, es wird ein Fehler ausgelöst, und es werden keine neuen Mitgliedschaftsänderungen exportiert.
* Ein neues Objekt kann nicht bereitgestellt werden, wenn eine gestaffelte Löschung mit dem gleichen DN bereits im Connectorbereich vorhanden ist.
* Einige Objekte werden für die Synchronisierung während einer Deltasynchronisierung markiert, obwohl keine Änderung für das Objekt bereitgestellt ist.
* Bei Erzwingen einer Kennwortsynchronisierung wird auch die Liste der bevorzugten Domänencontroller entfernt.
* CSExportAnalyzer hat Probleme mit einigen Objektstatus.

**Neue Features:**

* Mit einer Verknüpfung ist jetzt eine Verbindung zum Objekttyp "ANY" im Metaverse möglich.

## <a name="104850222"></a>1.0.485.0222
Veröffentlicht im Februar 2015

**Verbesserungen:**

* Verbesserte Importleistung.

**Behobene Probleme:**

* Die Kennwortsynchronisierung berücksichtigt das zur Attributfilterung verwendete Attribut cloudFiltered. Gefilterte Objekte fallen nicht mehr in den Bereich der Synchronisierung von Kennwörtern.
* In seltenen Fällen, in denen die Topologie sehr viele Domänencontroller aufweist, funktioniert die Synchronisierung von Kennwörtern nicht.
* "Stopped-server" beim Importieren vom Azure AD-Connector in die Geräteverwaltung wurde in Azure AD/Intune aktiviert.
* Verknüpfen der fremden Sicherheitsprinzipale (FSPs) aus mehreren Domänen in der gleichen Gesamtstruktur verursacht den Fehler mehrdeutiger Verknüpfungen.

## <a name="104751202"></a>1.0.475.1202
Veröffentlicht im Dezember 2014

**Neue Features:**

* Die Synchronisierung von Kennwörtern mit attributbasierter Filterung wird jetzt unterstützt. Weitere Informationen finden Sie unter [Synchronisierung von Kennwörtern mit Filterung](active-directory-aadconnectsync-configure-filtering.md).
* Das Attribut msDS-ExternalDirectoryObjectID wird nach AD zurückgeschrieben. Damit wird Unterstützung für Office 365-Anwendungen hinzugefügt, die in einer Hybrid-Exchange-Bereitstellung mithilfe von OAuth2 sowohl auf Online- als auch lokale Postfächer zugreifen.

**Behobene Upgrade-Probleme:**

* Eine neuere Version des Anmelde-Assistenten ist auf dem Server verfügbar.
* Ein benutzerdefinierter Installationspfad wurde verwendet, um Azure AD Sync zu installieren.
* Ein ungültiges benutzerdefiniertes Verknüpfungskriterium blockiert das Upgrade.

**Weitere Problembehebungen:**

* Probleme mit den Vorlagen für Office Pro Plus wurden behoben.
* Installationsprobleme, die durch Benutzernamen verursacht wurden, die mit einem Gedankenstrich beginnen, wurden behoben.
* Das Problem, dass die Einstellung sourceAnchor bei der zweiten Ausführung des Installations-Assistenten verloren geht, wurde behoben.
* Probleme mit der ETW zur Kennwortsynchronisierung wurden behoben.

## <a name="104701023"></a>1.0.470.1023
Veröffentlicht im Oktober 2014

**Neue Features:**

* Synchronisierung von Kennwörtern von mehreren lokalen AD-Instanzen mit Azure AD.
* Lokalisierte Benutzeroberfläche für die Installation für alle Windows Server-Sprachen.

**Upgrade von AADSync 1.0 GA**

Wenn Sie bereits Azure AD Sync installiert haben, müssen Sie einen zusätzlichen Schritt ausführen, falls Sie eine der Out-of-Box-Synchronisierungsregeln geändert haben. Nach dem Upgrade auf die Version 1.0.470.1023 sind die Synchronisierungsregeln, die Sie geändert haben, dupliziert. Führen Sie für jede geänderte Synchronisierungsregel die folgenden Schritte aus:

* Suchen Sie die Synchronisierungsregel, die Sie geändert haben, und notieren Sie sich die Änderungen.
* Löschen Sie die Synchronisierungsregel.
* Suchen Sie die neue, von Azure AD Sync erstellte Synchronisierungsregel, und wenden Sie die Änderungen erneut an.

**Berechtigungen für das AD-Konto**

Dem AD-Konto müssen zusätzliche Berechtigungen gewährt werden, um die Kennworthashes aus AD zu lesen. Die zu gewährenden Berechtigungen heißen "Verzeichnisänderungen replizieren" und "Alle Verzeichnisänderungen replizieren". Beide Berechtigungen sind zum Lesen von Kennworthashes erforderlich.

## <a name="104190911"></a>1.0.419.0911
Veröffentlicht im September 2014

**Erste Version von Azure AD Sync**

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).




<!--HONumber=Jan17_HO1-->


