<properties
	pageTitle="Azure AD Connect: Erste Schritte mit Expresseinstellungen | Microsoft Azure"
	description="Informationen zum Herunterladen, Installieren und Ausführen des Setup-Assistenten für Azure AD Connect."
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/10/2016"
	ms.author="billmath;andkjell"/>

# Erste Schritte mit Azure AD Connect mit Expresseinstellungen
Dieses Thema hilft Ihnen bei Ihren ersten Schritten mit Azure Active Directory Connect. In dieser Dokumentation wird die Expressinstallation für Azure AD Connect erklärt. Expresseinstellungen werden in einer Topologie mit einer einzelnen Gesamtstruktur mit Kennwortsynchronisierung verwendet. Mit nur wenigen Klicks können Sie Ihr lokales Verzeichnis auf die Cloud erweitern.

## Verwandte Dokumentation
Wenn Sie die Dokumentation zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md) nicht gelesen haben, finden Sie in der folgenden Tabelle Links zu verwandten Themen. Sie müssen die ersten beiden, fett hervorgehobenen Themen gelesen haben, bevor Sie mit der Installation beginnen.

| Thema | |
| --------- | --------- |
| **Azure AD Connect herunterladen** | [Azure AD Connect herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771) |
| **Hardware und Voraussetzungen** | [Azure AD Connect: Hardware und Voraussetzungen](active-directory-aadconnect-prerequisites.md) |
| Installation mit benutzerdefinierten Einstellungen | [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md) |
| Upgrade von DirSync | [Upgrade von Azure AD-Synchronisierungstools (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md) |
| Nach der Installation | [Überprüfen der Installation und Zuweisen von Lizenzen ](active-directory-aadconnect-whats-next.md) |
| Für die Installation verwendete Konten | [Weitere Informationen zu Azure AD Connect-Konten und -Berechtigungen](active-directory-aadconnect-accounts-permissions.md) |


## Expressinstallation von Azure AD Connect
Die **Expresseinstellungen** sind die Standardoption und werden für die häufigsten Bereitstellungsszenarien verwendet. Bei der Verwendung der Expresseinstellungen stellt Azure AD Connect die Synchronisierung für eine Topologie mit einer einzelnen Gesamtstruktur bereit. Die [Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) ist aktiviert und ermöglicht den Benutzern die Verwendung ihres lokalen Kennworts beim Anmelden in der Cloud. [Automatische Upgrades](active-directory-aadconnect-feature-automatic-upgrade.md) sind aktiviert, was die Wartung vereinfacht. Bei Verwendung der Expresseinstellungen wird nach Abschluss der Installation automatisch eine Synchronisierung gestartet (dies können Sie jedoch auch deaktivieren).

### So installieren Sie Azure AD Connect mit Expresseinstellungen

1. Melden Sie sich als lokaler Administrator auf dem Server an, auf dem Sie Azure AD Connect installieren möchten. Dies sollte der Server sein, der als Synchronisierungsserver verwendet werden soll.
2. Navigieren Sie zu **AzureADConnect.msi** und doppelklicken Sie darauf.
3. Aktivieren Sie auf der Willkommensseite das Kontrollkästchen zum Zustimmen zu den Lizenzbedingungen, und klicken Sie auf **Weiter**. ![Willkommen bei Azure AD Connect](./media/active-directory-aadconnect-get-started-express/welcome.png)
4. Klicken Sie im Bildschirm "Expresseinstellungen" auf **Expresseinstellungen verwenden**. ![Willkommen bei Azure AD Connect](./media/active-directory-aadconnect-get-started-express/express.png)
5. Geben Sie auf dem Bildschirm „Mit Azure AD verbinden“ den Benutzernamen und das Kennwort eines globalen Azure-Administrators für Azure AD ein. Klicken Sie auf **Weiter**. ![Herstellen einer Verbindung mit Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) Falls ein Fehler auftritt und Sie Probleme mit der Konnektivität haben, können Sie unter [Problembehebung bei Konnektivitätsproblemen](active-directory-aadconnect-troubleshoot-connectivity.md) nach einer Lösung suchen.
6. Geben Sie auf dem Bildschirm „Mit AD DS verbinden“ den Benutzernamen und das Kennwort für ein Enterprise-Administratorkonto ein. Sie können den Domänenteil entweder im NetBIOS- oder FQDN-Format eingeben, also „FABRIKAM\\administrator“ oder „fabrikam.com\\administrator“. Klicken Sie auf **Weiter**. ![Herstellen einer Verbindung mit AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Wenn in Ihrer lokalen Active Directory-Instanz nicht vorhandene oder nicht überprüfte UPN-Domänen registriert sind, wird diese Seite angezeigt. Wenn alle UPN-Domänen in Ihrer lokalen AD DS-Instanz überprüft wurden, wird diese Seite nicht angezeigt. ![Nicht überprüfte Domänen](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png) Wenn diese Seite angezeigt wird, überprüfen Sie jede Domäne, die als **Nicht hinzugefügt** und **Nicht überprüft** markiert ist. Stellen Sie sicher, dass die verwendeten Domänen in Azure AD überprüft wurden. Klicken Sie auf das Symbol zum Aktualisieren, wenn Sie Ihre Domänen überprüft haben. Weitere Informationen finden Sie im Artikel mit Informationen zum [Hinzufügen und Überprüfen der Domäne](active-directory-add-domain.md).
8. Klicken Sie im Bildschirm "Bereit zur Konfiguration" auf **Installieren**.
	- Optional können Sie auf der Seite „Bereit zur Konfiguration“ das Kontrollkästchen **Starten Sie den Synchronisierungsvorgang, sobald die Anfangskonfiguration abgeschlossen wurde** deaktivieren. Deaktivieren Sie dieses Kontrollkästchen, wenn Sie eine zusätzliche Konfiguration wie z.B. [Filtern](active-directory-aadconnectsync-configure-filtering.md) vornehmen möchten. Wenn Sie diese Option deaktivieren, konfiguriert der Assistent die Synchronisierung, der Scheduler bleibt jedoch deaktiviert. Er wird erst ausgeführt, wenn Sie ihn manuell aktivieren, indem Sie den Installationsassistenten erneut ausführen.
	- Durch Aktivieren des entsprechenden Kontrollkästchens können Sie optional auch die Konfiguration von Synchronisierungsdiensten für die **Exchange-Hybridbereitstellung** festlegen. Aktivieren Sie diese Option, wenn Sie Exchange-Postfächer gleichzeitig in der Cloud und lokal bereitstellen möchten. ![Bereit zur Konfiguration von Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Klicken Sie nach Abschluss der Installation auf **Beenden**.
10. Melden Sie sich nach Abschluss der Installation ab und erneut wieder an, ehe Sie den Synchronisierungsdienst-Manager oder Synchronisierungsregel-Editor verwenden.

Ein Video zur Expressinstallation finden Sie hier:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## Nächste Schritte
Nachdem Sie Azure AD Connect installiert haben, können Sie [die Installation überprüfen und Lizenzen zuweisen](active-directory-aadconnect-whats-next.md).

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0518_2016-->