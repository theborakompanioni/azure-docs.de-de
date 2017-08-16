---
title: 'Azure AD Connect: Behandlung von Passthrough-Authentifizierungsproblemen | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Probleme bei der Azure AD-Passthrough-Authentifizierung (Azure Active Directory) behandelt werden.
services: active-directory
keywords: "Probleme bei der Passthrough-Authentifizierung mit Azure AD Connect behandeln, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: 72bd39bcf720cf5704274fcdfa0f2b8fc44a77bc
ms.contentlocale: de-de
ms.lasthandoff: 08/07/2017

---

# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Behandlung von Problemen bei der Azure Active Directory-Passthrough-Authentifizierung

In diesem Artikel hilft Ihnen beim Auffinden von Informationen zur Problembehandlung von bekannten Problemen mit der Azure AD-Passthrough-Authentifizierung.

>[!IMPORTANT]
>Wenn Sie es mit Benutzeranmeldeproblemen bei der Passthrough-Authentifizierung zu tun haben, deaktivieren das Feature nicht. Deinstallieren auch keine Agents für die Passthrough-Authentifizierung ohne ein ausschließlich cloudbasiertes globales Administratorkonto, auf das ein Fallback ausgeführt werden kann. Erfahren Sie, wie Sie ein [rein cloudbasiertes Konto für den globalen Administrator hinzufügen](../active-directory-users-create-azure-portal.md). Dieser Schritt ist unerlässlich und stellt sicher, dass Sie sich nicht aus Ihrem Mandanten aussperren.

## <a name="general-issues"></a>Allgemeine Probleme

### <a name="check-status-of-the-feature-and-authentication-agents"></a>Überprüfen des Status der Funktion und der Authentifizierungs-Agents

Stellen Sie sicher, dass die Passthrough-Authentifizierung in Ihrem Mandanten immer noch **aktiviert** ist und der Status der Authentifizierungs-Agents als **aktiv** angezeigt wird, nicht als **inaktiv**. Um den Status zu überprüfen, können Sie zum **Azure AD Connect**-Blatt im [Azure Active Directory-Admin Center](https://aad.portal.azure.com/) navigieren.

![Azure Active Directory-Admin Center – Azure AD Connect-Blatt](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory-Admin Center – Passthrough-Authentifizierungs-Blatt](./media/active-directory-aadconnect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Benutzerseitige Fehler bei der Anmeldung

Wenn der Benutzer sich nicht mit der Passthrough-Authentifizierung anmelden kann, wird ihm möglicherweise einer der folgenden benutzerseitigen Fehler auf der Azure AD-Anmeldeseite angezeigt: 

|Error|Beschreibung|Lösung
| --- | --- | ---
|AADSTS80001|Verbindung mit Active Directory kann nicht hergestellt werden.|Ensure that agent servers are members of the same AD forest as the users whose passwords need to be validated and they are able to connect to Active Directory. (Stellen Sie sicher, dass die Agent-Server Mitglieder derselben AD-Gesamtstruktur wie die Benutzer sind, deren Kennwörter überprüft werden müssen, und dass sie eine Verbindung zu Active Directory herstellen können.)  
|AADSTS8002|Bei der Verbindung mit Active Directory ist ein Timeout aufgetreten.|Check to ensure that Active Directory is available and is responding to requests from the agents. (Überprüfen Sie, ob Active Directory verfügbar ist und auf Anforderungen der Agents antwortet.)
|AADSTS80004|The username passed to the agent was not valid (Der an den Agent übergebene Benutzername war ungültig.)|Stellen Sie sicher, dass der Benutzer den richtigen Benutzernamen für die Anmeldung verwendet.
|AADSTS80005|Bei der Überprüfung ist eine unvorhersehbare WebException aufgetreten.|A transient error. (Vorübergehender Fehler.) Wiederholen Sie die Anforderung. Sollte der Fehler weiterhin auftreten, wenden Sie sich an den Microsoft-Support.
|AADSTS80007|Bei der Kommunikation mit Active Directory ist ein Fehler aufgetreten.|Check the agent logs for more information and verify that Active Directory is operating as expected. (Suchen Sie in den Agent-Protokollen nach weiteren Informationen, und überprüfen Sie, ob Active Directory erwartungsgemäß funktioniert.)

### <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center"></a>Gründe für Anmeldefehler im Azure Active Directory Admin Center

Beginnen Sie mit der Behebung von Problemen bei der Benutzeranmeldung, indem Sie sich den [Bericht zu Anmeldeaktivitäten](../active-directory-reporting-activity-sign-ins.md) im [Azure Active Directory-Admin Center](https://aad.portal.azure.com/) ansehen.

![Azure Active Directory-Admin Center – Bericht zu Anmeldeaktivitäten](./media/active-directory-aadconnect-pass-through-authentication/pta4.png)

Navigieren Sie im [Azure Active Directory Admin Center](https://aad.portal.azure.com/) zu **Azure Active Directory** -> **Anmeldungen**, und klicken Sie auf die Anmeldeaktivität eines bestimmten Benutzers. Suchen Sie nach dem Feld **Code des Anmeldefehlers**. Ordnen Sie den Wert in diesem Feld mithilfe der folgenden Tabelle einer Ursache und einer Lösung zu:

|Anmeldefehler|Grund des Anmeldefehlers|Lösung
| --- | --- | ---
| 50144 | Das Active Directory-Kennwort des Benutzers ist abgelaufen. | Reset the user's password in your on-premises Active Directory. (Setzen Sie das Kennwort des Benutzers in Ihrem lokalen Active Directory zurück.)
| 80001 | Kein Authentifizierungs-Agent verfügbar. | Install and register an Authentication Agent. (Installieren und registrieren Sie einen Authentifizierungs-Agent.)
| 80002 | Zeitüberschreitung für die Anforderung zur Kennwortüberprüfung des Authentifizierungs-Agents. | Überprüfen Sie, ob Ihr Active Directory über den Authentifizierungs-Agent erreichbar ist.
| 80003 | Der Authentifizierungs-Agent hat eine ungültige Antwort erhalten. | If the problem is consistently reproducible across multiple users, check your Active Directory configuration. (Wenn das Problem bei mehreren Benutzer genauso reproduziert werden kann, überprüfen Sie die Active Directory-Konfiguration.)
| 80004 | In der Anmeldeanforderung wurde ein falscher Benutzerprinzipalname (UPN) verwendet. | Ask the user to sign in with the correct username. (Fordern Sie den Benutzer dazu auf, sich mit dem richtigen Benutzernamen anzumelden.)
| 80005 | Authentifizierungs-Agent: Fehler aufgetreten. | Transient error. (Vorübergehender Fehler.) Versuchen Sie es später erneut.
| 80007 | Der Authentifizierungs-Agent kann keine Verbindung mit Active Directory herstellen. | Überprüfen Sie, ob Ihr Active Directory über den Authentifizierungs-Agent erreichbar ist.
| 80010 | Der Authentifizierungs-Agent kann das Kennwort nicht entschlüsseln. | If the problem is consistently reproducible, install and register a new Authentication Agent. (Wenn das Problem konsistent reproduziert werden kann, installieren und registrieren Sie einen neuen Authentifizierungs-Agent.) And uninstall the current one. (Deinstallieren der Sie außerdem den aktuellen.) 
| 80011 | Der Authentifizierungs-Agent kann den Entschlüsselungsschlüssel nicht abrufen. | If the problem is consistently reproducible, install and register a new Authentication Agent. (Wenn das Problem konsistent reproduziert werden kann, installieren und registrieren Sie einen neuen Authentifizierungs-Agent.) And uninstall the current one. (Deinstallieren der Sie außerdem den aktuellen.)

## <a name="authentication-agent-installation-issues"></a>Probleme bei der Installation des Authentifizierungs-Agents

### <a name="an-unexpected-error-occurred"></a>Ein unerwarteter Fehler ist aufgetreten.

[Sammeln Sie Agent-Protokolle](#collecting-pass-through-authentication-agent-logs) vom Server, und wenden Sie sich mit Ihrem Problem an den Microsoft-Support.

## <a name="authentication-agent-registration-issues"></a>Probleme bei der Registrierung des Authentifizierungs-Agents

### <a name="registration-of-the-authentication-agent-failed-due-to-blocked-ports"></a>Registration of the Authentication Agent failed due to blocked ports (Fehler bei der Registrierung des Authentifizierungs-Agents aufgrund von blockierten Ports)

Stellen Sie sicher, dass der Server, auf dem der Authentifizierungs-Agent installiert wurde, mit unseren Dienst-URLs und den [hier](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites) aufgeführten Ports kommunizieren kann.

### <a name="registration-of-the-authentication-agent-failed-due-to-token-or-account-authorization-errors"></a>Registration of the Authentication Agent failed due to token or account authorization errors (Fehler bei der Registrierung des Authentifizierungs-Agents aufgrund von Token- oder Kontoautorisierungsfehlern)

Stellen Sie sicher, dass Sie ein ausschließlich für die Cloud geltendes globales Administratorkonto für alle Vorgänge zur Installation und Registrierung von Azure AD Connect- oder eigenständigen Authentifizierungs-Agents verwenden. Es gibt ein bekanntes Problem mit MFA-fähigen globalen Administratorkonten. Deaktivieren Sie als Umgehungsmaßnahme vorübergehend MFA (nur bis zum Abschluss der Vorgänge).

### <a name="an-unexpected-error-occurred"></a>Ein unerwarteter Fehler ist aufgetreten.

[Sammeln Sie Agent-Protokolle](#collecting-pass-through-authentication-agent-logs) vom Server, und wenden Sie sich mit Ihrem Problem an den Microsoft-Support.

## <a name="authentication-agent-uninstallation-issues"></a>Probleme bei der Deinstallation von Authentifizierungs-Agents

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Warnmeldung bei der Deinstallation von Azure AD Connect

Wenn Sie die Passthrough-Authentifizierung für Ihren Mandanten aktiviert haben und Sie versuchen, Azure AD Connect zu deinstallieren, wird folgende Warnmeldung angezeigt: „Users will not be able to sign-in to Azure AD unless you have other Pass-through Authentication agents installed on other servers“ (Benutzern ist es nur gestattet, sich bei Azure AD anzumelden, wenn Sie andere Passthrough-Authentifizierung-Agents auf anderen Servern installiert haben).

Stellen Sie sicher, dass das Setup [hoch verfügbar](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) ist, bevor Sie Azure AD Connect deinstallieren, um eine Unterbrechung von Benutzeranmeldungen zu vermeiden.

## <a name="issues-with-enabling-the-feature"></a>Probleme bei der Aktivierung des Features

### <a name="enabling-the-feature-failed-because-there-were-no-authentication-agents-available"></a>Enabling the feature failed because there were no Authentication Agents available (Beim Aktivieren des Features ist ein Fehler aufgetreten, weil keine Authentifizierungs-Agents verfügbar waren)

Es muss mindestens ein Authentifizierungs-Agent aktiv sein, damit die Passthrough-Authentifizierung in Ihrem Mandanten aktiviert werden kann. Sie können einen Authentifizierungs-Agent über Azure AD Connect oder als eigenständigen Authentifizierungs-Agent installieren.

### <a name="enabling-the-feature-failed-due-to-blocked-ports"></a>Enabling the feature failed due to blocked ports (Beim Aktivierung des Features ist aufgrund blockierter Ports ein Fehler aufgetreten)

Stellen Sie sicher, dass der Server, auf dem Azure AD Connect installiert ist, mit unseren Dienst-URLs und den [hier](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites) aufgeführten Ports kommunizieren kann.

### <a name="enabling-the-feature-failed-due-to-token-or-account-authorization-errors"></a>Enabling the feature failed due to token or account authorization errors (Beim Aktivieren des Features ist aufgrund von Token- oder Kontoautorisierungsfehlern ein Fehler aufgetreten)

Stellen Sie sicher, dass Sie ein ausschließlich für die Cloud geltendes globales Administratorkonto verwenden, wenn Sie das Feature aktivieren. Es gibt ein bekanntes Problem mit MFA-fähigen (Multi-Factor Authentication) globalen Administratorkonten. Deaktivieren Sie als Umgehungsmaßnahme vorübergehend MFA (nur bis zum Abschluss der Vorgänge).

## <a name="exchange-activesync-configuration-issues"></a>Exchange ActiveSync – Konfigurationsprobleme

Dies sind die allgemeine Probleme beim Konfigurieren der Exchange ActiveSync-Unterstützung für die Passthrough-Authentifizierung.

### <a name="exchange-powershell-issue"></a>Exchange PowerShell-Problem

Wenn Sie die Fehlermeldung „**Es wurde kein Parameter gefunden, der dem Parameternamen ‚PerTenantSwitchToESTSEnabled‘ entspricht\.**“ sehen,  wenn Sie den Exchange PowerShell-Befehl `Set-OrganizationConfig` ausführen, wenden Sie sich an den Microsoft-Support.

### <a name="exchange-activesync-not-working"></a>Exchange ActiveSync funktioniert nicht

Es dauert einige Zeit, bis die Konfiguration wirksam wird – der Zeitraum hängt von Ihrer Umgebung ab. Wenn die Situation längere Zeit anhält, wenden Sie sich an den Microsoft-Support.

## <a name="collecting-pass-through-authentication-agent-logs"></a>Sammeln von Protokollen von Passthrough-Authentifizierungs-Agent

Je nach Problem müssen Sie an verschiedenen Stellen nach Protokollen von Passthrough-Authentifizierungs-Agents suchen.

### <a name="authentication-agent-event-logs"></a>Ereignisprotokolle von Authentifizierungs-Agents

Öffnen Sie bei Fehlern in Zusammenhang mit dem Authentifizierungs-Agent die Ereignisanzeige auf dem Server unter **Anwendungs- und Dienstprotokolle\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**, und prüfen Sie sie.

Aktivieren Sie für die detaillierte Analyse da Protokoll „Session“ (Sitzung). Führen Sie den Authentifizierungs-Agent im Normalbetrieb nicht mit diesem Protokoll aus. Verwenden Sie es ausschließlich zur Problembehandlung. Die Protokollinhalte werden nur angezeigt, nachdem das Protokoll wieder deaktiviert wird.

### <a name="detailed-trace-logs"></a>Ausführliche Ablaufverfolgungsprotokolle

Um Benutzeranmeldefehler zu beheben, suchen Sie unter **%programdata%\Microsoft\Azure AD Connect Authentication Agent\Trace\\** nach Ablaufverfolgungsprotokollen. Diese Protokolle enthalten die Gründe, warum eine bestimmte Benutzeranmeldung mittels der Passthrough-Authentifizierungsfunktion fehlgeschlagen ist. Diese Fehler werden auch den Gründe für benutzerseitige Anmeldefehler zugeordnet, die zuvor in der [Tabelle](#sign-in-failure-reasons-on-the-Azure-portal) gezeigt wurden. Es folgt ein Beispiel für einen Protokolleintrag:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

Sie können aussagekräftige Details zum Fehler (im obigen Beispiel „1328“) abrufen, indem Sie die Eingabeaufforderung öffnen und den folgenden Befehl ausführen. Hinweis: Ersetzen Sie 1328 durch die aktuelle Fehlernummer in Ihren Protokollen:

`Net helpmsg 1328`

![Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Domänencontrollerprotokolle

Weitere Informationen finden Sie auch in den Sicherheitsprotokollen Ihrer Domänencontroller, sofern die Überwachungsprotokollierung aktiviert ist. Eine einfache Möglichkeit zum Abfragen von Anmeldeanforderungen, die über Passthrough-Authentifizierungs-Agents gesendet werden, ist Folgende:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

### <a name="performance-monitor-counters"></a>Leistungsüberwachungsindikatoren

Eine weitere Möglichkeit zum Überwachen des Authentifizierung-Agents ist das Nachverfolgen bestimmter Leistungsüberwachungsindikatoren auf jedem Server, auf dem der Authentifizierung-Agent installiert ist. Verwenden Sie die folgenden globalen Indikatoren **# PTA authentications**, **#PTA failed authentications** und **#PTA successful authentications**) und Fehlerzähler (**# PTA authentication errors**):

![Passthrough-Authentifizierungs-Leistungsüberwachungsindikatoren](./media/active-directory-aadconnect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Die Passthrough-Authentifizierung bietet mit mehreren Authentifizierungs-Agents hohe Verfügbarkeit, jedoch _keinen_ Lastenausgleich. Abhängig von Ihrer Konfiguration erhalten _nicht_ alle Ihre Authentifizierungs-Agents ungefähr die _gleiche_ Anzahl von Anforderungen. Es ist möglich, dass ein bestimmter Authentifizierungs-Agent überhaupt keinen Datenverkehr empfängt.

