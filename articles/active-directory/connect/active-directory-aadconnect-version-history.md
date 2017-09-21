---
title: "Azure AD Connect: Versionsveröffentlichungsverlauf | Microsoft-Dokumentation"
description: "In diesem Artikel sind alle Versionen von Azure AD Connect und Azure AD Sync aufgeführt."
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
ms.date: 08/30/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 895b538680230170cd29817997a7739b1ba89cfc
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Versionsveröffentlichungsverlauf
Das Azure Active Directory-Team (Azure AD) aktualisiert Azure AD Connect regelmäßig mit neuen Features und Funktionen. Nicht alle Erweiterungen gelten für alle Benutzergruppen.

Dieser Artikel soll Ihnen helfen, die Versionen zu verfolgen, die veröffentlicht wurden, und zu wissen, ob Sie auf die neueste Version aktualisieren müssen oder nicht.

Liste der verwandten Themen:


Thema |  Details
--------- | --------- |
Schritte zum Upgrade von Azure AD Connect | Verschiedene Methoden zum [Aktualisieren von einer früheren Version auf die aktuelle Version](active-directory-aadconnect-upgrade-previous-version.md) von Azure AD Connect.
Erforderliche Berechtigungen | Informationen zu den zum Anwenden eines Updates erforderlichen Berechtigungen finden Sie unter [Konten und Berechtigungen](./active-directory-aadconnect-accounts-permissions.md#upgrade).
Download| [Azure AD Connect herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="116140"></a>1.1.614.0
Status: 5. September 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="known-issues"></a>Bekannte Probleme
* Es besteht ein bekanntes Problem mit dem Azure AD Connect-Upgrade, das Kunden betrifft, die [Nahtloses einmaliges Anmelden](active-directory-aadconnect-sso.md) aktiviert haben. Nach dem Aktualisieren von Azure AD Connect wird das Feature im Assistenten als deaktiviert angezeigt, obwohl das Feature aktiviert bleibt. In einem zukünftigen Release wird dieses Problem behoben. Kunden, die wg. dieses Anzeigeproblems besorgt sind, können es manuell beheben, indem sie „Nahtloses einmaliges Anmelden“ im Assistenten aktivieren.

#### <a name="fixed-issues"></a>Behobene Probleme
* Das Problem wurde behoben, aufgrund dessen verhindert wurde, dass Azure AD Connect die Anspruchsregeln im lokalen AD FS aktualisiert, wenn die Funktion [msDS-ConsistencyGuid as Source Anchor](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) aktiviert wird. Dieses Problem tritt auf, wenn Sie versuchen, die Funktion für eine vorhandene Azure AD Connect-Bereitstellung zu aktivieren, bei der AD FS als Anmeldemethode konfiguriert ist. Die Ursache des Problems liegt darin, dass der Assistent nicht zur Eingabe der AD FS-Anmeldeinformationen auffordert, bevor versucht wird, die Anspruchsregeln in AD FS zu aktualisieren.
* Das Problem wurde behoben, aufgrund dessen bei der Azure AD Connect-Installation ein Fehler auftrat, wenn in der lokalen AD-Gesamtstruktur NTLM deaktiviert war. Ursache des Problems ist, dass der Azure AD Connect-Assistent bei der Erstellung der für die Kerberos-Authentifizierung erforderlichen Sicherheitskontexte nicht die vollqualifizierten Anmeldeinformationen angibt. Dies bewirkt, dass bei der Kerberos-Authentifizierung ein Fehler auftritt, und der Azure AD Connect-Assistent auf die Verwendung von NTLM zurückgeht.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect-Synchronisierung
#### <a name="fixed-issues"></a>Behobene Probleme
* Das Problem wurde behoben, dass die neue Synchronisierungsregel nicht erstellt werden kann, wenn das Tag-Attribut nicht gefüllt wird.
* Das Problem wurde behoben, aufgrund dessen Azure AD Connect zur Kennwortsynchronisierung eine Verbindung mit dem lokalen AD unter Verwendung von NTLM herstellte, obwohl Kerberos verfügbar war. Dieses Problem tritt auf, wenn die lokale AD-Topologie über mindestens einen Domänencontroller verfügt, der aus einer Sicherung wiederhergestellt wurde.
* Das Problem wurde behoben, aufgrund dessen die Schritte der vollständigen Synchronisierung unnötigerweise nach dem Upgrade auftraten. Im Allgemeinen ist die Ausführung der Schritte der vollständigen Synchronisierung nach dem Upgrade erforderlich, wenn Änderungen der vordefinierten Synchronisierungsregeln vorliegen. Das Problem trat aufgrund eines Fehlers im Änderungserkennungscode auf, der fälschlicherweise eine Änderung erkannte, wenn er auf einen Synchronisierungsregelausdruck traf, der Zeilenumbruchzeichen enthielt. Zeilenumbruchzeichen werden zur besseren Lesbarkeit in Synchronisierungsregelausdrücke eingefügt.
* Das Problem wurde behoben, aufgrund dessen die Azure AD Connect-Serverinstanz nach dem automatischen Upgrade möglicherweise nicht richtig funktioniert. Dieses Problem betrifft Azure AD Connect-Serverinstanzen mit Version 1.1.443.0 (oder früher). Weitere Informationen zu diesem Problem finden Sie im Artikel [Azure AD Connect is not working correctly after an automatic upgrade](https://support.microsoft.com/help/4038479/azure-ad-connect-is-not-working-correctly-after-an-automatic-upgrade) (Azure AD Connect funktioniert nach einem automatischen Upgrade nicht ordnungsgemäß).
* Das Problem wurde behoben, aufgrund dessen die Ausführung des automatischen Upgrades möglicherweise alle 5 Minuten neu versucht wird, wenn Fehler auftreten. Mit der Behebung wird die Ausführung des automatischen Upgrades mit exponentiellem Backoff neu versucht, wenn Fehler auftreten.
* Ein Problem wurde behoben, bei dem das Kennwortsynchronisierungsereignis 611 im Windows-Anwendungsereignisprotokoll fälschlicherweise als **Informativ** anstatt als **Fehler** angezeigt wurde. Ereignis 611 wird generiert, wenn bei der Kennwortsynchronisierung ein Problem festgestellt wird. 
* Ein Problem im Azure AD Connect-Assistenten wurde behoben, das die Aktivierung der Gruppenrückschreibfunktion ohne Auswahl einer für das Gruppenrückschreiben erforderlichen Organisationseinheit ermöglichte.

#### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen
* Eine Problembehandlungsaufgabe wurde dem Azure AD Connect-Assistenten unter „Weitere Aufgaben“ hinzugefügt. Kunden können diese Aufgabe zum Behandeln von Problemen im Zusammenhang mit der Synchronisierung von Kennwörtern und dem Erfassen der allgemeinen Diagnose nutzen. In der Zukunft wird die Aufgabe zur Problembehandlung erweitert, sodass sie auch andere, mit der Verzeichnissynchronisierung verbundene Probleme umfasst.
* Azure AD Connect unterstützt jetzt einen neuen, als **Vorhandene Datenbank verwenden** bezeichneten Installationsmodus. Dieser Installationsmodus ermöglicht Kunden, Azure AD Connect zu installieren, das eine vorhandene ADSync-Datenbank angibt. Weitere Informationen zu diesem Feature finden Sie im Artikel [Install Azure AD Connect using an existing ADSync database](active-directory-aadconnect-existing-database.md) (Installieren von Azure AD Connect mithilfe einer vorhandenen ADSync-Datenbank).
* Zur Sicherheitsverbesserung verwendet Azure AD Connect jetzt standardmäßig TLS 1.2 für die Herstellung der Verbindung mit Azure AD für die Verzeichnissynchronisierung. Bisher war der Standard TLS1.0.
* Wenn der Kennwortsynchronisierungs-Agent von Azure AD Connect startet, versucht er, eine Verbindung mit dem bekannten Azure AD-Endpunkt für die Synchronisierung von Kennwörtern herzustellen. Bei erfolgreicher Verbindung wird er zu einem regionsspezifischen Endpunkt umgeleitet. Vorher speicherte der Kennwortsynchronisierungs-Agent den regionsspezifischen Endpunkt zwischen, bis er neu gestartet wurde. Jetzt löscht der Agent den Cache und unternimmt einen erneuten Versuch mit dem bekannten Endpunkt, falls er ein Verbindungsproblem bei dem regionsspezifischen Endpunkt erkennt. Diese Änderung stellt sicher, dass die Synchronisierung von Kennwörtern ein Failover zu einem anderen regionsspezifischen Endpunkt ausführen kann, wenn der zwischengespeicherte regionsspezifische Endpunkt nicht mehr verfügbar ist.
* Zum Synchronisieren der Änderungen von einer lokalen AD-Gesamtstruktur ist ein AD DS-Konto erforderlich. Sie können entweder (i) das AD DS-Konto selbst erstellen und Azure AD Connect die Anmeldeinformationen angeben, oder (ii) eine Enterprise-Administratoranmeldeinfo eingeben und Azure AD Connect das AD DS-Konto für Sie erstellen lassen. Früher war (i) die Standardoption im Azure AD Connect-Assistenten. Jetzt ist (ii) die Standardoption.

### <a name="azure-ad-connect-health"></a>Azure AD Connect Health

#### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen
* Unterstützung für Microsoft Azure Government-Cloud und Microsoft Cloud Deutschland wurde hinzugefügt.

### <a name="ad-fs-management"></a>AD FS-Verwaltung
#### <a name="fixed-issues"></a>Behobene Probleme
* Das Initialize-ADSyncNGCKeysWriteBack-Cmdlet im AD-Vorbereitungs-Powershell-Modul wandte fälschlicherweise die Zugriffsteuerungsliste auf den Geräteregistrierungscontainer an und erbte daher nur vorhandene Berechtigungen.  Dies wurde aktualisiert, sodass das Synchronisierungsdienstkonto über die richtigen Berechtigungen verfügt.

#### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen
* Die AAD-Verbindungsüberprüfungs-AD FS-Anmelde-Aufgabe wurde aktualisiert, sodass sie Anmeldungen anhand von Microsoft Online überprüft und nicht einfach einen Tokenabruf in AD FS durchführt.
* Beim Einrichten einer neuen AD FS-Farm mit AAD Connect wurde die Seite, die ADFS-Anmeldeinformationen anfordert, verschoben, sodass sie jetzt angezeigt wird, bevor der Benutzer aufgefordert wird, den AD FS- und WAP-Server anzugeben.  So kann AAD Connect überprüfen, ob das angegebene Konto über die richtigen Berechtigungen verfügt.
* Während des AAD Connect-Upgrades tritt kein Upgradefehler mehr auf, wenn ein Fehler beim Update der AD FS-AAD-Vertrauensstellung auftritt.  Wenn dies der Fall ist, erhält der Benutzer eine entsprechende Warnmeldung, und sollte fortfahren, indem er die Vertrauensstellung über die zusätzliche AAD Connect-Aufgabe zurücksetzt.

### <a name="seamless-single-sign-on"></a>Nahtloses einmaliges Anmelden
#### <a name="fixed-issues"></a>Behobene Probleme
* Ein Problem wurde behoben, aufgrund dessen der Azure AD Connect-Assistent zu einem Fehler zurückkehrte, wenn Sie versuchten, das [Nahtlose einmalige Anmelden](active-directory-aadconnect-sso.md) zu aktivieren. Die Fehlermeldung lautet *„Fehler bei Konfiguration des Microsoft Azure AD Connect-Authentifizierungs-Agent“*. Dieses Problem betrifft bestehende Kunden, die manuell die Vorschauversion der Authentifizierung-Agents für [Passthrough-Authentifizierung](active-directory-aadconnect-sso.md) anhand der in diesem [Artikel](active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md) beschriebenen Schritte aktualisiert hatten.


## <a name="115610"></a>1.1.561.0
Status: 23. Juli 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Behobenes Problem

* Es wurde ein Problem behoben, das die Out-of-Box-Synchronisierungsregel „Ausgehend nach AD - User ImmutableId“ entfernt hat:

  * Dieses Problem tritt auf, wenn Azure AD Connect aktualisiert wird oder wenn die Registerkartenoption *Update Synchronization Configuration* (Synchronisierungskonfiguration aktualisieren) im Assistenten von Azure AD Connect verwendet wird, um die Synchronisierungskonfiguration für Azure AD Connect zu aktualisieren.
  
  * Diese Synchronisierungsregel gilt für Kunden, die [msDS-ConsistencyGuid as Source Anchor feature (msDS-ConsistencyGuid als Feature für den Quellanker)](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor) aktiviert haben. Dieses Feature wurde mit Version 1.1.524.0 und höher eingeführt. Wenn diese Synchronisierungsregel entfernt wird, kann Azure AD Connect das lokale Attribut „AD ms-DS-ConsistencyGuid“ nicht mehr mit dem Attributwert „ObjectGuid“ füllen. Es verhindert nicht, dass neue Benutzer in Azure AD bereitgestellt werden.
  
  * Diese Lösung gewährleistet, dass die Synchronisierungsregel während des Upgrades oder einer Konfigurationsänderung nicht mehr entfernt wird, sofern das Feature aktiviert ist. Für bestehende Kunden, die von diesem Problem betroffen waren, gewährleistet die Lösung ebenfalls, dass die Synchronisierungsregel nach der Aktualisierung auf diese Version von Azure AD Connect wieder hinzugefügt wird.

* Es wurde ein Problem behoben, bei dem Out-of-Box-Synchronisierungsregeln einen Rangfolgenwert von weniger als 100 besitzen:

  * Die Rangfolgenwerte von 0 bis 99 sind im Allgemeinen für benutzerdefinierte Synchronisierungsregeln reserviert. Während des Upgrades werden die Rangfolgenwerte für die Out-of-Box-Synchronisierungsregeln aktualisiert, um Änderungen an den Synchronisierungsregeln zu ermöglichen. Aufgrund des Problems konnte Out-of-Box-Synchronisierungsregeln ein Rangfolgenwert von weniger als 100 zugewiesen werden.
  
  * Die Lösung verhindert, dass das Problem während des Upgrades auftritt. Der Rangfolgenwert für bestehende Kunden, die von diesem Problem betroffen waren, wird jedoch nicht wiederhergestellt. Eine separate Lösung zur Unterstützung bei der Wiederherstellung wird in Zukunft noch bereitgestellt.

* Es wurde ein Problem behoben, bei dem der [Domain and OU Filtering screen (Bildschirm zum Filtern von Domänen und Organisationseinheiten (OEs))](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) im Assistenten für Azure AD Connect die Option *Sync all domains and OUs* (Alle Domänen und Organisationseinheiten synchronisieren) als ausgewählt anzeigt, obwohl das OE-basierte Filtern aktiviert ist.

*   Es wurde ein Problem behoben, bei dem der [Configure Directory Partitions screen (Bildschirm zum Konfigurieren von Verzeichnispartitionen)](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) im Synchronization Service Manager einen Fehler zurückgibt, wenn die Schaltfläche *Refresh* (Erneuern) angeklickt wird. Die Fehlermeldung lautet: *“An error was encountered while refreshing domains: Unable to cast object of type ‘System.Collections.ArrayList’ to type ‘Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject.”* („Beim Aktualisieren der Domänen ist ein Fehler aufgetreten: Objekt des Typs ‚System.Collections.ArrayList‘ konnte nicht in den Typ ‚Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject‘ umgewandelt werden.“) Dieser Fehler tritt auf, wenn eine neue AD-Domäne zu einer bestehenden AD-Gesamtstruktur hinzugefügt wurde und Sie versuchen, Azure AD Connect mit der Schaltfläche „Erneuern“ zu aktualisieren.

#### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Das [Automatic Upgrade feature (Feature für automatische Upgrades)](active-directory-aadconnect-feature-automatic-upgrade.md) wurde erweitert, um Kunden zu unterstützen, die folgende Konfigurationen verwenden:
  * Sie haben das Feature „Geräterückschreiben“ aktiviert.
  * Sie haben das Feature „Gruppenrückschreiben“ aktiviert.
  * Die Installation ist keine Express-Einstellung und kein DirSync-Upgrade.
  * Es sind mehr als 100.000 Objekte im Metaverse enthalten.
  * Sie stellen Verbindungen mit mehr als einer Gesamtstruktur her. Beim Express-Setup wird nur eine Verbindung mit einer Gesamtstruktur hergestellt.
  * Das AD-Connector-Konto ist nicht mehr das Standardkonto vom Typ „MSOL_“.
  * Der Server ist auf den Stagingmodus festgelegt.
  * Sie haben das Feature „Benutzerrückschreiben“ aktiviert.
  
  >[!NOTE]
  >Die Bereichserweiterung des Features für automatische Upgrades betrifft Kunden mit Azure AD Connect Build 1.1.105.0 und höher. Wenn Sie nicht möchten, dass Ihr Azure AD Connect-Server automatisch aktualisiert wird, müssen Sie auf diesem folgendes Cmdlet ausführen: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Weitere Informationen zum Aktivieren und Deaktivieren von automatischen Upgrades finden Sie im Artikel [Azure AD Connect: Automatic upgrade (Azure AD Connect: Automatisches Upgrade)](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115580"></a>1.1.558.0
Status: Wird nicht veröffentlicht werden. Änderungen in diesem Build sind in Version 1.1.561.0 enthalten.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Behobenes Problem

* Es wurde ein Problem behoben, das die Out-of-Box-Synchronisierungsregel „Ausgehend nach AD - User ImmutableId“ entfernt hat, wenn die Konfiguration für das OE-basierte Filtern aktualisiert wird. Diese Synchronisierungsregel wird für [msDS-ConsistencyGuid as Source Anchor feature (msDS-ConsistencyGuid als Feature für den Quellanker)](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor) benötigt.

* Es wurde ein Problem behoben, bei dem der [Domain and OU Filtering screen (Bildschirm zum Filtern von Domänen und Organisationseinheiten (OEs))](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) im Assistenten für Azure AD Connect die Option *Sync all domains and OUs* (Alle Domänen und Organisationseinheiten synchronisieren) als ausgewählt anzeigt, obwohl das OE-basierte Filtern aktiviert ist.

*   Es wurde ein Problem behoben, bei dem der [Configure Directory Partitions screen (Bildschirm zum Konfigurieren von Verzeichnispartitionen)](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) im Synchronization Service Manager einen Fehler zurückgibt, wenn die Schaltfläche *Refresh* (Erneuern) angeklickt wird. Die Fehlermeldung lautet: *“An error was encountered while refreshing domains: Unable to cast object of type ‘System.Collections.ArrayList’ to type ‘Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject.”* („Beim Aktualisieren der Domänen ist ein Fehler aufgetreten: Objekt des Typs ‚System.Collections.ArrayList‘ konnte nicht in den Typ ‚Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject‘ umgewandelt werden.“) Dieser Fehler tritt auf, wenn eine neue AD-Domäne zu einer bestehenden AD-Gesamtstruktur hinzugefügt wurde und Sie versuchen, Azure AD Connect mit der Schaltfläche „Erneuern“ zu aktualisieren.

#### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Das [Automatic Upgrade feature (Feature für automatische Upgrades)](active-directory-aadconnect-feature-automatic-upgrade.md) wurde erweitert, um Kunden zu unterstützen, die folgende Konfigurationen verwenden:
  * Sie haben das Feature „Geräterückschreiben“ aktiviert.
  * Sie haben das Feature „Gruppenrückschreiben“ aktiviert.
  * Die Installation ist keine Express-Einstellung und kein DirSync-Upgrade.
  * Es sind mehr als 100.000 Objekte im Metaverse enthalten.
  * Sie stellen Verbindungen mit mehr als einer Gesamtstruktur her. Beim Express-Setup wird nur eine Verbindung mit einer Gesamtstruktur hergestellt.
  * Das AD-Connector-Konto ist nicht mehr das Standardkonto vom Typ „MSOL_“.
  * Der Server ist auf den Stagingmodus festgelegt.
  * Sie haben das Feature „Benutzerrückschreiben“ aktiviert.
  
  >[!NOTE]
  >Die Bereichserweiterung des Features für automatische Upgrades betrifft Kunden mit Azure AD Connect Build 1.1.105.0 und höher. Wenn Sie nicht möchten, dass Ihr Azure AD Connect-Server automatisch aktualisiert wird, müssen Sie auf diesem folgendes Cmdlet ausführen: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Weitere Informationen zum Aktivieren und Deaktivieren von automatischen Upgrades finden Sie im Artikel [Azure AD Connect: Automatic upgrade (Azure AD Connect: Automatisches Upgrade)](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115570"></a>1.1.557.0
Status: Juli 2017

>[!NOTE]
>Dieser Build steht Kunden nicht über das Feature zum automatischen Upgrade von Azure AD Connect zur Verfügung.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Behobenes Problem
* Es wurde ein Problem mit dem Cmdlet „Initialize-ADSyncDomainJoinedComputerSync“ behoben, durch das sich die überprüfte Domäne geändert hat, die auf dem bestehenden Dienstverbindungspunkt-Objekt konfiguriert wurde, obwohl die Domäne noch gültig war. Dieses Problem tritt auf, wenn Ihr Azure AD-Mandant über mehr als eine überprüfte Domäne verfügt, die für die Konfiguration des Dienstverbindungspunkts verwendet werden kann.

#### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen
* Das Rückschreiben von Kennwörtern ist nun für die Vorschauversion von Microsoft Azure Government-Cloud und Microsoft Cloud Germany verfügbar. Weitere Informationen über die Unterstützung der verschiedenen Dienstinstanzen in Azure AD Connect finden Sie im Artikel [Azure AD Connect: Special considerations for instances (Azure AD Connect: Besondere Überlegungen zu Instanzen)](active-directory-aadconnect-instances.md).

* Das Cmdlet „Initialize-ADSyncDomainJoinedComputerSync“ besitzt jetzt den neuen optionalen Parameter „AzureADDomain“. Durch diesen Parameter können Sie angeben, welche überprüfte Domäne zum Konfigurieren des Dienstverbindungspunkts verwendet werden soll.

### <a name="pass-through-authentication"></a>Passthrough-Authentifizierung

#### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen
* Der Name des Agenten, der für die Passthrough-Authentifizierung benötigt wird, wurde von *Microsoft Azure AD Application Proxy Connector (Microsoft Azure AD-Anwendungsproxyconnector)* zu *Microsoft Azure AD Connect Authentication Agent (Microsoft Azure AD Connect-Authentifizierungs-Agent)* geändert.

* Das Aktivieren der Passthrough-Authentifizierung aktiviert nicht mehr standardmäßig die Kennworthashsynchronisierung.


## <a name="115530"></a>1.1.553.0
Status: Juni 2017

> [!IMPORTANT]
> Bei diesem Build wurden Änderungen an Schemas und Synchronisierungsregeln vorgenommen. Der Azure AD Connect-Synchronisierungsdienst löst nach dem Upgrade die Schritte „Vollständiger Import“ und „Vollständige Synchronisierung“ aus. Details zu den Änderungen werden nachfolgend beschrieben. Informationen zum vorübergehenden Verzögern der Schritte „Vollständiger Import“ und „Vollständige Synchronisierung“ finden Sie im Artikel [How to defer full synchronization after upgrade (Vorgehensweise: Verzögern der vollständigen Synchronisierung nach einem Upgrade)](active-directory-aadconnect-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade).
>
>

### <a name="azure-ad-connect-sync"></a>Azure AD Connect-Synchronisierung

#### <a name="known-issue"></a>Bekannte Probleme
* Es besteht ein Problem, das Kunden betrifft, die [OU-based filtering (OE-basiertes Filtern)](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) mit der Azure AD Connect-Synchronisierung verwenden. Wenn Sie zu der [Domain and OU Filtering page (Seite zum Filtern von Domänen und Organisationseinheiten)](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) im Assistenten für Azure AD Connect navigieren, wird das folgende Verhalten erwartet:
  * Wenn OE-basiertes Filtern aktiviert ist, wird die Option **Sync selected domains and OUs** („Ausgewählte Domänen und Organisationseinheiten synchronisieren“) ausgewählt.
  * Andernfalls wird die Option **Sync all domains and OUs** („Alle Domänen und Organisationseinheiten synchronisieren“) ausgewählt.

Dabei entsteht das Problem, dass die **Sync all domains and OUs option** (Option „Alle Domänen und Organisationseinheiten synchronisieren“) immer ausgewählt ist, wenn Sie den Assistenten starten.  Dieses Problem tritt auch dann auf, wenn das OE-basierte Filtern zuvor konfiguriert wurde. Stellen Sie vor dem Speichern von Konfigurationsänderungen in AAD Connect sicher, dass die**Sync selected domains and OUs option is selected** (Option „Ausgewählte Domänen und Organisationseinheiten“ ausgewählt ist) und dass alle Organisationseinheiten, die synchronisiert werden müssen, wieder aktiviert sind. Andernfalls wird das OE-basierte Filtern deaktiviert.

#### <a name="fixed-issues"></a>Behobene Probleme

* Es wurde ein Problem mit dem Rückschreiben von Kennwörtern behoben, das es dem Azure AD-Administrator ermöglichte, das Kennwort eines privilegierten lokalen AD-Benutzerkontos zurückzusetzen. Das Problem tritt auf, wenn Azure AD Connect die Berechtigung „Kennwort zurücksetzen“ durch das privilegierte Konto gewährt wird. Das Problem wird in dieser Version von Azure AD Connect behoben, indem es einem Azure AD-Administrator nicht gestattet wird, das Kennwort eines lokalen privilegierten AD-Benutzerkontos zurückzusetzen, wenn der Administrator nicht der Besitzer dieses Kontos ist. Weitere Informationen finden Sie in der [Security Advisory 4033453 (Sicherheitsempfehlung 4033453)](https://technet.microsoft.com/library/security/4033453).

* Es wurde ein Problem mit dem Feature [msDS-ConsistencyGuid as Source Anchor (msDS-ConsistencyGuid als Quellanker)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) behoben, bei dem Azure AD Connect keine Rückschreibung auf das lokale Attribut „msDS-ConsistencyGuid“ durchführt. Das Problem tritt auf, wenn Azure AD Connect mehrere lokale AD-Gesamtstrukturen hinzugefügt wurden und die *User identities exist across multiple directories option* (Option „Benutzeridentitäten sind in mehreren Verzeichnissen vorhanden“) ausgewählt ist. Wenn diese Konfiguration verwendet wird, füllen die resultierenden Synchronisierungsregeln das Attribut „sourceAnchorBinary“ nicht in der Metaverse auf. Das Attribut „sourceAnchorBinary“ wird als Quellattribut für das Attribut „msDS-ConsistencyGuid“ verwendet. Dadurch wird keine Rückschreibung in das Attribut „msDS-ConsistencyGuid“ ausgeführt. Um das Problem zu beheben wurden die folgenden Synchronisierungsregeln aktualisiert, um zu gewährleisten, dass das Attribut „sourceAnchorBinary“ in der Metaverse immer aufgefüllt ist:
  * Eingehend aus AD: InetOrgPerson AccountEnabled.xml
  * Eingehend aus AD: InetOrgPerson Common.xml
  * Eingehend aus AD: User AccountEnabled.xml
  * Eingehend aus AD: User Common.xml
  * Eingehend aus AD: User Join SOAInAAD.xml

* Zuvor wurde die Synchronisierungsregel „Ausgehend nach AD: User ImmutableId“ dennoch zu Azure AD Connect hinzugefügt, auch wenn das Feature [msDS-ConsistencyGuid as Source Anchor (msDS-ConsistencyGuid als Quellanker)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) deaktiviert war. Die Auswirkungen sind unkritisch und verursachen lediglich, dass keine Rückschreibung auf das Attribut „msDS-ConsistencyGuid“ erfolgen kann. Es wurde zum Vermeiden von Verwirrung eine Logik hinzugefügt, um zu gewährleisten, dass die Synchronisierungsregel nur hinzugefügt wird, wenn das Feature aktiviert ist.

* Es wurde ein Problem behoben, bei dem die Kennworthashsynchronisierung mit dem Fehlerereignis 611 fehlgeschlagen ist. Dieses Problem tritt auf, wenn mindestens ein Domänencontroller aus dem lokalen AD entfernt wurde. Am Ende jedes Kennwortsynchronisierungszyklus enthält das Cookie zur Synchronisierung, das vom lokalen AD verwendet wird, die Aufruferkennungen der entfernten Domänencontroller mit einem USN-Wert (Updatesequenznummern-Wert) von 0. Der Manager für Kennwortsynchronisierung kann das Cookie zur Synchronisierung, das den USN-Wert von 0 enthält, nicht speichern und schlägt deshalb mit der Fehlermeldung 611 fehl. Während des nächsten Synchronisierungszyklus verwendet der Manager für Kennwortsynchronisierung das letzte gespeicherte Cookie zur Synchronisierung, das keinen USN-Wert von 0 enthält. Dadurch werden dieselben Kennwortänderungen erneut synchronisiert. Durch diese Lösung speichert der Manager für Kennwortsynchronisierung das Cookie zur Synchronisierung korrekt.

* Zuvor hat der Vorgang für automatische Upgrades weiterhin regelmäßig auf Upgrades überprüft, auch wenn automatische Upgrades durch das Cmdlet „Set-ADSyncAutoUpgrade“ deaktiviert wurden und die Berücksichtigung der Deaktivierung basierte auf dem heruntergeladenen Installationsprogramm. Durch diese Lösung überprüft der Vorgang für automatische Upgrades nicht mehr regelmäßig auf Upgrades. Die Lösung wird automatisch angewendet, wenn das Installationsprogramm für Upgrades für diese Version von Azure AD Connect einmal ausgeführt wurde.

#### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Zuvor war das Feature [msDS-ConsistencyGuid as Source Anchor (msDS-ConsistencyGuid als Quellanker)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) nur für neue Bereitstellungen verfügbar. Nun ist es auch für vorhandene Bereitstellungen verfügbar. Dies gilt insbesondere in folgenden Fällen:
  * Starten Sie den Assistenten für Azure AD Connect und klicken Sie die Option *Update Source Anchor* (Quellanker aktualisieren) an, um auf dieses Feature zuzugreifen.
  * Diese Option wird nur in bestehenden Bereitstellungen angezeigt, die „objectGuid“ als Attribut für den Quellanker verwenden.
  * Wenn Sie diese Option konfigurieren, überprüft der Assistent den Status des Attributs „msDS-ConsistencyGuid“ in Ihrem lokalen Active Directory. Wenn das Attribut in keinem Benutzerobjekt im Verzeichnis konfiguriert ist, verwendet der Assistent „msDS-ConsistencyGuid“ als „sourceAnchor“-Attribut. Wenn das Attribut in einem oder in mehreren Objekten oder Benutzern im Verzeichnis konfiguriert ist, folgert der Assistent daraus, dass das Attribut von anderen Anwendungen verwendet wird und nicht als „sourceAnchor“-Attribut infrage kommt. Deshalb gestattet der Assistent nicht, dass die Änderung des Quellankers fortgesetzt wird. Wenn Sie sicher sind, dass das Attribut nicht von vorhandenen Anwendungen verwendet wird, wenden Sie sich an den Support, um Informationen zum Unterdrücken der Fehlermeldung zu erhalten.

* Speziell für das Attribut**userCertificate** der Geräteobjekte sucht Azure AD Connect nun nach den Zertifikatwerten, die für das [Connecting domain-joined devices to Azure AD for Windows 10 experience (Verbinden von in Domänen eingebunden Geräten mit Azure AD für Windows 10)](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) benötigt werden und filtert den Rest heraus, bevor mit Azure AD synchronisiert wird. Um dieses Verhalten zu ermöglichen, wurde die Out-of-Box-Synchronisierungsregel „Ausgehend nach AAD: Gerät Join SOAInAD“ aktualisiert.

* Azure AD Connect unterstützt jetzt die Rückschreibung des Exchange Online-Attributs **cloudPublicDelegates** auf das lokale AD-Attribut **publicDelegates**. Dadurch wird einem Exchange Online-Postfach ermöglicht, dass Benutzern mit lokalem Exchange-Postfach SendOnBehalfTo-Rechte erteilt werden. Zur Unterstützung dieses Features wurde die neue Out-of-Box-Synchronisierungsregel „Ausgehend nach AD: User Exchange Hybrid PublicDelegates writeback“ hinzugefügt. Diese Synchronisierungsregel wird nur zu Azure AD Connect hinzugefügt, wenn das Feature „Exchange Hybrid“ aktiviert ist.

*   Azure AD Connect unterstützt nun die Synchronisierung des Attributs**altRecipient** von Azure AD. Um diese Änderung zu unterstützen wurden folgende Out-of-Box-Synchronisierungsregeln aktualisiert, um den erforderlichen Attributfluss einzuschließen:
  * Ein von AD – Benutzer Exchange
  * Ausgehend nach AAD: User ExchangeOnline
  
* Das Attribut**cloudSOAExchMailbox** in der Metaverse gibt an, ob ein bestimmter Benutzer über ein Exchange Online-Postfach verfügt oder nicht. Die Definition wurde aktualisiert, sodass zusätzlich „RecipientDisplayTypes“ für Exchange Online hinzugefügt wurde und dadurch auch Postfächer für Anlagen und Konferenzräume. Um diese Änderungen vorzunehmen wurde die Definition des Attributs „cloudSOAExchMailbox“, das unter der Out-of-Box-Synchronisierungsregel „Eingehend von AAD: User Exchange Hybrid“ gefunden werden kann, von Folgendem aktualisiert:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

...auf das Folgende:

```
CBool(
  IIF(IsPresent([cloudMSExchRecipientDisplayType]),(
    IIF([cloudMSExchRecipientDisplayType]=0,True,(
      IIF([cloudMSExchRecipientDisplayType]=2,True,(
        IIF([cloudMSExchRecipientDisplayType]=7,True,(
          IIF([cloudMSExchRecipientDisplayType]=8,True,(
            IIF([cloudMSExchRecipientDisplayType]=10,True,(
              IIF([cloudMSExchRecipientDisplayType]=16,True,(
                IIF([cloudMSExchRecipientDisplayType]=17,True,(
                  IIF([cloudMSExchRecipientDisplayType]=18,True,(
                    IIF([cloudMSExchRecipientDisplayType]=1073741824,True,(
                       IF([cloudMSExchRecipientDisplayType]=1073741840,True,False)))))))))))))))))))),False))

```

* Der folgende Satz X509Certificate2-kompatibler Funktionen wurde hinzugefügt, damit Ausdrücke für Synchronisierungsregeln erstellt werden können, um die Zertifikatwerte im Attribut „userCertificate“ zu verarbeiten:

    ||||
    | --- | --- | --- |
    |CertSubject|CertIssuer|CertKeyAlgorithm|
    |CertSubjectNameDN|CertIssuerOid|CertNameInfo|
    |CertSubjectNameOid|CertIssuerDN|IsCert|
    |CertFriendlyName|CertThumbprint|CertExtensionOids|
    |CertFormat|CertNotAfter|CertPublicKeyOid|
    |CertSerialNumber|CertNotBefore|CertPublicKeyParametersOid|
    |CertVersion|CertSignatureAlgorithmOid|Auswahl|
    |CertKeyAlgorithmParams|CertHashString|Hierbei gilt:|
    |||With|

* Folgende Schemaänderungen wurden eingeführt, um es Kunden zu ermöglichen, benutzerdefinierte Synchronisierungsregeln zu erstellen, um „sAMAccountName“, „domainNetBios“ und „domainFQDN“ an Gruppenobjekte und „distinguishedName“ an Benutzerobjekte zu übergeben:

  * Folgende Attribute wurden dem MV-Schema hinzugefügt:
    * Gruppe: AccountName
    * Gruppe: domainNetBios
    * Gruppe: domainFQDN
    * Person: distinguishedName

  * Folgende Attribute wurden dem Azure AD-Connector-Schema hinzugefügt:
    * Gruppe: OnPremisesSamAccountName
    * Gruppe: NetBiosName
    * Gruppe: DnsDomainName
    * Benutzer: OnPremisesDistinguishedName

* Das Cmdlet-Skript „ADSyncDomainJoinedComputerSync“ besitzt jetzt den neuen optionalen Parameter „AzureEnvironment“. Der Parameter wird verwendet, um anzugeben, in welcher Region der entsprechende Azure Active Directory-Mandant gehostet wird. Folgende Werte sind gültig:
  * AzureCloud (Standard)
  * AzureChinaCloud
  * AzureGermanyCloud
  * USGovernment
 
* Der aktualisierte Synchronisierungsregel-Editor, um „Join“ (statt „Provision“) als Standardwert für den Verknüpfungstyp während der Erstellung von Synchronisierungsregeln zu verwenden.

### <a name="ad-fs-management"></a>AD FS-Verwaltung

#### <a name="issues-fixed"></a>Behobene Fehler

* Folgende URLs sind neue Endpunkte für den WS-Verbund und werden von Azure AD eingeführt, um die Resilienz gegen Authentifizierungsausfälle zu verbessern. Sie werden zur lokalen AD FS-Konfiguration für die Vertrauensstellung der vertrauenden Seite hinzugefügt:
  * https://ests.login.microsoftonline.com/login.srf
  * https://stamp2.login.microsoftonline.com/login.srf
  * https://ccs.login.microsoftonline.com/login.srf
  * https://ccs-sdf.login.microsoftonline.com/login.srf
  
* Es wurde ein Fehler behoben, bei dem AD FS einen falschen Anspruchswert für IssuerID generiert hat. Das Problem tritt auf, wenn mehrere überprüfte Domänen im Azure Ad-Mandanten vorhanden sind und die Domänenendung des Attributs „userPrincipalName“, das verwendet wird, um den Anspruch „IssuerID“ zu generieren, mindestens drei Ebenen tief ist (beispielsweise johndoe@us.contoso.com). Das Problem wird behoben, indem der reguläre Ausdruck aktualisiert wird, der von den Anspruchsregeln verwendet wird.

#### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen
* Zuvor konnte das Feature „ADFS-Zertifikatverwaltung“, das von Azure AD Connect bereitgestellt wird, nur mit ADFS-Farmen verwendet werden, die über Azure AD Connect verwaltet werden. Nun können Sie das Feature auch mit ADFS-Farmen verwenden, die nicht mithilfe von Azure AD Connect verwaltet werden.

## <a name="115240"></a>1.1.524.0
Veröffentlichung: Mai 2017

> [!IMPORTANT]
> Bei diesem Build wurden Änderungen an Schemas und Synchronisierungsregeln vorgenommen. Der Azure AD Connect-Synchronisierungsdienst löst nach dem Upgrade die Schritte „Vollständiger Import“ und „Vollständige Synchronisierung“ aus. Details zu den Änderungen werden nachfolgend beschrieben.
>
>

**Behobene Probleme:**

Azure AD Connect-Synchronisierung

* Das Problem, dass das automatische Upgrade auf dem Azure AD Connect-Server ausgeführt wird, selbst wenn der Kunde die Funktion mit dem Cmdlet „Set-ADSyncAutoUpgrade“ deaktiviert hat, wurde behoben. Durch diese Fehlerbehebung wird im Rahmen des Vorgangs zum automatischen Upgrade auf dem Server weiterhin in regelmäßigen Abständen nach Upgrades gesucht, das heruntergeladene Installationsprogramm berücksichtigt jedoch die Konfiguration des automatischen Upgrades.
* Während des direkten Upgrades von DirSync erstellt Azure AD Connect ein Azure AD-Dienstkonto, das vom Azure AD-Connector zur Synchronisierung mit Azure AD verwendet werden soll. Nachdem das Konto erstellt wurde, wird Azure AD Connect anhand des Kontos bei Azure AD authentifiziert. In einigen Fällen schlägt die Authentifizierung aufgrund vorübergehender Probleme fehl, wodurch wiederum das direkte Upgrade von DirSync mit folgender Fehlermeldung fehlschlägt: *„Bei der Ausführung der Aufgabe AADSTS50034 zur Konfiguration der AAD-Synchronisierung ist ein Fehler aufgetreten: Um sich bei dieser Anwendung anzumelden, muss das Konto zum Verzeichnis „xxx.onmicrosoft.com“ hinzugefügt werden.“* Um die Stabilität beim Upgrade von DirSync zu verbessern, wiederholt Azure AD Connect nun den Authentifizierungsschritt.
* Bei Build 443 ist ein Problem aufgetreten, bei dem das direkte Upgrade von DirSync zwar erfolgreich ausgeführt wurde, jedoch die erforderlichen Ausführungsprofile für die Verzeichnissynchronisierung nicht erstellt wurden. Dieser Build von Azure AD Connect enthält eine Logik zur Systemreparatur. Wenn Kunden ein Upgrade auf diesen Build durchführen, erkennt und erstellt Azure AD Connect fehlende Ausführungsprofile.
* Das Problem, dass der Vorgang zur Kennwortsynchronisierung aufgrund der Ereignis-ID 6900 und der Fehlermeldung *„Ein Element mit demselben Schlüssel wurde bereits hinzugefügt“* nicht gestartet werden kann, wurde behoben. Dieses Problem tritt auf, wenn Sie beim Aktualisieren der Konfiguration der OE-Filterung die AD-Konfigurationspartition einschließen. Zur Behebung dieses Problems werden beim Vorgang zur Kennwortsynchronisierung nur noch Kennwortänderungen bei AD-Domänenpartitionen synchronisiert. Partitionen, die nicht zu Domänen gehören (z.B. Konfigurationspartitionen), werden übersprungen.
* Während der Express-Installation erstellt Azure AD Connect ein lokales AD DS-Konto, das vom AD-Connector zur Kommunikation mit dem lokalen AD verwendet werden soll. Vorher wurde das Konto mit dem für das Benutzerkontosteuerungs-Attribut festgelegte Flag „PASSWD_NOTREQD“ erstellt, und ein zufälliges Kennwort wurde für das Konto festgelegt. Nun entfernt Azure AD Connect explizit das Flag „PASSWD_NOTREQD“, nachdem das Kennwort im Konto festgelegt wurde.
* Das Problem, dass das Upgrade von DirSync mit der Fehlermeldung *„Deadlock beim Versuch, eine Anwendungssperre abzurufen“*, wenn das Attribut „mailNickname“ im lokalen AD-Schema gefunden, aber nicht an die Objektklasse „AD-Benutzer“ gebunden wird, wurde behoben.
* Das Problem, dass die Funktion zum Geräterückschreiben automatisch deaktiviert wird, wenn ein Administrator die Konfiguration der Azure AD Connect-Synchronisierung mithilfe des Azure AD Connect-Assistenten aktualisiert, wurde behoben. Dieses Problem tritt auf, wenn der Assistent die Voraussetzungen für die vorhandene Konfiguration des Geräterückschreibens im lokalen AD durchführt und die Überprüfung fehlschlägt. Das Problem lässt sich beheben, indem die Überprüfung übersprungen wird, wenn das Geräterückschreiben vorher bereits aktiviert wurde.
* Um die OE-Filterung zu konfigurieren, können Sie entweder den Azure AD Connect-Assistenten oder den Synchronization Service Manager verwenden. Wenn Sie vorher die OE-Filterung mithilfe des Azure AD Connect-Assistenten konfiguriert haben, wurden neue erstellte Organisationseinheiten (OEs) zur Verzeichnissynchronisierung im Anschluss eingeschlossen. Wenn neue Organisationseinheiten nicht eingeschlossen werden sollten, musste die OE-Filterung mithilfe des Synchronization Service Manager konfiguriert werden. Nun können Sie dasselbe Verhalten mit dem Azure AD Connect-Assistenten erzielen.
* Das Problem, dass gespeicherte, von Azure AD Connect erforderliche Verfahren anstelle des dbo-Schemas unter dem Schema des Administrators, der die Installation durchführt, erstellt werden, wurde behoben.
* Das Problem, dass das von Azure AD zurückgegebene Attribut „TrackingId“ in den AAD Connect-Serverereignisprotokollen ausgelassen wird, wurde behoben. Das Problem tritt auf, wenn Azure AD Connect eine Umleitungsmeldung von Azure AD erhält und keine Verbindung zum bereitgestellten Endpunkt herstellen kann. Das Attribut „TrackingId“ wird von Supporttechnikern zur Korrelation mit serverseitigen Protokollen bei der Problembehandlung verwendet.
* Wenn von Azure AD der Fehler „LargeObject“ an Azure AD Connect ausgegeben wird, generiert Azure AD Connect ein Ereignis mit der EventID 6941 und der Meldung *„Das bereitgestellte Objekt ist zu groß. Verringern Sie die Anzahl der Attributwerte für dieses Objekt.“*. Gleichzeitig generiert Azure AD Connect auch ein irreführendes Ereignis mit der EventID 6900 und der Meldung *„Microsoft.Online.Coexistence.ProvisionRetryException: Kommunikation mit dem Microsoft Azure Active Directory-Dienst nicht möglich.“* Um Verwirrungen zu vermeiden, generiert Azure AD Connect nicht mehr das letztere Ereignis, wenn die Fehlermeldung „LargeObject“ empfangen wird.
* Das Problem, dass der Synchronization Service Manager bei dem Versuch, die Konfiguration des generischen LDAP-Connectors zu aktualisieren, nicht mehr reagiert, wurde behoben.

**Neue Features/Verbesserungen:**

Azure AD Connect-Synchronisierung
* Änderungen an Synchronisierungsregeln – Folgende Änderungen an Synchronisierungsregeln wurden implementiert:
  * Der standardmäßige Synchronisierungsregelsatz, der festlegt, dass die Attribute **userCertificate** und **userSMIMECertificate** bei mehr als 15 Werten nicht exportiert werden, wurde aktualisiert.
  * Die AD-Attribute **employeeID** und **msExchBypassModerationLink** sind nun im standardmäßigen Synchronisierungsregelsatz enthalten.
  * Das AD-Attribut **photo** wurde aus dem standardmäßigen Synchronisierungsregelsatz entfernt.
  * Das Attribut **preferredDataLocation** wurde zum Metaverse- und AAD-Connectorschema hinzugefügt. Kunden, die beide Attribute in Azure AD aktualisieren möchten, können hierfür benutzerdefinierte Synchronisierungsregeln implementieren. Weitere Informationen zum Attribut finden Sie im Abschnitt [Azure AD Connect-Synchronisierung: Ändern der Standardkonfiguration – Aktivieren der Synchronisierung von PreferredDataLocation](active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-preferreddatalocation) des Artikels.
  * Das Attribut **userType** wurde zum Metaverse- und AAD-Connectorschema hinzugefügt. Kunden, die beide Attribute in Azure AD aktualisieren möchten, können hierfür benutzerdefinierte Synchronisierungsregeln implementieren.

* Azure AD Connect ermöglicht jetzt automatisch die Verwendung des Attributs „ConsistencyGuid“ als Quellankerattribut für lokale AD-Objekte. Weiterhin füllt Azure AD Connect das Attribut „ConsistencyGuid“ mit dem Wert des Attributs „objectGuid“ auf, wenn es leer ist. Diese Funktion gilt nur für neue Bereitstellungen. Weitere Informationen zu dieser Funktion finden Sie im Abschnitt [Azure AD Connect: Entwurfskonzepte – Verwendung von „msDS-ConsistencyGuid“ als „sourceAnchor“](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor) des Artikels.
* Um Probleme in Bezug auf die Kennworthashsynchronisierung zu diagnostizieren, wurde das neue Problembehandlungs-Cmdlet „Invoke-ADSyncDiagnostics“ hinzugefügt. Informationen zur Verwendung des Cmdlets finden Sie im Artikel [Troubleshoot password synchronization with Azure AD Connect sync (Problembehandlung: Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-synchronization).
* Azure AD Connect unterstützt nun die Synchronisierung von Objekten von E-Mail-aktivierten öffentlichen Ordnern aus dem lokalen AD mit Azure AD. Die Funktion kann mit dem Azure AD Connect-Assistenten unter „Optionale Funktionen“ aktiviert werden. Weitere Informationen zu diesem Feature finden Sie im Artikel [Office 365 Directory Based Edge Blocking support for on-premises Mail Enabled Public Folders (Unterstützung für verzeichnisbasierte Edge-Blockierung in Office 365 für lokale, E-Mail-aktivierte öffentliche Ordner)](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders).
* Azure AD Connect erfordert ein AD DS-Konto, um lokal von AD zu synchronisieren. Zuvor konnten Sie bei der Installation von Azure AD Connect im Express-Modus die Anmeldeinformationen eines Unternehmensadministratorkontos angeben, mit denen Azure AD Connect dann das erforderliche AD DS-Konto erstellt hat. Allerdings mussten Sie für eine benutzerdefinierte Installationen und beim Hinzufügen von Gesamtstrukturen zu einer vorhandenen Bereitstellung stattdessen das AD DS-Konto bereitstellen. Nun haben Sie die Möglichkeit, die Anmeldeinformationen für das Konto eines Unternehmensadministrators auch während einer benutzerdefinierten Installation bereitzustellen und Azure AD Connect die Erstellung des erforderlichen AD DS-Kontos zu überlassen.
* Azure AD Connect unterstützt nun SQL AOA. Vor der Installation von Azure AD Connect müssen Sie SQL AOA aktivieren. Während der Installation erkennt Azure AD Connect, ob die bereitgestellte SQL-Instanz für SQL AOA aktiviert ist. Wenn SQL AOA aktiviert ist, prüft Azure AD Connect darüber hinaus, ob SQL AOA für die Verwendung synchroner oder asynchroner Replikationen konfiguriert ist. Bei der Einrichtung des Verfügbarkeitsgruppenlisteners wird empfohlen, die Eigenschaft „RegisterAllProvidersIP“ auf 0 festzulegen. Dies liegt daran, weil Azure AD Connect Verbindungen zu SQL derzeit über den SQL Native Client herstellt und der SQL Native Client die Verwendung der Eigenschaft „MultiSubNetFailover“ nicht unterstützt.
* Wenn Sie „LocalDB“ als Datenbank für den Azure AD Connect-Server verwenden und das Größenlimit von 10 GB erreicht haben, wird der Synchronisierungsdienst nicht mehr gestartet. Vorher mussten Sie den Vorgang „ShrinkDatabase“ in LocalDB durchführen, um genügend Datenbankspeicherplatz zum Starten des Synchronisierungsdiensts freizugeben. Anschließend konnten Sie mithilfe des Synchronization Service Manager den Ausführungsverlauf löschen, um weiteren Datenbankspeicherplatz freizugeben. Nun können Sie das Cmdlet „Start-ADSyncPurgeRunHistory“ verwenden, um zur Freigabe von Datenbankspeicherplatz Daten zum Ausführungsverlauf aus LocalDB zu löschen. Dieses Cmdlet unterstützt darüber hinaus einen Offlinemodus (durch Festlegen des Parameters „-offline“), der verwendet werden kann, wenn der Synchronisierungsdienst nicht ausgeführt wird. Hinweis: Der Offlinemodus kann nur verwendet werden, wenn der Synchronisierungsdienst nicht ausgeführt und die Datenbank „LocalDB“ verwendet wird.
* Um den erforderlichen Speicherplatzbedarf zu reduzieren, komprimiert Azure AD Connect nun Details zu Synchronisierungsfehlern, bevor diese in LocalDB-/SQL-Datenbanken gespeichert werden. Bei der Durchführung eines Upgrades von einer älteren Azure AD Connect-Version auf diese Version führt Azure AD Connect eine einmalige Komprimierung von Details zu bestehenden Synchronisierungsfehlern durch.
* Vorher mussten Sie nach der Aktualisierung der OE-Filterung den Schritt „Vollständiger Import“ manuell ausführen, um sicherzustellen, dass vorhandene Objekte ordnungsgemäß bei aus der Verzeichnissynchronisierung ein- bzw. ausgeschlossen werden. Azure AD Connect löst den Schritt „Vollständiger Import“ nun automatisch während des nächsten Synchronisierungszyklus aus. Darüber hinaus wird „Vollständiger Import“ nur auf die von dem Update betroffenen AD-Connectors angewendet. Hinweis: Diese Verbesserung gilt nur für Updates zur OE-Filterung, die mit dem Azure AD Connect-Assistenten durchgeführt werden. Sie gilt nicht für Updates zur OE-Filterung, die mit dem Synchronization Service Manager durchgeführt wurden.
* Vorher unterstützte die gruppenbasierte Filterung nur Benutzer-, Gruppen- und Kontaktobjekte. Nun unterstützt die gruppenbasierte Filterung auch Computerobjekte.
* Vorher konnten Sie Daten aus dem Connectorbereich löschen, ohne den Azure AD Connect-Synchronisierungsplaner deaktivieren zu müssen. Nun blockiert der Synchronization Service Manager die Löschung von Daten aus dem Connectorbereich, wenn er erkennt, dass der Planer aktiviert ist. Darüber hinaus wird eine Warnung zurückgegeben, um Kunden über einen potenziellen Datenverlust zu informieren, wenn Daten aus dem Connectorbereich gelöscht werden.
* Vorher mussten Sie die PowerShell-Aufzeichnung deaktivieren, damit der Azure AD Connect-Assistent korrekt ausgeführt wird. Dieses Problem wurde teilweise behoben. Sie können die PowerShell-Aufzeichnung aktivieren, wenn Sie den Azure AD Connect-Assistenten zur Verwaltung der Synchronisierungskonfiguration verwenden. Sie müssen die PowerShell-Aufzeichnung deaktivieren, wenn Sie den Azure AD Connect-Assistenten zur Verwaltung der AD FS-Konfiguration verwenden.



## <a name="114860"></a>1.1.486.0
Veröffentlichung: April 2017

**Behobene Probleme:**
* Das Problem, dass Azure AD Connect in einer lokalisierten Version von Windows Server nicht erfolgreich installiert wird, wurde behoben.

## <a name="114840"></a>1.1.484.0
Veröffentlichung: April 2017

**Bekannte Probleme:**

* Diese Version von Azure AD Connect wird nicht erfolgreich installiert, wenn alle der folgenden Bedingungen zutreffen:
   1. Sie führen ein direktes Upgrade von DirSync oder eine Neuinstallation von Azure AD Connect aus.
   2. Sie verwenden eine lokalisierte Version von Windows Server, wobei der Name der integrierten Administratorgruppe auf dem Server nicht „Administratoren“ lautet.
   3. Sie verwenden die mit Azure AD Connect installierte SQL Server 2012 Express LocalDB-Standardinstanz, statt eine eigene vollständige Version von SQL Server bereitzustellen.

**Behobene Probleme:**

Azure AD Connect-Synchronisierung
* Das Problem, dass der Synchronisierungsplaner den gesamten Synchronisierungsschritt überspringt, wenn bei einem oder mehreren Connectors das Ausführungsprofil für diesen Synchronisierungsschritt fehlt, wurde behoben. Das Problem kann beispielsweise auftreten, wenn Sie mit Synchronization Service Manager manuell einen Connector hinzugefügt haben, ohne für diesen das Ausführungsprofil „Deltaimport“ zu erstellen. Durch diese Korrektur wird sichergestellt, dass der Synchronisierungsplaner „Deltaimport“ für andere Connectors weiterhin ausführt.
* Das Problem, dass der Synchronisierungsdienst die Verarbeitung eines Ausführungsprofils sofort beendet, wenn bei einem der Ausführungsschritte ein Problem auftritt, wurde behoben. Mit dieser Korrektur wird sichergestellt, dass der Synchronisierungsdienst diesen Ausführungsschritt überspringt und mit der Verarbeitung der restlichen Schritte fortfährt. Beispiel: Sie verfügen für den AD-Connector über das Ausführungsprofil „Deltaimport“ mit mehreren Ausführungsschritten (einer für jede lokale AD-Domäne). Der Synchronisierungsdienst führt „Deltaimport“ für die anderen AD-Domänen aus, selbst wenn bei einer von ihnen Netzwerkverbindungsprobleme auftreten.
* Das Problem, aufgrund dessen das Update des Azure AD-Connectors während des automatischen Upgrades übersprungen wird, wurde behoben.
* Das Problem, aufgrund dessen Azure AD Connect während des Setups falsch bestimmt, ob der Server ein Domänencontroller ist, sodass das DirSync-Upgrade fehlschlägt, wurde behoben.
* Das Problem, aufgrund dessen beim direkten DirSync-Upgrade kein Ausführungsprofil für den Azure AD-Connector erstellt wird, wurde behoben.
* Das Problem, dass beim Konfigurieren des generischen LDAP-Connectors die Benutzeroberfläche von Synchronization Service Manager nicht mehr reagiert, wurde behoben.

AD FS-Verwaltung
* Das Problem, dass der Azure AD Connect-Assistent nach dem Verschieben des primären AD FS-Knotens auf einen anderen Server fehlschlägt, wurde behoben.

Desktop-SSO
* Das Problem beim Azure AD Connect-Assistenten, dass Desktop-SSO im Anmeldebildschirm nicht aktiviert werden kann, wenn während der Neuinstallation „Kennwortsynchronisierung“ als Anmeldeoption gewählt wurde, wurde behoben.

**Neue Features/Verbesserungen:**

Azure AD Connect-Synchronisierung
* Die Azure AD Connect-Synchronisierung unterstützt jetzt die Verwendung von virtuellen Dienstkonten, verwalteten Dienstkonten und gruppenverwalteten Dienstkonten. Dies gilt nur für die Neuinstallation von Azure AD Connect. Beim Installieren von Azure AD Connect:
    * Der Azure AD Connect-Assistent erstellt standardmäßig ein virtuelles Dienstkonto und verwendet dieses als Dienstkonto.
    * Bei der Installation auf einem Domänencontroller greift Azure AD Connect auf das frühere Verhalten zurück. Das heißt, es wird ein Domänenbenutzerkonto erstellt, und dieses wird als Dienstkonto verwendet.
    * Sie können das Standardverhalten überschreiben, indem Sie eines der folgenden Konten angeben:
      * Ein gruppenverwaltetes Dienstkonto
      * Ein verwaltetes Dienstkonto
      * Ein Domänenbenutzerkonto
      * Ein lokales Benutzerkonto
* Wenn früher ein Upgrade auf einen neuen Build von Azure AD Connect mit Updateänderungen für Connectors oder mit Änderungen von Synchronisierungsregeln durchgeführt wurde, wurde von Azure AD Connect ein vollständiger Synchronisierungszyklus ausgelöst. Jetzt löst Azure AD Connect den Schritt „Vollständiger Import“ nur für Connectors mit Updateänderungen und den Schritt „Vollständige Synchronisierung“ nur für Connectors mit Änderungen von Synchronisierungsregeln aus.
* Früher galt der Exportlöschschwellenwert nur für Exporte, die über den Synchronisierungsplaner ausgelöst werden. Jetzt umfasst das Feature auch Exporte, die vom Kunden mithilfe von Synchronization Service Manager manuell ausgelöst werden.
* Eine Dienstkonfiguration im Azure AD-Mandanten gibt an, ob die Kennwortsynchronisierung für den Mandanten aktiviert ist. Früher konnte es leicht zu einer falschen Dienstkonfiguration durch Azure AD Connect kommen, wenn ein aktiver Server und ein Stagingserver vorhanden waren. Jetzt versucht Azure AD Connect, die Dienstkonfiguration nur entsprechend dem aktiven Azure AD Connect-Server vorzunehmen.
* Der Azure AD Connect-Assistent erkennt jetzt, wenn im lokalen AD der AD-Papierkorb nicht aktiviert ist, und gibt eine entsprechende Warnung aus.
* Früher trat beim Export nach Azure AD eine Zeitüberschreitung auf, und der Export schlug fehl, wenn die Gesamtgröße der Objekte im Batch einen bestimmten Schwellenwert überschritt. Jetzt versucht der Synchronisierungsdienst, die Objekte in einzelnen kleineren Batches erneut zu senden, wenn das Problem auftritt.
* Die Anwendung Synchronization Service Key Management wurde aus dem Windows-Startmenü entfernt. Die Verwaltung der Verschlüsselungsschlüssel wird weiterhin über die Befehlszeilenschnittstelle mithilfe von „miiskmu.exe“ unterstützt. Informationen zum Verwalten von Verschlüsselungsschlüsseln finden Sie im Artikel [Abandoning the Azure AD Connect Sync encryption key](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-azure-ad-connect-sync-encryption-key) (Verwerfen des Verschlüsselungsschlüssels der Azure AD Connect-Synchronisierung) (in englischer Sprache).
* Wenn Sie früher das Kennwort für das Dienstkonto der Azure AD Connect-Synchronisierung geändert heben, konnte der Synchronisierungsdienst nicht ordnungsgemäß gestartet werden, bis der Verschlüsselungsschlüssel verworfen und das Kennwort für das Dienstkonto der Azure AD Connect-Synchronisierung erneut initialisiert wurde. Dies ist jetzt nicht mehr erforderlich.

Desktop-SSO

* Für den Azure AD Connect-Assistenten muss beim Konfigurieren von Passthrough-Authentifizierung und Desktop-SSO nicht mehr Port 9090 im Netzwerk geöffnet werden. Es ist lediglich Port 443 erforderlich. 

## <a name="114430"></a>1.1.443.0
Veröffentlichung: März 2017

**Behobene Probleme:**

Azure AD Connect-Synchronisierung
* Ein Problem wurde behoben, durch das der Azure AD Connect-Assistent fehlschlägt, wenn der Anzeigename des Azure AD-Connectors nicht die anfängliche dem Azure AD-Mandanten zugewiesene onmicrosoft.com-Domäne enthält.
* Ein Problem wurde behoben, durch das der Azure AD Connect-Assistent beim Herstellen einer Verbindung mit der SQL-Datenbank fehlschlägt, wenn das Kennwort des Sync-Dienstkonto Sonderzeichen wie z. B. Apostrophe, Doppelpunkte und Leerzeichen enthält.
* Ein Problem wurde behoben, durch das der Fehler „The dimage has an anchor that is different than the image“ (Das Image weist einen Anker auf, der vom Image abweicht) auf einem Azure AD Connect-Server im Stagingmodus ausgegeben wird, wenn ein lokales AD-Objekt temporär vom Synchronisieren ausgeschlossen und dann wieder in die Synchronisierung eingeschlossen wird.
* Ein Problem wurde behoben, durch das der Fehler „The object located by DN is a phantom“ (Das Objekt in DN ist ein Phantom) auf einem Azure AD Connect-Server im Stagingmodus ausgegeben wird, wenn ein lokales AD-Objekt temporär vom Synchronisieren ausgeschlossen und dann wieder in die Synchronisierung eingeschlossen wird.

AD FS-Verwaltung
* Ein Problem wurde behoben, durch das der Azure AD Connect-Assistent die AD FS-Konfiguration nicht aktualisiert und nicht die richtigen Ansprüche auf die Vertrauensstellung der vertrauenden Seite festlegt, nachdem die alternative Anmelde-ID konfiguriert wurde.
* Ein Problem wurde behoben, durch das der Azure AD Connect-Assistent AD FS-Server nicht ordnungsgemäß verarbeiten kann, wenn deren Dienstkonten das Format „userPrincipalName“ statt „sAMAccountName“ verwenden.

Passthrough-Authentifizierung
* Ein Problem wurde behoben, durch das der Azure AD Connect-Assistent fehlschlägt, wenn die Pass-Through-Authentifizierung ausgewählt wird, aber die Registrierung des Connectors fehlschlägt.
* Ein Problem wurde behoben, durch das der Azure AD Connect-Assistent Überprüfungen der gewählten Anmeldemethode umgeht, wenn Desktop SSO aktiviert ist.

Kennwortzurücksetzung
* Es wurde ein Fehler behoben, der dazu führte, dass der Azure AD Connect-Server nicht versuchte, die Verbindung wiederherzustellen, wenn die Verbindung durch eine Firewall oder einen Proxy beendet wurde.

**Neue Features/Verbesserungen:**

Azure AD Connect-Synchronisierung
* Das Get-ADSyncScheduler-Cmdlet gibt jetzt eine neue boolesche Eigenschaft namens „SyncCycleInProgress“ zurück. Lautet der zurückgegebene Wert „true“, wird ein geplanter Synchronisierungszyklus bearbeitet.
* Der Zielordner zum Speichern der Installations- und Setupprotokolle von Azure AD Connect wurde von %localappdata%\AADConnect zu %programdata%\AADConnect verschoben, um den Zugriff auf die Protokolldateien zu vereinfachen.

AD FS-Verwaltung
* Die Aktualisierung des SSL-Zertifikats für AD FS-Farm wird jetzt unterstützt.
* Die Unterstützung für die Verwaltung von AD FS 2016 wird jetzt unterstützt.
* Sie können jetzt bei der Installation von AD FS vorhandene gruppenverwaltete Dienstkonten angeben.
* Sie können jetzt SHA-256 als Signaturhashalgorithmus für die Azure AD-Vertrauensstellung der vertrauenden Seite konfigurieren.

Kennwortzurücksetzung
* Es wurden Verbesserungen vorgenommen, damit das Produkt auch in Umgebungen mit strengeren Firewallregeln ausgeführt werden kann.
* Verbesserte Zuverlässigkeit der Verbindung mit Azure Service Bus

## <a name="113800"></a>1.1.380.0
Veröffentlichung: Dezember 2016

**Behobenes Problem:**

* Das Problem der fehlenden Anspruchsregel „issuerid“ für Active Directory-Verbunddienste (AD FS) in diesem Build wurde behoben.

>[!NOTE]
>Dieser Build steht Kunden nicht über das Feature zum automatischen Upgrade von Azure AD Connect zur Verfügung.

## <a name="113710"></a>1.1.371.0
Veröffentlichung: Dezember 2016

**Bekanntes Problem:**

* Die Anspruchsregel „issuerid“ für AD FS fehlt in diesem Build. Die Anspruchsregel „issuerid“ist erforderlich, wenn Sie mehrere Domänen mit Azure Active Directory (Azure AD) in einem Verbund zusammenfassen. Wenn Sie Azure AD Connect zum Verwalten Ihrer lokalen AD FS-Bereitstellung verwenden, wird durch ein Upgrade auf diesen Build die vorhandene Anspruchsregel „issuerid“ aus Ihrer AD FS-Konfiguration entfernt. Sie können dieses Problem umgehen, indem Sie die Anspruchsregel „issuerid“ nach der Installation bzw. dem Upgrade hinzufügen. Informationen zum Hinzufügen der Anspruchsregel „issuerid“ finden Sie in diesem Artikel unter [Unterstützung mehrerer Domänen für den Verbund mit Azure AD](active-directory-aadconnect-multiple-domains.md).

**Behobenes Problem:**

* Bei der Installation oder Aktualisierung von Azure AD Connect tritt ein Fehler auf, wenn Port 9090 nicht für ausgehende Verbindungen geöffnet ist.

>[!NOTE]
>Dieser Build steht Kunden nicht über das Feature zum automatischen Upgrade von Azure AD Connect zur Verfügung.

## <a name="113700"></a>1.1.370.0
Veröffentlichung: Dezember 2016

**Bekannte Probleme:**

* Die Anspruchsregel „issuerid“ für AD FS fehlt in diesem Build. Die Anspruchsregel issuerid ist erforderlich, wenn Sie mehrere Domänen mit Azure AD in einem Verbund zusammenfassen. Wenn Sie Azure AD Connect zum Verwalten Ihrer lokalen AD FS-Bereitstellung verwenden, wird durch ein Upgrade auf diesen Build die vorhandene Anspruchsregel „issuerid“ aus Ihrer AD FS-Konfiguration entfernt. Sie können dieses Problem umgehen, indem Sie die Anspruchsregel „issuerid“ nach der Installation bzw. dem Upgrade hinzufügen. Informationen zum Hinzufügen der Anspruchsregel issuerid finden Sie in diesem Artikel unter [Unterstützung mehrerer Domänen für den Verbund mit Azure AD](active-directory-aadconnect-multiple-domains.md).
* Port 9090 muss für ausgehende Verbindungen geöffnet werden, um die Installation abzuschließen.

**Neue Features:**

* Passthrough-Authentifizierung (Vorschau)

>[!NOTE]
>Dieser Build steht Kunden nicht über das Feature zum automatischen Upgrade von Azure AD Connect zur Verfügung.

## <a name="113430"></a>1.1.343.0
Veröffentlichung: November 2016

**Bekanntes Problem:**

* Die Anspruchsregel „issuerid“ für AD FS fehlt in diesem Build. Die Anspruchsregel issuerid ist erforderlich, wenn Sie mehrere Domänen mit Azure AD in einem Verbund zusammenfassen. Wenn Sie Azure AD Connect zum Verwalten Ihrer lokalen AD FS-Bereitstellung verwenden, wird durch ein Upgrade auf diesen Build die vorhandene Anspruchsregel „issuerid“ aus Ihrer AD FS-Konfiguration entfernt. Sie können dieses Problem umgehen, indem Sie die Anspruchsregel „issuerid“ nach der Installation bzw. dem Upgrade hinzufügen. Informationen zum Hinzufügen der Anspruchsregel issuerid finden Sie in diesem Artikel unter [Unterstützung mehrerer Domänen für den Verbund mit Azure AD](active-directory-aadconnect-multiple-domains.md).

**Behobene Probleme:**

* Manchmal tritt beim Installieren von Azure AD Connect ein Fehler auf, da kein lokales Dienstkonto erstellt werden kann, dessen Kennwort die von der Kennwortrichtlinie der Organisation angegebenen Komplexitätsanforderungen erfüllt.
* Behebung eines Fehlers, bei dem Verknüpfungsregeln nicht neu ausgewertet werden, wenn sich ein Objekt im Connectorbereich gleichzeitig für eine Verknüpfungsregel im Bereich und für eine andere Regel außerhalb des Bereichs befindet. Dies kann geschehen, wenn Sie über zwei oder mehr Verknüpfungsregeln verfügen, deren Verknüpfungsbedingungen sich gegenseitig ausschließen.
* Behebung eines Problems, bei dem Synchronisierungsregeln für eingehenden Datenverkehr (von Azure AD), die keine Verknüpfungsregeln enthalten, nicht verarbeitet werden, wenn sie niedrigere Rangfolgenwerte aufweisen als Regeln mit Verknüpfungsregeln.

**Verbesserungen:**

* Unterstützung für die Installation von Azure AD Connect unter Windows Server 2016 Standard oder höher wurde hinzugefügt.
* Unterstützung für die Verwendung von SQL Server 2016 als Remotedatenbank für Azure AD Connect wurde hinzugefügt.

## <a name="112810"></a>1.1.281.0
Veröffentlichung: August 2016

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
* Mit dem hinzugefügten Cmdlet „Stop-ADSyncScheduler“ kann der derzeit ausgeführte Synchronisierungszyklus bzw. -vorgang beendet werden.
* Im Azure AD Connect-Assistenten kann nun beim Konfigurieren von [Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md) das Azure AD-Attribut des Typs „Teletex-Zeichenfolge“ ausgewählt werden.

## <a name="111890"></a>1.1.189.0
Veröffentlichung: Juni 2016

**Behobene Probleme und Verbesserungen:**

* Azure AD Connect kann jetzt auf einem FIPS-kompatiblen Server installiert werden.
  * Informationen zur Kennwortsynchronisierung finden Sie unter [Kennwortsynchronisierung und FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips).
* Ein Problem, bei dem ein NetBIOS-Name im FQDN im Active Directory Connector nicht aufgelöst werden konnte, wurde behoben.

## <a name="111800"></a>1.1.180.0
Veröffentlichung: Mai 2016

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
Veröffentlichung: April 2016

**Neue Features:**

* Unterstützung für mehrwertige Attribute in [Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md) wurde hinzugefügt.
* Unterstützung für weitere Konfigurationsvarianten für das [automatische Upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) wurde hinzugefügt, damit diese für ein Upgrade infrage kommen.
* Einige Cmdlets für den [benutzerdefinierten Scheduler](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)wurden hinzugefügt.

## <a name="111190"></a>1.1.119.0
Veröffentlichung: März 2016

**Behobene Probleme:**

* Es wurde sichergestellt, dass die Express-Installation unter Windows Server 2008 (vor R2) nicht verwendet werden kann, da die Kennwortsynchronisierung unter diesem Betriebssystem nicht unterstützt wird.
* Upgrade von DirSync mit einer benutzerdefinierten Filterkonfiguration funktionierte nicht wie erwartet.
* Beim Upgrade auf eine neuere Version ohne Änderungen an der Konfiguration sollte kein vollständiger Import-/Synchronisierungsvorgang geplant werden.

## <a name="111100"></a>1.1.110.0
Veröffentlichung: Februar 2016

**Behobene Probleme:**

* Ein Upgrade von früheren Versionen kann nicht ausgeführt werden, wenn sich die Installation nicht im Standardordner „C:\Programme“ befindet.
* Wenn Sie bei der Installation am Ende des Installations-Assistenten die Option zum **Starten des Synchronisierungsvorgangs** deaktivieren, wird der Scheduler durch erneutes Ausführen des Installations-Assistenten nicht aktiviert.
* Auf Servern, auf denen das Datums-/Uhrzeitformat nicht dem US-englischen Format entspricht, wird der Scheduler nicht wie erwartet ausgeführt. Außerdem wird verhindert, dass `Get-ADSyncScheduler` korrekte Zeitangaben zurückgibt.
* Wenn Sie eine frühere Version von Azure AD Connect mit AD FS als Anmeldeoption und Upgrade installiert haben, können Sie den Installations-Assistenten nicht erneut ausführen.

## <a name="111050"></a>1.1.105.0
Veröffentlichung: Februar 2016

**Neue Features:**

* [Automatic upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) für Kunden mit Expresseinstellungen.
* Unterstützung für den globalen Administrator durch Verwenden von Azure Multi-Factor Authentication und Privileged Identity Management im Installations-Assistenten.
  * Bei Verwendung von Multi-Factor Authentication muss Ihr Proxy Datenverkehr über „https://secure.aadcdn.microsoftonline-p.com“ zulassen.
  * Außerdem müssen Sie „https://secure.aadcdn.microsoftonline-p.com“ in Ihre Liste vertrauenswürdiger Websites aufnehmen, um eine einwandfreie Funktionsweise von Multi-Factor Authentication sicherzustellen.
* Das Ändern der Anmeldemethode des Benutzers nach der Erstinstallation ist zulässig.
* Lassen Sie im Installations-Assistenten das [Filtern von Domänen und Organisationseinheiten](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) zu. Dadurch wird außerdem das Herstellen einer Verbindung mit Gesamtstrukturen ermöglicht, in denen nicht alle Domänen verfügbar sind.
* [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) ist in das Synchronisierungsmodul integriert.

**Features, die von der Vorschau auf die allgemeine Verfügbarkeit hochgestuft wurden:**

* [Geräterückschreiben](active-directory-aadconnect-feature-device-writeback.md).
* [Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md).

**Neue Vorschaufeatures:**

* Das neue Standardintervall für den Synchronisierungszyklus beträgt 30 Minuten. In allen früheren Versionen wurde ein Intervall von drei Stunden verwendet. Unterstützung zum Ändern des [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) -Verhaltens wurde hinzugefügt.

**Behobene Probleme:**

* Von der Seite zum Überprüfen von DNS-Domänen wurden die Domänen nicht immer erkannt.
* Anmeldeinformationen von Domänenadministratoren werden beim Konfigurieren von AD FS abgefragt.
* Die lokalen AD-Konten werden vom Installations-Assistenten nicht erkannt, wenn sie sich in einer Domäne befinden, die eine andere DNS-Struktur als die Stammdomäne aufweist.

## <a name="1091310"></a>1.0.9131.0
Veröffentlichung: Dezember 2015

**Behobene Probleme:**

* Möglicherweise funktioniert die Kennwortsynchronisierung in Active Directory Domain Services (AD DS) nicht beim Ändern von Kennwörtern, jedoch beim Festlegen eines Kennworts.
* Bei Verwenden eines Proxyservers schlägt die Azure AD-Authentifizierung während der Installation möglicherweise fehl, oder ein Upgrade wird auf der Konfigurationsseite abgebrochen.
* Das Aktualisieren einer vorherigen Version von Azure AD Connect mit einer vollständigen SQL Server-Instanz schlägt fehl, wenn Sie kein SQL Server-Systemadministrator (sa) sind.
* Beim Aktualisieren einer vorherigen Version von Azure AD Connect über einen Remotecomputer mit SQL Server wird die Fehlermeldung „Kein Zugriff auf die ADSync SQL-Datenbank“ angezeigt.

## <a name="1091250"></a>1.0.9125.0
Veröffentlichung: November 2015

**Neue Features:**

* Die Vertrauensstellung zwischen AD FS und Azure AD kann neu konfiguriert werden.
* Das Active Directory-Schema kann aktualisiert und die Synchronisierungsregeln können neu generiert werden.
* Synchronisierungsregeln können deaktiviert werden.
* In einer Synchronisierungsregel kann „AuthoritativeNull“ als neues Literal definiert werden.

**Neue Vorschaufeatures:**

* [Azure AD Connect Health für die Synchronisierung](../connect-health/active-directory-aadconnect-health-sync.md)
* Unterstützung für die Kennwortsynchronisierung mit [Azure AD-Domänendiensten](../active-directory-passwords-update-your-own-password.md)

**Neues unterstütztes Szenario:**

* Es werden mehrere lokale Exchange-Organisationen unterstützt. Weitere Informationen finden Sie unter [Hybrid-Bereitstellungen mit mehreren Active Directory-Gesamtstrukturen](https://technet.microsoft.com/library/jj873754.aspx).

**Behobene Probleme:**

* Probleme bei der Kennwortsynchronisierung:
  * Für ein Objekt, das sich bisher außerhalb des Bereichs befand und das dann in den Bereich verschoben wurde, wird das Kennwort nicht synchronisiert. Dies schließt die Organisationseinheit und Attributfilterung ein.
  * Wenn eine neue Organisationseinheit in die Synchronisierung einbezogen wird, ist keine vollständige Kennwortsynchronisierung erforderlich.
  * Nach dem Aktivieren eines bisher deaktivierten Benutzers wird das Kennwort nicht synchronisiert.
  * Für die Warteschlange mit Kennwortwiederholungsversuchen gilt kein Limit, der bisherige Grenzwert von 5.000 Objekten bis zum Außerkraftsetzen wurde entfernt.
* Mit der Gesamtstrukturfunktionsebene von Windows Server 2016 kann keine Verbindung mit Active Directory hergestellt werden.
* Nach der Erstinstallation kann die für das gruppenspezifische Filtern verwendete Gruppe nicht geändert werden.
* Wenn Benutzer, für die das Kennwortrückschreiben aktiviert ist, ihr Kennwort ändern, wird auf dem Azure AD Connect-Server kein neues Benutzerprofil mehr angelegt.
* In den Geltungsbereichen der Synchronisierungsregeln können keine langen Ganzzahlen als Werte verwendet werden.
* Das Kontrollkästchen „Geräterückschreiben“ bleibt deaktiviert, wenn nicht erreichbare Domänencontroller vorhanden sind.

## <a name="1086670"></a>1.0.8667.0
Veröffentlichung: August 2015

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
* DirSync kann nicht aktualisiert werden, wenn ein definierter Name (DN) bei der Attributfilterung verwendet wird.
* Beim Verwenden der Kennwortzurücksetzung wird die CPU übermäßig ausgelastet.

**Entfernte Vorschaufunktionen:**

* Die Vorschaufunktion [Benutzerrückschreiben](active-directory-aadconnect-feature-preview.md#user-writeback) wurde aufgrund von Rückmeldungen unserer Vorschaukunden vorübergehend entfernt. Sie wird später erneut hinzugefügt werden, nachdem das gemeldete Feedback umgesetzt wurde.

## <a name="1086410"></a>1.0.8641.0
Veröffentlichung: Juni 2015

**Erste Version von Azure AD Connect.**

Änderung des Namens von Azure AD Sync zu Azure AD Connect.

**Neue Features:**

* Installation mit [Express-Einstellungen](active-directory-aadconnect-get-started-express.md)
* [Konfigurieren von AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) möglich
* [Upgrade von DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) möglich
* [Verhindern von versehentlichen Löschungen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
* Neuer [Stagingmodus](active-directory-aadconnectsync-operations.md#staging-mode)

**Neue Vorschaufeatures:**

* [Rückschreiben von Benutzern](active-directory-aadconnect-feature-preview.md#user-writeback)
* [Gruppenrückschreiben](active-directory-aadconnect-feature-preview.md#group-writeback)
* [Geräterückschreiben](active-directory-aadconnect-feature-device-writeback.md)
* [Verzeichniserweiterungen](active-directory-aadconnect-feature-preview.md)

## <a name="104940501"></a>1.0.494.0501
Veröffentlichung: Mai 2015

**Neue Voraussetzung:**

* Azure AD Sync erfordert jetzt die Installation der .NET Framework-Version 4.5.1.

**Behobene Probleme:**

* Das Rückschreiben von Kennwörtern aus Azure AD schlägt mit einem Azure-Service Bus-Konnektivitätsfehler fehl.

## <a name="104910413"></a>1.0.491.0413
Veröffentlichung: April 2015

**Behobene Probleme und Verbesserungen:**

* Der Active Directory Connector verarbeitet Löschvorgänge nicht ordnungsgemäß, wenn der Papierkorb aktiviert ist, und mehrere Domänen in der Gesamtstruktur vorhanden sind.
* Die Leistung bei Importvorgängen wurde für den Azure Active Directory Connector verbessert.
* Wenn eine Gruppe die Mitgliedergrenze überschritten hatte (standardmäßig ist der Grenzwert auf 50.000 Objekte festgelegt), wurde die Gruppe in Azure Active Directory gelöscht. Beim neuen Verhalten wird die Gruppe nicht gelöscht. Jedoch wird ein Fehler ausgelöst, und neue Mitgliedschaftsänderungen werden nicht exportiert.
* Ein neues Objekt kann nicht bereitgestellt werden, wenn eine gestaffelte Löschung mit dem gleichen DN bereits im Connectorbereich vorhanden ist.
* Einige Objekte werden für die Synchronisierung während einer Deltasynchronisierung markiert, obwohl keine Änderung für das Objekt bereitgestellt ist.
* Bei Erzwingen einer Kennwortsynchronisierung wird auch die Liste der bevorzugten Domänencontroller entfernt.
* CSExportAnalyzer hat Probleme mit einigen Objektstatus.

**Neue Features:**

* Mit einer Verknüpfung ist jetzt eine Verbindung zum Objekttyp "ANY" im Metaverse möglich.

## <a name="104850222"></a>1.0.485.0222
Veröffentlichung: Februar 2015

**Verbesserungen:**

* Verbesserte Importleistung.

**Behobene Probleme:**

* Die Kennwortsynchronisierung berücksichtigt das zur Attributfilterung verwendete Attribut „cloudFiltered“. Gefilterte Objekte fallen nicht mehr in den Bereich der Synchronisierung von Kennwörtern.
* In seltenen Fällen, in denen die Topologie viele Domänencontroller aufweist, funktioniert die Synchronisierung von Kennwörtern nicht.
* "Stopped-server" beim Importieren vom Azure AD-Connector in die Geräteverwaltung wurde in Azure AD/Intune aktiviert.
* Verknüpfen der fremden Sicherheitsprinzipale (FSPs) aus mehreren Domänen in der gleichen Gesamtstruktur verursacht den Fehler mehrdeutiger Verknüpfungen.

## <a name="104751202"></a>1.0.475.1202
Veröffentlichung: Dezember 2014

**Neue Features:**

* Die Synchronisierung von Kennwörtern mit attributbasierter Filterung wird jetzt unterstützt. Weitere Informationen finden Sie unter [Synchronisierung von Kennwörtern mit Filterung](active-directory-aadconnectsync-configure-filtering.md).
* Das Attribut „msDS-ExternalDirectoryObjectID“ wird in Active Directory zurückgeschrieben. Dieses Feature fügt Unterstützung für Office 365-Anwendungen hinzu. Es verwendet „Oauth2“, um auf Online- und lokale Postfächer in einer hybriden Exchange-Bereitstellung zuzugreifen.

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
Veröffentlichung: Oktober 2014

**Neue Features:**

* Synchronisierung von Kennwörtern mehrerer lokaler Active Directory-Instanzen mit Azure AD.
* Lokalisierte Benutzeroberfläche für die Installation für alle Windows Server-Sprachen.

**Upgrade von AADSync 1.0 GA**

Wenn Sie Azure AD Sync bereits installiert haben, müssen Sie einen zusätzlichen Schritt ausführen, falls Sie eine der standardmäßigen Synchronisierungsregeln geändert haben. Nach dem Upgrade auf die Version 1.0.470.1023 sind die Synchronisierungsregeln, die Sie geändert haben, dupliziert. Führen Sie für jede geänderte Synchronisierungsregel die folgenden Schritte aus:

1.  Suchen Sie die Synchronisierungsregel, die Sie geändert haben, und notieren Sie sich die Änderungen.
* Löschen Sie die Synchronisierungsregel.
* Suchen Sie die neue von Azure AD Sync erstellte Synchronisierungsregel, und wenden Sie die Änderungen erneut an.

**Berechtigungen für das Active Directory-Konto**

Dem Active Directory-Konto müssen zusätzliche Berechtigungen erteilt werden, um die Kennworthashes aus Active Directory zu lesen. Die zu erteilenden Berechtigungen heißen „Verzeichnisänderungen replizieren“ und „Alle Verzeichnisänderungen replizieren“. Beide Berechtigungen sind zum Lesen von Kennworthashes erforderlich.

## <a name="104190911"></a>1.0.419.0911
Veröffentlichung: September 2014

**Erste Version von Azure AD Sync**

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

