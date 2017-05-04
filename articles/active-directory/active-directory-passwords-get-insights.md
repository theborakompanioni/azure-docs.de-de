---
title: 'Operative Einblicke: Kennwortverwaltungsberichte in Azure AD | Microsoft-Dokumentation'
description: "Dieser Artikel beschreibt, wie Sie Berichte verwenden, um einen Einblick in die Vorgänge zur Kennwortverwaltung in Ihrem Unternehmen zu erhalten."
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: 1472b51d-53f4-4b0f-b1be-57f6fa88fa65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: joflore
translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: b010891a177f27eef99bd65e0d940c83375aa5ec
ms.lasthandoff: 04/27/2017


---
# <a name="how-to-get-operational-insights-with-password-management-reports"></a>Operative Einblicke durch Berichte zur Kennwortverwaltung
> [!IMPORTANT]
> **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
>
>

In diesem Abschnitt wird beschrieben, wie Sie Berichte zur Kennwortverwaltung in Azure Active Directory verwenden können, um die Verwendung der Kennwortzurücksetzung und -änderung durch Benutzer in Ihrer Organisation zu verfolgen.

* [**Übersicht über Berichte zur Kennwortverwaltung**](#overview-of-password-management-reports)
* [**Anzeigen von Kennwortverwaltungsberichten im neuen Azure-Portal**](#how-to-view-password-management-reports)
 * [Verzeichnisrollen mit Leseberechtigung für Berichte](#directory-roles-allowed-to-read-reports)
* [**Aktivitätstypen für die Self-Service-Kennwortverwaltung im neuen Azure-Portal**](#self-service-password-management-activity-types)
 * [Blocked from self-service password reset](#activity-type-blocked-from-self-service-password-reset) (Von Self-Service-Kennwortzurücksetzung ausgeschlossen)
 * [Change password (self-service)](#activity-type-change-password-self-service) (Kennwort ändern (Self-Service))
 * [Reset password (by admin)](#activity-type-reset-password-by-admin) (Kennwort zurücksetzen (durch Administrator))
 * [Reset password (self-service)](#activity-type-reset-password-self-service) (Kennwort zurücksetzen (Self-Service))
 * [Self serve password reset flow activity progress](#activity-type-self-serve-password-reset-flow-activity-progress) (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs)
 * [Unlock user account (self-service)](#activity-type-unlock-user-account-self-service) (Benutzerkonto entsperren (Self-Service))
 * [User registered for self-service password reset](#activity-type-user-registered-for-self-service-password-reset) (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer)
* [**Abrufen von Kennwortverwaltungsereignissen von der API für Azure AD-Berichte und -Ereignisse**](#how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api)
 * [Datenabrufeinschränkungen der Reporting-API](#reporting-api-data-retrieval-limitations)
* [**Schnelles Herunterladen von Ereignissen im Zusammenhang mit der Registrierung für die Kennwortzurücksetzung mit PowerShell**](#how-to-download-password-reset-registration-events-quickly-with-powershell)
* [**Anzeigen von Kennwortverwaltungsberichten im klassischen Portal**](#how-to-view-password-management-reports-in-the-classic-portal)
* [**Anzeigen der Aktivität zur Registrierung für die Kennwortzurücksetzung in Ihrer Organisation im klassischen Portal**](#view-password-reset-registration-activity-in-the-classic-portal)
* [**Anzeigen der Kennwortzurücksetzungsaktivität in Ihrer Organisation im klassischen Portal**](#view-password-reset-activity-in-the-classic-portal)


## <a name="overview-of-password-management-reports"></a>Übersicht über Berichte zur Kennwortverwaltung
Sobald Sie die Kennwortzurücksetzung bereitgestellt haben, besteht einer der nächsten Schritte üblicherweise darin, herauszufinden, wie diese in Ihrer Organisation verwendet wird.  Beispielsweise möchten Sie einen Einblick gewinnen, wie Benutzer sich für die Kennwortzurücksetzung registrieren oder wie viele Kennwortzurücksetzungen in den letzten paar Tagen stattgefunden haben.  Im Folgenden sind einige der häufigsten Fragen aufgeführt, die Sie anhand der Berichte zur Kennwortverwaltung beantworten können, die derzeit im [Azure-Verwaltungsportal](https://manage.windowsazure.com) zur Verfügung stehen:

* Wie viele Personen haben sich für die Kennwortzurücksetzung registriert?
* Wer hat sich für das Zurücksetzen von Kennwörtern registriert?
* Welche Daten registrieren die Benutzer?
* Wie viele Personen haben ihre Kennwörter in den letzten 7 Tagen zurückgesetzt?
* Welche Methoden werden von Benutzern und Administratoren am häufigsten zum Zurücksetzen von Kennwörtern eingesetzt?
* Welche Probleme treten häufig für Benutzer oder Administratoren bei dem Versuch auf, das Kennwort zurückzusetzen?
* Welche Administratoren setzen häufig ihre eigenen Kennwörter zurück?
* Gibt es verdächtige Aktivitäten beim Zurücksetzen des Kennworts?

## <a name="how-to-view-password-management-reports"></a>Anzeigen von Kennwortverwaltungsberichten
Im neuen [Azure-Portal](https://portal.azure.com) können Kennwortzurücksetzungen sowie Aktivitäten zur Registrierung für die Kennwortzurücksetzung besser angezeigt werden.  Gehen Sie wie folgt vor, um die Ereignisse zur Kennwortzurücksetzung und zur Registrierung für die Kennwortzurücksetzung im neuen [Azure-Portal](https://portal.azure.com) anzuzeigen:

1. Navigieren Sie zu [**portal.azure.com**](https://portal.azure.com).
2. Klicken Sie auf der linken Seite im Hauptnavigationsbereich des Azure-Portals auf das Menü **Weitere Dienste**.
3. Suchen Sie in der Liste mit den Diensten nach **Azure Active Directory**, und wählen Sie die entsprechende Option aus.
4. Klicken Sie im Azure Active Directory-Navigationsmenü auf **Benutzer/Gruppen**.
5. Klicken Sie im Navigationsmenü für Benutzer/Gruppen auf das Navigationselement **Überwachungsprotokolle**. Daraufhin werden sämtliche Überwachungsereignisse für alle Benutzer in Ihrem Verzeichnis angezeigt. Diese Ansicht kann auch gefiltert werden, um alle kennwortbezogenen Ereignisse anzuzeigen.
6. Wenn Sie diese Ansicht nach den Ereignissen filtern möchten, die mit der Kennwortverwaltung zusammenhängen, klicken Sie im oberen Bereich des Blatts auf die Schaltfläche **Filter**.
7. Wählen Sie im Menü **Filter** die Dropdownliste **Kategorie** und anschließend den Kategorietyp **Self-Service-Kennwortverwaltung** aus.
8. Grenzen Sie die Liste optional weiter ein, indem Sie die spezifische **Aktivität** auswählen, für die Sie sich interessieren.
### <a name="direct-link-to-user-audit-blade"></a>Direkter Link zum Blatt für die Benutzerüberwachung
Wenn Sie sich bei Ihrem Portal angemeldet haben, finden Sie hier einen direkten Link zum Blatt für die Benutzerüberwachung, auf dem Sie diese Ereignisse anzeigen können:

* [Direkt zur Überwachungsansicht für die Benutzerverwaltung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit)

### <a name="directory-roles-allowed-to-read-reports"></a>Verzeichnisrollen mit Leseberechtigung für Berichte
Derzeit verfügen die folgenden Verzeichnisrollen über Lesezugriff auf Azure AD-Kennwortverwaltungsberichte im klassischen Azure-Portal:

* Globaler Administrator

Damit ein globaler Administrator diese Berichte lesen kann, muss er mindestens einmal die Berichterstellungsregisterkarte oder Überwachungsprotokolle aufgerufen haben, um den Abruf dieser Daten im Auftrag der Organisation zu aktivieren. Bis dahin werden für Ihre Organisation keine Daten gesammelt.

Weitere Informationen zu Verzeichnisrollen und zu ihren Möglichkeiten finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-assign-admin-roles).

## <a name="self-service-password-management-activity-types"></a>Aktivitätstypen für die Self-Service-Kennwortverwaltung
In der Überwachungsereigniskategorie **Self-Service-Kennwortverwaltung** werden folgende Aktivitätstypen angezeigt.  Danach ist jeweils eine Beschreibung angegeben.

* [**Blocked from self-service password reset**](#activity-type-blocked-from-self-service-password-reset) (Von Self-Service-Kennwortzurücksetzung ausgeschlossen): Gibt an, dass ein Benutzer in den letzten 24 Stunden mehr als fünfmal versucht hat, ein Kennwort zurückzusetzen, ein bestimmtes Gate zu verwenden oder eine Telefonnummer zu bestätigen.
* [**Change password (self-service)**](#activity-type-change-password-self-service) (Kennwort ändern (Self-Service)): Gibt an, dass ein Benutzer ein Kennwort geändert hat (entweder freiwillig oder weil das Kennwort abgelaufen ist).
* [**Reset password (by admin)**](#activity-type-reset-password-by-admin) (Kennwort zurücksetzen (durch Administrator)): Gibt an, dass ein Administrator im Auftrag eines Benutzers ein Kennwort über das Azure-Portal zurückgesetzt hat.
* [**Reset password (self-service)**](#activity-type-reset-password-self-service) (Kennwort zurücksetzen (Self-Service)): Gibt an, dass ein Benutzer sein Kennwort über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) zurückgesetzt hat.
* [**Self serve password reset flow activity progress**](#activity-type-self-serve-password-reset-flow-activity-progress) (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs): Gibt die einzelnen Schritte an, die ein Benutzer im Rahmen des Kennwortzurücksetzungsprozesses durchläuft (beispielsweise ein bestimmtes Authentifizierungsgate für die Kennwortzurücksetzung).
* [**Unlock user account (self-service)**](#activity-type-unlock-user-account-self-service) (Benutzerkonto entsperren (Self-Service)): Gibt an, dass ein Benutzer sein Active Directory-Konto über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) mithilfe des Features [AD account unlock without reset](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) (AD-Kontoentsperrung ohne Zurücksetzen) entsperrt hat.
* [**User registered for self-service password reset**](#activity-type-user-registered-for-self-service-password-reset) (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer): Gibt an, dass ein Benutzer alle erforderlichen Informationen registriert hat, um sein Kennwort gemäß der aktuellen Mandanten-Kennwortzurücksetzungsrichtlinie zurücksetzen zu können.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Aktivitätstyp: „Blocked from self-service password reset“ (Von Self-Service-Kennwortzurücksetzung ausgeschlossen)
In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer in den letzten 24 Stunden mehr als fünfmal versucht hat, ein Kennwort zurückzusetzen, ein bestimmtes Gate zu verwenden oder eine Telefonnummer zu bestätigen.
* **Actor der Aktivität:** Der Benutzer, dessen weitere Zurücksetzungsvorgänge gedrosselt wurden. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, dessen weitere Zurücksetzungsvorgänge gedrosselt wurden. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
 * _Erfolgreich:_ Gibt an, dass ein Benutzer gedrosselt wurde, sodass er in den nächsten 24 Stunden keine weiteren Zurücksetzungsvorgänge mehr ausführen, keine weiteren Authentifizierungsmethoden mehr verwenden und keine zusätzlichen Telefonnummern mehr bestätigen kann.
* **Ursache für Aktivitätsstatusfehler:** nicht zutreffend

### <a name="activity-type-change-password-self-service"></a>Aktivitätstyp: „Change password (self-service)“ (Kennwort ändern (Self-Service))
In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer ein Kennwort geändert hat (entweder freiwillig oder weil das Kennwort abgelaufen ist).
* **Actor der Aktivität:** Der Benutzer, der sein Kennwort geändert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sein Kennwort geändert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
 * _Erfolgreich:_ Gibt an, dass ein Benutzer sein Kennwort geändert hat.
 * _Fehler:_ Gibt an, dass ein Benutzer sein Kennwort nicht ändern konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.
* **Ursache für Aktivitätsstatusfehler** -
 * _FuzzyPolicyViolationInvalidPassword:_ Das vom Benutzer gewählte Kennwort wurde durch die Erkennung nicht zulässiger Kennwörter von Microsoft automatisch blockiert, da es zu allgemein oder besonders unsicher war.

### <a name="activity-type-reset-password-by-admin"></a>Aktivitätstyp: „Reset password (by admin)“ (Kennwort zurücksetzen (durch Administrator))
In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Administrator im Auftrag eines Benutzers ein Kennwort über das Azure-Portal zurückgesetzt hat.
* **Actor der Aktivität:** Der Administrator, der das Kennwort im Auftrag eines anderen Benutzers oder Administrators zurückgesetzt hat. Hierbei muss es sich um einen globalen Administrator, um einen Kennwortadministrator, um einen Benutzeradministrator oder um einen Helpdeskadministrator handeln.
* **Ziel der Aktivität:** Der Benutzer, dessen Kennwort zurückgesetzt wurde. Hierbei kann es sich um einen Endbenutzer oder um einen anderen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
 * _Erfolgreich:_ Gibt an, dass ein Administrator das Kennwort eines Benutzers zurückgesetzt hat.
 * _Fehler:_ Gibt an, dass ein Administrator das Kennwort eines Benutzers nicht ändern konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.

### <a name="activity-type-reset-password-self-service"></a>Aktivitätstyp: „Reset password (self-service)“ (Kennwort zurücksetzen (Self-Service))
In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer sein Kennwort über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) zurückgesetzt hat.
* **Actor der Aktivität:** Der Benutzer, der sein Kennwort zurückgesetzt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sein Kennwort zurückgesetzt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
 * _Erfolgreich:_ Gibt an, dass ein Benutzer sein eigenes Kennwort zurückgesetzt hat.
 * _Fehler:_ Gibt an, dass ein Benutzer sein eigenes Kennwort nicht zurücksetzen konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.
* **Ursache für Aktivitätsstatusfehler** -
 * _FuzzyPolicyViolationInvalidPassword:_ Das vom Administrator gewählte Kennwort wurde durch die Erkennung nicht zulässiger Kennwörter von Microsoft automatisch blockiert, da es zu allgemein oder besonders unsicher war.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Aktivitätstyp: „Self serve password reset flow activity progress“ (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs)
In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt die einzelnen Schritte an, die ein Benutzer im Rahmen des Kennwortzurücksetzungsprozesses durchläuft (beispielsweise ein bestimmtes Authentifizierungsgate für die Kennwortzurücksetzung).
* **Actor der Aktivität:** Der Benutzer, der einen Teil des Kennwortzurücksetzungsablaufs ausgeführt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der einen Teil des Kennwortzurücksetzungsablaufs ausgeführt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
 * _Erfolgreich:_ Gibt an, dass ein Benutzer einen bestimmten Schritt des Kennwortzurücksetzungsablaufs ausgeführt hat.
 * _Fehler:_ Gibt an, dass ein bestimmter Schritt des Kennwortzurücksetzungsablaufs nicht erfolgreich war. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.
* **Zulässige Ursachen für Aktivitätsstatus**
 * Eine Tabelle mit allen zulässigen Statusgründen für die Zurücksetzungsaktivität finden Sie [weiter unten](#allowed-values-for-details-column).

### <a name="activity-type-unlock-user-account-self-service"></a>Aktivitätstyp: „Unlock user account (self-service)“ (Benutzerkonto entsperren (Self-Service))
In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer sein Active Directory-Konto über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) mithilfe des Features [AD account unlock without reset](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) (AD-Kontoentsperrung ohne Zurücksetzen) entsperrt hat.
* **Actor der Aktivität:** Der Benutzer, der sein Konto entsperrt hat, ohne das Kennwort zurückzusetzen. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sein Konto entsperrt hat, ohne das Kennwort zurückzusetzen. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
 * _Erfolgreich:_ Gibt an, dass ein Benutzer sein eigenes Konto entsperrt hat.
 * _Fehler:_ Gibt an, dass ein Benutzer sein Konto nicht entsperren konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Aktivitätstyp: „User registered for self-service password reset“ (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer)
In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:**: Gibt an, dass ein Benutzer alle erforderlichen Informationen registriert hat, um sein Kennwort gemäß der aktuellen Mandanten-Kennwortzurücksetzungsrichtlinie zurücksetzen zu können.
* **Actor der Aktivität:** Der Benutzer, der sich für die Kennwortzurücksetzung registriert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sich für die Kennwortzurücksetzung registriert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
 * _Erfolgreich:_ Gibt an, dass sich ein Benutzer gemäß aktueller Richtlinie für die Kennwortzurücksetzung registriert hat.
 * _Fehler:_ Gibt an, dass sich ein Benutzer nicht für die Kennwortzurücksetzung registrieren konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren. Das bedeutet nicht, dass ein Benutzer sein eigenes Kennwort nicht zurücksetzen kann, sondern lediglich, dass er den Registrierungsprozess nicht abgeschlossen hat. Wenn das Konto des Benutzers korrekte Daten (beispielsweise eine noch nicht bestätigte Telefonnummer) enthält, können diese trotzdem zum Zurücksetzen des Kennworts verwendet werden, auch wenn die Telefonnummer noch nicht bestätigt wurde. Weitere Informationen finden Sie unter [Was geschieht bei Registrierung eines Benutzers?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers).

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Abrufen von Kennwortverwaltungsereignissen von der API für Azure AD-Berichte und -Ereignisse
Ab August 2015 unterstützt die API für Azure AD-Berichte und -Ereignisse das Abrufen aller Informationen, die in den Berichten „Kennwortzurücksetzung“ und „Registrierung für Zurücksetzen des Kennworts“ enthalten sind. Mithilfe dieser API können Sie einzelne Ereignisse zur Kennwortzurücksetzung und zur Registrierung für die Kennwortzurücksetzung herunterladen und in die Berichtstechnologie Ihrer Wahl integrieren.

### <a name="how-to-get-started-with-the-reporting-api"></a>Erste Schritte mit der Reporting-API
Zum Zugreifen auf diese Daten müssen Sie eine kleine App oder ein Skript schreiben, um sie von unseren Servern abzurufen. [Hier erfahren Sie, wie Sie die ersten Schritte mit der Azure AD Reporting-API ausführen](active-directory-reporting-api-getting-started.md).

Nachdem Sie über ein funktionierendes Skript verfügen, sollten Sie sich als Nächstes mit den Kennwortzurücksetzungs- und Registrierungsereignissen befassen, die Sie zum Erfüllen Ihrer Szenarien abrufen können.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): Listet die Spalten auf, die für Ereignisse zum Zurücksetzen des Kennworts verfügbar sind.
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): Listet die Spalten auf, die für Ereignisse zum Registrieren der Kennwortzurücksetzung verfügbar sind.

### <a name="reporting-api-data-retrieval-limitations"></a>Datenabrufeinschränkungen der Reporting-API
Derzeit ruft die API für Azure AD-Berichte und -Ereignisse bis zu **75.000 Einzelereignisse** vom Typ [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) und [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) aus den **letzten 30 Tagen** ab.

Wenn Sie weiter zurückliegende Daten abrufen oder speichern möchten, sollten Sie sie in einer externen Datenbank speichern und mithilfe der API die resultierenden Deltas abfragen. Dabei hat sich Folgendes bewährt: Beginnen Sie mit dem Abrufen der Daten, wenn in Ihrer Organisation ein Prozess zur Registrierung für die Kennwortzurücksetzung gestartet wird, speichern Sie die Daten extern, und verfolgen Sie ab diesem Punkt die Deltas nach.

## <a name="how-to-download-password-reset-registration-events-quickly-with-powershell"></a>Schnelles Herunterladen von Ereignissen im Zusammenhang mit der Registrierung für die Kennwortzurücksetzung mit PowerShell
Neben der direkten Verwendung der API für Azure AD-Berichte und -Ereignisse können Sie auch das folgende PowerShell-Skript verwenden, um aktuelle Registrierungsereignisse in Ihrem Verzeichnis herunterzuladen. Dadurch können Sie ermitteln, wer sich kürzlich registriert hat, oder sich vergewissern, dass das Rollout Ihrer Kennwortzurücksetzung wie erwartet erfolgt.

* [PowerShell-Skript: Aktivität zur Registrierung für die Self-Service-Kennwortzurücksetzung von Azure AD](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

## <a name="how-to-view-password-management-reports-in-the-classic-portal"></a>Anzeigen von Kennwortverwaltungsberichten im klassischen Portal
Zum Finden der Berichte für die Kennwortverwaltung führen Sie die folgenden Schritte aus:

1. Klicken Sie im **klassischen Azure-Portal** auf die [Active Directory-Erweiterung](https://manage.windowsazure.com).
2. Wählen Sie Ihr Verzeichnis aus der Liste aus, die im Portal angezeigt wird.
3. Klicken Sie auf die Registerkarte **Berichte** .
4. Sehen Sie im Abschnitt **Aktivitätsprotokolle** nach.
5. Wählen Sie entweder den Bericht **Aktivität „Zurücksetzen des Kennworts“** oder den Bericht **Aktivität „Registrierung für Zurücksetzen des Kennworts“** aus.

   ![][001]

## <a name="view-password-reset-registration-activity-in-the-classic-portal"></a>Anzeigen der Aktivität zur Registrierung für die Kennwortzurücksetzung im klassischen Portal
Der Bericht "Aktivität "Registrierung für Zurücksetzen des Kennworts"" zeigt alle Registrierungen für die Kennwortzurücksetzung, die in Ihrer Organisation erfolgt sind.  Eine Registrierung für die Kennwortzurücksetzung wird in diesem Bericht für jeden Benutzer angezeigt, der erfolgreich Authentifizierungsinformationen beim Registrierungsportal für die Kennwortzurücksetzung registriert hat ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)).

* **Max. Zeitraum**: 30 Tage
* **Max. Anzahl von Zeilen**: 75.000
* **Zum Herunterladen**: Ja, über eine CSV-Datei

    ![][002]

### <a name="description-of-report-columns"></a>Beschreibung der Berichtsspalten
In der folgende Liste werden alle Berichtsspalten im Detail beschrieben:

* **Benutzer** – Der Benutzer, der versucht hat, sich für die Kennwortzurücksetzung zu registrieren.
* **Rolle** – Die Rolle des Benutzers im Verzeichnis.
* **Datum und Uhrzeit** – Datum und Uhrzeit des Versuchs.
* **Registrierte Daten** – Die Authentifizierungsdaten, die vom Benutzer während der Registrierung für die Kennwortzurücksetzung bereitgestellt wurden.

### <a name="description-of-report-values"></a>Beschreibung der Berichtswerte
Die folgende Tabelle beschreibt die verschiedenen Werte, die für die einzelnen Spalten zulässig sind:

| Column | Zulässige Werte und ihre Bedeutung |
| --- | --- |
| Registrierte Daten |**Alternative E-Mail-Adresse** – Benutzer hat für die Authentifizierung eine alternative E-Mail-Adresse oder eine E-Mail-Adresse zur Authentifizierung verwendet.<p><p>**Bürotelefon** – Benutzer hat seine Bürotelefonnummer zur Authentifizierung verwendet.<p>**Mobiltelefon** – Benutzer hat sein Mobiltelefon oder sein Authentifizierungstelefon zum Authentifizieren verwendet.<p>**Sicherheitsfragen** – Benutzer hat Sicherheitsfragen zur Authentifizierung verwendet.<p>**Eine beliebige Kombination der oben genannten Daten (z.B. alternative E-Mail-Adresse und Mobiltelefon)** – tritt auf, wenn eine Richtlinie für die zweistufige Überprüfung angegeben ist, und zeigt, welche beiden Methoden der Benutzer zur Authentifizierung seiner Anforderung zum Zurücksetzen des Kennworts verwendet hat. |

## <a name="view-password-reset-activity-in-the-classic-portal"></a>Anzeigen der Kennwortzurücksetzungsaktivität im klassischen Portal
Dieser Bericht zeigt alle Versuche der Kennwortzurücksetzung an, die in Ihrer Organisation erfolgt sind.

* **Max. Zeitraum**: 30 Tage
* **Max. Anzahl von Zeilen**: 75.000
* **Zum Herunterladen**: Ja, über eine CSV-Datei

    ![][003]

### <a name="description-of-report-columns"></a>Beschreibung der Berichtsspalten
In der folgende Liste werden alle Berichtsspalten im Detail beschrieben:

1. **Benutzer** – Der Benutzer, der versucht hat, ein Kennwort zurückzusetzen (basierend auf dem Feld "Benutzer-ID", das bereitgestellt wird, wenn der Benutzer ein Kennwort zurückzusetzen versucht).
2. **Rolle** – Die Rolle des Benutzers im Verzeichnis.
3. **Datum und Uhrzeit** – Datum und Uhrzeit des Versuchs.
4. **Verwendete Methode(n)** – Die Authentifizierungsmethoden, die der Benutzer für diesen Zurücksetzungsvorgang verwendet hat.
5. **Ergebnis** – Das Endergebnis des Vorgangs zum Zurücksetzen des Kennworts.
6. **Details** – Die Einzelheiten dazu, warum die Kennwortzurücksetzung zu dem entsprechenden Wert geführt hat.  Enthält auch alle Maßnahmen, die Sie ergreifen können, um einen unerwarteten Fehler zu beheben.

### <a name="description-of-report-values"></a>Beschreibung der Berichtswerte
Die folgende Tabelle beschreibt die verschiedenen Werte, die für die einzelnen Spalten zulässig sind:

| Column | Zulässige Werte und ihre Bedeutung |
| --- | --- |
| Verwendete Methoden |**Alternative E-Mail-Adresse** – Benutzer hat für die Authentifizierung eine alternative E-Mail-Adresse oder eine E-Mail-Adresse zur Authentifizierung verwendet.<p>**Bürotelefon** – Benutzer hat seine Bürotelefonnummer zur Authentifizierung verwendet.<p>**Mobiltelefon** – Benutzer hat sein Mobiltelefon oder sein Authentifizierungstelefon zum Authentifizieren verwendet.<p>**Sicherheitsfragen** – Benutzer hat Sicherheitsfragen zur Authentifizierung verwendet.<p>**Eine beliebige Kombination der oben genannten Daten (z.B. alternative E-Mail-Adresse und Mobiltelefon)** – tritt auf, wenn eine Richtlinie für die zweistufige Überprüfung angegeben ist, und zeigt, welche beiden Methoden der Benutzer zur Authentifizierung seiner Anforderung zum Zurücksetzen des Kennworts verwendet hat. |
| Ergebnis |**Vorzeitig beendet** – Der Benutzer hat die Kennwortzurücksetzung gestartet, den Vorgang jedoch mittendrin beendet und nicht abgeschlossen.<p>**Blockiert** – Das Konto des Benutzers wurde an der Kennwortzurücksetzung gehindert, weil die Seite zur Kennwortzurücksetzung oder eine einzige Überprüfungsmethode zur Kennwortzurücksetzung in einem Zeitraum von 24 Stunden zu häufig verwendet wurde.<p>**Abgebrochen** – Der Benutzer hat die Kennwortzurücksetzung gestartet, aber dann auf die Schaltfläche „Abbrechen“ geklickt, um die Sitzung mittendrin abzubrechen. <p>**Administrator kontaktiert** – Beim Benutzer ist während der Sitzung ein Problem aufgetreten, das er nicht auflösen konnte. Daher hat der Benutzer auf den Link Wenden Sie sich an Ihren Administrator geklickt, statt die Kennwortzurücksetzung abzuschließen.<p>**Fehler** – Der Benutzer konnte ein Kennwort nicht zurücksetzen, wahrscheinlich weil der Benutzer nicht für die Verwendung dieses Features konfiguriert wurde (z.B. keine Lizenz, fehlende Informationen für die Authentifizierung, Kennwort lokal verwaltet ohne Aktivierung der Rückschreibungsfunktion).<p>**Erfolgreich** – Die Kennwortzurücksetzung war erfolgreich. |
| Details |Beachten Sie die folgende Tabelle. |

### <a name="allowed-values-for-details-column"></a>Zulässige Werte für die Spalte "Details"
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
| Benutzer hat vor Übergabe der erforderlichen Authentifizierungsmethoden abgebrochen. |Abgebrochen |
| Benutzer hat vor Übermittlung eines neuen Kennworts abgebrochen. |Abgebrochen |
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

## <a name="next-steps"></a>Nächste Schritte
Im Folgenden finden Sie Links zu allen Webseiten mit Informationen zur Kennwortzurücksetzung für Azure AD:

* **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – Erfahren Sie mehr über die sechs verschiedenen Komponenten des Diensts und deren Funktionen.
* [**Erste Schritte**](active-directory-passwords-getting-started.md) – Erfahren Sie, wie Sie Benutzern das Zurücksetzen und Ändern ihrer Cloud- oder lokalen Kennwörter erlauben.
* [**Anpassen**](active-directory-passwords-customize.md) – Erfahren Sie, wie Sie das Aussehen und Verhalten des Diensts an die Anforderungen Ihrer Organisation anpassen.
* [**Best Practices**](active-directory-passwords-best-practices.md) – Erfahren Sie, wie Sie Kennwörter in Ihrer Organisation schnell bereitstellen und effektiv verwalten.
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) – Hier erhalten Sie Antworten auf häufig gestellte Fragen.
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – Erfahren Sie, wie Sie Probleme mit dem Dienst schnell beheben.
* [**Weitere Informationen**](active-directory-passwords-learn-more.md) – Erhalten Sie tiefgehende technische Details zur Funktionsweise des Diensts.

[001]: ./media/active-directory-passwords-get-insights/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-get-insights/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-get-insights/003.jpg "Image_003.jpg"

