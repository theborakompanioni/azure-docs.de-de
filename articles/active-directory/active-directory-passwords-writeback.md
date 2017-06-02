---
title: "Azure AD SSPR mit Kennwortrückschreiben | Microsoft-Dokumentation"
description: "Verwenden von Azure AD und Azure AD Connect zum Rückschreiben von Kennwörtern in ein lokales Verzeichnis"
services: active-directory
keywords: "Active Directory-Kennwortverwaltung, Kennwortverwaltung, Self-Service-Kennwortzurücksetzung in Azure AD"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: joflore
ms.editor: gahug
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: fde08bfc3a73c54ee53b5d8efffd3001894416b3
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="password-writeback-overview"></a>Übersicht über die Kennwortrückschreibung

Über Kennwortrückschreiben können Sie Azure AD so konfigurieren, dass Kennwörter in das lokale Active Directory zurückgeschrieben werden. Auf diese Weise ist es nicht erforderlich, eine komplizierte lokale Self-Service-Lösung für das Zurücksetzen von Kennwörtern einzurichten und zu verwalten. Gleichzeitig bietet diese Funktion Ihren Benutzern eine bequeme, cloudbasierte Möglichkeit zum Zurücksetzen ihrer lokalen Kennwörter – unabhängig davon, wo sie sich gerade befinden. Kennwortrückschreiben ist eine Komponente von [Azure Active Directory Connect](./connect/active-directory-aadconnect.md), die von den aktuellen Abonnenten von [Azure Active Directory Premium-Editionen](active-directory-editions.md) aktiviert und verwendet werden kann.

Kennwortrückschreiben bietet die folgenden Features:

* **Feedback ohne Verzögerung**: Kennwortrückschreiben ist ein synchroner Vorgang. Ihre Benutzer werden sofort benachrichtigt, wenn ihre Kennwörter nicht der Richtlinie entsprechen oder das Zurücksetzen oder Ändern des Kennworts aus beliebigen Gründen nicht möglich war.
* **Unterstützung für das Zurücksetzen von Kennwörtern für Benutzer, die AD FS oder andere Verbundtechnologien nutzen**: Solange die Verbundbenutzerkonten mit Ihrem Azure AD-Mandanten synchronisiert sind, können die Benutzer durch das Kennwortrückschreiben ihre lokalen AD-Kennwörter über die Cloud verwalten.
* **Unterstützung für das Zurücksetzen von Kennwörtern für Benutzer, die eine [Kennworthashsynchronisierung](./connect/active-directory-aadconnectsync-implement-password-synchronization.md) verwenden**: Wenn der Dienst für die Kennwortzurücksetzung ermittelt, dass ein synchronisiertes Benutzerkonto für die Kennworthashsynchronisierung aktiviert ist, werden das lokale Kennwort und das Cloudkennwort für dieses Konto simultan zurückgesetzt.
* **Unterstützung für das Ändern von Kennwörtern über den Zugriffsbereich und Office 365**: Wenn Verbundbenutzer oder Benutzer mit Kennwortsynchronisierung ihre abgelaufenen oder noch nicht abgelaufenen Kennwörter ändern möchten, werden diese Kennwörter in die lokale AD-Umgebung zurückgeschrieben.
* **Unterstützung für das Zurückschreiben von Kennwörtern, wenn diese im Azure-Portal von einem Administrator zurückgesetzt werden**: Wenn ein Administrator das Kennwort eines Benutzers im [Azure-Portal](https://portal.azure.com) zurücksetzt und der Benutzer ein Verbundkonto oder ein Konto mit Kennwortsynchronisierung verwendet, wird das vom Administrator festgelegte Kennwort auch im lokalen AD festgelegt. Diese Funktion wird im Office-Verwaltungsportal derzeit nicht unterstützt.
* **Erzwingung von lokalen AD-Kennwortrichtlinien:**: Wenn ein Benutzer sein Kennwort zurücksetzt, wird sichergestellt, dass es die lokalen AD-Richtlinien erfüllt, bevor es in das Verzeichnis geschrieben wird. Dies schließt Verlauf, Komplexität, Alter, Kennwortfilter und alle weiteren Kennwortbeschränkungen ein, die Sie im lokalen AD definiert haben.
* **Keine Notwendigkeit zur Festlegung von eingehenden Firewallregeln**: Beim Kennwortrückschreiben wird als zugrunde liegender Kommunikationskanal ein Azure Service Bus Relay verwendet. Dies bedeutet, dass Sie keine eingehenden Ports in Ihrer Firewall öffnen müssen, damit diese Funktion ordnungsgemäß arbeitet.
* **Keine Unterstützung für Benutzerkonten in geschützten Gruppen, die im lokalen Active Directory vorliegen**: Weitere Informationen zu geschützten Gruppen finden Sie unter [Geschützte Konten und Gruppen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Funktionsweise der Kennwortrückschreibung

Wenn Verbundbenutzer oder Benutzer mit Kennworthashsynchronisierung ihr Kennwort in der Cloud zurücksetzen oder ändern, geschieht Folgendes:

1. Es wird überprüft, über welche Art von Kennwort der Benutzer verfügt.
    * Wenn wir feststellen, dass das Kennwort lokal verwaltet wird
        * Wir überprüfen, ob der Dienst zum Rückschreiben aktiviert ist und ausgeführt wird. Ist dies der Fall, kann der Benutzer fortfahren.
        * Wenn der Dienst zum Rückschreiben nicht aktiviert ist, wird dem Benutzer mitgeteilt, dass das Kennwort jetzt nicht zurückgesetzt werden kann.
2. Im nächsten Schritt muss der Benutzer sich erfolgreich authentifizieren und gelangt dann zum Bildschirm für die Kennwortzurücksetzung.
3. Der Benutzer wählt ein neues Kennwort aus und bestätigt es.
4. Beim Klicken auf "Senden" wird das Klartextkennwort mit einem symmetrischen Schlüssel verschlüsselt, der beim Einrichten der Kennwortrückschreibung erstellt wurde.
5. Nachdem das Kennwort verschlüsselt wurde, wird es in eine Nutzlast eingeschlossen, die über einen HTTPS-Kanal an das mandantenspezifische Service Bus Relay gesendet wird (dieses wurde ebenfalls beim Einrichten der Kennwortrückschreibung festgelegt). Dieses Relay wird durch ein zufällig generiertes Kennwort geschützt, das nur der lokalen Installation bekannt ist.
6. Sobald die Nachricht vom Service Bus empfangen wird, wird der Endpunkt für die Kennwortzurücksetzung automatisch aktiviert und erkennt, dass eine Anforderung zur Kennwortzurücksetzung aussteht.
7. Der Dienst sucht anschließend unter Verwendung des Cloudankerattributs nach dem betreffenden Benutzer. Damit diese Suche erfolgreich ist:

    * Das Benutzerobjekt muss im AD-Connectorbereich vorhanden sein.
    * Das Benutzerobjekt muss mit dem entsprechenden MV-Objekt verknüpft sein.
    * Das Benutzerobjekt muss mit dem entsprechenden AAD-Connectorobjekt verknüpft sein.
    * Für die Verbindung zwischen dem AD-Connectorobjekt und MV muss die Synchronisierungsregel `Microsoft.InfromADUserAccountEnabled.xxx` festgelegt sein. <br> <br>
    Wenn der Aufruf von der Cloud eingeht, verwendet das Synchronisierungsmodul das cloudAnchor-Attribut, um das AAD-Connectorbereichsobjekt zu ermitteln, und folgt der Verbindung zum MV-Objekt und anschließend der Verbindung zurück zum AD-Objekt. Da mehrere AD-Objekte (mehrere Gesamtstrukturen) für denselben Benutzer vorliegen können, wählt das Synchronisierungsmodul das richtige Objekt basierend auf der `Microsoft.InfromADUserAccountEnabled.xxx` -Verbindung aus.

    > [!Note]
    > Als Ergebnis dieser Logik muss Azure AD Connect mit dem PDC-Emulator kommunizieren können, damit das Kennwortrückschreiben funktioniert. Wenn Sie dies manuell aktivieren möchten, können Sie Azure AD Connect mit dem PDC-Emulator verbinden, indem Sie mit der rechten Maustaste auf die **Eigenschaften** des Active Directory-Synchronisierungsconnectors klicken und die Option **configure directory partitions** (Verzeichnispartitionen konfigurieren) auswählen. Suchen Sie nach dem Abschnitt **domain controller connection settings** (Domänencontroller-Verbindungseinstellungen), und aktivieren Sie das Kontrollkästchen **only use preferred domain controllers** (Nur bevorzugte Domänencontroller verwenden). Auch wenn der bevorzugte DC kein PDC-Emulator ist, versucht Azure AD Connect für das Kennwortrückschreiben eine Verbindung mit dem PDC herzustellen.

8. Sobald das Benutzerkonto ermittelt wurde, wird versucht, das Kennwort direkt in der geeigneten AD-Gesamtstruktur zurückzusetzen.
9. Wenn die Kennwortzurücksetzung erfolgreich war, wird der Benutzer darüber informiert, dass das Kennwort geändert wurde.
    > [!NOTE]
    > Wenn das Kennwort des Benutzers mithilfe der Kennwortsynchronisierung mit Azure AD synchronisiert wird, kann es vorkommen, dass die lokale Kennwortrichtlinie schwächer als die Kennwortrichtlinie der Cloud ist. In diesem Fall erzwingen wir die lokale Kennwortrichtlinie und ermöglichen das Synchronisieren des Kennworthashs durch die Kennworthashsynchronisierung. So wird das Erzwingen der lokalen Richtlinie in der Cloud sichergestellt, unabhängig davon, ob Sie für die Bereitstellung von einmaligem Anmelden Kennwortsynchronisierung oder Verbund verwenden.

10. Wenn beim Zurücksetzen des Kennworts ein Fehler auftritt, wird dem Benutzer eine Fehlermeldung angezeigt, und der Benutzer kann es erneut versuchen.
    * Der Vorgang kann aus folgenden Gründen fehlschlagen:
        * Der Dienst war nicht verfügbar.
        * Das ausgewählte Kennwort entsprach nicht den Richtlinien der Organisation.
        * Der Benutzer wurde im lokalen AD nicht gefunden.

    Es gibt für jeden dieser Fälle eine spezifische Meldung, die den Benutzer darüber informiert, was zur Problemlösung unternommen werden kann.

## <a name="configuring-password-writeback"></a>Konfigurieren des Kennwortrückschreibens

Es empfiehlt sich, das automatische Update von [Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) zu verwenden, wenn Sie das Kennwortrückschreiben nutzen möchten.

DirSync und Azure AD Sync stellen keine unterstützten Tools für die Aktivierung des Kennwortrückschreibens mehr dar. Im Artikel [Durchführen eines Upgrades von DirSync und Azure AD Sync](connect/active-directory-aadconnect-dirsync-deprecated.md) finden Sie weitere Informationen zur Umstellung.

Bei den folgenden Schritten wird davon ausgegangen, dass Azure AD Connect bereits mithilfe der [Express](./connect/active-directory-aadconnect-get-started-express.md)- oder [benutzerdefinierten](./connect/active-directory-aadconnect-get-started-custom.md) Einstellungen in Ihrer Umgebung konfiguriert wurde.

1. Um das Kennwortrückschreiben zu konfigurieren und zu aktivieren, melden Sie sich bei Ihrem Azure AD Connect-Server an, und starten Sie den **Azure AD Connect**-Konfigurations-Assistenten.
2. Klicken Sie auf der Willkommensseite auf **Konfigurieren**.
3. Klicken Sie auf dem Bildschirm „Weitere Aufgaben“ auf **Synchronisierungsoptionen anpassen**, und wählen Sie dann **Weiter** aus.
4. Geben Sie auf dem Bildschirm „Mit Azure AD verbinden“ die Anmeldeinformationen eines globalen Administrators ein, und wählen Sie **Weiter** aus.
5. Auf den Bildschirmen „Verzeichnisse verbinden“ und „Filtern von Domänen und Organisationseinheiten“ können Sie **Weiter** auswählen.
6. Aktivieren Sie auf dem Bildschirm „Optionale Features“ das Kontrollkästchen **Kennwortrückschreiben**, und klicken Sie auf **Weiter**.
   ![Aktivieren des Kennwortrückschreibens in Azure AD Connect][Writeback]
7. Klicken Sie auf dem Bildschirm „Bereit zur Konfiguration“ auf **Konfigurieren**, und warten Sie, bis der Vorgang abgeschlossen ist.
8. Wenn die Konfiguration abgeschlossen ist, können Sie auf **Beenden** klicken.

## <a name="licensing-requirements-for-password-writeback"></a>Lizenzierungsanforderungen für das Kennwortrückschreiben

Informationen zur Lizenzierung finden Sie im Thema [Erforderliche Lizenzen für das Kennwortrückschreiben](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) oder auf den folgenden Websites:

* [Website mit Preisen für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Secure Productive Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-supported-for-password-writeback"></a>Für das Kennwortrückschreiben unterstützte lokale Authentifizierungsmodi

Das Kennwortrückschreiben kann für folgende Arten von Benutzerkennwörtern verwendet werden:

* **Reine Cloudbenutzer:** In diesem Szenario ist das Kennwortrückschreiben nicht relevant, da kein lokales Kennwort vorhanden ist.
* **Benutzer mit synchronisiertem Kennwort:** Kennwortrückschreiben wird unterstützt.
* **Verbundbenutzer:** Kennwortrückschreiben wird unterstützt.
* **Benutzer mit Passthrough-Authentifizierung:** Kennwortrückschreiben wird unterstützt.

### <a name="user-and-admin-operations-supported-for-password-writeback"></a>Für das Kennwortrückschreiben unterstützte Benutzer- und Administratorvorgänge

Kennwörter werden in den folgenden Situationen zurückgeschrieben:

* **Unterstützte Vorgänge für Endbenutzer**
  * Jegliche freiwillige Self-Service-Kennwortänderung durch einen Endbenutzer
  * Jegliche erzwungene Self-Service-Kennwortänderung durch einen Endbenutzer (beispielsweise der Ablauf des Kennworts)
  * Jegliche Self-Service-Kennwortzurücksetzung durch einen Endbenutzer über das [Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com)
* **Unterstützte Vorgänge für Administratoren**
  * Jegliche freiwillige Self-Service-Kennwortänderung durch einen Administrator
  * Jegliche erzwungene Self-Service-Kennwortänderung durch einen Administrator (beispielsweise der Ablauf des Kennworts)
  * Jegliche Self-Service-Kennwortzurücksetzung durch einen Administrator über das [Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com)
  * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung über das [klassische Azure-Portal](https://manage.windowsazure.com)
  * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung über das [Azure-Portal](https://portal.azure.com)

### <a name="user-and-admin-operations-not-supported-for-password-writeback"></a>Für das Kennwortrückschreiben nicht unterstützte Benutzer- und Administratorvorgänge

Kennwörter werden in folgenden Situationen **nicht** zurückgeschrieben:

* **Nicht unterstützte Vorgänge für Endbenutzer**
  * Jegliches Zurücksetzen des Kennworts durch Endbenutzer mithilfe von PowerShell v1, v2 oder der Azure AD-Graph-API
* **Nicht unterstützte Vorgänge für Administratoren**
  * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung über das [Office-Verwaltungsportal](https://portal.office.com)
  * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung mithilfe von PowerShell v1, v2 oder der Azure AD-Graph-API

Wir arbeiten zwar daran, diese Einschränkungen zu entfernen, ein genauer Zeitplan steht jedoch noch nicht fest.

## <a name="password-writeback-security-model"></a>Sicherheitsmodell für die Kennwortrückschreibung

Das Kennwortrückschreiben ist ein äußerst sicherer Dienst.  Zum Schutz Ihrer Informationen wird ein vierstufiges Sicherheitsmodell angewendet, das nachfolgend beschrieben wird.

* **Mandantenspezifisches Service Bus Relay**
  * Beim Einrichten des Diensts wird ein mandantenspezifisches Service Bus Relay eingerichtet, das durch ein zufällig generiertes, sicheres Kennwort geschützt wird, auf das Microsoft keinen Zugriff hat.
* **Nicht zugänglicher, sicherer Kryptografieschlüssel für die Kennwortverschlüsselung**
  * Nach der Erstellung des Service Bus Relays wird ein sicherer symmetrischer Schlüssel erstellt, mit dem das Kennwort verschlüsselt wird, bevor es gesendet wird. Dieser Schlüssel liegt ausschließlich im Speicher für geheime Schlüssel Ihres Unternehmens in der Cloud vor, der streng geschützt und überwacht wird, wie jedes Kennwort im Verzeichnis.
* **TLS nach Industriestandard**
  1. Beim Zurücksetzen oder Ändern eines Kennworts in der Cloud wird das Klartextkennwort mit Ihrem öffentlichen Schlüssel verschlüsselt.
  2. Anschließend wird es in eine HTTPS-Nachricht eingefügt und über einen mithilfe der SSL-Zertifikate von Microsoft verschlüsselten Kanal an Ihr Service Bus Relay gesendet.
  3. Wenn die Nachricht beim Service Bus eingeht, wird Ihr lokaler Agent reaktiviert und in Service Bus mithilfe des sicheren, zuvor generierten Kennworts authentifiziert.
  4. Der lokale Agent nimmt die verschlüsselte Nachricht an und entschlüsselt sie mit dem generierten privaten Schlüssel.
  5. Der lokale Agent versucht dann, das Kennwort über die SetPassword-API von AD DS festzulegen.
     * In diesem Schritt kann Ihre lokale AD-Kennwortrichtlinie (Komplexität, Alter, Verlauf, Filter usw.) in der Cloud erzwungen werden.
* **Richtlinien zum Nachrichtenablauf** 
  * Falls die Nachricht im Service Bus verbleibt, weil der lokale Dienst nicht verfügbar ist, kommt es nach wenigen Minuten zu einem Timeout, und die Nachricht wird entfernt, um die Sicherheit weiter zu erhöhen.

### <a name="password-writeback-encryption-details"></a>Verschlüsselungsdetails für das Kennwortrückschreiben

Die Verschlüsselungsschritte, die eine vom Benutzer übermittelte Kennwortzurücksetzungsanforderung vor dem Erreichen Ihrer lokalen Umgebung durchläuft, um ein Höchstmaß an Dienstzuverlässigkeit und Sicherheit zu gewährleisten, werden im Folgenden beschrieben.

* **Schritt 1: Kennwortverschlüsselung mit 2048-Bit-RSA-Schlüssel:** Wenn ein Benutzer ein zurückzuschreibendes Kennwort an die lokale Umgebung übermittelt, wird zunächst das eigentliche Kennwort mit einem 2048-Bit-RSA-Schlüssel verschlüsselt.
* **Schritt 2: Verschlüsselung auf Paketebene mit AES-GCM:** Als Nächstes wird das gesamte Paket (bestehend aus Kennwort und den erforderlichen Metadaten) mithilfe von AES-GCM verschlüsselt. So wird verhindert, dass jemand mit direktem Zugriff auf den zugrunde liegenden ServiceBus-Kanal den Inhalt anzeigen und/oder manipulieren kann.
* **Schritt 3: Abwicklung der gesamten Kommunikation über TLS/SSL:** Die gesamte Kommunikation wird mit ServiceBus über einen TLS/SSL-Kanal abgewickelt. Dadurch wird der Inhalt vor nicht autorisierten Dritten geschützt.
* **Automatischer Schlüsselrollover im Abstand von sechs Monaten:** Alle sechs Monate (sowie bei jeder Deaktivierung/Reaktivierung des Kennwortrückschreibens in Azure AD Connect) erfolgt ein Rollover aller entsprechenden Schlüssel, um ein Höchstmaß an Dienstzuverlässigkeit und Sicherheit zu erreichen.

### <a name="password-writeback-bandwidth-usage"></a>Bandbreitennutzung für das Kennwortrückschreiben

Kennwortrückschreiben ist ein Dienst, für den wenig Bandbreite erforderlich ist und der nur unter folgenden Umständen Anforderungen zurück an den lokalen Agent sendet:

1. Beim Aktivieren oder Deaktivieren des Features über Azure AD Connect werden zwei Nachrichten gesendet.
2. Während der gesamten Ausführungsdauer des Diensts wird als Diensttakt alle fünf Minuten eine Nachricht gesendet.
3. Zwei Nachrichten werden jedes Mal gesendet, wenn ein neues Kennwort übermittelt wird.
    * Die erste Nachricht ist eine Anforderung zum Ausführen des Vorgangs.
    * Die zweite Nachricht enthält das Ergebnis des Vorgangs und wird in den folgenden Situationen gesendet:
        * Bei jeder Übermittlung eines neuen Kennworts während einer Self-Service-Kennwortzurücksetzung durch Benutzer
        * Bei jeder Übermittlung eines neuen Kennworts während einer Benutzerkennwortänderung
        * Bei jeder Übermittlung eines neuen Kennworts während einer vom Administrator initiierten Benutzerkennwortzurücksetzung (nur über die Azure-Administratorportale)

#### <a name="message-size-and-bandwidth-considerations"></a>Überlegungen zur Nachrichtengröße und Bandbreite

Die Größe der einzelnen oben beschriebenen Nachrichten beträgt in der Regel weniger als 1 KB. Selbst bei extrem hoher Auslastung nutzt der Dienst zum Kennwortrückschreiben höchstens eine Bandbreite von einigen wenigen Kilobits pro Sekunde. Da jede Nachricht in Echtzeit und nur dann gesendet wird, wenn dies für eine Kennwortaktualisierung erforderlich ist, und die Nachrichten nicht besonders groß sind, ist die Bandbreitennutzung der Rückschreibfunktion zu klein, um messbare Auswirkungen zu haben.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [**Schnellstart:**](active-directory-passwords-getting-started.md) Informieren Sie sich schnell über die Self-Service-Kennwortverwaltung von Azure AD. 
* [**Lizenzierung:**](active-directory-passwords-licensing.md) Konfigurieren Sie Ihre Azure AD-Lizenzierung.
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Rollout:**](active-directory-passwords-best-practices.md) Mithilfe der hier enthaltenen Anleitungen können Sie SSPR planen und für Ihre Benutzer bereitstellen.
* [**Anpassung:**](active-directory-passwords-customize.md) Passen Sie das Aussehen und Verhalten von SSPR für Ihr Unternehmen an.
* [**Richtlinie:**](active-directory-passwords-policy.md) Machen Sie sich mit den Azure AD-Kennwortrichtlinien vertraut, und legen Sie sie fest.
* [**Berichterstellung:**](active-directory-passwords-reporting.md) Ermitteln Sie, ob, wann und wo Ihre Benutzer auf SSPR-Funktionen zugreifen.
* [**Ausführliche technische Informationen:**](active-directory-passwords-how-it-works.md) Steigen Sie tiefer ein, um zu verstehen, wie alles funktioniert.
* [**Häufig gestellte Fragen (FAQ):**](active-directory-passwords-faq.md) Wie? Warum? Was? Wo? Wer? Wann? - Antworten auf Fragen, die Sie schon immer stellen wollten
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Aktivieren des Kennwortrückschreibens in Azure AD Connect"
