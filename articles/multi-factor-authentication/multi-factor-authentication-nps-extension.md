---
title: Bereitstellen von Azure MFA-Funktionen mithilfe vorhandener NPS-Server | Microsoft-Dokumentation
description: "Die Erweiterung „Netzwerkrichtlinienserver“ für Azure Multi-Factor Authentication ist eine einfache Lösung, um der vorhandenen Authentifizierungsinfrastruktur cloudbasierte Funktionen für eine Überprüfung in zwei Schritten hinzuzufügen."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017; it-pro
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: b9061283952ae6b14431f5e88295eefac173ae01
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>Integrieren Ihrer vorhandenen NPS-Infrastruktur in Azure Multi-Factor Authentication

Die NPS-Erweiterung (Network Policy Server, Netzwerkrichtlinienserver) für Azure MFA fügt Ihrer Authentifizierungsinfrastruktur unter Verwendung Ihrer vorhandenen Server cloudbasierte MFA-Funktionen hinzu. Mit der NPS-Erweiterung können Sie Ihrem bestehenden Authentifizierungsvorgang eine Überprüfung per Telefonanruf, SMS oder Telefon-App hinzufügen, ohne neue Server installieren, konfigurieren und verwalten zu müssen. 

Diese Erweiterung wurde für Organisationen entwickelt, die VPN-Verbindungen schützen möchten, ohne den Azure MFA-Server bereitzustellen. Die NPS-Erweiterung fungiert als Adapter zwischen RADIUS und cloudbasierter Azure MFA, um eine zweite Stufe der Authentifizierung für Verbund- oder synchronisierte Benutzer bereitzustellen.

Bei Verwenden der NPS-Erweiterung für Azure MFA umfasst der Authentifizierungsprozess die folgenden Komponenten: 

1. Der **NAS/VPN-Server** empfängt Anforderungen von VPN-Clients und wandelt sie in RADIUS-Anforderungen an NPS-Server um. 
2. Der **NPS-Server** stellt eine Verbindung mit Active Directory her, um die primäre Authentifizierung für die RADIUS-Anforderungen durchzuführen und übergibt bei Erfolg die Anforderung an installierte Erweiterungen.  
3. Die **NPS-Erweiterung** löst eine Anforderung einer sekundären Authentifizierung an Azure MFA aus. Sobald die Erweiterung die Antwort empfängt und die MFA-Abfrage erfolgreich ist, wird die Authentifizierungsanforderung abgeschlossen, indem dem NPS-Server Sicherheitstoken bereitgestellt werden, die einen von Azure STS ausgegebenen MFA-Anspruch enthalten.  
4. **Azure MFA** kommuniziert mit Azure Active Directory zum Abrufen der Details des Benutzers und führt die sekundäre Authentifizierung mithilfe einer Überprüfungsmethode aus, die für den Benutzer konfiguriert ist.

Das folgende Diagramm veranschaulicht den allgemeinen Ablauf dieser Authentifizierungsanforderung: 

![Diagramm des Authentifizierungsablaufs](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Planen der Bereitstellung

Die NPS-Erweiterung kümmert sich automatisch um die Redundanz, sodass keine spezielle Konfiguration erforderlich ist.

Sie können beliebig viele Azure MFA-fähige NPS-Server erstellen. Wenn Sie mehrere Server installieren, sollten Sie für jeden ein anderes Clientzertifikat verwenden. Durch das Erstellen eines Zertifikats für jeden Server können Sie jedes Zertifikat einzeln aktualisieren und müssen sich so keine Gedanken über Ausfallzeiten für Ihre einzelnen Server machen.

Da VPN-Server die Authentifizierungsanforderungen weiterleiten, müssen sie über neue NPS-Server mit Azure MFA informiert werden.

## <a name="prerequisites"></a>Voraussetzungen

Die NPS-Erweiterung soll mit der vorhandenen Infrastruktur zusammenarbeiten. Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie beginnen.

### <a name="licenses"></a>Lizenzen

Die NPS-Erweiterung für Azure MFA steht Kunden mit [Lizenzen für Azure Multi-Factor Authentication](multi-factor-authentication.md) zur Verfügung (die in Azure AD Premium, EMS oder einem MFA-Abonnement enthalten sind).

### <a name="software"></a>Software

Windows Server 2008 R2 SP1 oder höher

### <a name="libraries"></a>Bibliotheken

Diese Bibliotheken werden automatisch mit der Erweiterung installiert.
-   [Visual C++ Redistributable Packages für Visual Studio 2013 (x64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory-Modul für Windows PowerShell, Version 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a>Azure Active Directory

Alle Benutzer der NPS-Erweiterung müssen mithilfe von Azure AD Connect mit Azure Active Directory synchronisiert und für MFA registriert werden.

Wenn Sie die Erweiterung installieren, benötigen Sie die Verzeichnis-ID und die Administratoranmeldeinformationen für Ihren Azure AD-Mandanten. Ihre Verzeichnis-ID finden Sie im [Azure-Portal](https://portal.azure.com). Melden Sie sich als Administrator an, wählen Sie links das Symbol **Azure Active Directory** und anschließend **Eigenschaften** aus. Kopieren Sie die GUID in das Feld **Verzeichnis-ID**, und speichern Sie sie. Wenn Sie die NPS-Erweiterung installieren, verwenden Sie diese GUID als Mandanten-ID.

![Ihre Verzeichnis-ID finden Sie in den Azure Active Directory-Eigenschaften](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a>Vorbereiten der Umgebung

Vor der Installation der NPS-Erweiterung müssen Sie Ihre Umgebung für die Verarbeitung des Authentifizierungsdatenverkehrs vorbereiten.

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a>Aktivieren der NPS-Rolle auf einem in die Domäne eingebundenen Server

Der NPS-Server stellt eine Verbindung mit Azure Active Directory her und authentifiziert die MFA-Anforderungen. Wählen Sie einen Server für diese Rolle aus. Sie sollten einen Server auswählen, der keine Anforderungen von anderen Diensten verarbeitet, da die NPS-Erweiterung für alle Anforderungen Fehler auslöst, die keine RADIUS-Anforderungen sind.

1. Öffnen Sie auf dem Server den **Assistenten zum Hinzufügen von Rollen und Features** über das Schnellstartmenü im Server-Manager.
2. Wählen Sie als Installationstyp **Rollenbasierte oder featurebasierte Installation** aus.
3. Wählen Sie die Serverrolle **Netzwerkrichtlinien- und Zugriffsdienste** aus. Es wird möglicherweise ein Fenster angezeigt, in dem Sie über die erforderlichen Features für die Ausführung dieser Rolle informiert werden.
4. Gehen Sie den Assistenten bis zur Bestätigungsseite durch. Wählen Sie **Installieren** aus.

Nachdem Sie einen Server für NPS festgelegt haben, sollten Sie diesen Server für die Verarbeitung eingehender RADIUS-Anforderungen von der VPN-Lösung konfigurieren.

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a>Konfigurieren der VPN-Lösung für die Kommunikation mit dem NPS-Server

Die Schritte zum Konfigurieren der RADIUS-Authentifizierungsrichtlinie variieren in Abhängigkeit von der verwendeten VPN-Lösung. Konfigurieren Sie diese Richtlinie mit einem Verweis auf Ihren RADIUS-NPS-Server.

### <a name="sync-domain-users-to-the-cloud"></a>Synchronisieren von Domänenbenutzern in der Cloud

Dieser Schritt wurde möglicherweise bereits auf Ihrem Mandanten durchgeführt. Es ist aber ratsam zu überprüfen, ob Azure AD Connect die Datenbanken kürzlich synchronisiert hat.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Wählen Sie **Azure Active Directory** > **Azure AD Connect** aus.
3. Vergewissern Sie sich, dass der Synchronisierungsstatus **Aktiviert** lautet und dass die letzte Synchronisierung weniger als eine Stunde zurückliegt.

Wenn Sie einen neuen Synchronisierungslauf starten möchten, gehen Sie anhand der Anweisungen in [Azure AD Connect-Synchronisierung: Scheduler](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) vor.

### <a name="determine-which-authentication-methods-your-users-can-use"></a>Bestimmen Sie die Authentifizierungsmethoden, die Ihre Benutzer verwenden können

Zwei Faktoren haben Einfluss darauf, welche Authentifizierungsmethoden mit der Bereitstellung einer NPS-Erweiterung verfügbar sind:

1. Der Kennwortverschlüsselungsalgorithmus wird zwischen dem RADIUS-Client (VPN, NetScaler-Server oder andere) und den NPS-Servern verwendet.
   - **PAP** unterstützt alle Authentifizierungsmethoden von Azure MFA in der Cloud: Telefonanruf, unidirektionale Textnachricht, Benachrichtigung über eine mobile App und Überprüfungscode in der mobilen App.
   - **CHAPV2** und **EAP** unterstützt Telefonanruf und Benachrichtigung über eine mobile App.
2. Die Eingabemethoden, die von der Clientanwendung (VPN, NetScaler-Server oder andere) verarbeitet werden kann. Beispiel: Verfügt der VPN-Client über Mittel, die es dem Benutzer erlauben, einen Überprüfungscode aus einem Text oder einer mobilen App einzugeben?

Verwenden Sie bei der Bereitstellung der NPS-Erweiterung diese Faktoren, um auszuwerten, welche Methoden für Benutzer verfügbar sind. Wenn Ihr RADIUS-Client PAP unterstützt, der Client UX jedoch über kein Eingabefeld für einen Überprüfungscode verfügt, sind der Telefonanruf und die Benachrichtigung über eine mobile App die zwei unterstützten Optionen.

Sie können [nicht unterstützte Authentifizierungsmethoden](multi-factor-authentication-whats-next.md#selectable-verification-methods) in Azure deaktivieren.

### <a name="enable-users-for-mfa"></a>Aktivieren von Benutzern für MFA

Bevor Sie die vollständige NPS-Erweiterung bereitstellen, müssen Sie MFA für die Benutzer aktivieren, die eine Überprüfung in zwei Schritten durchführen sollen. Vorher benötigen Sie zum Testen der Erweiterung bei der Bereitstellung zumindest ein Testkonto, das vollständig für Multi-Factor Authentication registriert ist.

Führen Sie die folgende Schritte aus, um ein Testkonto einzurichten:
1. Melden Sie sich unter [https://aka.ms/mfasetup](https://aka.ms/mfasetup) mit einem Testkonto an. 
2. Befolgen Sie die Anweisungen zum Einrichten einer Überprüfungsmethode.
3. Erstellen Sie eine Richtlinie für den bedingten Zugriff, oder [ändern Sie den Benutzerstatus](multi-factor-authentication-get-started-user-states.md), sodass eine Überprüfung in zwei Schritten für das Testkonto erforderlich ist. 

Ihre Benutzer müssen auch diese Schritte zum Registrieren befolgen, bevor sie sich mit der NPS-Erweiterung authentifizieren können.

## <a name="install-the-nps-extension"></a>Installieren der NPS-Erweiterung

> [!IMPORTANT]
> Installieren Sie die NPS-Erweiterung auf einem anderen Server als dem VPN-Zugriffspunkt.

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a>Herunterladen und Installieren der NPS-Erweiterung für Azure MFA

1.  [Laden Sie die NPS-Erweiterung aus dem Microsoft Download Center herunter](https://aka.ms/npsmfa).
2.  Kopieren Sie die Binärdatei auf den Netzwerkrichtlinienserver, der konfiguriert werden soll.
3.  Führen Sie *setup.exe* aus, und folgen Sie den Installationsanweisungen. Wenn Fehler auftreten, überprüfen Sie, ob die beiden Bibliotheken im Abschnitt „Voraussetzungen“ installiert wurden.

### <a name="run-the-powershell-script"></a>Ausführen des PowerShell-Skripts

Das Installationsprogramm erstellt ein PowerShell-Skript an diesem Speicherort: `C:\Program Files\Microsoft\AzureMfa\Config` (wobei „C:\“ Ihr Installationslaufwerk ist). Dieses PowerShell-Skript führt folgende Aktionen aus:

-   Erstellen eines selbstsignierten Zertifikats
-   Zuordnen des öffentlichen Schlüssels des Zertifikats zum Dienstprinzipal in Azure AD
-   Speichern des Zertifikats im lokalen Zertifikatspeicher
-   Gewähren des Zugriffs auf den privaten Schlüssel des Zertifikats für den Netzwerkbenutzer
-   Neustarten des NPS

Außer wenn Sie Ihre eigenen Zertifikate nutzen möchten (anstelle der selbstsignierten Zertifikate, die das PowerShell-Skript generiert), führen Sie zum Abschließen der Installation das PowerShell-Skript aus. Wenn Sie die Erweiterung auf mehreren Servern installieren möchten, sollte jeder über ein eigenes Zertifikat verfügen.

1. Führen Sie Windows PowerShell als Administrator aus.
2. Wechseln Sie das Verzeichnis.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. Führen Sie das PowerShell-Skript aus, das vom Installationsprogramm erstellt wurde.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. PowerShell fordert Sie zur Angabe Ihrer Mandanten-ID auf. Verwenden Sie die Verzeichnis-ID-GUID, die Sie im Abschnitt „Voraussetzungen“ aus dem Azure-Portal kopiert haben.
5. Melden Sie sich bei Azure AD als Administrator an.
6. PowerShell zeigt eine Erfolgsmeldung an, wenn die Skriptausführung abgeschlossen wurde.  

Wiederholen Sie diese Schritte für alle zusätzlichen NPS-Server, die Sie für den Lastenausgleich einrichten möchten.

>[!NOTE]
>Wenn Sie Ihre eigenen Zertifikate verwenden, statt diese mit dem PowerShell-Skript zu generieren, stellen Sie sicher, dass sie den NPS-Namenskonventionen entsprechen. Der Antragstellernamen muss **CN=\<Mandanten-ID\>,OU=Microsoft NPS Extension** sein. 

## <a name="configure-your-nps-extension"></a>Konfigurieren der NPS-Erweiterung

Dieser Abschnitt enthält Überlegungen zum Entwurf und Vorschläge für erfolgreiche Bereitstellungen der NPS-Erweiterung.

### <a name="configuration-limitations"></a>Einschränkungen der Konfiguration

- Die NPS-Erweiterung bietet für Azure MFA keine Tools zum Migrieren von Benutzern und Einstellungen vom MFA-Server in die Cloud. Aus diesem Grund wird die Verwendung der Erweiterung für neue Bereitstellungen statt vorhandener Bereitstellung empfohlen. Wenn Sie die Erweiterung für eine vorhandene Bereitstellung verwenden, müssen die Benutzer die Bestätigung erneut ausführen, um ihre MFA-Details in der Cloud anzugeben.  
- Die NPS-Erweiterung verwendet für die Durchführung der sekundären Authentifizierung den Benutzerprinzipalnamen (User Principal Name, UPN) aus dem lokalen Active Directory zur Identifizierung des Benutzers in Azure MFA. Die Erweiterung kann für die Verwendung eines anderen Bezeichners wie einer alternativen Anmelde-ID oder eines benutzerdefinierten Active Directory-Felde (außer dem UPN) konfiguriert werden. Weitere Informationen finden Sie unter [Erweiterte Konfigurationsoptionen für die NPS-Erweiterung für Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md).
- Nicht alle Verschlüsselungsprotokolle unterstützen alle Überprüfungsmethoden.
   - **PAP** unterstützt Telefonanruf, unidirektionale Textnachricht, Benachrichtigung über eine mobile App und Überprüfungscode in der mobilen App
   - **CHAPV2** und **EAP** unterstützt Telefonanruf und Benachrichtigung über eine mobile App

### <a name="control-radius-clients-that-require-mfa"></a>Steuern von RADIUS-Clients, die MFA erfordern

Nachdem Sie MFA für einen RADIUS-Client mithilfe der NPS-Erweiterung aktiviert haben, muss bei allen Authentifizierungen für diesen Client MFA erfolgen. Wenn Sie MFA nur für einige RADIUS-Clients aktivieren möchten, für andere dagegen nicht, können Sie zwei NPS-Server konfigurieren und die Erweiterung nur auf einem installieren. Verweisen Sie RADIUS-Clients, die MFA zum Senden von Anforderungen auffordern sollen, an den mit der Erweiterung konfigurierten NPS-Server und andere RADIUS-Clients an den NPS-Server, der nicht mit der Erweiterung konfiguriert ist.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Vorbereiten von Benutzern, die nicht für MFA registriert sind

Wenn es Benutzer gibt, die nicht für MFA registriert sind, können Sie bestimmen, was geschieht, wenn sie versuchen, sie zu authentifizieren. Verwenden Sie die Registrierungseinstellung *REQUIRE_USER_MATCH* im Registrierungspfad *HKLM\Software\Microsoft\AzureMFA* zum Steuern des Verhaltens des Features. Diese Einstellung hat eine einzelne Konfigurationsoption:

| Schlüssel | Wert | Standard |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | TRUE/FALSE | Nicht festgelegt (gleichwertig mit TRUE) |

Diese Einstellung dient zum Bestimmen, was zu tun, wenn ein Benutzer nicht für MFA registriert ist. Wenn der Schlüssel nicht vorhanden, nicht festgelegt oder auf TRUE festgelegt ist und der Benutzer nicht registriert ist, erfüllt die Erweiterung die MFA-Abfrage nicht. Wenn der Schlüssel auf FALSE festgelegt und der Benutzer nicht registriert ist, wird die Authentifizierung ohne MFA fortgesetzt.

Sie können diesen Schlüssel erstellen und auf FALSE festlegen, während Ihre Benutzer eingebunden werden und möglicherweise noch nicht alle für Azure MFA registriert sind. Da das Festlegen des Schlüssels Benutzern, die nicht für MFA registriert sind, die Anmeldung erlaubt, müssen Sie diesen Schlüssel vor dem Wechsel in die Produktionsumgebung entfernen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>Wie überprüfe ich, ob das Clientzertifikat wie erwartet installiert ist?

Suchen Sie das vom Installationsprogramm erstellte selbstsignierte Zertifikat im Zertifikatspeicher, und prüfen Sie, ob der private Schlüssel dem Benutzer **NETZWERKDIENST** Berechtigungen erteilt. Das Zertifikat hat den Antragstellernamen **CN \<Mandanten-ID\>, OU = Microsoft NPS Extension**.

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>Wie kann ich überprüfen, ob mein Clientzertifikat meinem Mandanten in Azure Active Directory zugeordnet ist?

Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die folgenden Befehle aus:

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Diese Befehle geben alle Zertifikate aus, die Ihren Mandanten Ihrer Instanz der NPS-Erweiterung in der PowerShell-Sitzung zuordnen. Suchen Sie Ihr Zertifikat, indem Sie Ihr Clientzertifikat als Datei des Typs „Base64-codiert X.509 (.CER)“ ohne den privaten Schlüssel exportieren und es mit der Liste aus PowerShell vergleichen.

Die Zeitstempel „Valid-From“ und „Valid-Until“ in einem vom Menschen lesbaren Format dienen zum Herausfiltern offensichtlicher Fehler, wenn der Befehl mehrere Zertifikate zurückgibt.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Warum misslingen meine Anforderungen mit ADAL-Tokenfehler?

Dieser Fehler kann verschiedene Gründe haben. Gehen Sie zur Problembehandlung folgendermaßen vor:

1. Starten Sie den NPS-Server neu.
2. Überprüfen Sie , ob das Clientzertifikat wie erwartet installiert ist.
3. Überprüfen Sie, ob das Zertifikat Ihrem Mandanten in Azure AD zugeordnet ist.
4. Überprüfen Sie auf dem Server, auf dem die Erweiterung ausgeführt wird, ob auf https://login.microsoftonline.com/ zugegriffen werden kann.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Warum misslingt die Authentifizierung mit einem Fehler in den HTTP-Protokollen, der besagt, dass der Benutzer nicht gefunden wurde?

Überprüfen Sie, ob AD Connect ausgeführt wird und der Benutzer in Windows Active Directory und Azure Active Directory vorhanden ist.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Warum enthalten meine Protokolle HTTP-Verbindungs- und Authentifizierungsfehler?

Überprüfen Sie auf dem Server, auf dem die Erweiterung ausgeführt wird, ob „https://adnotifications.windowsazure.com“ erreichbar ist.


## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie alternative IDs für die Anmeldung, oder richten Sie unter [Erweiterte Konfigurationsoptionen für die NPS-Erweiterung für Multi-Factor Authentication](nps-extension-advanced-configuration.md) eine Ausnahmeliste für IP-Adressen ein, welche die Überprüfung in zwei Schritten nicht ausführen müssen.

- Erfahren Sie, wie Sie [Remotedesktopgateway](nps-extension-remote-desktop-gateway.md) und [VPN-Server](nps-extension-vpn.md) mithilfe der NPS-Erweiterung integrieren.

- [Auflösen von Fehlermeldungen in der NPS-Erweiterung für Azure Multi-Factor Authentication](multi-factor-authentication-nps-errors.md)

