---
title: 'Weitere Informationen: Azure Active Directory-Kennwortverwaltung | Microsoft-Dokumentation'
description: "Weiterführende Themen zur Azure AD-Kennwortverwaltung, einschließlich Kennwortrückschreibung, Sicherheit bei der Kennwortrückschreibung, Funktionsweise des Portals für die Kennwortzurücksetzung und der zur Kennwortzurücksetzung verwendeten Daten."
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: d3be2912-76c8-40a0-9507-b7b1a7ce2f8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: joflore
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: a07051ea0be58cafcf1a7d7ae800b44e7abd05cd
ms.lasthandoff: 04/13/2017


---
# <a name="learn-more-about-password-management"></a>Weitere Informationen zur Kennwortverwaltung
> [!IMPORTANT]
> **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-my-password).
>
>

Wenn Sie die Kennwortverwaltung bereits bereitgestellt haben oder vor der Bereitstellung einfach nur mehr über die technischen Grundlagen erfahren möchten, liefert dieser Abschnitt einen guten Überblick über die technischen Konzepte, die hinter dem Dienst stehen. In diesem Artikel werden die folgenden Themen abgedeckt:

* [**Wie funktioniert das Portal für die Kennwortzurücksetzung?**](#how-does-the-password-reset-portal-work)
* [**Übersicht über die Kennwortrückschreibung**](#password-writeback-overview)
 * [Funktionsweise der Kennwortrückschreibung](#how-password-writeback-works)
* [**Unterstützte Szenarien für das Kennwortrückschreiben**](#scenarios-supported-for-password-writeback)
 * [Unterstützte Clients: Azure AD Connect, Azure AD Sync und DirSync](#supported-clients)
 * [Erforderliche Lizenzen für das Kennwortrückschreiben](#licenses-required-for-password-writeback)
 * [Für das Kennwortrückschreiben unterstützte lokale Authentifizierungsmodi](#on-premises-authentication-modes-supported-for-password-writeback)
 * [Für das Kennwortrückschreiben unterstützte Benutzer- und Administratorvorgänge](#user-and-admin-operations-supported-for-password-writeback)
 * [Für das Kennwortrückschreiben nicht unterstützte Benutzer- und Administratorvorgänge](#user-and-admin-operations-not-supported-for-password-writeback)
* [**Sicherheitsmodell für das Kennwortrückschreiben**](#password-writeback-security-model)
 * [Verschlüsselungsdetails für das Kennwortrückschreiben](#password-writeback-encryption-details)
 * [Bandbreitennutzung für das Kennwortrückschreiben](#password-writeback-bandwidth-usage)
* [**Bereitstellen, Verwalten und Zugreifen auf Kennwortzurücksetzungsdaten für Ihre Benutzer**](#deploying-managing-and-accessing-password-reset-data-for-your-users)
 * [Welche Daten werden bei der Kennwortzurücksetzung verwendet?](#what-data-is-used-by-password-reset)
 * [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](#deploying-password-reset-without-requiring-end-user-registration)
 * [Was passiert, wenn sich ein Benutzer für die Kennwortzurücksetzung registriert?](#what-happens-when-a-user-registers)
 * [Zugriff auf Daten zur Kennwortzurücksetzung für Ihre Benutzer](#how-to-access-password-reset-data-for-your-users)
 * [Festlegen der Daten für die Kennwortzurücksetzung mit PowerShell](#setting-password-reset-data-with-powershell)
 * [Lesen der Daten für die Kennwortzurücksetzung mit PowerShell](#reading-password-reset-data-with-powershell)
* [**Funktionsweise der Kennwortzurücksetzung für B2B-Benutzer**](#how-does-password-reset-work-for-b2b-users)

## <a name="how-does-the-password-reset-portal-work"></a>Wie funktioniert das Portal für die Kennwortzurücksetzung?
Wenn ein Benutzer zum Portal für die Kennwortzurücksetzung navigiert, wird ein Workflow gestartet. Über diesen Workflow wird geprüft, ob das Benutzerkonto gültig ist, welcher Organisation der Benutzer angehört, wo das Benutzerkennwort verwaltet wird und ob der Benutzer zur Verwendung der Funktion lizenziert ist.  Nachfolgend wird beschrieben, welche Logik hinter der Seite zur Kennwortzurücksetzung steckt.

1. Der Benutzer klickt auf den Link "Können Sie nicht auf Ihr Konto zugreifen?" oder wechselt direkt zu [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2. Der Benutzer gibt eine Benutzer-ID ein und durchläuft erfolgreich die Captchaprüfung.
3. Azure AD prüft folgendermaßen, ob der Benutzer diese Funktion verwenden darf:
   * Es wird geprüft, ob die Funktion für diesen Benutzer aktiviert ist und ob eine Azure AD-Lizenz zugewiesen ist.
     * Wenn diese Funktion für den Benutzer nicht aktiviert ist oder keine Lizenz vorliegt, wird der Benutzer aufgefordert, sich zum Zurücksetzen des Kennworts an den Administrator zu wenden.
   * Es wird überprüft, ob der Benutzer in seinem Konto Daten für die Überprüfung in mehreren Schritten definiert hat, die der Administratorrichtlinie entsprechen.
     * Wenn die Richtlinie nur eine Überprüfung in einem Schritt vorsieht, wird sichergestellt, dass der Benutzer für mindestens eine der durch die Administratorrichtlinie aktivierten Überprüfungen geeignete Daten definiert hat.
       * Sind keine entsprechenden Daten konfiguriert, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
     * Wenn die Richtlinie eine Überprüfung in zwei Schritten vorsieht, wird sichergestellt, dass der Benutzer für mindestens zwei der durch die Administratorrichtlinie aktivierten Überprüfungen geeignete Daten definiert hat.
       * Sind keine entsprechenden Daten konfiguriert, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
   * Es wird überprüft, ob das Benutzerkennwort lokal verwaltet wird oder nicht (im Verbund oder mit Kennworthashsynchronisierung).
     * Wenn das Rückschreiben von Kennwörtern konfiguriert ist und das Benutzerkennwort lokal verwaltet wird, kann der Benutzer mit der Authentifizierung fortfahren und sein Kennwort zurücksetzen.
     * Wenn das Rückschreiben von Kennwörtern nicht konfiguriert ist und das Benutzerkennwort lokal verwaltet wird, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
4. Wenn festgestellt wird, dass der Benutzer sein Kennwort zurücksetzen darf, wird er durch den Vorgang für die Kennwortzurücksetzung geleitet.

Weitere Informationen zum Bereitstellen der Kennwortrückschreibung finden Sie unter [Erste Schritte: Azure AD-Kennwortverwaltung](active-directory-passwords-getting-started.md).

## <a name="password-writeback-overview"></a>Übersicht über die Kennwortrückschreibung
Die Kennwortrückschreibung ist eine Komponente von [Azure Active Directory Connect](connect/active-directory-aadconnect.md) , die von den aktuellen Abonnenten von Azure Active Directory Premium aktiviert und verwendet werden kann. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

Mit der Kennwortrückschreibung können Sie Ihren Cloudmandanten so konfigurieren, dass Kennwörter in das lokale Active Directory zurückgeschrieben werden.  Auf diese Weise ist es nicht erforderlich, eine lokale Self-Service-Lösung für das Zurücksetzen von Kennwörtern einzurichten und zu verwalten. Gleichzeitig bietet diese Funktion Ihren Benutzern eine bequeme, cloudbasierte Möglichkeit zum Zurücksetzen ihrer lokalen Kennwörter – unabhängig davon, wo sie sich gerade befinden.  Nachfolgend werden einige der wichtigsten Merkmale der Kennwortrückschreibung aufgeführt:

* **Sofortiges Feedback.**  Die Kennwortrückschreibung ist ein synchroner Vorgang.  Ihre Benutzer werden sofort benachrichtigt, wenn ihre Kennwörter nicht der Richtlinie entsprechen oder das Zurücksetzen oder Ändern des Kennworts aus beliebigen Gründen nicht möglich war.
* **Unterstützung für das Zurücksetzen von Kennwörtern für Benutzer, die AD FS oder andere Verbundtechnologien nutzen.**  Solange die Verbundbenutzerkonten mit Ihrem Azure AD-Mandanten synchronisiert sind, können die Benutzer bei Verwendung der Kennwortrückschreibung ihre lokalen AD-Kennwörter über die Cloud verwalten.
* **Unterstützung für das Zurücksetzen von Kennwörtern für Benutzer, die eine Kennworthashsynchronisierung verwenden.** Wenn der Dienst für die Kennwortzurücksetzung ermittelt, dass ein synchronisiertes Benutzerkonto für die Kennworthashsynchronisierung aktiviert ist, werden das lokale Kennwort und das Cloudkennwort für dieses Konto simultan zurückgesetzt.
* **Unterstützung für das Ändern von Kennwörtern über den Zugriffsbereich und Office 365.**  Wenn Verbundbenutzer oder Benutzer mit Kennwortsynchronisierung ihre abgelaufenen oder noch nicht abgelaufenen Kennwörter ändern möchten, werden diese Kennwörter in die lokale AD-Umgebung zurückgeschrieben.
* **Unterstützung für das Zurückschreiben von Kennwörtern, wenn diese im [**Azure-Verwaltungsportal**](https://manage.windowsazure.com) von einem Administrator zurückgesetzt werden**.  Wenn ein Administrator ein Kennwort im [Azure-Verwaltungsportal](https://manage.windowsazure.com) zurücksetzt und es sich um ein Verbundkonto oder um ein Konto mit Kennwortsynchronisierung handelt, wird das vom Administrator festgelegte Kennwort auch im lokalen AD festgelegt.  Diese Funktion wird im Office-Verwaltungsportal derzeit nicht unterstützt.
* **Erzwingung von lokalen AD-Kennwortrichtlinien.**  Wenn ein Benutzer sein Kennwort zurücksetzt, wird sichergestellt, dass es die lokalen AD-Richtlinien erfüllt, bevor es in das Verzeichnis geschrieben wird.  Dies schließt Verlauf, Komplexität, Alter, Kennwortfilter und alle weiteren Kennwortbeschränkungen ein, die Sie im lokalen AD definiert haben.
* **Keine Notwendigkeit zur Festlegung von eingehenden Firewallregeln.**  Bei der Kennwortrückschreibung wird als zugrunde liegender Kommunikationskanal ein Azure Service Bus Relay verwendet. Dies bedeutet, dass Sie keine eingehenden Ports in Ihrer Firewall öffnen müssen, damit diese Funktion ordnungsgemäß arbeitet.
* **Keine Unterstützung für Benutzerkonten in geschützten Gruppen, die im lokalen Active Directory vorliegen.** Weitere Informationen zu geschützten Gruppen finden Sie unter [Geschützte Konten und Gruppen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx)weiter.

### <a name="how-password-writeback-works"></a>Funktionsweise der Kennwortrückschreibung
Die Kennwortrückschreibung umfasst drei Hauptkomponenten:

* Einen Clouddienst für das Zurücksetzen von Kennwörtern (auch integriert in die Azure AD-Seiten für die Kennwortzurücksetzung)
* Ein mandantenspezifisches Azure Service Bus Relay
* Einen lokalen Endpunkt für die Kennwortzurücksetzung

Diese Komponenten arbeiten folgendermaßen zusammen:

  ![][001]

Wenn ein  Verbundbenutzer oder ein Benutzer mit Kennworthashsynchronisierung ihr Kennwort in der Cloud zurücksetzen oder ändern, geschieht Folgendes:

1. Es wird überprüft, über welche Art von Kennwort der Benutzer verfügt.  Falls das Kennwort lokal verwaltet wird, wird zunächst sichergestellt, dass der Dienst für das Zurückschreiben aktiv ist und ausgeführt wird.  Ist der Dienst aktiv, kann der Benutzer fortfahren. Ist der Dienst nicht aktiv, wird der Benutzer darüber informiert, dass das Kennwort an dieser Stelle nicht zurückgesetzt werden kann.
2. Im nächsten Schritt muss der Benutzer sich erfolgreich authentifizieren und gelangt dann zum Bildschirm für die Kennwortzurücksetzung.
3. Der Benutzer wählt ein neues Kennwort aus und bestätigt es.
4. Beim Klicken auf "Senden" wird das Klartextkennwort mit einem symmetrischen Schlüssel verschlüsselt, der beim Einrichten der Kennwortrückschreibung erstellt wurde.
5. Nachdem das Kennwort verschlüsselt wurde, wird es in eine Nutzlast eingeschlossen, die über einen HTTPS-Kanal an das mandantenspezifische Service Bus Relay gesendet wird (dieses wurde ebenfalls beim Einrichten der Kennwortrückschreibung festgelegt).  Dieses Relay wird durch ein zufällig generiertes Kennwort geschützt, das nur der lokalen Installation bekannt ist.
6. Sobald die Nachricht vom Service Bus empfangen wird, wird der Endpunkt für die Kennwortzurücksetzung automatisch aktiviert und erkennt, dass eine Anforderung zur Kennwortzurücksetzung aussteht.
7. Der Dienst sucht anschließend unter Verwendung des Cloudankerattributs nach dem betreffenden Benutzer.  Wenn diese Suche erfolgreich war, muss das Benutzerobjekt im AD-Connectorbereich vorliegen, dem entsprechenden MV-Objekt und dem entsprechenden AAD-Connectorobjekt zugeordnet sein. Und schließlich muss – damit dieses Benutzerkonto über das Synchronisierungsmodul gefunden wird – für die Verbindung zwischen AD-Connectorobjekt und MV die Synchronisierungsregel `Microsoft.InfromADUserAccountEnabled.xxx` gelten.  Dies ist erforderlich, weil der Aufruf von der Cloud eingeht, das Synchronisierungsmodul das cloudAnchor-Attribut verwendet, um das AAD-Connectorbereichsobjekt zu ermitteln, der Verbindung zum MV-Objekt und anschließend der Verbindung zurück zum AD-Objekt folgt. Da mehrere AD-Objekte (mehrere Gesamtstrukturen) für denselben Benutzer vorliegen können, wählt das Synchronisierungsmodul das richtige Objekt basierend auf der `Microsoft.InfromADUserAccountEnabled.xxx` -Verbindung aus. Als Folge dieser Logik müssen Sie für Azure AD Connect eine Verbindung mit dem primären Domänencontroller herstellen, damit das Kennwortrückschreiben funktioniert.  In diesem Fall können Sie Azure AD Connect für die Verwendung eines primären Domänencontrolleremulators konfigurieren, indem Sie mit der rechten Maustaste auf die **Eigenschaften** des Active Directory-Synchronisierungsconnectors klicken und dann die Option **Verzeichnispartitionen konfigurieren** auswählen. Suchen Sie nach dem Abschnitt **domain controller connection settings** (Domänencontroller-Verbindungseinstellungen), und aktivieren Sie das Kontrollkästchen **only use preferred domain controllers** (Nur bevorzugte Domänencontroller verwenden). Hinweis: Wenn der bevorzugte DC kein PDC-Emulator ist, nutzt Azure AD Connect trotzdem den PDC für das Kennwortrückschreiben.
8. Sobald das Benutzerkonto ermittelt wurde, wird versucht, das Kennwort direkt in der geeigneten AD-Gesamtstruktur zurückzusetzen.
9. Wenn die Kennwortzurücksetzung erfolgreich war, wird der Benutzer darüber informiert, dass das Kennwort geändert wurde, und der Vorgang ist damit abgeschlossen. Wenn das Kennwort des Benutzers mithilfe von Kennwortsynchronisierung mit Azure AD synchronisiert wird, kann es vorkommen, dass die lokale Kennwortrichtlinie schwächer als die Cloud-Kennwortrichtlinie ist. In diesem Fall erzwingen wir die lokale Kennwortrichtlinie und ermöglichen das Synchronisieren des Kennworthashs durch die Kennworthashsynchronisierung. So wird das Erzwingen der lokalen Richtlinie in der Cloud sichergestellt, unabhängig davon, ob Sie für die Bereitstellung von einmaligem Anmelden Kennwortsynchronisierung oder Verbund verwenden.
10. Wenn beim Zurücksetzen des Kennworts ein Fehler auftritt, wird dem Benutzer eine Fehlermeldung angezeigt, und der Benutzer kann es erneut versuchen.  Es kann zu Fehlern kommen, weil der Dienst nicht verfügbar ist, weil das ausgewählte Kennwort nicht den Organisationsrichtlinien entspricht, weil der Benutzer nicht im lokalen AD gefunden wurde, oder weil andere Gründe vorliegen.  Es gibt für jeden dieser Fälle eine spezifische Meldung, die den Benutzer darüber informiert, was zur Problemlösung unternommen werden kann.

## <a name="scenarios-supported-for-password-writeback"></a>Unterstützte Szenarien für die Kennwortrückschreibung
Im folgenden Abschnitt erfahren Sie, welche Szenarien für welche Versionen der Synchronisierungsfunktionen unterstützt werden.  Grundsätzlich empfiehlt es sich, das automatische Update von Azure AD Connect zu verwenden oder die neueste Version des [Azure AD Connect](connect/active-directory-aadconnect.md#install-azure-ad-connect) zu installieren, wenn Sie das Kennwortrückschreiben verwenden möchten.

* [**Unterstützte Clients: Azure AD Connect, Azure AD Sync und DirSync**](#supported-clients)
* [**Erforderliche Lizenzen für das Kennwortrückschreiben**](#licenses-required-for-password-writeback)
* [**Für das Kennwortrückschreiben unterstützte lokale Authentifizierungsmodi**](#on-premises-authentication-modes-supported-for-password-writeback)
* [**Für das Kennwortrückschreiben unterstützte Benutzer- und Administratorvorgänge**](#user-and-admin-operations-supported-for-password-writeback)
* [**Für das Kennwortrückschreiben nicht unterstützte Benutzer- und Administratorvorgänge**](#user-and-admin-operations-not-supported-for-password-writeback)

### <a name="supported-clients"></a>Unterstützte Clients
Es empfiehlt sich, das automatische Update von Azure AD Connect zu verwenden oder die neueste Version des [Azure AD Connect](connect/active-directory-aadconnect.md#install-azure-ad-connect) zu installieren, wenn Sie das Kennwortrückschreiben verwenden möchten.

* **DirSync (beliebige Version > 1.0.6862)** - _NICHT UNTERSTÜTZT:_ Unterstützt nur grundlegende Rückschreibfunktionen und wird von der Produktgruppe nicht mehr unterstützt.
* **Azure AD Sync** - _VERALTET:_ Unterstützt nur grundlegende Rückschreibfunktionen, und es stehen keine Kontoentsperrfunktionen, keine umfangreiche Protokollierung und keine Verbesserungen bei der Zuverlässigkeit aus Azure AD Connect zur Verfügung. Daher empfehlen wir **dringend** ein Upgrade.
* **Azure AD Connect** - _VOLLSTÄNDIG UNTERSTÜTZT:_ Unterstützt alle Rückschreibfunktionen. Führen Sie ein Upgrade auf die neueste Version aus, um von den besten neuen Features sowie von höchstmöglicher Stabilität/Zuverlässigkeit zu profitieren.

### <a name="licenses-required-for-password-writeback"></a>Erforderliche Lizenzen für das Kennwortrückschreiben
Zur Verwendung des Kennwortrückschreibens muss in Ihrem Mandanten eine der folgenden Lizenzen zugewiesen sein:

* **Azure AD Premium P1:** Uneingeschränkte Verwendung des Kennwortrückschreibens
* **Azure AD Premium P2:** Uneingeschränkte Verwendung des Kennwortrückschreibens
* **Enterprise Mobility Suite:** Uneingeschränkte Verwendung des Kennwortrückschreibens
* **Enterprise Cloud Suite:** Uneingeschränkte Verwendung des Kennwortrückschreibens

Mit einem Office 365-Lizenzierungsplan kann das Kennwortrückschreiben nicht verwendet werden. Dies gilt sowohl für Testversionen als auch für kostenpflichtige Versionen. Zur Verwendung des Features ist ein Upgrade auf einen der oben aufgeführten Pläne erforderlich.

Eine Aktivierung des Kennwortrückschreibens für eine der Office 365-SKUs ist nicht geplant.

### <a name="on-premises-authentication-modes-supported-for-password-writeback"></a>Für das Kennwortrückschreiben unterstützte lokale Authentifizierungsmodi
Das Kennwortrückschreiben kann für folgende Arten von Benutzerkennwörtern verwendet werden:

* **Reine Cloudbenutzer:** In diesem Szenario ist das Kennwortrückschreiben nicht relevant, da kein lokales Kennwort vorhanden ist.
* **Benutzer mit synchronisiertem Kennwort:** Kennwortrückschreiben wird unterstützt.
* **Verbundbenutzer:** Kennwortrückschreiben wird unterstützt.
* **Benutzer mit Passthrough-Authentifizierung:** Kennwortrückschreiben wird unterstützt.

### <a name="user-and-admin-operations-supported-for-password-writeback"></a>Für das Kennwortrückschreiben unterstützte Benutzer- und Administratorvorgänge
Kennwörter werden in folgenden Szenarien zurückgeschrieben:

* **Unterstützte Vorgänge für Endbenutzer**
 * Jegliche freiwillige Self-Service-Kennwortänderung durch einen Endbenutzer
 * Jegliche erzwungene Self-Service-Kennwortänderung durch einen Endbenutzer (beispielsweise bei Ablauf des Kennworts)
 * Jegliche Self-Service-Kennwortzurücksetzung durch einen Endbenutzer über das [Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com)
* **Unterstützte Vorgänge für Administratoren**
 * Jegliche freiwillige Self-Service-Kennwortänderung durch einen Administrator
 * Jegliche erzwungene Self-Service-Kennwortänderung durch einen Administrator (beispielsweise bei Ablauf des Kennworts)
 * Jegliche Self-Service-Kennwortzurücksetzung durch einen Administrator über das [Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com)
 * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung über das [klassische Azure-Verwaltungsportal](https://manage.windowsazure.com)
 * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung über das [Azure-Portal](https://portal.azure.com)

### <a name="user-and-admin-operations-not-supported-for-password-writeback"></a>Für das Kennwortrückschreiben nicht unterstützte Benutzer- und Administratorvorgänge
Kennwörter werden in folgenden Szenarien nicht zurückgeschrieben:

* **Nicht unterstützte Vorgänge für Endbenutzer**
 * Jegliches Zurücksetzen des Kennworts durch Endbenutzer mithilfe von PowerShell v1, v2 oder der Azure AD-Graph-API
* **Nicht unterstützte Vorgänge für Administratoren**
 * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung über das [Office-Verwaltungsportal](https://portal.office.com)
 * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung mithilfe von PowerShell v1, v2 oder der Azure AD-Graph-API

Wir arbeiten zwar daran, diese Einschränkungen zu entfernen, ein genauer Zeitplan steht jedoch noch nicht fest.

## <a name="password-writeback-security-model"></a>Sicherheitsmodell für die Kennwortrückschreibung
Die Kennwortrückschreibung ist ein äußerst sicherer und zuverlässiger Dienst.  Zum Schutz Ihrer Informationen wird ein vierstufiges Sicherheitsmodell angewendet, das nachfolgend beschrieben wird.

* **Mandantenspezifisches Service Bus Relay** – Beim Einrichten des Diensts wird ein mandantenspezifisches Service Bus Relay eingerichtet, das durch ein zufällig generiertes, sicheres Kennwort geschützt wird, auf das Microsoft keinen Zugriff hat.
* **Nicht zugänglicher, sicherer Kryptografieschlüssel für die Kennwortverschlüsselung** – Nach der Erstellung des Service Bus Relays wird ein sicherer symmetrischer Schlüssel erstellt, mit dem das Kennwort verschlüsselt wird, bevor es gesendet wird.  Dieser Schlüssel liegt ausschließlich im Speicher für geheime Schlüssel Ihres Unternehmens in der Cloud vor, der streng geschützt und überwacht wird, wie jedes Kennwort im Verzeichnis.
* **TLS nach Industriestandard** – Beim Zurücksetzen oder Ändern eines Kennworts in der Cloud wird das Klartextkennwort mit Ihrem öffentlichen Schlüssel verschlüsselt.  Anschließend wird es in eine HTTPS-Nachricht eingefügt und über einen mithilfe der SSL-Zertifikate von Microsoft verschlüsselten Kanal an Ihre Service Bus Relay gesendet.  Nachdem die Nachricht vom Service Bus empfangen wurde, wird der lokale Agent aktiviert, authentifiziert sich beim Service Bus mit dem zuvor generierten sicheren Kennwort, nimmt die verschlüsselte Nachricht entgegen, entschlüsselt diese mit dem generierten privaten Schlüssel und versucht anschließend, das Kennwort über die AD DS-SetPassword-API festzulegen.  In diesem Schritt kann Ihre lokale AD-Kennwortrichtlinie (Komplexität, Alter, Verlauf, Filter usw.) in der Cloud erzwungen werden.
* **Richtlinien zum Nachrichtenablauf** – Falls die Nachricht im Service Bus verbleibt, weil der lokale Dienst nicht verfügbar ist, kommt es nach wenigen Minuten zu einem Timeout, und die Nachricht wird entfernt, um die Sicherheit weiter zu erhöhen.

### <a name="password-writeback-encryption-details"></a>Verschlüsselungsdetails für das Kennwortrückschreiben
In diesem Abschnitt erfahren Sie, welche Verschlüsselungsschritte eine vom Benutzer übermittelte Kennwortzurücksetzungsanforderung vor dem Erreichen Ihrer lokalen Umgebung durchläuft, um ein Höchstmaß an Dienstzuverlässigkeit und Sicherheit zu gewährleisten.

* **Schritt 1: Kennwortverschlüsselung mit 2048-Bit-RSA-Schlüssel:** Wenn ein Benutzer ein zurückzuschreibendes Kennwort an die lokale Umgebung übermittelt, wird zunächst das eigentliche Kennwort mit einem 2048-Bit-RSA-Schlüssel verschlüsselt.

* **Schritt 2: Verschlüsselung auf Paketebene mit AES-GCM:** Als Nächstes wird das gesamte Paket (bestehend aus Kennwort und den erforderlichen Metadaten) mithilfe von AES-GCM verschlüsselt. So wird verhindert, dass jemand mit direktem Zugriff auf den zugrunde liegenden ServiceBus-Kanal den Inhalt anzeigen und/oder manipulieren kann.

* **Schritt 3: Abwicklung der gesamten Kommunikation über TLS/SSL:** Darüber hinaus wird die gesamte Kommunikation mit ServiceBus über einen TLS/SSL-Kanal abgewickelt. Dadurch wird der Inhalt vor nicht autorisierten Dritten geschützt.

* **Schritt 4: Automatischer Schlüsselrollover im Abstand von sechs Monaten:** Außerdem erfolgt alle sechs Monate (sowie bei jeder Deaktivierung/Reaktivierung des Kennwortrückschreibens in Azure AD Connect) ein Rollover aller entsprechenden Schlüssel, um ein Höchstmaß an Dienstzuverlässigkeit und Sicherheit zu erreichen.

### <a name="password-writeback-bandwidth-usage"></a>Bandbreitennutzung für das Kennwortrückschreiben

Kennwortrückschreiben ist ein Dienst, für den sehr wenig Bandbreite erforderlich ist und der nur unter folgenden Umständen Anforderungen zurück an den lokalen Agent sendet:

1. Beim Aktivieren oder Deaktivieren des Features über Azure AD Connect werden zwei Nachrichten gesendet.
2. Während der gesamten Ausführungsdauer des Diensts wird als Diensttakt alle fünf Minuten eine Nachricht gesendet.
3. Bei jeder Übermittlung eines neuen Kennworts werden zwei Nachrichten gesendet: eine Nachricht als Anforderung zur Ausführung des Vorgangs und eine nachfolgende Nachricht mit dem Ergebnis des Vorgangs. Diese Nachrichten werden in folgenden Situationen gesendet:
4. Bei jeder Übermittlung eines neuen Kennworts während einer Self-Service-Kennwortzurücksetzung durch Benutzer
5. Bei jeder Übermittlung eines neuen Kennworts während einer Benutzerkennwortänderung
6. Bei jeder Übermittlung eines neuen Kennworts während einer vom Administrator initiierten Benutzerkennwortzurücksetzung (nur über die Azure-Administratorportale)

#### <a name="message-size-and-bandwidth-considerations"></a>Überlegungen zur Nachrichtengröße und Bandbreite

Die Größe der einzelnen oben beschriebenen Nachrichten beträgt in der Regel weniger als 1 KB. Das bedeutet, dass selbst bei extrem hoher Auslastung der Dienst für die Kennwortrückschreibung höchstens eine Bandbreite von einigen wenigen Kilobits pro Sekunde nutzt. Da jede Nachricht in Echtzeit und nur dann gesendet wird, wenn dies für eine Kennwortaktualisierung erforderlich ist, und die Nachrichten nicht besonders groß sind, ist die Bandbreitennutzung der Rückschreibfunktion zu klein, um messbare Auswirkungen zu haben.

## <a name="deploying-managing-and-accessing-password-reset-data-for-your-users"></a>Bereitstellen, Verwalten und Zugreifen auf Kennwortzurücksetzungsdaten für Ihre Benutzer
Kennwortzurücksetzungsdaten für Ihre Benutzer können über Azure AD Connect, PowerShell, Graph oder unsere Registrierungsumgebungen verwaltet und aufgerufen werden.  Und dank der im Anschluss aufgeführten Optionen haben Sie sogar die Möglichkeit, die Kennwortzurücksetzung für die gesamte Organisation bereitzustellen, ohne dass sich Benutzer dafür registrieren müssen.

  * [Welche Daten werden bei der Kennwortzurücksetzung verwendet?](#what-data-is-used-by-password-reset)
  * [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](#deploying-password-reset-without-requiring-end-user-registration)
  * [Was passiert, wenn sich ein Benutzer für die Kennwortzurücksetzung registriert?](#what-happens-when-a-user-registers)
  * [Zugriff auf Daten zur Kennwortzurücksetzung für Ihre Benutzer](#how-to-access-password-reset-data-for-your-users)
  * [Festlegen der Daten für die Kennwortzurücksetzung mit PowerShell](#setting-password-reset-data-with-powershell)
  * [Lesen der Daten für die Kennwortzurücksetzung mit PowerShell](#reading-password-reset-data-with-powershell)

### <a name="what-data-is-used-by-password-reset"></a>Welche Daten werden bei der Kennwortzurücksetzung verwendet?
In der folgenden Tabelle wird beschrieben, wo und wie diese Daten während des Vorgangs der Kennwortzurücksetzung verwendet werden. Diese Aufstellung soll Sie dabei unterstützen, die für Ihre Organisation geeigneten Authentifizierungsoptionen auszuwählen. Diese Tabelle zeigt auch Formatierungsanforderungen für Fälle, in denen Sie Daten im Namen des Benutzers aus Eingabepfaden bereitstellen, die diese Daten nicht validieren.

> [!NOTE]
> Die Option "Bürotelefon" wird im Registrierungsportal nicht angezeigt, da Benutzer diese Eigenschaft zurzeit nicht im Verzeichnis bearbeiten können. Dieser Wert kann nur von Administratoren festgelegt werden.
>
>

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Name der Kontaktmethode</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Active Directory-Datenelement</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Azure Active Directory-Datenelement</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Wo verwendet / festlegbar?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Formatanforderungen</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Bürotelefon</p>
            </td>
            <td>
              <p>telephoneNumber</p>
              <p>Diese Eigenschaft kann mit dem Attribut „PhoneNumber“ in Azure Active Directory synchronisiert und umgehend (OHNE vorherige Registrierung des Benutzers) für die Kennwortzurücksetzung verwendet werden.</p>
            </td>
            <td>
              <p>PhoneNumber</p>
              <p>e.g. Set-MsolUser -UserPrincipalName JWarner@contoso.com -PhoneNumber "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>Verwendet in:</p>
              <p>Kennwortzurücksetzungsportal</p>
              <p>Festlegbar wo:</p>
              <p>"PhoneNumber" kann in PowerShell, DirSync, im Azure-Verwaltungsportal und im Office-Verwaltungsportal festgelegt werden.</p>
            </td>
            <td>
              <p>+ccc xxxyyyzzzz (z. B. +1 1234567890)</p>
              <ul>
                <li class="unordered">
Muss eine Landeskennzahl angeben.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss eine Ortskennzahl angeben (sofern zutreffend).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss über ein Pluszeichen (+) vor der Landeskennzahl verfügen.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss ein Leerzeichen zwischen Landeskennzahl und den übrigen Ziffern enthalten.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Durchwahlnummern werden nicht unterstützt. Wenn Sie eine Durchwahl angeben, wird diese von der Nummer abgeschnitten, bevor der Telefonanruf erfolgt.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobiltelefon</p>
            </td>
            <td>
              <p>Mobile</p>
              <p>Diese Eigenschaft kann mit dem Attribut „MobilePhone“ in Azure Active Directory synchronisiert und umgehend (OHNE vorherige Registrierung des Benutzers) für die Kennwortzurücksetzung verwendet werden.</p>
              <p>Sie kann derzeit nicht mit „AuthenticationPhone“ synchronisiert werden.</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>OR</p>
              <p>MobilePhone</p>
              <p>("AuthenticationPhone" wird verwendet, wenn Daten vorhanden sind, andernfalls wird das Feld "Mobiltelefon" verwendet.)</p>
              <p>z.B. Set-MsolUser -UserPrincipalName JWarner@contoso.com -MobilePhone "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>Verwendet in:</p>
              <p>Kennwortzurücksetzungsportal</p>
              <p>Registrierungsportal</p>
              <p>Festlegbar wo: </p>
              <p>"AuthenticationPhone" kann im Kennwortzurücksetzungsportal oder im MFA-Registrierungsportal festgelegt werden.</p>
              <p>„MobilePhone“ kann in PowerShell, Azure AD Connect, im Azure-Verwaltungsportal und im Office-Verwaltungsportal festgelegt werden.</p>
            </td>
            <td>
              <p>+ccc xxxyyyzzzz (z. B. +1 1234567890)</p>
              <ul>
                <li class="unordered">
Muss eine Landeskennzahl angeben.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss eine Ortskennzahl angeben (sofern zutreffend).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss über ein Pluszeichen (+) vor der Landeskennzahl verfügen.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss ein Leerzeichen zwischen Landeskennzahl und den übrigen Ziffern enthalten.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Durchwahlnummern werden nicht unterstützt. Wenn Sie eine Durchwahl angeben, wird diese ignoriert, wenn der Telefonanruf erfolgt.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Alternative E-Mail-Adresse</p>
            </td>
            <td>
              <p>Nicht verfügbar</p>
              <p>Werte aus Active Directory können derzeit weder mit der Eigenschaft „AuthenticationEmail“ noch mit der Eigenschaft „AlternateEmailAddresses[0]“ synchronisiert werden. </p>
              <p>„AlternateEmailAddresses[0]“ kann mithilfe von PowerShell festgelegt werden. Eine entsprechende Anleitung finden Sie im Abschnitt nach dieser Tabelle.</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>OR</p>
              <p>AlternateEmailAddresses[0] </p>
              <p>("AuthenticationEmail" wird verwendet, wenn Daten vorhanden sind, andernfalls wird das Feld "Alternative E-Mail-Adresse" verwendet.)</p>
              <p>Hinweis: Das Feld "Alternative E-Mail-Adresse" wird als Array aus Zeichenfolgen im Verzeichnis angegeben.  Es wird der erste Eintrag im Array verwendet.</p>
              <p>z.B. Set-MsolUser -UserPrincipalName JWarner@contoso.com -AlternateEmailAddresses „email@live.com“</p>
            </td>
            <td>
              <p>Verwendet in:</p>
              <p>Kennwortzurücksetzungsportal</p>
              <p>Registrierungsportal</p>
              <p>Festlegbar wo: </p>
              <p>"AuthenticationEmail" kann im Kennwortzurücksetzungsportal oder im MFA-Registrierungsportal festgelegt werden.</p>
              <p>"AlternateEmail" kann in PowerShell, im Azure-Verwaltungsportal und im Office-Verwaltungsportal festgelegt werden.</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a> oder 甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
E-Mails-Adressen sollten der Standardformatierung entsprechen.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
E-Mail-Adressen im Unicode-Format werden unterstützt.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Sicherheitsfragen und -antworten</p>
            </td>
            <td>
              <p>Nicht verfügbar</p>
              <p>Sicherheitsfragen oder -antworten aus Active Directory können derzeit nicht synchronisiert werden.</p>
            </td>
            <td>
              <p>Stehen nicht zur direkten Änderung im Verzeichnis zur Verfügung.</p>
              <p>Können nur während der Endbenutzerregistrierung für die Kennwortzurücksetzung festgelegt werden.</p>
            </td>
            <td>
              <p>Verwendet in:</p>
              <p>Kennwortzurücksetzungsportal</p>
              <p>Registrierungsportal </p>
              <p>Festlegbar wo: </p>
              <p>Sicherheitsfragen können ausschließlich im Azure-Verwaltungsportal festgelegt werden.</p>
              <p>Die Antworten auf die für einen Benutzer festgelegten Sicherheitsfragen können nur über das Registrierungsportal festgelegt werden.</p>
            </td>
            <td>
              <p>Sicherheitsfragen können maximal 200 Zeichen und müssen mindestens 3 Zeichen umfassen.</p>
              <p>Antworten können maximal 40 Zeichen und müssen mindestens 3 Zeichen umfassen.</p>
            </td>
          </tr>
        </tbody></table>


### <a name="deploying-password-reset-without-requiring-end-user-registration"></a>Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung
Falls Sie die Kennwortzurücksetzung ohne erforderliche Benutzerregistrierung bereitstellen möchten, können Sie eine der beiden folgenden Optionen verwenden. Dies kann eine gute Möglichkeit darstellen, eine größere Anzahl von Benutzern zum Verwenden von SSPR zu entsperren und Benutzer dennoch zu ermöglichen, diese Informationen im Registrierungsprozess zu prüfen.

Viele unserer größten Kunden verwenden dieses Verfahren, um sehr schnell eine Kennwortzurücksetzung einzurichten.

#### <a name="synchronize-phone-numbers-with-azure-ad-connect"></a>Synchronisieren von Telefonnummern mit Azure AD Connect
Wenn Sie Daten mit einem oder beiden der folgenden Felder synchronisieren, können diese umgehend und ohne vorherige Benutzerregistrierung für die Kennwortzurücksetzung verwendet werden:

* Mobiltelefon
* Bürotelefon

Informationen zu den Eigenschaften, die lokal aktualisiert werden müssen, finden Sie im Abschnitt [Welche Daten werden bei der Kennwortzurücksetzung verwendet?](#what-data-is-used-by-password-reset) unter den oben angegebenen Feldern.  

Alle Telefonnummern müssen im Format „+1 1234567890“ vorliegen, damit sie in unserem System ordnungsgemäß verwendet werden können.

#### <a name="set-phone-numbers-or-emails-with-powershell"></a>Festlegen von Telefonnummern oder E-Mail-Adressen mit PowerShell
Wenn Sie eines oder mehrere der folgenden Felder festlegen, können diese umgehend und ohne vorherige Benutzerregistrierung für die Kennwortzurücksetzung verwendet werden:

* Mobiltelefon
* Bürotelefon
* Alternative E-Mail-Adresse

Informationen zum Festlegen dieser Eigenschaften mit PowerShell finden Sie im Abschnitt [Festlegen der Daten zur Kennwortzurücksetzung mit PowerShell](#setting-password-reset-data-with-powershell).

Alle Telefonnummern müssen im Format „+1 1234567890“ vorliegen, damit sie in unserem System ordnungsgemäß verwendet werden können.

### <a name="what-happens-when-a-user-registers"></a>Was geschieht bei Registrierung eines Benutzers?
Wenn sich ein Benutzer registriert, zeigt die Registrierungsseite **immer** die folgenden Felder an:

* Telefonnummer für Authentifizierung
* E-Mail-Adresse für Authentifizierung
* Sicherheitsfragen und -antworten

Wenn Sie einen Wert für **Mobiltelefon** oder **Alternative E-Mail-Adresse** angegeben haben, können Benutzer diesen sofort verwenden, um ihre Kennwörter zurücksetzen, selbst wenn sie sich nicht für den Dienst registriert haben.  Benutzer sehen diese Werte zudem bei der ersten Registrierung und können sie bei Bedarf ändern.  Nachdem sie sich erfolgreich registriert haben, werden diese Werte jedoch in den Feldern **Authentifizierungstelefon** und **E-Mail-Adresse für Authentifizierung** beibehalten.

### <a name="how-to-access-password-reset-data-for-your-users"></a>Zugriff auf Daten zur Kennwortzurücksetzung für Ihre Benutzer
#### <a name="data-settable-via-synchronization"></a>Per Synchronisierung festlegbare Daten
Die folgenden Felder können lokal synchronisiert werden:

* Mobiltelefon
* Bürotelefon

#### <a name="data-settable-with-azure-ad-powershell--azure-ad-graph"></a>Mit Azure AD PowerShell und Azure AD Graph festlegbare Daten
Die folgenden Felder können über Azure AD PowerShell und die Azure AD-Graph-API festgelegt werden:

* Alternative E-Mail-Adresse
* Mobiltelefon
* Bürotelefon

#### <a name="data-settable-with-registration-ui-only"></a>Nur über die Benutzeroberfläche für die Registrierung festlegbare Daten
Auf die folgenden Felder können Sie nur über die Benutzeroberfläche für die SSPR-Registrierung zugreifen (https://aka.ms/ssprsetup):

* Sicherheitsfragen und -antworten

#### <a name="data-readable-with-azure-ad-powershell--azure-ad-graph"></a>Mit Azure AD PowerShell und Azure AD Graph lesbare Daten
Auf die folgenden Felder kann über Azure AD PowerShell und die Azure AD-Graph-API zugegriffen werden:

* Alternative E-Mail-Adresse
* Mobiltelefon
* Bürotelefon
* Telefonnummer für Authentifizierung
* E-Mail-Adresse für Authentifizierung

### <a name="setting-password-reset-data-with-powershell"></a>Festlegen der Daten zur Kennwortzurücksetzung mit PowerShell
Sie können Werte für die folgenden Felder mit Azure AD PowerShell festlegen.

* Alternative E-Mail-Adresse
* Mobiltelefon
* Bürotelefon

**_PowerShell V1_**

Zunächst müssen Sie [das Azure AD PowerShell-Modul herunterladen und installieren](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).  Sobald Sie es installiert haben, können Sie die folgenden Schritte befolgen, um jedes Feld zu konfigurieren.

**_PowerShell V2_**

Als Erstes müssen Sie das [Azure AD V2 PowerShell-Modul herunterladen und installieren](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Sobald Sie es installiert haben, können Sie die folgenden Schritte befolgen, um jedes Feld zu konfigurieren.

Führen Sie die folgenden Befehle aus, um auf der Grundlage aktueller PowerShell-Versionen mit Install-Module-Unterstützung eine schnelle Installation durchzuführen. (Die erste Zeile überprüft, ob das Modul bereits installiert ist.)

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="alternate-email---how-to-set-alternate-email-with-powershell"></a>Alternative E-Mail-Adresse: Festlegen der alternativen E-Mail-Adresse mit PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

**_PowerShell V2_**

```
Connect-AzureAD
Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
```

#### <a name="mobile-phone---how-to-set-mobile-phone-with-powershell"></a>Mobiltelefon: Festlegen des Mobiltelefons mit PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

**_PowerShell V2_**

```
Connect-AzureAD
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 1234567890"
```

#### <a name="office-phone---how-to-set-office-phone-with-powershell"></a>Bürotelefon: Festlegen des Bürotelefons mit PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

**_PowerShell V2_**

```
Connect-AzureAD
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"
```

### <a name="reading-password-reset-data-with-powershell"></a>Lesen der Daten zur Kennwortzurücksetzung mit PowerShell
Sie können Werte für die folgenden Felder mit Azure AD PowerShell lesen.

* Alternative E-Mail-Adresse
* Mobiltelefon
* Bürotelefon
* Telefonnummer für Authentifizierung
* E-Mail-Adresse für Authentifizierung

#### <a name="alternate-email---how-to-read-alternate-email-with-powershell"></a>Alternative E-Mail-Adresse: Lesen der alternativen E-Mail-Adresse mit PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

**_PowerShell V2_**

```
Connect-AzureAD
Get-AzureADUser -ObjectID user@domain.com | select otherMails
```

#### <a name="mobile-phone---how-to-read-mobile-phone-with-powershell"></a>Mobiltelefon: Lesen des Mobiltelefons mit PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

**_PowerShell V2_**

```
Connect-AzureAD
Get-AzureADUser -ObjectID user@domain.com | select Mobile
```

#### <a name="office-phone---how-to-read-office-phone-with-powershell"></a>Bürotelefon: Lesen des Bürotelefons mit PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

**_PowerShell V2_**

```
Connect-AzureAD
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber
```

#### <a name="authentication-phone---how-to-read-authentication-phone-with-powershell"></a>Telefonnummer für Authentifizierung: Lesen der Telefonnummer für die Authentifizierung mit PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

**_PowerShell V2_**

```
Not possible in PowerShell V2
```

#### <a name="authentication-email---how-to-read-authentication-email-with-powershell"></a>E-Mail-Adresse für Authentifizierung: Lesen der E-Mail-Adresse für die Authentifizierung mit PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

**_PowerShell V2_**

```
Not possible in PowerShell V2
```
## <a name="how-does-password-reset-work-for-b2b-users"></a>Funktionsweise der Kennwortzurücksetzung für B2B-Benutzer
Das Zurücksetzen und Ändern von Kennwörtern wird von allen B2B-Konfigurationen uneingeschränkt unterstützt.  Im Anschluss finden Sie Informationen zu den drei expliziten B2B-Szenarien, die von der Kennwortzurücksetzung unterstützt werden.

1. **Benutzer aus einer Partnerorganisation mit einem vorhandenen Azure AD-Mandanten:** Falls die Organisation, mit der Sie eine Partnerschaft eingegangen sind, über einen Azure AD-Mandanten verfügt, **respektieren wir die in diesem Mandanten aktivierten Kennwortzurücksetzungsrichtlinien**. Für die Kennwortzurücksetzung muss die Partnerorganisation lediglich sicherstellen, dass die Self-Service-Kennwortzurücksetzung von Azure AD aktiviert ist. Für Office 365-Kunden fallen dadurch keine zusätzlichen Gebühren an. Eine entsprechende Anleitung finden Sie unter [Erste Schritte mit der Kennwortverwaltung](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords).
2. **Benutzer, die sich mit der [Self-Service-Registrierung](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-signup) registriert haben:** Wenn die Organisation, mit der Sie eine Partnerschaft eingegangen sind, das [Self-Service-Registrierungsfeature](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-signup) verwendet, um auf einen Mandanten zuzugreifen, steht eine vorgefertigte Zurücksetzung unter Verwendung der E-Mail-Adresse zur Verfügung, mit der die Registrierung erfolgt ist.
3. **B2B-Benutzer:** Alle neuen B2B-Benutzer, die mithilfe der neuen [Azure AD-B2B-Funktionen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) erstellt werden, können ihre Kennwörter standardmäßig unter Verwendung der E-Mail-Adresse zurücksetzen, mit der sie sich im Rahmen des Einladungsprozesses registriert haben.

Navigieren Sie zum Testen dieser Optionen mit einem dieser Partnerbenutzer zu „http://passwordreset.microsoftonline.com“.  Sofern keine alternative E-Mail-Adresse und keine E-Mail-Adresse für die Authentifizierung definiert sind, funktioniert die Kennwortzurücksetzung wie erwartet.  Weitere Informationen zu den Daten, die bei der Self-Service-Kennwortzurücksetzung verwendet werden, finden Sie unter [Welche Daten werden bei der Kennwortzurücksetzung verwendet?](https://azure.microsoft.com/en-us/documentation/articles/active-directory-passwords-learn-more/#what-data-is-used-by-password-reset).

## <a name="next-steps"></a>Nächste Schritte
Im Folgenden finden Sie Links zu allen Webseiten mit Informationen zur Kennwortzurücksetzung für Azure AD:

* **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-my-password).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – Erfahren Sie mehr über die sechs verschiedenen Komponenten des Diensts und deren Funktionen.
* [**Erste Schritte**](active-directory-passwords-getting-started.md) – Erfahren Sie, wie Sie Benutzern das Zurücksetzen und Ändern ihrer Cloud- oder lokalen Kennwörter erlauben.
* [**Anpassen**](active-directory-passwords-customize.md) – Erfahren Sie, wie Sie das Aussehen und Verhalten des Diensts an die Anforderungen Ihrer Organisation anpassen.
* [**Best Practices**](active-directory-passwords-best-practices.md) – Erfahren Sie, wie Sie Kennwörter in Ihrer Organisation schnell bereitstellen und effektiv verwalten.
* [**Einblicke erhalten**](active-directory-passwords-get-insights.md) – Erfahren Sie mehr über unsere integrierten Berichtsfunktionen.
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) – Hier erhalten Sie Antworten auf häufig gestellte Fragen.
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – Erfahren Sie, wie Sie Probleme mit dem Dienst schnell beheben.

[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"

