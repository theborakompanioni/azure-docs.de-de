<properties
	pageTitle="Azure AD Connect: Integrieren Ihrer lokalen Identitäten in Azure Active Directory | Microsoft Azure"
	description="Azure AD Connect integriert Ihre lokalen Verzeichnisse in Azure Active Directory. Dadurch können Sie eine einzige Identität für in Azure AD integrierte in Office 365-, Azure- und SaaS-Anwendungen bereitstellen."
    keywords="Einführung in Azure AD Connect, Übersicht über Azure AD Connect, was ist Azure AD Connect, Active Directory installieren"
	services="active-directory"
	documentationCenter=""
	authors="andkjell"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="07/14/2016"
	ms.author="andkjell;billmath"/>

# Integrieren Ihrer lokalen Identitäten in Azure Active Directory
Azure AD Connect integriert Ihre lokalen Verzeichnisse in Azure Active Directory. Dadurch können Sie für Ihre Benutzer eine einzige Identität für in Azure AD integrierte in Office 365-, Azure- und SaaS-Anwendungen bereitstellen. Dieses Thema beschreibt die Planung, Bereitstellung und den Betrieb. Es umfasst Links zu Themen, die mit diesem Bereich zusammenhängen.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

> [AZURE.IMPORTANT] [Azure AD Connect bietet die beste Möglichkeit, um Ihr lokales Verzeichnis mit Azure AD und Office 365 zu verbinden. Da Windows Azure Active Directory Sync (DirSync) und Azure AD Sync veraltet sind und der Support für diese Tools am 13. April 2017 endet, ist dies ein guter Zeitpunkt, um ein Upgrade auf Azure AD Connect durchzuführen.]( https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-dirsync-deprecated/?WT.mc_id=DirSyncDepACOM)

![Was ist Azure AD Connect?](./media/active-directory-aadconnect/arch.png)

## Gründe für die Verwendung von Azure AD Connect
Die Integration Ihrer lokalen Verzeichnisse in Azure AD steigert die Produktivität Ihrer Benutzer, da für den Zugriff auf die Cloud und lokale Ressourcen nur eine Identität benötigt wird. Benutzer und Organisationen können die folgenden Vorteile nutzen:

- Benutzer können eine einzelne Identität verwenden, um auf lokale Anwendungen und Clouddienste wie z. B. Office 365 zuzugreifen.

- Einzelnes Tool zum Bereitstellen einer einfachen Bereitstellungserfahrung für die Synchronisierung und Anmeldung.

- Bietet die neuesten Funktionen für Ihre Szenarien. Azure AD Connect ersetzt ältere Versionen von Identitätsintegrationstools, wie z. B. DirSync und Azure AD Sync. Weitere Informationen finden Sie unter [Vergleich von Tools für die Verzeichnisintegration für Hybrid-Identitäten](active-directory-hybrid-identity-design-considerations-tools-comparison.md).


### Funktionsweise von Azure AD Connect
Azure Active Directory Connect besteht aus drei Hauptkomponenten: den Synchronisierungsdiensten, der optionalen Active Directory-Verbunddienstkomponente und eine Überwachungskomponente namens [Azure AD Connect Health](active-directory-aadconnect-health.md).

<center>![Azure AD Connect Stack](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png) </center>

- Synchronisierung: Diese Komponente ist verantwortlich für das Erstellen von Benutzern, Gruppen und anderen Objekten. Er stellt ebenfalls sicher, dass Identitätsinformationen für Ihre lokalen Benutzer und Gruppen denen in der Cloud entsprechen.
- AD FS: Der Verbund ist eine optionale Komponente von Azure AD Connect und kann zum Konfigurieren einer Hybridumgebung mithilfe einer lokalen AD FS-Infrastruktur verwendet werden. Dies kann von Organisationen verwendet werden, um sich mit komplexen Bereitstellungen zu befassen, z.B. Domänenbeitritts-SSO, Erzwingen von AD-Anmelderichtlinien und Smartcard- bzw. Drittanbieter-MFA.
- Systemüberwachung: Azure AD Connect Health bietet eine stabile Überwachung und einen zentralen Speicherort im Azure-Portal, um diese Aktivität anzuzeigen. Weitere Informationen finden Sie unter [Azure Active Directory Connect Health](active-directory-aadconnect-health.md).

## Installieren von Azure AD Connect

Den Download für Azure AD Connect finden Sie im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).


Lösung | Szenario
----- | ----- |
Vorbereitung: [Hardware und Voraussetzungen](active-directory-aadconnect-prerequisites.md) | <li>Hier erfahren Sie mehr zu den Schritten, die vor dem Installieren von Azure AD Connect ausgeführt werden müssen.</li>
[Express-Einstellungen](active-directory-aadconnect-get-started-express.md) | <li>Wenn Sie über eine einzelne Gesamtstruktur-AD verfügen, ist dies die empfohlene Option.</li> <li>Benutzer melden Sie sich mit dem gleichen Kennwort mithilfe der Kennwortsynchronisierung an.</li>
[Benutzerdefinierte Einstellungen](active-directory-aadconnect-get-started-custom.md) | <li>Verwendet bei mehreren Gesamtstrukturen. Unterstützt viele lokale [Topologien](active-directory-aadconnect-topologies.md).</li> <li>Passen Sie die Anmeldeoption an, z.B. ADFS für den Verbund, oder verwenden Sie einen Drittanbieter-Identitätsanbieter.</li> <li>Passen Sie Synchronisierungsfunktionen an, wie das Filtern und Rückschreiben.</li>
[Upgrade von DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Wird bei einem vorhandenen DirSync-Server verwendet, der bereits ausgeführt wird.</li>
[Upgrade von Azure AD Sync oder Azure AD Connect](active-directory-aadconnect-upgrade-previous-version.md)| <li>Je nach Bedarf gibt es verschiedene Methoden.</li>


[Nach der Installation](active-directory-aadconnect-whats-next.md) sollten Sie überprüfen, ob es wie erwartet funktioniert und den Benutzern Lizenzen zuweisen.

### Nächste Schritte für die Installation von Azure AD Connect

Thema |  
--------- | ---------
Azure AD Connect herunterladen | [Azure AD Connect herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771)
Installieren mit den Express-Einstellungen | [Expressinstallation von Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Installieren mit benutzerdefinierten Einstellungen | [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Upgrade von DirSync | [Upgrade von Azure AD-Synchronisierungstools (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Nach der Installation | [Überprüfen der Installation und Zuweisen von Lizenzen ](active-directory-aadconnect-whats-next.md)

### Weitere Informationen über die Installation von Azure AD Connect

Sie sollten sich auch auf [betriebliche](active-directory-aadconnectsync-operations.md) Probleme vorbereiten. Ein Standbyserver empfiehlt sich, damit Sie im [Notfall](active-directory-aadconnectsync-operations.md#disaster-recovery) problemlos ein Failover durchführen können. Wenn Sie häufig Konfigurationsänderungen vornehmen möchten, sollten Sie einen [Stagingmodus](active-directory-aadconnectsync-operations.md#staging-mode)-Server einplanen.

Thema |  
--------- | ---------
Unterstützte Topologien | [Topologien für Azure AD Connect](active-directory-aadconnect-topologies.md)
Entwurfskonzepte | [Entwurfskonzepte für Azure AD Connect](active-directory-aadconnect-design-concepts.md)
Für die Installation verwendete Konten | [Weitere Informationen zu den Anmeldeinformationen und Berechtigungen von Azure AD Connect](active-directory-aadconnect-accounts-permissions.md)
Operative Planung | [Azure AD Connect Sync: Operative Aufgaben und Überlegungen](active-directory-aadconnectsync-operations.md)
Optionen für die Benutzeranmeldung | [Azure AD Connect-Optionen für die Benutzeranmeldung](active-directory-aadconnect-user-signin.md)

## Konfigurieren der Synchronisierungsfunktionen
Azure AD Connect verfügt über mehrere Funktionen, die Sie optional aktivieren können oder die standardmäßig aktiviert sind. Einige Funktionen benötigen möglicherweise eine zusätzliche Konfiguration in bestimmten Szenarien und Topologien.

[Filtern](active-directory-aadconnectsync-configure-filtering.md) wird verwendet, wenn Sie begrenzen möchten, welche Objekte mit Azure AD synchronisiert werden. Standardmäßig werden alle Benutzer, Kontakte, Gruppen und Windows 10-Computer synchronisiert. Sie können die Filterung ausgehend von Domänen, Organisationseinheiten oder Attributen ändern.

Die [Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) synchronisiert das Kennworthash in Active Directory nach Azure AD. So kann der Endbenutzer das gleiche Kennwort lokal und in der Cloud verwenden, es jedoch nur an einem Ort verwalten. Da es das lokale Active Directory als Autorität verwendet, können Sie auch Ihre eigene Kennwortrichtlinie verwenden.

Das [Kennwortrückschreiben](active-directory-passwords-getting-started.md) ermöglicht es den Benutzern, ihre Kennwörter in der Cloud zu ändern und zurückzusetzen und die lokale Kennwortrichtlinie anzuwenden.

Das [Geräterückschreiben](active-directory-aadconnect-feature-device-writeback.md) ermöglicht es Ihnen, ein in Azure AD registriertes Gerät wieder in das lokale Active Directory zurückzuschreiben, damit es für bedingten Zugriff verwendet werden kann.

Die Funktion zum [Verhindern eines versehentlichen Löschvorgangs](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) ist standardmäßig aktiviert und schützt Ihr Cloudverzeichnis vor mehreren gleichzeitigen Löschvorgängen. Standardmäßig sind 500 Löschvorgänge pro Durchlauf zulässig. Diese Einstellung kann abhängig von der Größe Ihres Unternehmens geändert werden.

Das [automatische Upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) ist für Installationen mit Expresseinstellungen standardmäßig aktiviert und stellt sicher, dass Ihre Azure AD Connect-Instanz immer mit der neuesten Version aktualisiert wird.

### Nächste Schritte zum Konfigurieren der Synchronisierungsfunktionen

Thema |  
--------- | --------- |
Konfigurieren der Filterung | [Azure AD Connect-Synchronisierung: Konfigurieren der Filterung](active-directory-aadconnectsync-configure-filtering.md)
Kennwortsynchronisierung | [Azure AD Connect-Synchronisierung: Implementieren der Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md)
Rückschreiben von Kennwörtern | [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md)
Geräterückschreiben | [Aktivieren des Geräterückschreibens in Azure AD Connect](active-directory-aadconnect-feature-device-writeback.md)
Verhindern von versehentlichen Löschungen | [Azure AD Connect-Synchronisierung: Verhindern von versehentlichen Löschvorgängen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
Automatisches Upgrade | [Azure AD Connect: Automatisches Upgrade](active-directory-aadconnect-feature-automatic-upgrade.md)

## Anpassen der Azure AD Connect-Synchronisierung
Für die Azure AD Connect-Synchronisierung ist eine Standardkonfiguration vorgegeben, die für die meisten Kunden und Topologien konzipiert wurde. Es gibt jedoch immer Situationen, in denen die Standardkonfiguration nicht funktioniert und angepasst werden muss. Sie können Änderungen wie in diesem Abschnitt und in den verknüpften Themen beschrieben vornehmen.

Wenn Sie noch nicht mit einer Synchronisierungstopologie gearbeitet haben, sollten Sie sich zunächst mit den Grundlagen und verwendeten Begriffen vertraut machen, die unter [Technische Konzepte](active-directory-aadconnectsync-technical-concepts.md) beschrieben werden. Azure AD Connect ist die Weiterentwicklung von MIIS2003, ILM2007 und FIM2010. Selbst wenn einige Dinge identisch sind, hat sich doch auch eine Menge verändert.

Bei der [Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md) wird davon ausgegangen, dass die Konfiguration möglicherweise mehrere Gesamtstrukturen umfasst. In diesen Topologien kann ein Benutzerobjekt möglicherweise in einer anderen Gesamtstruktur als Kontakt dargestellt werden. Der Benutzer kann in einer anderen Ressourcengesamtstruktur auch über ein verknüpftes Postfach verfügen. Das Verhalten der Standardkonfiguration wird unter [Benutzer und Kontakte](active-directory-aadconnectsync-understanding-users-and-contacts.md) beschrieben.

Das synchronisierte Konfigurationsmodell wird als [deklarative Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) bezeichnet. Die erweiterten Attributflüsse verwenden [Funktionen](active-directory-aadconnectsync-functions-reference.md), um Attributtransformationen auszudrücken. Sie können die gesamte Konfiguration mithilfe von Tools, die im Lieferumfang von Azure AD Connect enthalten sind, anzeigen und überprüfen. Wenn Sie Änderungen an der Konfiguration vornehmen müssen, sollten Sie sicherstellen, dass Sie die [bewährten Methoden](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) befolgen, damit neue Versionen leichter übernommen werden.

### Nächste Schritte zur Anpassung der Azure AD Connect-Synchronisierung

Thema |  
--------- | ---------
Alle Artikel zur Azure AD Connect-Synchronisierung | [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md)
Technische Konzepte | [Azure AD Connect-Synchronisierung: Technische Konzepte](active-directory-aadconnectsync-technical-concepts.md)
Grundlegendes zur Standardkonfiguration | [Azure AD Connect-Synchronisierung: Grundlegendes zur Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md)
Grundlegendes zu Benutzern und Kontakten | [Azure AD Connect-Synchronisierung: Grundlegendes zu Benutzern und Kontakten](active-directory-aadconnectsync-understanding-users-and-contacts.md)
Deklarative Bereitstellung | [Azure AD Connect-Synchronisierung: Grundlegendes zu Ausdrücken für die deklarative Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
Ändern der Standardkonfiguration | [Bewährte Methoden zum Ändern der Standardkonfiguration](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)

## Konfigurieren von Verbundfunktionen
AD FS kann für die Unterstützung von [mehreren Domänen](active-directory-aadconnect-multiple-domains.md) konfiguriert werden. Beispiel: Sie besitzen mehrere Domänen der obersten Ebene, die Sie für den Verbund verwenden müssen.

Wenn Ihr AD FS-Server nicht für die automatische Aktualisierung von Zertifikaten in Azure AD konfiguriert wurde oder wenn Sie eine Lösung ohne AD FS nutzen, werden Sie benachrichtigt, wenn Sie [Zertifikate aktualisieren](active-directory-aadconnect-o365-certs.md) müssen.

### Nächste Schritte zum Konfigurieren der Verbundfunktionen

Thema |  
--------- | ---------
Alle AD FS-Artikel | [Azure AD Connect und Verbund](active-directory-aadconnectfed-whatis.md)
Konfigurieren von AD FS mit Unterdomänen | [Unterstützung mehrerer Domänen für den Verbund mit Azure AD](active-directory-aadconnect-multiple-domains.md)
Verwalten der AD FS-Farm | [Verwaltung und Anpassung von AD FS mit Azure AD Connect](active-directory-aadconnect-federation-management.md)
Manuelles Aktualisieren von Verbundzertifikaten | [Erneuern von Verbundzertifikaten für Office 365 und Azure AD](active-directory-aadconnect-o365-certs.md)

## Weitere Informationen und Referenzen

Thema |  
--------- | --------- |
Versionsverlauf | [Versionsverlauf](active-directory-aadconnect-version-history.md)
Vergleich von DirSync, Azure ADSync und Azure AD Connect | [Vergleich von Tools für die Verzeichnisintegration](active-directory-hybrid-identity-design-considerations-tools-comparison.md)
Liste mit Informationen zur Kompatibilität zwischen AD FS-fremden Lösungen und Azure AD | [Azure AD-Verbund – Kompatibilitätsliste](active-directory-aadconnect-federation-compatibility.md)
Synchronisierte Attribute | [Synchronisierte Attribute](active-directory-aadconnectsync-attributes-synchronized.md)
Überwachen mit Azure AD Connect Health | [Azure AD Connect Health](active-directory-aadconnect-health.md)
Häufig gestellte Fragen | [Häufig gestellte Fragen zu Azure AD Connect](active-directory-aadconnect-faq.md)


**Weitere Ressourcen**


Ignite 2015-Präsentation über die Erweiterung lokaler Verzeichnisse in die Cloud.

>[AZURE.VIDEO microsoft-ignite-2015-extending-on-premises-directories-to-the-cloud-made-easy-with-azure-active-directory-connect]

<!---HONumber=AcomDC_0928_2016-->