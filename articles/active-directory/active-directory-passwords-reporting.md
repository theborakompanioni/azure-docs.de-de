---
title: 'Berichterstellung: Azure AD SSPR | Microsoft-Dokumentation'
description: "Berichterstellung zu Ereignissen der Self-Service-Kennwortzurücksetzung von Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 4d7f05d626e6f718ca3597dff00715af4055f046
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---
# Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD
<a id="reporting-options-for-azure-ad-password-management" class="xliff"></a>

Nach der Bereitstellung möchten viele Organisationen wissen, wie oder ob SSPR tatsächlich verwendet wird. Azure AD stellt Berichtsfunktionen bereit, mit denen Sie Fragen mithilfe vordefinierter Berichte beantworten, die ordnungsgemäße Lizenzierung ermitteln und benutzerdefinierte Abfragen erstellen können.

Die folgenden Fragen können mit Berichten beantwortet werden, die im [Azure-Portal] (https://portal.azure.com/) vorhanden sind.

> [!NOTE]
> Sie müssen [ein globaler Administrator](active-directory-assign-admin-roles.md#assign-or-remove-administrator-roles) sein und müssen aktivieren, dass diese Daten im Auftrag Ihres Unternehmens erfasst werden, indem Sie die Registerkarte „Berichterstellung“ oder Überwachungsprotokolle mindestens einmal besuchen. Bis dahin werden für Ihre Organisation keine Daten gesammelt.

* Wie viele Personen haben sich für die Kennwortzurücksetzung registriert?
* Wer hat sich für das Zurücksetzen von Kennwörtern registriert?
* Welche Daten registrieren die Benutzer?
* Wie viele Personen haben ihre Kennwörter in den letzten sieben Tagen zurückgesetzt?
* Welche Methoden werden von Benutzern und Administratoren am häufigsten zum Zurücksetzen von Kennwörtern eingesetzt?
* Welche Probleme treten häufig für Benutzer oder Administratoren bei dem Versuch auf, das Kennwort zurückzusetzen?
* Welche Administratoren setzen häufig ihre eigenen Kennwörter zurück?
* Gibt es verdächtige Aktivitäten beim Zurücksetzen des Kennworts?

## Anzeigen von Kennwortverwaltungsberichten im Azure-Portal
<a id="how-to-view-password-management-reports-in-the-azure-portal" class="xliff"></a>

Im Azure-Portal können Kennwortzurücksetzungen sowie Aktivitäten zur Registrierung für die Kennwortzurücksetzung besser angezeigt werden.  Gehen Sie wie folgt vor, um die Ereignisse zur Kennwortzurücksetzung und zur Registrierung für die Kennwortzurücksetzung anzuzeigen:

1. Navigieren Sie zu [**portal.azure.com**](https://portal.azure.com).
2. Klicken Sie auf der linken Seite im Hauptnavigationsbereich des Azure-Portals auf das Menü **Weitere Dienste**.
3. Suchen Sie in der Liste mit den Diensten nach **Azure Active Directory**, und wählen Sie die entsprechende Option aus.
4. Klicken Sie im Azure Active Directory-Navigationsmenü auf **Benutzer und Gruppen**.
5. Klicken Sie im Navigationsmenü für „Benutzer und Gruppen“ auf das Navigationselement **Überwachungsprotokolle**. Daraufhin werden sämtliche Überwachungsereignisse für alle Benutzer in Ihrem Verzeichnis angezeigt. Diese Ansicht kann auch gefiltert werden, um alle kennwortbezogenen Ereignisse anzuzeigen.
6. Wenn Sie diese Ansicht nach den Ereignissen filtern möchten, die mit der Kennwortzurücksetzung zusammenhängen, klicken Sie im oberen Bereich des Blatts auf die Schaltfläche **Filter**.
7. Wählen Sie im Menü **Filter** die Dropdownliste **Kategorie** und anschließend den Kategorietyp **Self-Service-Kennwortverwaltung** aus.
8. Grenzen Sie die Liste optional weiter ein, indem Sie die spezifische **Aktivität** auswählen, für die Sie sich interessieren.

## Abrufen von Kennwortverwaltungsereignissen von der API für Azure AD-Berichte und -Ereignisse
<a id="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api" class="xliff"></a>

Die API für Azure AD-Berichte und -Ereignisse unterstützt das Abrufen aller Informationen, die in den Berichten „Kennwortzurücksetzung“ und „Registrierung für Zurücksetzen des Kennworts“ enthalten sind. Mithilfe dieser API können Sie einzelne Ereignisse zur Kennwortzurücksetzung und zur Registrierung für die Kennwortzurücksetzung herunterladen und in die Berichtstechnologie Ihrer Wahl integrieren.

### Erste Schritte mit der Reporting-API
<a id="how-to-get-started-with-the-reporting-api" class="xliff"></a>

Zum Zugreifen auf diese Daten müssen Sie eine kleine App oder ein Skript schreiben, um sie von unseren Servern abzurufen. [Hier erfahren Sie, wie Sie die ersten Schritte mit der Azure AD Reporting-API ausführen](active-directory-reporting-api-getting-started.md).

Nachdem Sie über ein funktionierendes Skript verfügen, sollten Sie sich als Nächstes mit den Kennwortzurücksetzungs- und Registrierungsereignissen befassen, die Sie zum Erfüllen Ihrer Szenarien abrufen können.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): Listet die Spalten auf, die für Ereignisse zum Zurücksetzen des Kennworts verfügbar sind.
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): Listet die Spalten auf, die für Ereignisse zum Registrieren der Kennwortzurücksetzung verfügbar sind.

### Datenabrufeinschränkungen der Reporting-API
<a id="reporting-api-data-retrieval-limitations" class="xliff"></a>

Derzeit ruft die API für Azure AD-Berichte und -Ereignisse bis zu **75.000 Einzelereignisse** vom Typ [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) und [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) aus den **letzten 30 Tagen** ab.

Wenn Sie weiter zurückliegende Daten abrufen oder speichern möchten, sollten Sie sie in einer externen Datenbank speichern und mithilfe der API die resultierenden Deltas abfragen. Es empfiehlt sich, mit dem Abrufen der Daten zu beginnen, wenn in Ihrer Organisation die SSPR-Verwendung gestartet wird. Speichern Sie die Daten extern, und verfolgen Sie ab diesem Punkt die Deltas nach.

## Schnelles Herunterladen von Ereignissen im Zusammenhang mit der Registrierung für die Kennwortzurücksetzung mit PowerShell
<a id="how-to-download-password-reset-registration-events-quickly-with-powershell" class="xliff"></a>

Neben der direkten Verwendung der API für Azure AD-Berichte und -Ereignisse können Sie auch das folgende PowerShell-Skript verwenden, um aktuelle Registrierungsereignisse in Ihrem Verzeichnis herunterzuladen. Dadurch können Sie ermitteln, wer sich kürzlich registriert hat, oder sich vergewissern, dass das Rollout Ihrer Kennwortzurücksetzung wie erwartet erfolgt.

* [PowerShell-Skript: Aktivität zur Registrierung für die Self-Service-Kennwortzurücksetzung von Azure AD](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

### Beschreibung der Berichtsspalten im Azure-Portal
<a id="description-of-report-columns-in-azure-portal" class="xliff"></a>

In der folgende Liste werden alle Berichtsspalten im Detail beschrieben:

* **Benutzer** – Der Benutzer, der versucht hat, sich für die Kennwortzurücksetzung zu registrieren.
* **Rolle** – Die Rolle des Benutzers im Verzeichnis.
* **Datum und Uhrzeit** – Datum und Uhrzeit des Versuchs.
* **Registrierte Daten** – Die Authentifizierungsdaten, die vom Benutzer während der Registrierung für die Kennwortzurücksetzung bereitgestellt wurden.

### Beschreibung der Berichtswerte im Azure-Portal
<a id="description-of-report-values-in-azure-portal" class="xliff"></a>

Die folgende Tabelle beschreibt die verschiedenen Werte, die für die einzelnen Spalten zulässig sind:

| Column | Zulässige Werte und ihre Bedeutung |
| --- | --- |
| Registrierte Daten |**Alternative E-Mail-Adresse** – Benutzer hat für die Authentifizierung eine alternative E-Mail-Adresse oder eine E-Mail-Adresse zur Authentifizierung verwendet.<p><p>**Bürotelefon** – Benutzer hat seine Bürotelefonnummer zur Authentifizierung verwendet.<p>**Mobiltelefon** – Benutzer hat sein Mobiltelefon oder sein Authentifizierungstelefon zum Authentifizieren verwendet.<p>**Sicherheitsfragen** – Benutzer hat Sicherheitsfragen zur Authentifizierung verwendet.<p>**Eine beliebige Kombination der oben genannten Daten (z.B. alternative E-Mail-Adresse und Mobiltelefon)** – Tritt auf, wenn eine Richtlinie für die zweistufige Überprüfung angegeben ist, und zeigt, welche beiden Methoden der Benutzer zur Authentifizierung seiner Anforderung zum Zurücksetzen des Kennworts verwendet hat. |

## Anzeigen der Kennwortzurücksetzungsaktivität im klassischen Portal
<a id="view-password-reset-activity-in-the-classic-portal" class="xliff"></a>

Dieser Bericht zeigt alle Versuche der Kennwortzurücksetzung an, die in Ihrer Organisation erfolgt sind.

* **Max. Zeitraum**: 30 Tage
* **Max. Anzahl von Zeilen**: 75.000
* **Zum Herunterladen**: Ja, über eine CSV-Datei

### Beschreibung der Berichtsspalten im klassischen Azure-Portal
<a id="description-of-report-columns-in-azure-classic-portal" class="xliff"></a>

In der folgende Liste werden alle Berichtsspalten im Detail beschrieben:

1. **Benutzer** – Der Benutzer, der versucht hat, ein Kennwort zurückzusetzen (basierend auf dem Feld "Benutzer-ID", das bereitgestellt wird, wenn der Benutzer ein Kennwort zurückzusetzen versucht).
2. **Rolle** – Die Rolle des Benutzers im Verzeichnis.
3. **Datum und Uhrzeit** – Datum und Uhrzeit des Versuchs.
4. **Verwendete Methoden** – Die Authentifizierungsmethoden, die der Benutzer für diesen Zurücksetzungsvorgang verwendet hat.
5. **Ergebnis** – Das Ergebnis des Vorgangs zum Zurücksetzen des Kennworts.
6. **Details** – Die Einzelheiten dazu, warum die Kennwortzurücksetzung zu dem entsprechenden Wert geführt hat.  Enthält auch alle Maßnahmen, die Sie ergreifen können, um einen unerwarteten Fehler zu beheben.

### Beschreibung der Berichtswerte im klassischen Azure-Portal
<a id="description-of-report-values-in-azure-classic-portal" class="xliff"></a>

Die folgende Tabelle beschreibt die verschiedenen Werte, die für die einzelnen Spalten zulässig sind:

| Column | Zulässige Werte und ihre Bedeutung |
| --- | --- |
| Verwendete Methoden |**Alternative E-Mail-Adresse** – Benutzer hat für die Authentifizierung eine alternative E-Mail-Adresse oder eine E-Mail-Adresse zur Authentifizierung verwendet.<p>**Bürotelefon** – Benutzer hat seine Bürotelefonnummer zur Authentifizierung verwendet.<p>**Mobiltelefon** – Benutzer hat sein Mobiltelefon oder sein Authentifizierungstelefon zum Authentifizieren verwendet.<p>**Sicherheitsfragen** – Benutzer hat Sicherheitsfragen zur Authentifizierung verwendet.<p>**Eine beliebige Kombination der oben genannten Daten (z.B. alternative E-Mail-Adresse und Mobiltelefon)** – Tritt auf, wenn eine Richtlinie für die zweistufige Überprüfung angegeben ist, und zeigt, welche beiden Methoden der Benutzer zur Authentifizierung seiner Anforderung zum Zurücksetzen des Kennworts verwendet hat. |
| Ergebnis |**Vorzeitig beendet** – Der Benutzer hat die Kennwortzurücksetzung gestartet, den Vorgang jedoch mittendrin beendet und nicht abgeschlossen.<p>**Blockiert** – Das Konto des Benutzers wurde an der Kennwortzurücksetzung gehindert, weil die Seite zur Kennwortzurücksetzung oder eine einzelne Überprüfungsmethode zur Kennwortzurücksetzung in einem Zeitraum von 24 Stunden zu häufig verwendet wurde.<p>**Abgebrochen** – Der Benutzer hat die Kennwortzurücksetzung gestartet, aber dann auf die Schaltfläche „Abbrechen“ geklickt, um die Sitzung mittendrin abzubrechen. <p>**Administrator kontaktiert** – Beim Benutzer ist während der Sitzung ein Problem aufgetreten, das er nicht auflösen konnte. Daher hat der Benutzer auf den Link Wenden Sie sich an Ihren Administrator geklickt, statt die Kennwortzurücksetzung abzuschließen.<p>**Fehler** – Der Benutzer konnte ein Kennwort nicht zurücksetzen, wahrscheinlich weil der Benutzer nicht für die Verwendung dieses Features konfiguriert wurde (z.B. keine Lizenz, fehlende Informationen für die Authentifizierung, Kennwort lokal verwaltet ohne Aktivierung der Rückschreibungsfunktion).<p>**Erfolgreich** – Die Kennwortzurücksetzung war erfolgreich. |
| Details |Beachten Sie die folgende Tabelle. |

### Zulässige Werte für die Spalte "Details"
<a id="allowed-values-for-details-column" class="xliff"></a>

Nachfolgend finden Sie die Liste der Ergebnistypen, die Sie im Bericht zur Aktivität "Zurücksetzen des Kennworts" erwarten können:

| Details | Ergebnistyp |
| --- | --- |
| Benutzer hat nach Abschluss der Überprüfung per E-Mail vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach Abschluss der Überprüfung per Mobiltelefon-SMS vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach Abschluss der Überprüfung per Mobiltelefon-Sprachanruf vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach Abschluss der Überprüfung per Bürotelefon-Sprachanruf vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach Abschluss der Überprüfung per Sicherheitsfragen vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach Eingabe der Benutzer-ID vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach dem Start der Überprüfung per E-Mail vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach dem Start der Überprüfung per Mobiltelefon-SMS vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach dem Start der Überprüfung per Mobiltelefon-Sprachanruf vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach dem Start der Überprüfung per Bürotelefon-Sprachanruf vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat nach dem Start der Überprüfung per Sicherheitsfragen vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat vor Auswahl eines neuen Kennworts vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat während Auswahl eines neuen Kennworts vorzeitig beendet. |Vorzeitig beendet |
| Benutzer hat zu viele ungültige SMS-Überprüfungscodes eingegeben und ist für 24 Stunden blockiert. |Blockiert |
| Benutzer hat die Überprüfung per Mobiltelefon-Sprachanruf zu oft versucht und ist für 24 Stunden blockiert. |Blockiert |
| Benutzer hat die Überprüfung per Bürotelefon-Sprachanruf zu oft versucht und ist für 24 Stunden blockiert. |Blockiert |
| Benutzer hat zu häufig versucht, die Sicherheitsfragen zu beantworten, und ist für 24 Stunden blockiert. |Blockiert |
| Benutzer hat zu oft versucht, eine Telefonnummer zu überprüfen, und ist für 24 Stunden blockiert. |Blockiert |
| Benutzer hat vor Übergabe der erforderlichen Authentifizierungsmethoden abgebrochen. |Canceled |
| Benutzer hat vor Übermittlung eines neuen Kennworts abgebrochen. |Canceled |
| Benutzer hat nach Versuch der Überprüfung per E-Mail den Administrator kontaktiert. |Administrator kontaktiert |
| Benutzer hat nach Versuch der Überprüfung per Mobiltelefon-SMS den Administrator kontaktiert. |Administrator kontaktiert |
| Benutzer hat nach Versuch der Überprüfung per Mobiltelefon-Sprachanruf den Administrator kontaktiert. |Administrator kontaktiert |
| Benutzer hat nach Versuch der Überprüfung per Bürotelefon-Sprachanruf den Administrator kontaktiert. |Administrator kontaktiert |
| Benutzer hat nach Versuch der Überprüfung per Sicherheitsfrage den Administrator kontaktiert. |Administrator kontaktiert |
| Die Kennwortzurücksetzung ist für diesen Benutzer nicht aktiviert. Aktivieren Sie die Kennwortzurücksetzung auf der Konfigurationsregisterkarte, um dieses Problem zu beheben. |Fehler |
| Der Benutzer hat keine Lizenz. Sie können dem Benutzer eine Lizenz hinzufügen, um dieses Problems zu beheben. |Fehler |
| Benutzer hat versucht, die Zurücksetzung von einem Gerät ohne aktivierte Cookies durchzuführen. |Fehler |
| Im Benutzerkonto sind nicht genügend Authentifizierungsmethoden definiert. Fügen Sie Authentifizierungsinformationen hinzu, um dieses Problem zu beheben. |Fehler |
| Das Kennwort des Benutzers wird lokal verwaltet. Sie können die Kennwortrückschreibung aktivieren, um dieses Problem zu beheben. |Fehler |
| Wir konnten den Dienst zum Zurücksetzen Ihres lokalen Kennworts nicht erreichen. Überprüfen Sie das Ereignisprotokoll des Synchronisierungscomputers. |Fehler |
| Beim Zurücksetzen des lokalen Kennworts des Benutzers ist ein Problem aufgetreten. Überprüfen Sie das Ereignisprotokoll des Synchronisierungscomputers. |Fehler |
| Der Benutzer ist kein Mitglied der Benutzergruppe für Kennwortzurücksetzung. Fügen Sie diesen Benutzer dieser Gruppe hinzu, um dieses Problem zu beheben. |Fehler |
| Die Kennwortzurücksetzung wurde für diesen Mandanten vollständig deaktiviert. [Hier](http://aka.ms/ssprtroubleshoot) finden Sie Informationen zur Lösung des Problems. |Fehler |
| Benutzer hat das Kennwort erfolgreich zurückgesetzt. |Erfolgreich |

## Aktivitätstypen für die Self-Service-Kennwortverwaltung
<a id="self-service-password-management-activity-types" class="xliff"></a>

In der Überwachungsereigniskategorie **Self-Service-Kennwortverwaltung** werden folgende Aktivitätstypen angezeigt.  Danach ist jeweils eine Beschreibung angegeben.

* [**Blocked from self-service password reset**](#activity-type-blocked-from-self-service-password-reset) (Von Self-Service-Kennwortzurücksetzung ausgeschlossen): Gibt an, dass ein Benutzer in den letzten 24 Stunden mehr als fünfmal versucht hat, ein Kennwort zurückzusetzen, ein bestimmtes Gate zu verwenden oder eine Telefonnummer zu bestätigen.
* [**Change password (self-service)**](#activity-type-change-password-self-service) (Kennwort ändern (Self-Service)): Gibt an, dass ein Benutzer ein Kennwort geändert hat (entweder freiwillig oder weil das Kennwort abgelaufen ist).
* [**Reset password (by admin)**](#activity-type-reset-password-by-admin) (Kennwort zurücksetzen (durch Administrator)): Gibt an, dass ein Administrator im Auftrag eines Benutzers ein Kennwort über das Azure-Portal zurückgesetzt hat.
* [**Reset password (self-service)**](#activity-type-reset-password-self-service) (Kennwort zurücksetzen (Self-Service)): Gibt an, dass ein Benutzer sein Kennwort über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) zurückgesetzt hat.
* [**Self serve password reset flow activity progress**](#activity-type-self-serve-password-reset-flow-activity-progress) (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs): Gibt die einzelnen Schritte an, die ein Benutzer im Rahmen des Kennwortzurücksetzungsprozesses durchläuft (beispielsweise ein bestimmtes Authentifizierungsgate für die Kennwortzurücksetzung).
* [**Unlock user account (self-service)**](#activity-type-unlock-user-account-self-service) (Benutzerkonto entsperren (Self-Service)): Gibt an, dass ein Benutzer sein Active Directory-Konto über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) mithilfe des Features zur AD-Kontoentsperrung ohne Zurücksetzen entsperrt hat, ohne das Kennwort zurückzusetzen.
* [**User registered for self-service password reset**](#activity-type-user-registered-for-self-service-password-reset) (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer): Gibt an, dass ein Benutzer alle erforderlichen Informationen registriert hat, um sein Kennwort gemäß der aktuellen Mandanten-Kennwortzurücksetzungsrichtlinie zurücksetzen zu können.

### Aktivitätstyp: „Blocked from self-service password reset“ (Von Self-Service-Kennwortzurücksetzung ausgeschlossen)
<a id="activity-type-blocked-from-self-service-password-reset" class="xliff"></a>

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer in den letzten 24 Stunden mehr als fünfmal versucht hat, ein Kennwort zurückzusetzen, ein bestimmtes Gate zu verwenden oder eine Telefonnummer zu bestätigen.
* **Actor der Aktivität:** Der Benutzer, dessen weitere Zurücksetzungsvorgänge gedrosselt wurden. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, dessen weitere Zurücksetzungsvorgänge gedrosselt wurden. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer gedrosselt wurde, sodass er in den nächsten 24 Stunden keine weiteren Zurücksetzungsvorgänge mehr ausführen, keine weiteren Authentifizierungsmethoden mehr verwenden und keine zusätzlichen Telefonnummern mehr bestätigen kann.
* **Ursache für Aktivitätsstatusfehler:** nicht zutreffend

### Aktivitätstyp: „Change password (self-service)“ (Kennwort ändern (Self-Service))
<a id="activity-type-change-password-self-service" class="xliff"></a>

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer ein Kennwort geändert hat (entweder freiwillig oder weil das Kennwort abgelaufen ist).
* **Akteur der Aktivität:** Der Benutzer, der sein Kennwort geändert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sein Kennwort geändert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer sein Kennwort geändert hat.
  * _Fehler:_ Gibt an, dass ein Benutzer sein Kennwort nicht ändern konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.
* **Ursache für Aktivitätsstatusfehler** - 
  * _FuzzyPolicyViolationInvalidPassword:_ Das vom Benutzer gewählte Kennwort wurde durch die Erkennung nicht zulässiger Kennwörter von Microsoft automatisch blockiert, da es zu allgemein oder besonders unsicher war.

### Aktivitätstyp: „Reset password (by admin)“ (Kennwort zurücksetzen (durch Administrator))
<a id="activity-type-reset-password-by-admin" class="xliff"></a>

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Administrator im Auftrag eines Benutzers ein Kennwort über das Azure-Portal zurückgesetzt hat.
* **Akteur der Aktivität:** Der Administrator, der das Kennwort im Auftrag eines anderen Benutzers oder Administrators zurückgesetzt hat. Hierbei muss es sich um einen globalen Administrator, um einen Kennwortadministrator, um einen Benutzeradministrator oder um einen Helpdeskadministrator handeln.
* **Ziel der Aktivität:** Der Benutzer, dessen Kennwort zurückgesetzt wurde. Hierbei kann es sich um einen Endbenutzer oder um einen anderen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Administrator das Kennwort eines Benutzers zurückgesetzt hat.
  * _Fehler:_ Gibt an, dass ein Administrator das Kennwort eines Benutzers nicht ändern konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.

### Aktivitätstyp: „Reset password (self-service)“ (Kennwort zurücksetzen (Self-Service))
<a id="activity-type-reset-password-self-service" class="xliff"></a>

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer sein Kennwort über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) zurückgesetzt hat.
* **Akteur der Aktivität:** Der Benutzer, der sein Kennwort zurückgesetzt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sein Kennwort zurückgesetzt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer sein eigenes Kennwort zurückgesetzt hat.
  * _Fehler:_ Gibt an, dass ein Benutzer sein eigenes Kennwort nicht zurücksetzen konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.
* **Ursache für Aktivitätsstatusfehler** -
  * _FuzzyPolicyViolationInvalidPassword:_ Das vom Administrator gewählte Kennwort wurde durch die Erkennung nicht zulässiger Kennwörter von Microsoft automatisch blockiert, da es zu allgemein oder besonders unsicher war.

### Aktivitätstyp: „Self serve password reset flow activity progress“ (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs)
<a id="activity-type-self-serve-password-reset-flow-activity-progress" class="xliff"></a>

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt die einzelnen Schritte an, die ein Benutzer im Rahmen des Kennwortzurücksetzungsprozesses durchläuft (beispielsweise ein bestimmtes Authentifizierungsgate für die Kennwortzurücksetzung).
* **Actor der Aktivität:** Der Benutzer, der einen Teil des Kennwortzurücksetzungsablaufs ausgeführt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der einen Teil des Kennwortzurücksetzungsablaufs ausgeführt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer einen bestimmten Schritt des Kennwortzurücksetzungsablaufs ausgeführt hat.
  * _Fehler:_ Gibt an, dass ein bestimmter Schritt des Kennwortzurücksetzungsablaufs nicht erfolgreich war. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.
* **Zulässige Ursachen für Aktivitätsstatus**
  * Eine Tabelle mit allen zulässigen Statusgründen für die Zurücksetzungsaktivität finden Sie [weiter unten](#allowed-values-for-details-column).

### Aktivitätstyp: „Unlock user account (self-service)“ (Benutzerkonto entsperren (Self-Service))
<a id="activity-type-unlock-user-account-self-service" class="xliff"></a>

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer sein Active Directory-Konto über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) mithilfe des Features zur AD-Kontoentsperrung ohne Zurücksetzen entsperrt hat, ohne das Kennwort zurückzusetzen.
* **Akteur der Aktivität:** Der Benutzer, der sein Konto entsperrt hat, ohne das Kennwort zurückzusetzen. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sein Konto entsperrt hat, ohne das Kennwort zurückzusetzen. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer sein eigenes Konto entsperrt hat.
  * _Fehler:_ Gibt an, dass ein Benutzer sein Konto nicht entsperren konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.

### Aktivitätstyp: „User registered for self-service password reset“ (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer)
<a id="activity-type-user-registered-for-self-service-password-reset" class="xliff"></a>

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:**: Gibt an, dass ein Benutzer alle erforderlichen Informationen registriert hat, um sein Kennwort gemäß der aktuellen Mandanten-Kennwortzurücksetzungsrichtlinie zurücksetzen zu können. 
* **Actor der Aktivität:** Der Benutzer, der sich für die Kennwortzurücksetzung registriert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sich für die Kennwortzurücksetzung registriert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass sich ein Benutzer gemäß aktueller Richtlinie für die Kennwortzurücksetzung registriert hat. 
  * _Fehler:_ Gibt an, dass sich ein Benutzer nicht für die Kennwortzurücksetzung registrieren konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren. Das bedeutet nicht, dass ein Benutzer sein eigenes Kennwort nicht zurücksetzen kann, sondern lediglich, dass er den Registrierungsprozess nicht abgeschlossen hat. Wenn das Konto des Benutzers korrekte Daten (beispielsweise eine noch nicht bestätigte Telefonnummer) enthält, können diese trotzdem zum Zurücksetzen des Kennworts verwendet werden, auch wenn die Telefonnummer noch nicht bestätigt wurde. Weitere Informationen finden Sie unter [Was geschieht bei Registrierung eines Benutzers?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers).

## Nächste Schritte
<a id="next-steps" class="xliff"></a>

Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD

* [Verknüpfung mit Überwachungsprotokollen der Benutzerverwaltung:](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit) Wechseln Sie direkt zu den Überwachungsprotokollen der Benutzerverwaltung Ihres Mandanten.
* [**Schnellstart:**](active-directory-passwords-getting-started.md) Informieren Sie sich schnell über die Self-Service-Kennwortverwaltung von Azure AD. 
* [**Lizenzierung:**](active-directory-passwords-licensing.md) Konfigurieren Sie Ihre Azure AD-Lizenzierung.
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Rollout:**](active-directory-passwords-best-practices.md) Mithilfe der hier enthaltenen Anleitungen können Sie SSPR planen und für Ihre Benutzer bereitstellen.
* [**Anpassung:**](active-directory-passwords-customize.md) Passen Sie das Aussehen und Verhalten von SSPR für Ihr Unternehmen an.
* [**Ausführliche technische Informationen:**](active-directory-passwords-how-it-works.md) Schauen Sie hinter den Vorhang, um zu verstehen, wie alles funktioniert.
* [**Häufig gestellte Fragen (FAQ):**](active-directory-passwords-faq.md) Wie? Warum? Was? Wo? Wer? Wann? Antworten auf Fragen, die Sie schon immer stellen wollten
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.
* [**Richtlinie:**](active-directory-passwords-policy.md): Verstehen Sie Azure AD-Kennwortrichtlinien, und legen Sie sie fest.

