---
title: Bereitstellen von Azure MFA-Funktionen mithilfe vorhandener NPS-Server | Microsoft-Dokumentation
description: "Die Erweiterung „Netzwerkrichtlinienserver“ für Azure Multi-Factor Authentication ist eine einfache Lösung, um der vorhandenen Authentifizierungsinfrastruktur cloudbasierte Funktionen für eine Überprüfung in zwei Schritten hinzuzufügen."
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
ms.date: 02/23/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 9c587378bc8ffe43f6153c6424748e814c7a1d89
ms.lasthandoff: 03/18/2017

---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication---public-preview"></a>Integrieren Ihrer vorhandenen NPS-Infrastruktur in Azure Multi-Factor Authentication – Public Preview

Die NPS-Erweiterung (Network Policy Server, Netzwerkrichtlinienserver) für Azure MFA fügt Ihrer Authentifizierungsinfrastruktur unter Verwendung Ihrer vorhandenen Server cloudbasierte MFA-Funktionen hinzu. Mit der NPS-Erweiterung können Sie Ihrem bestehenden Authentifizierungsvorgang eine Überprüfung per Telefonanruf, SMS oder Telefon-App hinzufügen, ohne neue Server installieren, konfigurieren und verwalten zu müssen. 
 
Bei Verwenden der NPS-Erweiterung für Azure MFA umfasst der Authentifizierungsprozess die folgenden Komponenten: 

1. Der **NAS/VPN-Server** empfängt Anforderungen von VPN-Clients und wandelt sie in RADIUS-Anforderungen an NPS-Server um. 
2. Der **NPS-Server** stellt eine Verbindung mit Active Directory her, um die primäre Authentifizierung für die RADIUS-Anforderungen durchzuführen und übergibt bei Erfolg die Anforderung an installierte Erweiterungen.  
3. Die **NPS-Erweiterung** löst eine Anforderung einer sekundären Authentifizierung an Azure MFA aus. Sobald die Erweiterung die Antwort empfängt und die MFA-Abfrage erfolgreich ist, wird die Authentifizierungsanforderung abgeschlossen, indem dem NPS-Server Sicherheitstoken bereitgestellt werden, die einen von Azure STS ausgegebenen MFA-Anspruch enthalten.  
4. **Azure MFA** kommuniziert mit Azure Active Directory zum Abrufen der Details des Benutzers und führt die sekundäre Authentifizierung mithilfe einer Überprüfungsmethode aus, die für den Benutzer konfiguriert ist.

Das folgende Diagramm veranschaulicht den allgemeinen Ablauf dieser Authentifizierungsanforderung: 

![Diagramm des Authentifizierungsablaufs](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="prerequisites"></a>Voraussetzungen

Die NPS-Erweiterung soll mit der vorhandenen Infrastruktur zusammenarbeiten. Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie beginnen.

### <a name="licenses"></a>Lizenzen

Die NPS-Erweiterung für Azure MFA steht Kunden mit [Lizenzen für Azure Multi-Factor Authentication](multi-factor-authentication.md) zur Verfügung (die in Azure AD Premium, EMS oder einem MFA-Abonnement enthalten sind).

### <a name="software"></a>Software

Windows Server 2008 R2 SP1 oder höher mit aktivierter NPS-Komponente.

### <a name="libraries"></a>Bibliotheken

-    [Visual C++ Redistributable Packages für Visual Studio 2013 (x64)](https://www.microsoft.com/download/details.aspx?id=40784)
-    [Microsoft Azure Active Directory-Modul für Windows PowerShell, Version 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a>Azure Active Directory

Alle Benutzer der NPS-Erweiterung müssen mit Azure Active Directory mithilfe von Azure AD Connect synchronisiert und für MFA aktiviert werden.

Wenn Sie die Erweiterung installieren, benötigen Sie die Verzeichnis-ID und die Administratoranmeldeinformationen für Ihren Azure AD-Mandanten. Ihre Verzeichnis-ID finden Sie im [Azure-Portal](https://portal.azure.com). Melden Sie sich als Administrator an, wählen Sie links das Symbol **Azure Active Directory** und anschließend **Eigenschaften** aus. Kopieren Sie die GUID in das Feld **Verzeichnis-ID**, und speichern Sie.

![Ihre Verzeichnis-ID finden Sie in den Azure Active Directory-Eigenschaften](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="install-the-nps-extension"></a>Installieren der NPS-Erweiterung

> [!IMPORTANT]
> Installieren Sie die NPS-Erweiterung auf einem anderen Server als dem VPN-Zugriffspunkt. 

So installieren Sie die NPS-Erweiterung für Azure MFA

1.    [Laden Sie die NPS-Erweiterung aus dem Microsoft Download Center herunter](https://aka.ms/npsmfa).
2.    Kopieren Sie die Binärdatei auf den Netzwerkrichtlinienserver, der konfiguriert werden soll.
3.    Führen Sie *setup.exe* aus, und folgen Sie den Installationsanweisungen.

Sobald die Installation abgeschlossen ist, erstellt das Installationsprogramm ein PowerShell-Skript an diesem Speicherort: `C:\Program Files\Microsoft\AzureMfa\Config` (wobei C:\ Ihr Installationslaufwerk ist). Dieses PowerShell-Skript führt folgende Aktionen aus:

-    Erstellen eines selbstsignierten Zertifikats
-    Zuordnen des öffentlichen Schlüssels des Zertifikats zum Dienstprinzipal in Azure AD
-    Speichern des Zertifikats im lokalen Zertifikatspeicher
-    Gewähren des Zugriffs auf den privaten Schlüssel des Zertifikats für den Netzwerkbenutzer
-    Neustarten des NPS

Außer wenn Sie Ihre eigenen Zertifikate nutzen möchten (anstelle der selbstsignierten Zertifikate, die das PowerShell-Skript generiert), führen Sie zum Abschließen der Installation das PowerShell-Skript aus.

## <a name="configure-your-nps-extension"></a>Konfigurieren der NPS-Erweiterung

Dieser Abschnitt enthält Überlegungen zum Entwurf und Vorschläge für erfolgreiche Bereitstellungen der NPS-Erweiterung.

### <a name="configurations-limitations"></a>Konfigurationseinschränkungen

- Die NPS-Erweiterung ist für neue Bereitstellungen und nicht für die Zusammenarbeit mit Ihrer vorhandenen Bereitstellung vorgesehen. Aus diesem Grund bietet die NPS-Erweiterung für Azure MFA keine Tools zum Migrieren von Benutzern und Einstellungen vom MFA-Server in die Cloud.

- Die NPS-Erweiterung verwendet für die Durchführung der sekundären Authentifizierung den Benutzerprinzipalnamen (User Principal Name, UPN) aus dem lokalen Active Directory zur Identifizierung des Benutzers in Azure MFA. Die Erweiterung kann nicht für die Verwendung eines anderen Bezeichners wie einer alternativen Anmelde-ID oder einem benutzerdefinierten AD-Feld (außer dem UPN) konfiguriert werden.  

### <a name="control-radius-clients-that-require-mfa"></a>Steuern von RADIUS-Clients, die MFA erfordern

Nachdem Sie MFA für einen RADIUS-Client mithilfe der NPS-Erweiterung aktiviert haben, muss bei allen Authentifizierungen für diesen Client MFA erfolgen. Wenn Sie MFA nur für einige RADIUS-Clients aktivieren möchten, für andere dagegen nicht, können Sie zwei NPS-Server konfigurieren und die Erweiterung nur auf einem installieren. Verweisen Sie RADIUS-Clients, die MFA zum Senden von Anforderungen auffordern sollen, an den mit der Erweiterung konfigurierten NPS-Server und andere RADIUS-Clients an den NPS-Server, der nicht mit der Erweiterung konfiguriert ist.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Vorbereiten von Benutzern, die nicht für MFA registriert sind

Wenn es Benutzer gibt, die nicht für MFA registriert sind, können Sie bestimmen, was geschieht, wenn sie versuchen, sie zu authentifizieren. Verwenden Sie die Registrierungseinstellung *REQUIRE_USER_MATCH* im Registrierungspfad *HKLM\Software\Microsoft\AzureMFA* zum Steuern des Verhaltens des Features. Diese Einstellung hat eine einzelne Konfigurationsoption:

| Schlüssel | Wert | Standard |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | TRUE/FALSE | Nicht festgelegt (gleichwertig mit TRUE) |

Diese Einstellung dient zum Bestimmen, was zu tun, wenn ein Benutzer nicht für MFA registriert ist. Wenn der Schlüssel nicht vorhanden, nicht festgelegt oder auf TRUE festgelegt ist und der Benutzer nicht registriert ist, erfüllt die Erweiterung die MFA-Abfrage nicht. Wenn der Schlüssel auf FALSE festgelegt und der Benutzer nicht registriert ist, wird die Authentifizierung ohne MFA fortgesetzt.

Während des Hinzufügens des Benutzers können Sie diesen Schlüssel erstellen und auf FALSE festlegen. Da das Festlegen des Schlüssels Benutzern, die nicht für MFA registriert sind, erlaubt, sich ohne weitere Abfrage anzumelden, müssen Sie diesen Schlüssel vor dem Wechsel in die Produktionsumgebung entfernen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>Wie überprüfe ich, ob das Clientzertifikat wie erwartet installiert ist?

Suchen Sie das vom Installationsprogramm erstellte selbstsignierte Zertifikat im Zertifikatspeicher, und prüfen Sie, ob der private Schlüssel dem Benutzer **NETZWERKDIENST** Berechtigungen erteilt. Das Zertifikat hat den Antragstellernamen **CN \<Mandanten-ID\>, OU = Microsoft NPS Extension**.

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>Wie kann ich überprüfen, ob mein Clientzertifikat meinem Mandanten in Azure Active Directory zugeordnet ist?

Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die folgenden Befehle aus:

```
> import-module MSOnline
> Connect-MsolService
> Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Diese Befehle geben alle Zertifikate aus, die Ihren Mandanten Ihrer Instanz der NPS-Erweiterung in der PowerShell-Sitzung zuordnen. Suchen Sie Ihr Zertifikat, indem Sie Ihr Clientzertifikat als Datei des Typs „Base64-codiert X.509 (.CER)“ ohne den privaten Schlüssel exportieren und es mit der Liste aus PowerShell vergleichen.

Die Zeitstempel „Valid-From“ und „Valid-Until“ in einem vom Menschen lesbaren Format dienen zum Herausfiltern offensichtlicher Fehler, wenn der Befehl mehrere Zertifikate zurückgibt.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Warum misslingen meine Anforderungen mit ADAL-Tokenfehler?

Dieser Fehler kann verschiedene Gründe haben. Gehen Sie zur Problembehandlung folgendermaßen vor:

1. Starten Sie den NPS-Server neu.
2. Überprüfen Sie , ob das Clientzertifikat wie erwartet installiert ist.
3. Überprüfen Sie, ob das Zertifikat Ihrem Mandanten in Azure AD zugeordnet ist.
4. Überprüfen Sie auf dem Server, auf dem die Erweiterung ausgeführt wird, ob auf „https://login.windows.net/“ zugegriffen werden kann.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Warum misslingt die Authentifizierung mit einem Fehler in den HTTP-Protokollen, der besagt, dass der Benutzer nicht gefunden wurde?

Überprüfen Sie, ob AD Connect ausgeführt wird und der Benutzer in Windows Active Directory und Azure Active Directory vorhanden ist.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Warum enthalten meine Protokolle HTTP-Verbindungs- und Authentifizierungsfehler?

Überprüfen Sie auf dem Server, auf dem die Erweiterung ausgeführt wird, ob „https://adnotifications.windowsazure.com“ erreichbar ist.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Azure MFA in [Active Directory](multi-factor-authentication-get-started-server-dirint.md), die [RADIUS-Authentifizierung](multi-factor-authentication-get-started-server-radius.md) und [LDAP-Authentifizierung](multi-factor-authentication-get-started-server-ldap.md) integriert wird.

