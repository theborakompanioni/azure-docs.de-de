---
title: 'Azure AD Connect: Passthrough-Authentifizierung | Microsoft-Dokumentation'
description: "In diesem Artikel wird beschrieben, wie die Passthrough-Authentifizierung mit Azure Active Directory (Azure AD) funktioniert und wie sie Azure AD-Anmeldungen durch die Überprüfung von Benutzerkennwörtern anhand Ihres lokalen Active Directory ermöglicht."
services: active-directory
keywords: "Was ist die Passthrough-Authentifizierung mit Azure AD Connect?, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: d3c3f6ba0da73a8297f437a56f190f90274957ab
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

---

# <a name="configure-user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Konfigurieren der Benutzeranmeldung mit der Azure Active Directory-Passthrough-Authentifizierung

## <a name="what-is-azure-active-directory-azure-ad-pass-through-authentication"></a>Was ist die Azure Active Directory(Azure AD)-Passthrough-Authentifizierung?

Für Ihre Benutzer und Ihre Organisation ist die Möglichkeit, dieselben Anmeldeinformationen (Kennwörter) sowohl für die Anmeldung bei lokalen Ressourcen als auch bei cloudbasierten Diensten von Vorteil. Benutzer müssen sich ein Kennwort weniger merken. Dies sorgt für eine bessere Benutzererfahrung und verringert die Wahrscheinlichkeit, dass Benutzer Ihre Anmeldeinformationen vergessen. Hierdurch sinken wiederum Ihre Helpdesk-Kosten, da Probleme in Bezug auf Kennwörter in der Regel den Großteil der Supportressourcen beanspruchen.

Viele Organisationen verwenden die [Azure AD-Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md), eine Funktion von [Azure AD Connect](active-directory-aadconnect.md). Sie synchronisiert die Benutzerkennwörter aus einem lokalen Active Directory mit Azure AD und stellt eine Möglichkeit dar, um Benutzern dieselben Anmeldeinformationen für lokale Ressourcen und cloudbasierte Dienste bereitzustellen. In anderen Organisationen dagegen dürfen diese Kennwörter selbst im Hashformat die internen Organisationsgrenzen nicht verfügbar.

Die Azure AD-Passthrough-Authentifizierung bietet diesen Organisationen eine einfache Lösung. Wenn sich Benutzer bei Azure AD anmelden, wird sichergestellt, dass die Kennwörter dieser Benutzer direkt anhand Ihres lokalen Active Directory überprüft werden. Diese Funktion bietet darüber hinaus folgende Vorteile:

- Benutzerfreundlich
  - Die Kennwortüberprüfung wird durchgeführt, ohne dass komplexe lokale Bereitstellungen oder Netzwerkkonfigurationen erforderlich sind.
  - Es wird lediglich ein einfacher lokaler Connector verwendet, der Kennwortüberprüfungsanforderungen überwacht und auf diese reagiert.
  - Der lokale Connector verfügt über ein Feature für automatische Updates, sodass Sie automatisch Verbesserungen und Programmfehlerbehebungen erhalten.
  - Sie kann zusammen mit [Azure AD Connect](active-directory-aadconnect.md) konfiguriert werden. Der einfache lokale Connector ist auf demselben Server wie Azure AD Connect installiert.
- Schützen
  - Lokale Kennwörter werden niemals in irgendeiner Form in der Cloud gespeichert.
  - Der einfache lokale Connector stellt lediglich ausgehende Verbindungen innerhalb Ihres Netzwerks her. Daher besteht keine Notwendigkeit für die Installation des Connectors in einer DMZ.
  - Die Passthrough-Authentifizierung funktioniert nahtlos mit der Azure Multi-Factor Authentication.
- Zuverlässig und skalierbar
  - Es können zusätzliche einfache lokale Connectors auf mehreren Servern installiert werden, um eine hohe Verfügbarkeit der Anmeldeanforderungen zu erzielen.

![Azure AD-Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

In Kombination mit der Funktion zum [nahtlosen einmaligen Anmelden](active-directory-aadconnect-sso.md) müssen Ihre Benutzer zur Anmeldung bei Azure AD nicht einmal ihre Kennwörter eingeben, wenn sie auf ihren Unternehmenscomputern innerhalb des Unternehmensnetzwerks arbeiten – eine rundum ganzheitliche Erfahrung.

## <a name="whats-available-during-preview"></a>Was ist in der Preview-Version verfügbar?

>[!NOTE]
>Die Azure AD-Passthrough-Authentifizierung ist aktuell in der Preview-Version verfügbar. Es handelt sich um eine kostenlose Funktion, sodass Sie für deren Verwendung keine kostenpflichtigen Versionen von Azure AD benötigen. 

Folgende Szenarien werden in der Preview-Version vollständig unterstützt:

- Alle webbrowserbasierte Anwendungen
- Office 365-Clientanwendungen, die die [moderne Authentifizierung](https://aka.ms/modernauthga) unterstützen

Folgende Szenarien werden in der Preview-Version NICHT unterstützt:

- Ältere Office-Clientanwendungen und Exchange ActiveSync (d.h. systemeigene E-Mail-Anwendungen auf Mobilgeräten)
  - Organisationen wird empfohlen, möglichst zur modernen Authentifizierung zu wechseln. Dies ermöglicht die Unterstützung für Passthrough-Authentifizierungen, hilft jedoch auch beim Schutz Ihrer Identitäten durch Funktionen für den [bedingten Zugriff](../active-directory-conditional-access.md) wie etwa Multi-Factor Authentication.
- Azure AD Join für Windows 10-Geräte

>[!IMPORTANT]
>Als Problemumgehung für Szenarien, in denen derzeit kein Passthrough-Authentifizierungsfeature unterstützt wird (ältere Office-Clientanwendungen, Exchange ActiveSync und Azure AD Join für Windows 10-Geräte), ist die Kennwortsynchronisierung standardmäßig ebenfalls aktiviert, wenn Sie die Passthrough-Authentifizierung aktivieren. Die Synchronisierung von Kennwörtern fungiert nur in diesen speziellen Szenarien als Fallback. Wenn Sie sie nicht benötigen, können Sie die Kennwortsynchronisierung im Azure AD Connect-Assistenten auf der Seite [Optionale Features](active-directory-aadconnect-get-started-custom.md#optional-features) deaktivieren.

## <a name="how-to-enable-azure-ad-pass-through-authentication"></a>Wie wird die Azure AD-Passthrough-Authentifizierung aktiviert?

### <a name="pre-requisites"></a>Voraussetzungen

Bevor Sie die Azure AD-Passthrough-Authentifizierung aktivieren können, müssen folgende Voraussetzungen erfüllt sein:

- Sie fungieren als globaler Administrator für einen Azure AD-Mandanten.

>[!NOTE]
>Es wird dringend empfohlen, für das Konto des globalen Administrators ein rein cloudbasiertes Konto zu verwenden, damit Sie die Konfiguration Ihres Mandanten auch dann verwalten können, wenn Ihre lokalen Dienste ausfallen oder nicht erreichbar sind. Sie können ein rein cloudbasiertes Konto für den globalen Administrator hinzufügen, wie [hier](../active-directory-users-create-azure-portal.md) gezeigt wird.

- Sie verfügen über Azure AD Connect Version 1.1.486.0 oder höher. Es wird empfohlen, die [neueste Version von Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) zu verwenden.
- Ein Server unter Windows Server 2012 R2 oder höher, auf dem Azure AD Connect ausgeführt wird.
  - Dieser Server muss ein Mitglied derselben AD-Gesamtstruktur wie die der Benutzer sein, deren Kennwörter überprüft werden müssen.
  - Beachten Sie, dass ein Connector für die Passthrough-Authentifizierung auf dem gleichen Server wie Azure AD Connect installiert wird. Vergewissern Sie sich, dass die Connectorversion 1.5.58.0 oder höher ist.

>[!NOTE]
>Umgebungen mit mehreren Gesamtstrukturen werden unterstützt, wenn Gesamtstrukturvertrauensstellungen zwischen den AD-Gesamtstrukturen bestehen und das Namensuffixrouting ordnungsgemäß konfiguriert ist.

- Wenn Sie eine hohe Verfügbarkeit anstreben, benötigen Sie zusätzliche Server, auf denen Windows Server 2012 R2 oder höher ausgeführt wird, um eigenständige Connectors zu installieren (erforderliche Mindestversion: 1.5.58.0).
- Wenn zwischen einem der Connectors und Azure AD eine Firewall vorhanden ist, stellen Sie Folgendes sicher:
    - Wenn die URL-Filterung aktiviert ist, stellen Sie sicher, dass die Connectors mit den folgenden URLs kommunizieren können:
        -  \*.msappproxy.net
        -  \*.servicebus.windows.net
    - Die Connectors stellen auch direkte IP-Verbindungen zu den [IP-Bereichen des Azure-Rechenzentrums](https://www.microsoft.com/en-us/download/details.aspx?id=41653) her.
    - Stellen Sie sicher, dass die Firewall keine SSL-Überprüfung durchführt, da die Connectors Clientzertifikate zur Kommunikation mit Azure AD verwenden.
    - Stellen Sie sicher, dass die Connectors über die Ports 80 und 443 ausgehende Anforderungen an Azure AD senden können.
      - Wenn Ihre Firewall Regeln gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden.
      - Die Connectors stellen HTTP-Anforderungen über Port 80, um SSL-Zertifikatsperrlisten herunterzuladen. Dies wird auch für die ordnungsgemäße Funktion automatischer Updates benötigt.
      - Die Connectors stellen die HTTPS-Anforderungen für alle anderen Vorgänge über Port 443 bereit. Dazu gehören z.B. das Aktivieren und Deaktivieren des Features, das Registrieren von Connectors, das Herunterladen von Connectorupdates und das Verarbeiten aller Benutzeranmeldeanforderungen.

>[!NOTE]
>Wir haben kürzlich Verbesserungen vorgenommen, um die Anzahl der Ports, die von den Connectors zur Kommunikation mit unserem Dienst benötigt werden, zu reduzieren. Wenn ältere Versionen von Azure AD Connect- und/oder eigenständige Connectors ausgeführt werden, sollten Sie diese zusätzlichen Ports (5671, 8080, 9090, 9091, 9350, 9352, 10100-10120) weiterhin geöffnet lassen.

### <a name="enabling-azure-ad-pass-through-authentication"></a>Aktivieren der Azure AD-Passthrough-Authentifizierung

Die Azure AD-Passthrough-Authentifizierung kann über Azure AD Connect aktiviert werden.

Wenn Sie eine neue Installation von Azure AD Connect durchführen, wählen Sie den [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md). Wählen Sie auf der Seite „Benutzeranmeldung“ die Option „Passthrough-Authentifizierung“ aus. Nach erfolgreicher Durchführung wird hierdurch auf demselben Server wie Azure AD Connect ein Passthrough-Authentifizierungs-Connector installiert. Darüber hinaus wird die Passthrough-Authentifizierungsfunktion für Ihren Mandanten aktiviert.

![Azure AD Connect – Benutzeranmeldung](./media/active-directory-aadconnect-sso/sso3.png)

Wenn bereits eine Installation von Azure AD Connect vorhanden ist, richten Sie den [Expressinstallations-](active-directory-aadconnect-get-started-express.md) oder [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md) ein, wählen Sie in Azure AD Connect „Benutzeranmeldeseite ändern“ und klicken Sie auf „Weiter“. Wählen Sie dann „Passthrough-Authentifizierung“, um auf demselben Server wie Azure AD Connect einen Passthrough-Authentifizierungs-Connector zu installieren, und aktivieren Sie die Funktion für Ihren Mandanten.

![Azure AD Connect – Benutzeranmeldung ändern](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Die Azure AD-Passthrough-Authentifizierung ist eine Funktion auf Mandantenebene. Sie wirkt sich auf die Benutzeranmeldung in allen verwalteten Domänen in Ihrem Mandanten aus. Benutzer von Verbunddomänen melden sich jedoch weiterhin mithilfe der Active Directory-Verbunddienste (AD FS) oder einem anderen Verbundanbieter an, den Sie zuvor konfiguriert haben. Wenn Sie eine Verbunddomäne in eine verwaltete Domäne konvertieren, melden sich alle Benutzer in dieser Domäne automatisch mit der Passthrough-Authentifizierung an. Reine Cloudbenutzer sind nicht von der Passthrough-Authentifizierung betroffen.

### <a name="ensuring-high-availability"></a>Sicherstellen einer hohen Verfügbarkeit

Wenn Sie den Einsatz der Passthrough-Authentifizierung in einer Produktionsbereitstellung planen, wird dringend empfohlen, einen zweiten Connector auf einem separaten Server zu installieren (neben dem Server, auf dem Azure AD Connect und der erste Connector ausgeführt wird), um eine hohe Verfügbarkeit der Anmeldeanforderungen sicherzustellen. Sie können beliebig viele zusätzliche Connectors installieren. Dies hängt von der Höchstanzahl und der durchschnittlichen Anzahl der Anmeldeanforderungen ab, die Ihr Mandant verarbeitet.

Befolgen Sie die nachstehenden Anweisungen zum Bereitstellen eines eigenständigen Connectors:

#### <a name="step-1-download-and-install-the-connector"></a>Schritt 1: Herunterladen und Installieren des Connectors

In diesem Schritt laden Sie die Connector-Software herunter und installieren diese auf Ihrem Server.

1.    Den aktuellen Connector können Sie [hier](https://go.microsoft.com/fwlink/?linkid=837580) herunterladen. Vergewissern Sie sich, dass die Connectorversion 1.5.58.0 oder höher ist.
2.    Öffnen Sie eine Eingabeaufforderung als Administrator.
3.    Führen Sie den folgenden Befehl aus („/q“ steht für die unbeaufsichtigte Installation, d.h., Sie werden bei der Installation nicht aufgefordert, den Endbenutzer-Lizenzvertrag zu akzeptieren.):

`
AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q
`
>[!NOTE]
>Sie können nur einen Connector pro Server installieren.

#### <a name="step-2-register-the-connector-with-azure-ad-for-pass-through-authentication"></a>Schritt 2: Registrieren des Connectors bei Azure AD für die Passthrough-Authentifizierung

In diesem Schritt registrieren Sie den installierten Connector auf Ihrem Server mithilfe unseres Dienstes und stellen ihn für den Empfang von Anmeldeanforderungen zur Verfügung.

1.    Öffnen Sie ein PowerShell-Fenster als Administrator.
2.    Navigieren Sie zu **C:\Programme\Microsoft AAD App Proxy Connector**, und führen Sie das folgende Skript aus: `.\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Feature PassthroughAuthentication`
3.    Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen des globalen Administratorkontos für Ihren Azure AD-Mandanten ein.

## <a name="how-does-azure-ad-pass-through-authentication-work"></a>Wie funktioniert die Azure AD-Passthrough-Authentifizierung?

Wenn ein Benutzer versucht, sich bei Azure AD anzumelden (und die Passthrough-Authentifizierung im Mandanten aktiviert ist), geschieht Folgendes:

1. Der Benutzer gibt auf der Seite „Azure AD-Anmeldung“ seinen Benutzernamen und sein Kennwort ein. Unser Dienst platziert den Benutzernamen und das (mit einem öffentlichen Schlüssel verschlüsselte) Kennwort in eine Warteschlange für die Überprüfung.
2. Einer der verfügbaren lokalen Connectors führt einen ausgehenden Aufruf an die Warteschlange durch und ruft den Benutzernamen und das Kennwort ab.
3. Der Connector überprüft dann mithilfe von Standard-Windows-APIs den Benutzernamen und das Kennwort anhand Ihres Active Directory (ein ähnlicher Mechanismus wie bei AD FS). Beachten Sie, dass es sich beim Benutzernamen entweder um den lokalen Benutzernamen (in der Regel „userPrincipalName“) oder um ein anderes (als „Alternative ID“ bezeichnetes) Attribut, das in Azure AD Connect konfiguriert ist, handeln kann.
4. Der lokale Domänencontroller wertet anschließend die Anforderung aus und gibt eine Antwort (erfolgreich oder fehlgeschlagen) an den Connector zurück.
5. Der Connector gibt diese Antwort wiederum an Azure AD zurück.
6. Azure AD wertet anschließend die Antwort aus und gibt eine entsprechende Antwort an den Benutzer zurück. Beispielsweise weist es der Anwendung einen Token zurück oder fordert zur Multi-Factor Authentication auf.

Die folgende Abbildung veranschaulicht ebenfalls die verschiedenen Schritte. Beachten Sie, dass alle Anforderungen und Antworten über den HTTPS-Kanal erfolgen.

![Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

### <a name="note-about-password-writeback"></a>Hinweis zum Kennwortrückschreiben

Falls Sie das [Kennwortrückschreiben](../active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) in Ihrem Mandanten und für einen bestimmten Benutzer konfiguriert haben, können diese ihre Kennwörter bei der Anmeldung über die Passthrough-Authentifizierung wie bisher ändern oder zurücksetzen. Die Kennwörter werden erwartungsgemäß in Ihr lokales Active Directory zurückgeschrieben.

Wenn eine der folgenden Bedingungen nicht wahr ist (das Kennwortrückschreiben ist nicht für Ihren Mandanten konfiguriert oder dem Benutzer ist keine gültige Azure AD-Lizenz zugewiesen), ist es dem Benutzer nicht gestattet, seine Kennwörter in der Cloud zu aktualisieren. Dies ist auch der Fall, wenn sein Kennwort abgelaufen ist. Der Benutzer sieht dann folgende Meldung: „Eine Kennwortänderung auf dieser Website wird von Ihrer Organisation nicht gestattet. Ändern Sie Ihr Kennwort anhand der von Ihrer Organisation empfohlenen Methode, oder bitten Sie Ihren Administrator um Hilfe.“.

## <a name="troubleshooting-pass-through-authentication"></a>Problembehandlung bei der Passthrough-Authentifizierung

In diesem Abschnitt finden Sie Informationen zur Behebung von einigen häufigen Problemen, die bei der Installation, Registrierung oder Deinstallation von Passthrough-Authentifizierungs-Connectors (entweder Azure AD Connect- oder eigenständige Connectors) sowie bei der Aktivierung und Verwendung der Funktion in Ihrem Mandanten auftreten können.

### <a name="issues-during-installation-of-connectors-either-via-azure-ad-connect-or-standalone"></a>Probleme bei der Installation von (Azure AD Connect- oder eigenständigen) Connectors

#### <a name="an-azure-ad-application-proxy-connector-already-exists"></a>Es ist bereits ein Azure AD-Anwendungsproxy-Connector vorhanden.

Ein Passthrough-Authentifizierungs-Connector kann nicht auf demselben Server wie dem eines [Azure AD-Anwendungsproxy](../../active-directory/active-directory-application-proxy-get-started.md)-Connectors installiert werden. Sie müssen den Passthrough-Authentifizierungs-Connector auf einem separaten Server installieren.

#### <a name="an-unexpected-error-occured"></a>Ein unerwarteter Fehler ist aufgetreten.

[Sammeln Sie Connector-Protokolle](#collecting-pass-through-authentication-connector-logs) vom Server und wenden Sie sich mit Ihrem Problem an den Microsoft-Support.

### <a name="issues-during-registration-of-connectors"></a>Probleme bei der Registrierung von Connectors

#### <a name="registration-of-the-connecter-failed-due-to-blocked-ports"></a>Die Registrierung des Connectors ist aufgrund blockierter Ports fehlgeschlagen.

Stellen Sie sicher, dass der Server, auf dem der Connector installiert wurde, mit unseren Dienst-URLs und den [hier](#pre-requisites) aufgeführten Ports kommunizieren kann.

#### <a name="registration-of-the-connector-failed-due-to-token-or-account-authorization-errors"></a>Fehler bei der Connectorregistrierung aufgrund von Token- oder Kontoautorisierungsfehlern

Stellen Sie sicher, dass Sie ein ausschließlich für die Cloud geltendes globales Administratorkonto für alle Vorgänge zur Installation und Registrierung von Azure AD Connect- oder eigenständigen Connectors verwenden. Es gibt ein bekanntes Problem mit MFA-fähigen globalen Administratorkonten. Deaktivieren Sie als Umgehungsmaßnahme vorübergehend MFA (nur bis zum Abschluss der Vorgänge).

#### <a name="an-unexpected-error-occurred"></a>Ein unerwarteter Fehler ist aufgetreten.

[Sammeln Sie Connector-Protokolle](#collecting-pass-through-authentication-connector-logs) vom Server und wenden Sie sich mit Ihrem Problem an den Microsoft-Support.

### <a name="issues-during-un-installation-of-connectors"></a>Probleme bei der Deinstallation von Connectors

#### <a name="warning-message-when-un-installing-azure-ad-connect"></a>Warnmeldung bei der Deinstallation von Azure AD Connect

Wenn Sie die Passthrough-Authentifizierung für Ihren Mandanten aktiviert haben und Sie versuchen, Azure AD Connect zu deinstallieren, wird folgende Warnmeldung angezeigt: „Benutzern ist es nur gestattet, sich bei Azure AD anzumelden, wenn Sie andere Passthrough-Authentifizierung-Agents auf anderen Servern installiert haben.“.

Vor der Deinstallation von Azure AD Connect muss eine [Hochverfügbarkeitskonfiguration](#ensuring-high-availability) vorhanden sein, um einen Abbruch der Benutzeranmeldung zu verhindern.

### <a name="issues-with-enabling-the-pass-through-authentication-feature"></a>Probleme bei der Aktivierung der Passthrough-Authentifizierungsfunktion

#### <a name="the-enabling-of-the-feature-failed-because-there-were-no-connectors-available"></a>Die Aktivierung der Funktion ist fehlgeschlagen, da keine Connectors verfügbar waren.

Es muss mindestens ein Connector aktiv sein, damit die Passthrough-Authentifizierung in Ihrem Mandanten aktiviert werden kann. Sie können einen Connector durch die Installation von Azure AD Connect oder als eigenständigen Connector installieren.

#### <a name="the-enabling-of-the-feature-failed-due-to-blocked-ports"></a>Die Aktivierung der Funktion ist aufgrund blockierter Ports fehlgeschlagen.

Stellen Sie sicher, dass der Server, auf dem Azure AD Connect installiert ist, mit unseren Dienst-URLs und den [hier](#pre-requisites) aufgeführten Ports kommunizieren kann.

#### <a name="the-enabling-of-the-feature-failed-due-to-token-or-account-authorization-errors"></a>Fehler bei der Aktivierung des Features aufgrund von Token- oder Kontoautorisierungsfehlern

Stellen Sie sicher, dass Sie ein ausschließlich für die Cloud geltendes globales Administratorkonto verwenden, wenn Sie das Feature aktivieren. Es gibt ein bekanntes Problem mit MFA-fähigen (Multi-Factor Authentication) globalen Administratorkonten. Deaktivieren Sie als Umgehungsmaßnahme vorübergehend MFA (nur bis zum Abschluss der Vorgänge).

### <a name="issues-while-operating-the-pass-through-authentication-feature"></a>Probleme bei der Verwendung der Passthrough-Authentifizierungsfunktion

#### <a name="user-facing-sign-in-errors"></a>Fehler bei der Benutzeranmeldung

Die Funktion meldet folgende Benutzerfehler auf dem Bildschirm „Azure AD-Anmeldung“. Diese werden nachfolgend zusammen mit den jeweiligen Lösungsschritten ausführlich erläutert.

|Error|Beschreibung|Lösung
| --- | --- | ---
|AADSTS80001|Verbindung mit Active Directory kann nicht hergestellt werden.|Stellen Sie sicher, dass die Connector-Server Mitglieder derselben AD-Gesamtstruktur wie die Benutzer sind, deren Kennwörter überprüft werden müssen, und dass sie eine Verbindung zu Active Directory herstellen können.  
|AADSTS8002|Bei der Verbindung mit Active Directory ist ein Timeout aufgetreten.|Überprüfen Sie, ob Active Directory verfügbar ist und auf Anforderungen der Connectors antwortet.
|AADSTS80004|Der an den Connector übergebene Benutzername war ungültig.|Stellen Sie sicher, dass der Benutzer den richtigen Benutzernamen für die Anmeldung verwendet.
|AADSTS80005|Bei der Überprüfung ist eine unvorhersehbare WebException aufgetreten.|Dies ist wahrscheinlich ein vorübergehender Fehler. Wiederholen Sie die Anforderung. Sollte der Fehler weiterhin auftreten, wenden Sie sich an den Microsoft-Support.
|AADSTS80007|Bei der Kommunikation mit Active Directory ist ein Fehler aufgetreten.|Suchen Sie in den Connectorprotokollen nach weiteren Informationen, und überprüfen Sie, ob Active Directory erwartungsgemäß funktioniert.

### <a name="collecting-pass-through-authentication-connector-logs"></a>Sammeln von Protokollen zu Passthrough-Authentifizierungs-Connectors

Je nach Art des Problems, müssen Sie an verschiedenen Stellen nach Protokollen zu Passthrough-Authentifizierungs-Connectors suchen.

#### <a name="connector-event-logs"></a>Connector-Ereignisprotokolle

Öffnen Sie bei Fehlern in Zusammenhang mit dem Connector die Ereignisanzeige auf dem Server unter **Application and Service Logs\Microsoft\AadApplicationProxy\Connector\Admin** und prüfen Sie sie.

Für detaillierte Analysen und Debugprotokolle können Sie das Protokoll „Sitzung“ aktivieren. Aktivieren Sie dieses Protokoll nicht zur Ausführung des Connectors im Normalbetrieb; verwenden Sie es ausschließlich zur Problembehandlung. Beachten Sie, dass die Protokollinhalte nur angezeigt werden, nachdem das Protokoll wieder deaktiviert wird.

#### <a name="detailed-trace-logs"></a>Ausführliche Ablaufverfolgungsprotokolle

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

#### <a name="domain-controller-logs"></a>Domänencontrollerprotokolle

Weitere Informationen finden Sie auch in den Sicherheitsprotokollen Ihrer Domänencontroller, sofern die Überwachungsprotokollierung aktiviert ist. Eine einfache Möglichkeit zum Abfragen von Anmeldeanforderungen, die über Passthrough-Authentifizierungs-Connectors gesendet werden, ist Folgende:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe')]]</Select>
    </Query>
    </QueryList>
```

## <a name="feedback"></a>Feedback

Ihr Feedback ist uns sehr wichtig. Sie können uns eine E-Mail an [aadopauthfeedback@microsoft.com](mailto:aadopauthfeedback@microsoft.com) senden. Wenn Sie Fragen zu neuen Funktionen haben, nutzen Sie unser [UserVoice-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect). Wir helfen Ihnen gerne weiter.

