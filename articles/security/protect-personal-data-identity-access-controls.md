---
title: "Schützen personenbezogener Daten mit der Identitäts- und Zugriffssteuerung von Azure | Microsoft-Dokumentation"
description: "Verwenden der Identitäts- und Zugriffssteuerung von Azure zum Schützen personenbezogener Daten"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 908bf4c34295168024afd11d24c7a3d2044ff518
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a>Azure Active Directory und Multi-Factor Authentication: Schützen personenbezogener Daten mit der Identitäts- und Zugriffssteuerung

Dieser Artikel enthält Informationen und Verfahren zum Schützen personenbezogener Daten mithilfe der Sicherheitsfunktionen und -dienste von Azure Active Directory und Multi-Factor Authentication.

## <a name="scenario"></a>Szenario

Ein großes Kreuzfahrtunternehmen mit Hauptsitz in den USA expandiert und bietet Routen im Mittelmeer, der Adria und der Ostsee sowie zu den Britischen Inseln an. Zu diesem Zweck hat das Unternehmen mehrere kleinere Kreuzfahrtunternehmen in Italien, Deutschland, Dänemark und Großbritannien erworben. 

Das Unternehmen verwendet Microsoft Azure zum Speichern von Unternehmensdaten in der Cloud. Dies umfasst personenbezogene Informationen, z.B. Namen, Adressen, Telefonnummern und Kreditkarteninformationen, in seiner globalen Kundendatenbank. Dies umfasst außerdem herkömmliche Informationen der Personalverwaltung wie Adressen, Telefonnummern, Steueridentifikationsnummern und andere Informationen zu Unternehmensmitarbeitern an allen Standorten. Das Kreuzfahrtunternehmen verwaltet zudem eine umfangreiche Datenbank für Mitglieder des Bonus- und Treueprogramms, die personenbezogene Informationen enthält, um Beziehungen zu aktuellen und früheren Kunden nachzuverfolgen.

Unternehmensmitarbeiter greifen in den externen Geschäftsstellen des Unternehmens auf das Netzwerk zu. Und Reiseveranstalter weltweit haben Zugriff auf bestimmte Unternehmensressourcen.

## <a name="problem-statement"></a>Problembeschreibung

Das Unternehmen muss personenbezogene Daten von Kunden und Mitarbeitern vor Angreifern schützen, die versuchen, über gefährdete Identitäten Zugriff auf die Daten zu erhalten. Das Unternehmen muss außerdem sicherstellen, dass der Zugriff auf personenbezogene Daten durch berechtigte Benutzer ausschließlich auf die Benutzer beschränkt ist, die sie zur Ausführung ihrer Aufgaben benötigen.

## <a name="company-goal"></a>Unternehmensziel

Unternehmensziel ist es, sicherzustellen, dass der Zugriff auf personenbezogene Daten streng kontrolliert wird. Es ist wichtig, dass die Identitäten der Benutzer mit Zugriff auf personenbezogene Daten durch strenge Authentifizierung geschützt werden. Eine Richtlinie der [geringsten Rechte] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) muss erzwungen werden, sodass berechtigte Benutzer nur über die für sie erforderliche Zugriffsebene und über keine höhere Zugriffsebene verfügen.

## <a name="solutions"></a>Lösungen

Microsoft Azure umfasst Tools zur Identitäts- und Zugriffsverwaltung, mit denen Unternehmen steuern können, welche Benutzer Zugriff auf Ressourcen mit personenbezogenen Daten haben.

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) verwaltet Identitäten und steuert den Zugriff auf Azure sowie auf andere Ressourcen, Daten und Anwendungen auf lokaler Ebene und in der Cloud. Mithilfe von [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) können Azure-Administratoren die Anzahl der Personen minimieren, die Zugriff auf bestimmte Informationen, z.B. personenbezogene Daten, haben. Administratoren können privilegierte Identitäten und deren Zugriff auf Ressourcen erkennen, beschränken und überwachen sowie berechtigten Benutzern temporäre Just-In-Time-Administratorrechte (JIT) zuweisen. Zudem erhalten sie eine Übersicht über die Benutzer, die über AAD-Administratorrechte verfügen.

Die Verwendung von AAD PIM umfasst folgende Aktivitäten:

- Aktivieren von Privileged Identity Management für Ihr Verzeichnis

- Verwenden des Privileged Identity Management-Administratordashboards zum Anzeigen wichtiger Informationen auf einen Blick

- Verwalten der privilegierten Identitäten (Administratoren) durch Hinzufügen oder Entfernen permanenter oder berechtigter Administratoren für die einzelnen Rollen

- Konfigurieren der Rollenaktivierungseinstellungen

- Aktivieren von Rollen

- Überprüfen der Rollenaktivität

#### <a name="how-do-i-enable-aad-pim"></a>Wie aktiviere ich AAD PIM?

Führen Sie folgende Schritte aus, um PIM für Ihr Verzeichnis zu verwenden:

1. Melden Sie sich beim Azure-Portal als globaler Administrator Ihres Verzeichnisses an.

2. Wenn Ihre Organisation über mehr als ein Verzeichnis verfügt, klicken Sie in der oberen rechten Ecke des Azure-Portals auf Ihren Benutzernamen. Wählen Sie das Verzeichnis aus, in dem Sie Azure AD Privileged Identity Management verwenden möchten.

3. Wählen Sie **Weitere Dienste** aus, und verwenden Sie das Textfeld **Filter**, um nach Azure AD Privileged Identity Management zu suchen.

4. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.

Nachdem Azure AD Privileged Identity Management eingerichtet wurde, wird beim Öffnen der Anwendung immer das Blatt für die Navigation angezeigt.

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

Weitere Informationen und Anweisungen zum Einstieg in AAD PIM finden Sie unter [Erste Schritte mit Azure AD Privileged Identity Management](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started).

### <a name="azure-role-based-access-control"></a>Rollenbasierte Zugriffssteuerung in Azure

Mit der [rollenbasierten Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) können Azure-Administratoren den Zugriff auf Azure-Ressourcen verwalten, indem sie die Zugriffsgewährung basierend auf der zugewiesenen Rolle des jeweiligen Benutzers aktivieren. Sie können Aufgaben innerhalb eines Teams verteilen und Benutzern, Gruppen und Anwendungen nur den Zugriff gewähren, den diese zur Ausführung ihrer Aufgaben benötigen.

Den Benutzern kann die rollenbasierte Zugriffssteuerung über das Azure-Portal, Azure Befehlszeilenwerkzeuge oder Azure Management APIs zugewiesen werden.

Weitere grundlegende Informationen zu Azure RBAC finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](https://docs.microsoft.com/active-directory/role-based-access-control-what-is).

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a>Wie verwalte ich Azure RBAC mit PowerShell?

Mithilfe von PowerShell-Cmdlets können Sie Azure RBAC, einschließlich der folgenden Verwaltungsaufgaben verwalten:

- Auflisten der Rollen

- Sehen, wer Zugriff hat

- Gewähren von Zugriff

- Zugriff entfernen

- Erstellen einer benutzerdefinierten Rolle

- Abrufen von Aktionen für einen Ressourcenanbieter

- Ändern einer benutzerdefinierten Rolle

- Löschen einer benutzerdefinierten Rolle

- Auflisten benutzerdefinierter Rollen 

Anweisungen zum Verwalten von Azure RBAC mit PowerShell finden Sie unter [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) ist eine Lösung zur Überprüfung in zwei Schritten, die zum Schutz des Zugriffs auf Daten und Anwendungen beiträgt und gleichzeitig ein einfaches Anmeldeverfahren für Benutzer bietet. Sie bietet eine sichere Authentifizierung mit verschiedenen Überprüfungsmethoden, einschließlich Telefonanruf, SMS oder mobile App.

Zur Bereitstellung von MFA in der Azure-Cloud müssen Sie zunächst MFA und dann die Überprüfung in zwei Schritten für Benutzer aktivieren.

#### <a name="how-do-i-enable-azure-to-use-mfa"></a>Wie aktiviere ich in Azure die Verwendung von MFA?

Wenn Ihre Benutzer über Lizenzen verfügen, die Azure Multi-Factor Authentication enthalten, müssen Sie nichts unternehmen, um Azure MFA zu aktivieren. Andernfalls müssen Sie einen Multi-Factor Authentication-Anbieter in Ihrem Verzeichnis erstellen. Gehen Sie dazu folgendermaßen vor:

1. Wählen Sie im klassischen Azure-Portal (Anmeldung als Administrator) die Option **Active Directory** aus.

2. Wählen Sie **Anbieter für mehrstufige Authentifizierung** aus.

3. Wählen Sie **Neu** und dann unter **App Services** die Option **Anbieter für mehrstufige Authentifizierung** aus.

4. Wählen Sie **Schnellerfassung** aus.

5. Füllen Sie das Feld „Name“ aus, und wählen Sie ein Nutzungsmodell (pro Authentifizierung oder pro aktiviertem Benutzer) aus.

6. Legen Sie ein Verzeichnis fest, dem der MFA-Anbieter zugeordnet ist.

7. Klicken Sie auf die Schaltfläche **Erstellen** .

![](media/protect-personal-data-identity-access-controls/quick-create.png)

Weitere Anweisungen zum Verwalten Ihres Multi-Factor Authentication-Anbieters finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication-Anbietern](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a>Wie aktiviere ich die Überprüfung in zwei Schritten für Benutzer?

Sie können die Überprüfung in zwei Schritten für alle Anmeldungen erzwingen oder Richtlinien für bedingten Zugriff erstellen, um die Überprüfung in zwei Schritten nur dann vorauszusetzen, wenn bestimmte Bedingungen gelten.

Die Aktivierung von Azure MFA durch Ändern der Benutzerstatus ist der herkömmliche Ansatz, um die Überprüfung in zwei Schritten zu erzwingen. Die Anforderung ist für alle aktivierten Benutzer identisch: Jedes Mal, wenn sie sich anmelden, müssen sie die Überprüfung in zwei Schritten ausführen. Durch die Aktivierung eines Benutzers werden alle Richtlinien für bedingten Zugriff überschrieben, die diesen Benutzer betreffen könnten.

Die Aktivierung von Azure MFA mit einer Richtlinie für bedingten Zugriff ist ein flexiblerer Ansatz, um die Überprüfung in zwei Schritten zu erzwingen. Sie können Richtlinien für bedingten Zugriff erstellen, die sowohl für Gruppen als auch für einzelne Benutzer gelten. Gruppen, bei denen ein hohes Risiko besteht, können umfassendere Einschränkungen zugewiesen werden. Alternativ kann die Überprüfung in zwei Schritten nur für Cloud-Apps mit hohem Risiko erzwungen und für Apps mit niedrigem Risiko übersprungen werden. Der bedingte Zugriff ist jedoch ein kostenpflichtiges Feature von Azure Active Directory.

Führen Sie folgende Schritte aus, um MFA durch Ändern des Benutzerstatus zu aktivieren:

1. Melden Sie sich beim Azure-Portal als Administrator an.
2. Navigieren Sie zu **Azure Active Directory \> Benutzer und Gruppen \> Alle Benutzer**.
3. Wählen Sie **Multi-Factor Authentication** aus.
4. Suchen Sie den Benutzer, den Sie für Azure MFA aktivieren möchten. Sie müssen möglicherweise oben die Ansicht ändern.
5. Aktivieren Sie das Kontrollkästchen neben dem Namen des Benutzers.
6. Wählen Sie auf der rechten Seite unter „QuickSteps“ die Option **Aktivieren** aus.

   ![](media/protect-personal-data-identity-access-controls/quick-create.png)

7. Bestätigen Sie Ihre Auswahl im Popupfenster, das geöffnet wird.  Benutzer, für die MFA aktiviert wurde, werden bei ihrer nächsten Anmeldung aufgefordert, sich zu registrieren.

Führen Sie folgende Schritte aus, um Azure MFA mit einer Richtlinie für bedingten Zugriff zu aktivieren:

1. Melden Sie sich beim Azure-Portal als Administrator an.

2. Navigieren Sie zu **Azure Active Directory \> Bedingter Zugriff**.

3. Wählen Sie **Neue Richtlinie**.

4. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**. Geben Sie über die Registerkarten **Einschließen** und **Ausschließen** an, welche Benutzer und Gruppen durch die Richtlinie verwaltet werden.

5. Wählen Sie unter **Zuweisungen** die Option **Cloud-Apps** aus. Wählen Sie **Alle Cloud-Apps** aus, um alle einzuschließen.
6.  Klicken Sie unter **Zugriffskontrollen** auf **Gewähren**. Wählen Sie **Mehrstufige Authentifizierung anfordern**.
7.  Legen Sie **Richtlinie aktivieren** auf **Ein** fest, und wählen Sie dann **Speichern**.

Informationen zum Konfigurieren von Azure MFA-Einstellungen zum Einrichten von Betrugswarnungen, Erstellen einer Einmalumgehung, Verwenden von benutzerdefinierten Sprachnachrichten, Konfigurieren der Zwischenspeicherung, Angeben von vertrauenswürdigen IPs, Erstellen von App-Kennwörtern, Aktivieren der Speicherung von MFA für Geräte, denen Benutzer vertrauen, und Auswählen von Überprüfungsmethoden finden Sie unter [Konfigurieren von Azure Multi-Factor Authentication-Einstellungen](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next).

## <a name="next-steps"></a>Nächste Schritte

- [Sichern des privilegierten Zugriffs in Azure AD](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [Häufig gestellte Fragen zu Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [Beheben von Problemen bei der rollenbasierten Zugriffssteuerung](https://docs.microsoft.com/azure/active-directory/role-based-access-control-troubleshooting)

- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

