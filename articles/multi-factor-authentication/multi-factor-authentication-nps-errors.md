---
title: "Fehlercodes zur Problembehandlung für die Azure MFA NPS-Erweiterung | Microsoft-Dokumentation"
description: "Hier erhalten Sie anhand von Lösungen für häufige Fehlermeldungen Hilfe beim Beheben von Problemen mit der NPS-Erweiterung für Azure Multi-Factor Authentication"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/28/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 95c1eb534b4b51db18a2caf46f17a559243ea036
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---

# <a name="resolve-error-messages-from-the-nps-extension-for-azure-multi-factor-authentication"></a>Auflösen von Fehlermeldungen in der NPS-Erweiterung für Azure Multi-Factor Authentication

Wenn Fehler in der NPS-Erweiterung für Azure Multi-Factor Authentication auftreten, können Sie mit diesem Artikel schneller eine Lösung erreichen. 

## <a name="troubleshooting-steps-for-common-errors"></a>Schritte zur Problembehandlung bei häufigen Fehlern

| Fehlercode | Schritte zur Problembehandlung |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Kontaktieren Sie den Support](#contact-microsoft-support), und geben Sie die Liste der Schritte zum Erfassen von Protokollen an. Stellen Sie so viele Informationen wie möglich über das bereit, was vor dem Fehler passiert ist, einschließlich Mandanten-ID und Benutzerprinzipalname (UPN). |
| **CLIENT_CERT_INSTALL_ERROR** | Möglicherweise gibt es ein Problem damit, wie das Clientzertifikat installiert oder Ihrem Mandanten zugeordnet wurde. Befolgen Sie die Anweisungen unter [Behandeln von Problemen mit der MFA NPS-Erweiterung](multi-factor-authentication-nps-extension.md#troubleshooting), um Clientzertifizierungsprobleme zu untersuchen. |
| **ESTS_TOKEN_ERROR** | Befolgen Sie die Anweisungen unter [Behandeln von Problemen mit der MFA NPS-Erweiterung](multi-factor-authentication-nps-extension.md#troubleshooting), um Clientzertifizierungs- und ADAL-Tokenprobleme zu untersuchen. |
| **HTTPS_COMMUNICATION_ERROR** | Der NPS-Server kann keine Antworten von Azure MFA empfangen. Stellen Sie sicher, dass Ihre Firewalls bidirektional für Datenverkehr an und von „https://adnotifications.windowsazure.com“ geöffnet sind. |
| **HTTP_CONNECT_ERROR** | Überprüfen Sie auf dem Server, auf dem die NPS-Erweiterung ausgeführt wird, ob Sie „https://adnotifications.windowsazure.com“ und „https://login.windows.net/“ erreichen können. Wenn diese Websites nicht geladen werden, beheben Sie Konnektivitätsprobleme auf diesem Server. |
| **REGISTRY_CONFIG_ERROR** | Ein Schlüssel fehlt in der Registrierung für die Anwendung. Die Ursache kann sein, dass ein [PowerShell-Skript](multi-factor-authentication-nps-extension.md#install-the-nps-extension) nach der Installation nicht ausgeführt wurde. Die Fehlermeldung sollte den fehlenden Schlüssel enthalten. Stellen Sie sicher, dass der Schlüssel unter „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa“ vorhanden ist. |
| **REQUEST_FORMAT_ERROR** <br> In der RADIUS-Anforderung fehlt das obligatorische userName\Identifier-Attribut für RADIUS. Stellen Sie sicher, dass NPS RADIUS-Anforderungen empfängt. | Dieser Fehler entsteht normalerweise durch ein Installationsproblem. Die NPS-Erweiterung muss auf NPS-Servern installiert sein, die RADIUS-Anforderungen empfangen können. NPS-Server, die als Abhängigkeiten für Dienste wie RDG und RRAS installiert sind, empfangen keine RADIUS-Anforderungen. Die NPS-Erweiterung funktioniert nicht, wenn sie über solche Installationen installiert wird, und es kommt zu Fehlern, da sie die Details aus der Authentifizierungsanforderung nicht lesen kann. |
| **REQUEST_MISSING_CODE** | Wenn SMS- oder OATH-Token für die sekundäre Authentifizierungsmethode verwendet werden, muss das Protokoll für die Kennwortverschlüsselung zwischen NPS- und NAS-Servern PAP sein. Die NPS-Erweiterung unterstützt derzeit keine anderen Kennwortverschlüsselungsmethoden.|
| **USERNAME_CANONICALIZATION_ERROR** | Stellen Sie sicher, dass der Benutzer in Ihrer lokalen Active Directory-Instanz vorhanden ist und dass der NPS-Dienst über Berechtigungen zum Zugriff auf das Verzeichnis verfügt. Wenden Sie sich bei Verwendung von gesamtstrukturübergreifenden Vertrauensstellungen [an den Support](#contact-microsoft-support), um weitere Hilfe zu erhalten. |


## <a name="errors-your-users-may-encounter"></a>Fehler, die bei Ihren Benutzern auftreten können

| Fehlercode | Fehlermeldung | Schritte zur Problembehandlung |
| ---------- | ------------- | --------------------- |
| **AccessDenied** | Der Mandant des Aufrufers verfügt nicht über Zugriffsberechtigungen zur Authentifizierung des Benutzers. | Überprüfen Sie, ob die Mandantendomäne und die Domäne des Benutzerprinzipalnamens (UPN) identisch sind. Stellen Sie beispielsweise sicher, dass user@contoso.com versucht, sich beim Contoso-Mandanten zu authentifizieren. Der UPN stellt einen gültigen Benutzer für den Mandanten in Azure dar. |
| **AuthenticationMethodNotConfigured** | Die angegebene Authentifizierungsmethode wurde für den Benutzer nicht konfiguriert. | Fordern Sie den Benutzer auf, die Überprüfungsmethoden gemäß der Anleitung in [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-manage-settings.md) hinzuzufügen oder zu überprüfen. |
| **AuthenticationMethodNotSupported** | Die angegebene Authentifizierungsmethode wird nicht unterstützt. | Erfassen Sie alle Protokolle, die diesen Fehler enthalten, und [wenden Sie sich an den Support](#contact-microsoft-support). Wenn Sie sich an den Support wenden, geben Sie den Benutzernamen und die sekundäre Überprüfungsmethode an, die den Fehler ausgelöst hat. |
| **BecAccessDenied** | Der MSODS BEC-Aufruf hat „Zugriff verweigert“ zurückgegeben, wahrscheinlich ist der Benutzername im Mandanten nicht definiert. | Der Benutzer ist in der lokalen Active Directory-Instanz vorhanden, wird aber nicht mit AD Connect in Azure AD synchronisiert. Oder der Benutzer fehlt für den Mandanten. Fügen Sie den Benutzer zu Azure AD hinzu, und fordern Sie ihn auf, die Überprüfungsmethoden gemäß der Anleitung in [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-manage-settings.md) hinzuzufügen oder zu überprüfen. |
| **InvalidFormat** oder **StrongAuthenticationServiceInvalidParameter** | Die Telefonnummer hat ein nicht erkennbares Format. | Bitten Sie die Benutzer, ihre Telefonnummern für die Überprüfung zu korrigieren. |
| **InvalidSession** | Die angegebene Sitzung ist ungültig oder möglicherweise abgelaufen | Der Abschluss der Sitzung hat mehr als drei Minuten in Anspruch genommen. Stellen Sie sicher, dass der Benutzer innerhalb von drei Minuten nach Initiierung der Authentifizierungsanforderung den Überprüfungscode eingibt oder auf die App-Benachrichtigung antwortet. Wenn das Problem dadurch nicht behoben wird, stellen Sie sicher, dass keine Netzwerklatenzen zwischen Client, NAS-Server, NPS-Server und Azure MFA-Endpunkt vorhanden sind.  |
| **NoDefaultAuthenticationMethodIsConfigured** | Für den Benutzer wurde keine Standardauthentifizierungsmethode konfiguriert. | Fordern Sie den Benutzer auf, die Überprüfungsmethoden gemäß der Anleitung in [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-manage-settings.md) hinzuzufügen oder zu überprüfen. Stellen Sie sicher, dass der Benutzer eine Standardauthentifizierungsmethode ausgewählt und diese Methode für sein Konto konfiguriert hat. |
| **OathCodePinIncorrect** | Falscher Code und falsche Pin eingegeben. | Dieser Fehler sollte in der NPS-Erweiterung nicht auftreten. Wenn bei Ihrem Benutzer der Fehler auftritt, [wenden Sie sich an den Support](#contact-microsoft-support), um Hilfe bei der Problembehandlung zu erhalten. |
| **ProofDataNotFound** | Nachweisdaten wurden für die angegebene Authentifizierungsmethode nicht konfiguriert. | Fordern Sie den Benutzer auf, eine andere Überprüfungsmethode auszuprobieren oder eine neue Überprüfungsmethode gemäß der Anleitung in [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-manage-settings.md) hinzuzufügen. Wenn dieser Fehler weiterhin angezeigt wird, nachdem Sie sichergestellt haben, dass die Überprüfungsmethode ordnungsgemäß eingerichtet ist, [wenden Sie sich an den Support](#contact-microsoft-support). |
| **SMSAuthFailedWrongCodePinEntered** | Falscher Code und falsche Pin eingegeben. (OneWaySMS) | Dieser Fehler sollte in der NPS-Erweiterung nicht auftreten. Wenn bei Ihrem Benutzer der Fehler auftritt, [wenden Sie sich an den Support](#contact-microsoft-support), um Hilfe bei der Problembehandlung zu erhalten. |
| **TenantIsBlocked** | Der Mandant wird blockiert. | [Wenden Sie sich an den Support](#contact-microsoft-support), und geben Sie die Verzeichnis-ID von der Azure AD-Eigenschaftenseite im Azure-Portal an. |
| **UserNotFound** | Der angegebene Benutzer wurde nicht gefunden. | Der Mandant wird in Azure AD nicht mehr als aktiv angezeigt. Überprüfen Sie, ob Ihr Abonnement aktiv ist und Sie über die erforderlichen Erstanbieter-Apps verfügen. Stellen Sie außerdem sicher, dass der Mandant im Zertifikatantragsteller den erwarteten Wert aufweist und dass das Zertifikat noch gültig und unter dem Dienstprinzipal registriert ist. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>Meldungen, die den Benutzern angezeigt werden können, aber keine Fehler sind

Manchmal können Ihren Benutzern Meldungen von Multi-Factor Authentication angezeigt werden, da ihre Authentifizierungsanforderung fehlgeschlagen ist. Dies sind keine Fehler als Folge der Konfiguration, sondern absichtliche Warnungen, die erläutern, warum eine Authentifizierungsanforderung verweigert wurde.

| Fehlercode | Fehlermeldung | Empfohlene Schritte | 
| ---------- | ------------- | ----------------- |
| **OathCodeIncorrect** | Falscher Code wurde eingegeben/der OATH-Code ist falsch | Dies ist kein Fehler. Der Benutzer hat einen falschen Code eingegeben. | Der Benutzer hat den falschen Code eingegeben. Fordern Sie ihn auf, es erneut zu versuchen, indem er einen neuen Code anfordert oder sich erneut anmeldet. | 
| **SMSAuthFailedMaxAllowedCodeRetryReached** | Maximal zulässige Anzahl von Codewiederholungen erreicht | Der Benutzer hat die Überprüfung zu oft nicht bestanden. Abhängig von Ihren Einstellungen muss die Sperre jetzt möglicherweise durch einen Administrator aufgehoben werden.  |
| **SMSAuthFailedWrongCodeEntered** | Falscher Code eingegeben/OTP der Textmeldung ist falsch | Der Benutzer hat den falschen Code eingegeben. Fordern Sie ihn auf, es erneut zu versuchen, indem er einen neuen Code anfordert oder sich erneut anmeldet. |

## <a name="errors-that-require-support"></a>Fehler, für die Support erforderlich ist

Wenn einer dieser Fehler auftritt, sollten Sie sich [an den Support wenden](#contact-microsoft-support), um Hilfe bei der Diagnose zu erhalten. Es gibt keine Standardschritte, um diese Fehler zu beheben. Wenn Sie sich an den Support wenden, sollten Sie möglichst viele Informationen zu den Schritten, die zu einem Fehler geführt haben, und Informationen zum Mandanten angeben.

| Fehlercode | Fehlermeldung |
| ---------- | ------------- |
| **InvalidParameter** | Die Anforderung darf nicht null sein. |
| **InvalidParameter** | ObjectId darf nicht null oder leer sein für ReplicationScope: {0} |
| **InvalidParameter** | Die Länge von CompanyName \{0}\ ist länger als die maximal zulässige Länge {1}. |
| **InvalidParameter** | UserPrincipalName darf nicht null oder leer sein. |
| **InvalidParameter** | Die angegebene TenantId hat nicht das richtige Format. |
| **InvalidParameter** | SessionId darf nicht null oder leer sein. |
| **InvalidParameter** | ProofData aus der Anforderung oder MSODS konnte nicht aufgelöst werden. ProofData ist unbekannt. |
| **InternalError** |  |
| **OathCodePinIncorrect** |  |
| **VersionNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Nächste Schritte

### <a name="troubleshoot-user-accounts"></a>Problembehandlung bei Benutzerkonten

Wenn Ihre Benutzer [Probleme mit der zweistufigen Überprüfung haben](./end-user/multi-factor-authentication-end-user-troubleshoot.md), unterstützen Sie sie beim Diagnostizieren der Probleme. 

### <a name="contact-microsoft-support"></a>Microsoft-Support kontaktieren

Wenn Sie zusätzliche Hilfe benötigen, wenden Sie sich über [Azure Multi-Factor Authentication-Server – Support](https://support.microsoft.com/oas/default.aspx?prid=14947) an einen Supportspezialisten. Es ist hilfreich, wenn Sie uns so viele Informationen wie möglich über Ihr Problem geben, wenn Sie sich mit uns in Verbindung setzen. Diese Informationen können die Seite umfassen, auf der der Fehler aufgetreten ist, den spezifischen Fehlercode, die spezifische Sitzungs-ID, die ID des Benutzers, der den Fehler beobachtet hat, und Debugprotokolle.

Um Debugprotokolle zur Unterstützung der Diagnose zu erfassen, verwenden Sie die folgenden Schritte: 

1. Öffnen Sie eine Administratoreingabeaufforderung, und führen Sie diese Befehle aus:

   ```
   Mkdir c:\NPS
   Cd NPS
   netsh trace start Scenario=NetConnection capture=yes tracefile=c:\NPS\nettrace.etl
   logman create trace "NPSExtension" -ow -o c:\NPS\NPSExtension.etl -p {7237ED00-E119-430B-AB0F-C63360C8EE81} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode Circular -f bincirc -max 4096 -ets
   logman update trace "NPSExtension" -p {EC2E6D3A-C958-4C76-8EA4-0262520886FF} 0xffffffffffffffff 0xff -ets
   ```

2. Reproduzieren Sie das Problem.

3. Beenden Sie die Ablaufverfolgung mit diesen Befehlen:

   ```
   logman stop "NPSExtension" -ets
   netsh trace stop
   wevtutil epl AuthNOptCh C:\NPS\%computername%_AuthNOptCh.evtx
   wevtutil epl AuthZOptCh C:\NPS\%computername%_AuthZOptCh.evtx
   wevtutil epl AuthZAdminCh C:\NPS\%computername%_AuthZAdminCh.evtx
   Start .
   ```

4. Zippen Sie den Inhalt des Ordners „C:\NPS“, und fügen Sie die ZIP-Datei an die Supportanfrage an.



