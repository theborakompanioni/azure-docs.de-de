---
title: 'Azure AD Connect: Passthrough-Authentifizierung | Microsoft-Dokumentation'
description: "In diesem Artikel wird beschrieben, wie die Pass-Through-Authentifizierung mit Azure Active Directory (Azure AD) funktioniert und wie sie Azure AD-Anmeldungen durch die Überprüfung von Benutzerkennwörtern anhand des lokalen Active Directory ermöglicht."
services: active-directory
keywords: "Was ist die Pass-Through-Authentifizierung mit Azure AD Connect?, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
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
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 4fc3c822e657ce1a66a59e15c1a7d636a9f699e6
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---

# <a name="configure-user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Konfigurieren der Benutzeranmeldung mit der Azure Active Directory-Pass-Through-Authentifizierung

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>Was ist die Azure Active Directory-Pass-Through-Authentifizierung?

Für Ihre Benutzer und Ihre Organisation ist die Möglichkeit, dieselben Anmeldeinformationen (Kennwörter) sowohl für die Anmeldung bei lokalen Ressourcen als auch bei cloudbasierten Diensten zu verwenden, von Vorteil. Die Benutzer müssen sich ein Kennwort weniger merken, sodass die Benutzererfahrung verbessert wird. Damit verringert sich auch die Wahrscheinlichkeit, dass Benutzer vergessen, wie sie sich registrieren. Dies wiederum senkt Ihre Helpdesk-Kosten, da Probleme in Bezug auf Kennwörter in der Regel den Großteil der Supportressourcen beanspruchen.

Viele Organisationen verwenden die [Azure AD-Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md), ein Feature von [Azure AD Connect](active-directory-aadconnect.md). Sie synchronisiert die Benutzerkennwörter aus einem lokalen Active Directory mit Azure Active Directory (Azure AD) und stellt eine Möglichkeit dar, Benutzern dieselben Anmeldeinformationen für lokale Ressourcen und cloudbasierte Dienste bereitzustellen. In anderen Organisationen dagegen dürfen diese Kennwörter selbst im Hashformat die internen Organisationsgrenzen nicht verfügbar.

Die Azure AD-Pass-Through-Authentifizierung bietet diesen Organisationen eine einfache Lösung. Wenn sich Benutzer bei Azure AD anmelden, stellt dieses Feature sicher, dass die Kennwörter dieser Benutzer direkt anhand Ihres lokalen Active Directory überprüft werden. Diese Funktion bietet darüber hinaus folgende Vorteile:

- Benutzerfreundlich
  - Die Kennwortüberprüfung wird durchgeführt, ohne dass komplexe lokale Bereitstellungen oder Netzwerkkonfigurationen erforderlich sind.
  - Ein einfacher lokaler Connector lauscht auf Kennwortüberprüfungsanforderungen und reagiert auf diese.
  - Der lokale Connector wird automatisch mit Funktionsverbesserungen und Fehlerbehebungen aktualisiert.
  - Sie kann zusammen mit [Azure AD Connect](active-directory-aadconnect.md) konfiguriert werden. Der einfache lokale Connector ist auf demselben Server wie Azure AD Connect installiert.
- Schützen
  - Lokale Kennwörter werden niemals in irgendeiner Form in der Cloud gespeichert.
  - Der einfache lokale Connector stellt lediglich ausgehende Verbindungen innerhalb Ihres Netzwerks her. Daher muss der Connector nicht in einem Umkreisnetzwerk (auch als DMZ bezeichnet) installiert werden.
  - Die Passthrough-Authentifizierung funktioniert nahtlos mit der Azure Multi-Factor Authentication.
- Zuverlässig und skalierbar
  - Es können zusätzliche einfache lokale Connectors auf mehreren Servern installiert werden, um eine hohe Verfügbarkeit der Anmeldeanforderungen zu erzielen.

![Azure AD-Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

Sie können die Pass-Through-Authentifizierung mit dem Feature zum [nahtlosen einmaligen Anmelden](active-directory-aadconnect-sso.md) (SSO) kombinieren. Auf diese Weise müssen Ihre Benutzer, wenn sie auf ihren Unternehmenscomputern innerhalb des Unternehmensnetzwerks arbeiten, zur Anmeldung bei Azure AD nicht einmal ihre Kennwörter eingeben – eine rundum ganzheitliche Erfahrung.

## <a name="whats-available-during-preview"></a>Was ist in der Preview-Version verfügbar?

>[!NOTE]
>Die Azure AD-Passthrough-Authentifizierung ist aktuell in der Preview-Version verfügbar. Es handelt sich um ein kostenloses Feature, sodass Sie für dessen Verwendung keine kostenpflichtigen Editionen von Azure AD benötigen.

Folgende Szenarien werden in der Preview-Version vollständig unterstützt:

- Alle webbrowserbasierte Anwendungen
- Office 365-Clientanwendungen, die die [moderne Authentifizierung](https://aka.ms/modernauthga) unterstützen

Folgende Szenarien werden in der Vorschauversion _nicht_ unterstützt:

- Ältere Office-Clientanwendungen und Exchange ActiveSync (d.h. native E-Mail-Anwendungen auf Mobilgeräten). <br>Organisationen wird empfohlen, möglichst zur modernen Authentifizierung zu wechseln. Dies ermöglicht die Unterstützung für Pass-Through-Authentifizierungen, hilft jedoch auch beim Schutz Ihrer Identitäten durch Features für den [bedingten Zugriff](../active-directory-conditional-access.md) wie etwa Multi-Factor Authentication.
- Azure AD Join für Windows 10-Geräte

>[!IMPORTANT]
>Als Problemumgehung für Szenarien, in denen derzeit kein Pass-Through-Authentifizierungsfeature unterstützt wird (ältere Office-Clientanwendungen, Exchange ActiveSync und Azure AD Join für Windows 10-Geräte), ist die Kennwortsynchronisierung standardmäßig ebenfalls aktiviert, wenn Sie die Pass-Through-Authentifizierung aktivieren. Die Synchronisierung von Kennwörtern fungiert nur in diesen speziellen Szenarien als Fallback. Wenn Sie sie nicht benötigen, können Sie die Kennwortsynchronisierung im Azure AD Connect-Assistenten auf der Seite [Optionale Features](active-directory-aadconnect-get-started-custom.md#optional-features) deaktivieren.

### <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Azure AD-Pass-Through-Authentifizierung aktivieren können, müssen folgende Voraussetzungen erfüllt sein:

- Ein Azure AD-Mandant, für den Sie globaler Administrator sind.

  >[!NOTE]
  >Es wird dringend empfohlen, dass das globale Administratorkonto ein ausschließliches Cloudkonto ist. Auf diese Weise können Sie die Konfiguration Ihres Mandanten verwalten, falls Ihre lokalen Dienste ausfallen oder nicht verfügbar sind. Erfahren Sie, wie Sie ein [rein cloudbasiertes Konto für den globalen Administrator hinzufügen](../active-directory-users-create-azure-portal.md).

- Azure AD Connect Version 1.1.486.0 oder höher. Es wird empfohlen, die [neueste Version von Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) zu verwenden.
- Ein Server unter Windows Server 2012 R2 oder höher, auf dem Azure AD Connect ausgeführt wird.
  - Dieser Server muss ein Mitglied derselben AD-Gesamtstruktur wie die der Benutzer sein, deren Kennwörter überprüft werden müssen.
  - Beachten Sie, dass ein Connector für die Passthrough-Authentifizierung auf dem gleichen Server wie Azure AD Connect installiert wird. Vergewissern Sie sich, dass die Connectorversion 1.5.58.0 oder höher ist.

    >[!NOTE]
    >Umgebungen mit mehreren Gesamtstrukturen werden unterstützt, wenn Gesamtstruktur-Vertrauensstellungen zwischen den AD-Gesamtstrukturen bestehen und das Namensuffixrouting ordnungsgemäß konfiguriert ist.

- Wenn Sie eine hohe Verfügbarkeit anstreben, benötigen Sie zusätzliche Server, auf denen Windows Server 2012 R2 oder höher ausgeführt wird, um eigenständige Connectors zu installieren. (Die Connectorversion muss 1.5.58.0 oder höher sein.)
- Wenn zwischen einem der Connectors und Azure AD eine Firewall vorhanden ist:
    - Wenn die URL-Filterung aktiviert ist, stellen Sie sicher, dass die Connectors mit den folgenden URLs kommunizieren können:
        -  \*.msappproxy.net
        -  \*.servicebus.windows.net
    - Stellen Sie sicher, dass die Connectors auch direkte IP-Verbindungen zu den [IP-Bereichen des Azure-Rechenzentrums](https://www.microsoft.com/en-us/download/details.aspx?id=41653) herstellen.
    - Stellen Sie sicher, dass die Firewall keine SSL-Überprüfung durchführt, da die Connectors Clientzertifikate zur Kommunikation mit Azure AD verwenden.
    - Stellen Sie sicher, dass die Connectors über die Ports 80 und 443 ausgehende Anforderungen an Azure AD senden können.
      - Wenn Ihre Firewall Regeln gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden.
      - Die Connectors stellen HTTP-Anforderungen über Port 80, um SSL-Zertifikatsperrlisten herunterzuladen. Dies wird auch für die ordnungsgemäße Funktion automatischer Updates benötigt.
      - Die Connectors stellen die HTTPS-Anforderungen für alle anderen Vorgänge über Port 443 bereit. Dazu gehören z.B. das Aktivieren und Deaktivieren des Features, das Registrieren von Connectors, das Herunterladen von Connectorupdates und das Verarbeiten aller Benutzeranmeldeanforderungen.

     >[!NOTE]
     >Wir haben kürzlich Verbesserungen vorgenommen, um die Anzahl der Ports, die von den Connectors zur Kommunikation mit unserem Dienst benötigt werden, zu reduzieren. Wenn ältere Versionen von Azure AD Connect- und/oder eigenständige Connectors ausgeführt werden, sollten Sie diese zusätzlichen Ports (5671, 8080, 9090, 9091, 9350, 9352, 10100–10120) weiterhin geöffnet lassen.

### <a name="enable-azure-ad-pass-through-authentication"></a>Aktivieren der Azure AD-Pass-Through-Authentifizierung

Die Azure AD-Passthrough-Authentifizierung kann über Azure AD Connect aktiviert werden.

Wenn Sie eine neue Installation von Azure AD Connect durchführen, wählen Sie den [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md). Wählen Sie auf der Seite **Benutzeranmeldung** die Option **Passthrough-Authentifizierung** aus. Nach erfolgreicher Durchführung wird hierdurch auf demselben Server wie Azure AD Connect ein Passthrough-Authentifizierungs-Connector installiert. Darüber hinaus wird die Passthrough-Authentifizierungsfunktion für Ihren Mandanten aktiviert.

![Azure AD Connect – Benutzeranmeldung](./media/active-directory-aadconnect-sso/sso3.png)

Wenn Azure AD Connect bereits installiert ist, richten Sie den [Expressinstallations-](active-directory-aadconnect-get-started-express.md) oder [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md) ein, wählen Sie in Azure AD Connect **Benutzeranmeldeseite ändern** aus, und klicken Sie auf **Weiter**. Wählen Sie dann **Passthrough-Authentifizierung** aus, um auf demselben Server wie Azure AD Connect einen Pass-Through-Authentifizierungs-Connector zu installieren, und aktivieren Sie das Feature für Ihren Mandanten.

![Azure AD Connect – Benutzeranmeldung ändern](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Die Azure AD-Passthrough-Authentifizierung ist eine Funktion auf Mandantenebene. Sie wirkt sich auf die Benutzeranmeldung in allen verwalteten Domänen in Ihrem Mandanten aus. Benutzer von Verbunddomänen melden sich jedoch weiterhin mithilfe der Active Directory-Verbunddienste (AD FS) oder einem anderen Verbundanbieter an, den Sie zuvor konfiguriert haben. Wenn Sie eine Verbunddomäne in eine verwaltete Domäne konvertieren, melden sich alle Benutzer in dieser Domäne automatisch mit der Pass-Through-Authentifizierung an. Reine Cloudbenutzer sind nicht von der Passthrough-Authentifizierung betroffen.

Wenn Sie den Einsatz der Pass-Through-Authentifizierung in einer Produktionsbereitstellung planen, wird dringend empfohlen, einen zweiten Connector auf einem separaten Server zu installieren (neben dem Server, auf dem Azure AD Connect und der erste Connector ausgeführt werden). Dadurch wird eine hohe Verfügbarkeit der Anmeldeanforderungen sichergestellt. Sie können beliebig viele zusätzliche Connectors installieren. Dies hängt von der Höchstanzahl und der durchschnittlichen Anzahl der Anmeldeanforderungen ab, die Ihr Mandant verarbeitet.

Befolgen Sie diese Anweisungen zum Bereitstellen eines eigenständigen Connectors.

#### <a name="step-1-download-and-install-the-connector"></a>Schritt 1: Herunterladen und Installieren des Connectors

In diesem Schritt laden Sie die Connector-Software herunter und installieren diese auf Ihrem Server.

1.    Den aktuellen Connector können Sie [hier](https://go.microsoft.com/fwlink/?linkid=837580) herunterladen. Vergewissern Sie sich, dass die Connectorversion 1.5.58.0 oder höher ist.
2.    Öffnen Sie die Eingabeaufforderung als Administrator.
3.    Führen Sie den folgenden Befehl aus: Die Option **/q** steht für die unbeaufsichtigte Installation, d.h., Sie werden bei der Installation nicht aufgefordert, den Endbenutzer-Lizenzvertrag zu akzeptieren: `
AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q
`

>[!NOTE]
>Sie können nur einen Connector pro Server installieren.

#### <a name="step-2-register-the-connector-with-azure-ad-for-pass-through-authentication"></a>Schritt 2: Registrieren des Connectors bei Azure AD für die Passthrough-Authentifizierung

In diesem Schritt registrieren Sie den installierten Connector auf Ihrem Server mithilfe unseres Dienstes und stellen ihn für den Empfang von Anmeldeanforderungen zur Verfügung.

1.    Öffnen Sie ein PowerShell-Fenster als Administrator.
2.    Navigieren Sie zu **C:\Programme\Microsoft AAD App Proxy Connector**, und führen Sie das folgende Skript aus: `.\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Feature PassthroughAuthentication`
3.    Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen des globalen Administratorkontos für Ihren Azure AD-Mandanten ein.

## <a name="how-does-azure-ad-pass-through-authentication-work"></a>Wie funktioniert die Azure AD-Pass-Through-Authentifizierung?

Wenn ein Benutzer versucht, sich bei Azure AD anzumelden (und die Pass-Through-Authentifizierung im Mandanten aktiviert ist), geschieht Folgendes:

1. Der Benutzer gibt auf der Seite „Azure AD-Anmeldung“ seinen Benutzernamen und sein Kennwort ein. Unser Dienst platziert den Benutzernamen und das (mit einem öffentlichen Schlüssel verschlüsselte) Kennwort in eine Warteschlange für die Überprüfung.
2. Einer der verfügbaren lokalen Connectors führt einen ausgehenden Aufruf an die Warteschlange durch und ruft den Benutzernamen und das Kennwort ab.
3. Der Connector überprüft dann mithilfe von Standard-Windows-APIs den Benutzernamen und das Kennwort in Active Directory (ein ähnlicher Mechanismus wie bei AD FS). Beachten Sie, dass es sich beim Benutzernamen entweder um den lokalen Benutzernamen (in der Regel `userPrincipalName`) handeln kann oder um ein anderes (als `Alternate ID` bezeichnetes) Attribut, das in Azure AD Connect konfiguriert ist.
4. Der lokale Domänencontroller wertet anschließend die Anforderung aus und gibt eine Antwort (Erfolg oder Fehler) an den Connector zurück.
5. Der Connector gibt diese Antwort wiederum an Azure AD zurück.
6. Azure AD wertet anschließend die Antwort aus und gibt eine entsprechende Antwort an den Benutzer zurück. Beispielsweise weist es der Anwendung einen Token zurück oder fordert zur Multi-Factor Authentication auf.

Das folgende Diagramm veranschaulicht die einzelnen Schritte. Alle Anforderungen und Antworten erfolgen über den HTTPS-Kanal.

![Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

### <a name="password-writeback"></a>Rückschreiben von Kennwörtern

Falls Sie das [Kennwortrückschreiben](../active-directory-passwords-update-your-own-password.md) in Ihrem Mandanten und für einen bestimmten Benutzer konfiguriert haben, kann dieser sein Kennwort bei der Anmeldung über die Pass-Through-Authentifizierung wie bisher ändern oder zurücksetzen. Die Kennwörter werden erwartungsgemäß in das lokale Active Directory zurückgeschrieben.

Wenn das Kennwortrückschreiben nicht für Ihren Mandanten konfiguriert ist oder dem Benutzer keine gültige Azure AD-Lizenz zugewiesen wurde, ist es dem Benutzer nicht gestattet, seine Kennwörter in der Cloud zu aktualisieren. Dies gilt auch, wenn das Kennwort abgelaufen ist. Der Benutzer sieht dann folgende Meldung: „Eine Kennwortänderung auf dieser Website wird von Ihrer Organisation nicht gestattet. Ändern Sie Ihr Kennwort anhand der von Ihrer Organisation empfohlenen Methode, oder bitten Sie Ihren Administrator um Hilfe.“.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie das Feature für [nahtloses einmaliges Anmelden (SSO) in Azure AD](active-directory-aadconnect-sso.md) für Ihren Mandanten aktivieren.
- Lesen Sie unsere [Anleitung zur Problembehandlung](active-directory-aadconnect-troubleshoot-pass-through-authentication.md), um zu erfahren, wie Sie häufig auftretende Probleme mit der Azure AD-Pass-Through-Authentifizierung beheben.

## <a name="feedback"></a>Feedback
Ihr Feedback ist uns sehr wichtig. Geben Sie etwaige Fragen im Abschnitt „Kommentare“ ein. Nutzen Sie unser [UserVoice-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect), um neue Features anzufordern.


