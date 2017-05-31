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
ms.date: 05/09/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: aceba2cfeac03eae78e89ef46926b2a388663f09
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---

# <a name="how-to-troubleshoot-azure-active-directory-pass-through-authentication"></a>Gewusst wie: Behandlung von Problemen bei der Azure Active Directory-Passthrough-Authentifizierung

In diesem Artikel finden Sie Informationen zur Behandlung von häufigen Problemen, die bei der Installation, Registrierung oder Deinstallation von Passthrough-Authentifizierungsconnectors (entweder Azure AD Connect- oder eigenständige Connectors) sowie bei der Aktivierung und Verwendung der Azure AD-Passthrough-Authentifizierungsfunktion (Azure Active Directory) in Ihrem Mandanten auftreten können.

## <a name="issues-during-installation-of-connectors-either-via-azure-ad-connect-or-standalone"></a>Probleme bei der Installation von (Azure AD Connect- oder eigenständigen) Connectors

### <a name="an-azure-ad-application-proxy-connector-already-exists"></a>Es ist bereits ein Azure AD-Anwendungsproxy-Connector vorhanden.

Ein Passthrough-Authentifizierungsconnector kann nicht auf demselben Server wie dem eines [Azure AD-Anwendungsproxyconnectors](../../active-directory/active-directory-application-proxy-get-started.md) installiert werden. Sie müssen den Passthrough-Authentifizierungsconnector auf einem separaten Server installieren.

### <a name="an-unexpected-error-occured"></a>Ein unerwarteter Fehler ist aufgetreten.

[Sammeln Sie Connector-Protokolle](#collecting-pass-through-authentication-connector-logs) vom Server und wenden Sie sich mit Ihrem Problem an den Microsoft-Support.

## <a name="issues-during-registration-of-connectors"></a>Probleme bei der Registrierung von Connectors

### <a name="registration-of-the-connecter-failed-due-to-blocked-ports"></a>Die Registrierung des Connectors ist aufgrund blockierter Ports fehlgeschlagen.

Stellen Sie sicher, dass der Server, auf dem der Connector installiert wurde, mit unseren Dienst-URLs und den [hier](active-directory-aadconnect-pass-through-authentication.md#prerequisites) aufgeführten Ports kommunizieren kann.

### <a name="registration-of-the-connector-failed-due-to-token-or-account-authorization-errors"></a>Fehler bei der Connectorregistrierung aufgrund von Token- oder Kontoautorisierungsfehlern

Stellen Sie sicher, dass Sie ein ausschließlich für die Cloud geltendes globales Administratorkonto für alle Vorgänge zur Installation und Registrierung von Azure AD Connect- oder eigenständigen Connectors verwenden. Es gibt ein bekanntes Problem mit MFA-fähigen globalen Administratorkonten. Deaktivieren Sie als Umgehungsmaßnahme vorübergehend MFA (nur bis zum Abschluss der Vorgänge).

### <a name="an-unexpected-error-occurred"></a>Ein unerwarteter Fehler ist aufgetreten.

[Sammeln Sie Connector-Protokolle](#collecting-pass-through-authentication-connector-logs) vom Server und wenden Sie sich mit Ihrem Problem an den Microsoft-Support.

## <a name="issues-during-uninstallation-of-connectors"></a>Probleme bei der Deinstallation von Connectors

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Warnmeldung bei der Deinstallation von Azure AD Connect

Wenn Sie die Passthrough-Authentifizierung für Ihren Mandanten aktiviert haben und Sie versuchen, Azure AD Connect zu deinstallieren, wird folgende Warnmeldung angezeigt: „Benutzern ist es nur gestattet, sich bei Azure AD anzumelden, wenn Sie andere Passthrough-Authentifizierung-Agents auf anderen Servern installiert haben.“.

Vor der Deinstallation von Azure AD Connect muss eine [Hochverfügbarkeitskonfiguration](active-directory-aadconnect-pass-through-authentication.md) vorhanden sein, um einen Abbruch der Benutzeranmeldung zu verhindern.

## <a name="issues-with-enabling-the-pass-through-authentication-feature"></a>Probleme bei der Aktivierung der Passthrough-Authentifizierungsfunktion

### <a name="the-enabling-of-the-feature-failed-because-there-were-no-connectors-available"></a>Die Aktivierung der Funktion ist fehlgeschlagen, da keine Connectors verfügbar waren.

Es muss mindestens ein Connector aktiv sein, damit die Passthrough-Authentifizierung in Ihrem Mandanten aktiviert werden kann. Sie können einen Connector durch die Installation von Azure AD Connect oder als eigenständigen Connector installieren.

### <a name="the-enabling-of-the-feature-failed-due-to-blocked-ports"></a>Die Aktivierung der Funktion ist aufgrund blockierter Ports fehlgeschlagen.

Stellen Sie sicher, dass der Server, auf dem Azure AD Connect installiert ist, mit unseren Dienst-URLs und den [hier](active-directory-aadconnect-pass-through-authentication.md#prerequisites) aufgeführten Ports kommunizieren kann.

### <a name="the-enabling-of-the-feature-failed-due-to-token-or-account-authorization-errors"></a>Fehler bei der Aktivierung des Features aufgrund von Token- oder Kontoautorisierungsfehlern

Stellen Sie sicher, dass Sie ein ausschließlich für die Cloud geltendes globales Administratorkonto verwenden, wenn Sie das Feature aktivieren. Es gibt ein bekanntes Problem mit MFA-fähigen (Multi-Factor Authentication) globalen Administratorkonten. Deaktivieren Sie als Umgehungsmaßnahme vorübergehend MFA (nur bis zum Abschluss der Vorgänge).

## <a name="issues-while-operating-the-pass-through-authentication-feature"></a>Probleme bei der Verwendung der Passthrough-Authentifizierungsfunktion

### <a name="user-facing-sign-in-errors"></a>Fehler bei der Benutzeranmeldung

Die Funktion meldet folgende Benutzerfehler auf dem Bildschirm „Azure AD-Anmeldung“. Diese werden nachfolgend zusammen mit den jeweiligen Lösungsschritten ausführlich erläutert.

|Error|Beschreibung|Lösung
| --- | --- | ---
|AADSTS80001|Verbindung mit Active Directory kann nicht hergestellt werden.|Stellen Sie sicher, dass die Connector-Server Mitglieder derselben AD-Gesamtstruktur wie die Benutzer sind, deren Kennwörter überprüft werden müssen, und dass sie eine Verbindung zu Active Directory herstellen können.  
|AADSTS8002|Bei der Verbindung mit Active Directory ist ein Timeout aufgetreten.|Überprüfen Sie, ob Active Directory verfügbar ist und auf Anforderungen der Connectors antwortet.
|AADSTS80004|Der an den Connector übergebene Benutzername war ungültig.|Stellen Sie sicher, dass der Benutzer den richtigen Benutzernamen für die Anmeldung verwendet.
|AADSTS80005|Bei der Überprüfung ist eine unvorhersehbare WebException aufgetreten.|Dies ist wahrscheinlich ein vorübergehender Fehler. Wiederholen Sie die Anforderung. Sollte der Fehler weiterhin auftreten, wenden Sie sich an den Microsoft-Support.
|AADSTS80007|Bei der Kommunikation mit Active Directory ist ein Fehler aufgetreten.|Suchen Sie in den Connectorprotokollen nach weiteren Informationen, und überprüfen Sie, ob Active Directory erwartungsgemäß funktioniert.

## <a name="collecting-pass-through-authentication-connector-logs"></a>Sammeln von Protokollen zu Passthrough-Authentifizierungsconnectors

Je nach Art des Problems, müssen Sie an verschiedenen Stellen nach Protokollen zu Passthrough-Authentifizierungsconnectors suchen.

### <a name="connector-event-logs"></a>Connector-Ereignisprotokolle

Öffnen Sie bei Fehlern in Zusammenhang mit dem Connector die Ereignisanzeige auf dem Server unter **Application and Service Logs\Microsoft\AadApplicationProxy\Connector\Admin** und prüfen Sie sie.

Für detaillierte Analysen und Debugprotokolle können Sie das Protokoll „Sitzung“ aktivieren. Aktivieren Sie dieses Protokoll nicht zur Ausführung des Connectors im Normalbetrieb; verwenden Sie es ausschließlich zur Problembehandlung. Beachten Sie, dass die Protokollinhalte nur angezeigt werden, nachdem das Protokoll wieder deaktiviert wird.

### <a name="detailed-trace-logs"></a>Ausführliche Ablaufverfolgungsprotokolle

Um Benutzeranmeldefehler zu beheben, suchen Sie unter **C:\Programdata\Microsoft\Microsoft AAD Application Proxy Connector\Trace** nach Ablaufverfolgungsprotokollen. Diese Protokolle enthalten die Gründe, warum eine bestimmte Benutzeranmeldung mittels der Passthrough-Authentifizierungsfunktion fehlgeschlagen ist. Nachfolgend wird ein Beispielprotokolleintrag aufgeführt:

```
    ApplicationProxyConnectorService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

Sie können aussagekräftige Details zum Fehler (im obigen Beispiel „1328“) abrufen, indem Sie die Eingabeaufforderung öffnen und den folgenden Befehl ausführen. Hinweis: Sie müssen „1328“ durch die tatsächliche Fehlernummer ersetzen, die in Ihren Protokollen angezeigt wird.

`Net helpmsg 1328`

Das Ergebnis sollte in etwa wie folgt aussehen:

![Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Domänencontrollerprotokolle

Weitere Informationen finden Sie auch in den Sicherheitsprotokollen Ihrer Domänencontroller, sofern die Überwachungsprotokollierung aktiviert ist. Eine einfache Möglichkeit zum Abfragen von Anmeldeanforderungen, die über Passthrough-Authentifizierungsconnectors gesendet werden, ist Folgende:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe')]]</Select>
    </Query>
    </QueryList>
```

