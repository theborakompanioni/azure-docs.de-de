<properties
	pageTitle="Einrichten von Azure Active Directory zur Self-Service-Verwaltung des Anwendungszugriffs | Microsoft Azure"
	description="Die Self-Service-Gruppenverwaltung ermöglicht Benutzern das Erstellen und Verwalten von Sicherheitsgruppen oder Office 365-Gruppen in Microsoft Azure Active Directory sowie das Anfordern der Mitgliedschaft in Sicherheitsgruppen oder Office 365-Gruppen."
	services="active-directory"
	documentationCenter=""
  authors="curtand"
	manager="stevenpo"
	editor=""
	/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="06/14/2016"
	ms.author="curtand"/>

# Einrichten von Azure Active Directory zur Self-Service-Gruppenverwaltung

Bei der Self-Service-Gruppenverwaltung können Benutzer Sicherheitsgruppen oder Office 365-Gruppen in Azure Active Directory (Azure AD) erstellen und verwalten. Benutzer können auch Mitgliedschaften in Sicherheitsgruppen oder Office 365-Gruppen anfordern. Der Besitzer der Gruppe kann dann die Mitgliedschaft genehmigen oder verweigern. Auf diese Weise kann die tagtägliche Steuerung der Gruppenmitgliedschaft an Personen delegiert werden, die den Geschäftskontext für die betreffende Mitgliedschaft verstehen. Self-Service-Funktionen zur Gruppenverwaltung sind nur für Sicherheitsgruppen und Office 365-Gruppen, aber nicht für E-Mail-aktivierte Sicherheitsgruppen oder Verteilerlisten verfügbar.

Die Self-Service-Gruppenverwaltung unterstützt derzeit zwei grundlegende Szenarien: delegierte Gruppenverwaltung und Self-Service-Gruppenverwaltung.

- **Delegierte Gruppenverwaltung** – Beispielsweise ein Administrator, der den Zugriff auf eine SaaS-Anwendung verwaltet, die im Unternehmen verwendet wird. Die Verwaltung dieser Zugriffsrechte wird immer mühsamer, und der Administrator bittet den Geschäftsinhaber, eine neue Gruppe zu erstellen. Der Administrator weist den Zugriff auf die Anwendung der neuen Gruppe zu und fügt der Gruppe alle Personen zu, die bereits auf die Anwendung zugreifen. Der Geschäftsinhaber kann dann weitere Benutzer hinzufügen, und die Benutzer erhalten automatisch Zugriff auf die Anwendung. Der Geschäftsinhaber muss nicht warten, bis der Administrator die Verwaltungsschritte für den Zugriff der Benutzer durchgeführt hat. Wenn der Administrator einem Manager in einer anderen Geschäftseinheit die gleiche Berechtigung erteilt, kann diese Person ebenfalls den Zugriff für ihre eigenen Benutzer verwalten. Weder der Geschäftsinhaber noch der Manager kann die Benutzer des jeweils anderen anzeigen oder verwalten. Der Administrator kann weiterhin alle Benutzer anzeigen, die auf die Anwendung zugreifen können, und kann die Zugriffsrechte bei Bedarf blockieren.

- **Self-Service-Gruppenverwaltung** Ein Beispiel für dieses Szenario sind zwei Benutzer, die beide über unabhängig voneinander eingerichtete SharePoint Online-Websites verfügen. Sie möchten dem anderen Team jeweils Zugriff auf ihre Websites gewähren. Hierzu können sie eine Gruppe in Azure AD erstellen, und in SharePoint Online wählt jeder der beiden Benutzer diese Gruppe dann aus, um Zugriff auf seine Websites bereitzustellen. Wenn ein weiterer Benutzer Zugriff benötigt, fordert er diesen einfach über den Zugriffsbereich an und erhält nach Genehmigung automatisch Zugriff auf beide SharePoint Online-Websites. Später entscheidet einer der Hauptbenutzer, dass alle Personen, die auf die Website zugreifen, auch Zugriff auf eine bestimmte SaaS-Anwendung erhalten sollen. Der Administrator der SaaS-Anwendung kann der SharePoint Online-Website Zugriffsrechte für die Anwendung hinzufügen. Anschließend können alle Personen, deren Zugriff genehmigt wird, auf die beiden SharePoint Online-Websites und diese SaaS-Anwendung zugreifen.

## Einrichten einer Gruppe für Self-Service durch Endbenutzer

1. Öffnen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) Ihr Azure AD-Verzeichnis.

2. Legen Sie auf der Registerkarte **Konfigurieren** die Option **Delegierte Gruppenverwaltung** auf „Aktiviert“ fest.

3. Legen Sie **Benutzer können Sicherheitsgruppen erstellen** oder **Benutzer können Office 365-Gruppen erstellen** auf „Aktiviert“ fest.

Wenn die Option **Benutzer können Sicherheitsgruppen erstellen** aktiviert ist, können alle Benutzer in Ihrem Verzeichnis neue Sicherheitsgruppen erstellen und diesen Gruppen Mitglieder hinzufügen. Diese neuen Gruppen werden auch für alle Benutzer im Zugriffsbereich angezeigt. Wenn die Richtlinieneinstellung der Gruppe dies zulässt, können andere Benutzer Anforderungen in Bezug auf den Beitritt zu diesen Gruppen erstellen. Wenn **Benutzer können Sicherheitsgruppen erstellen** deaktiviert ist, können Benutzer keine Gruppen erstellen und keine vorhandenen Gruppen ändern, deren Besitzer sie sind. Sie können aber trotzdem die Mitgliedschaften dieser Gruppen verwalten und Anforderungen anderer Benutzer zum Beitreten zu diesen Gruppen genehmigen.

Sie können auch mithilfe der Option **Benutzer, die Self-Service für Sicherheitsgruppen verwenden** eine genauer abgestufte Steuerung des Zugriffs auf die Self-Service-Gruppenverwaltung für Ihre Benutzer einrichten. Wenn die Option **Benutzer können Gruppen erstellen** aktiviert ist, können alle Benutzer in Ihrem Verzeichnis neue Gruppen erstellen und diesen Mitglieder hinzufügen. Indem Sie die Option **Benutzer, die Self-Service für Sicherheitsgruppen verwenden** auf „Einige“ festlegen, können Sie die Gruppenverwaltung auf eine begrenzte Benutzergruppe einschränken. Wenn dieser Switch auf „Some“ (Einige) festgelegt ist, müssen Sie Benutzer der Gruppe SSGMSecurityGroupsUsers hinzufügen, bevor diese neue Gruppen erstellen und Mitglieder hinzufügen können. Wenn Sie die Option **Benutzer, die Self-Service für Sicherheitsgruppen verwenden** auf „All“ (Alle) festlegen, können alle Benutzer in Ihrem Verzeichnis neue Gruppen erstellen.

Sie können auch das Feld **Gruppen, die Self-Service für Sicherheitsgruppen verwenden** verwenden, um einen benutzerdefinierten Namen für eine Gruppe anzugeben, deren Mitglieder den Self-Service nutzen können.

## Zusätzliche Informationen

Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)

* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

* [Was ist Azure Active Directory?](active-directory-whatis.md)

* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0615_2016-->