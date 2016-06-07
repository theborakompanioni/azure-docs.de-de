<properties
	pageTitle="Azure AD Privileged Identity Management | Microsoft Azure"
	description="Ein Thema, in dem erläutert wird, was Azure AD Privileged Identity Management ist und wie Sie damit die Sicherheit in Ihrer Cloud erhöhen."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/19/2016"
	ms.author="kgremban"/>

# Azure AD Privileged Identity Management

Mithilfe von Azure Active Directory (AD) Privileged Identity Management (PIM) können Sie Ihre privilegierten Identitäten und deren Zugriff auf Ressourcen in Azure AD und anderen Microsoft Online Services wie Office 365 oder Microsoft Intune verwalten, steuern und überwachen.

Manchmal müssen Benutzer privilegierte Vorgänge in Azure- oder Office 365-Ressourcen oder anderen SaaS-Apps ausführen. Dies bedeutet häufig, dass Organisationen diesen Benutzern in Azure AD dauerhaften privilegierten Zugriff gewähren müssen. Dies stellt ein wachsendes Sicherheitsrisiko für die in der Cloud gehosteten Ressourcen dar, da Organisationen die Aktionen, die diese Benutzer mit ihren Administratorberechtigungen ausführen, nicht ausreichend überwachen können. Darüber hinaus kann die Sicherheit der gesamten Cloud in Gefahr sein, wenn ein Benutzerkonto mit privilegiertem Zugriff kompromittiert wird. Mit Azure AD Privileged Identity Management können Sie dieses Risiko in den Griff bekommen.

Azure AD Privileged Identity Management ermöglicht Ihnen Folgendes:

- Ermitteln, welche Benutzer Azure AD-Administratoren sind
- Aktivieren von bedarfsgesteuertem Just-In-Time-Administratorzugriff auf Microsoft Online Services wie z. B. Office 365 und Intune
- Abrufen von Berichten zum Administratorzugriffsverlauf und zu Änderungen bei Administratorzuweisungen
- Aktivieren von Benachrichtigungen zum Zugriff auf eine privilegierte Rolle

Azure AD Privileged Identity Management kann die folgenden integrierten Azure AD-Organisationsrollen verwalten:

- Globaler Administrator
- Abrechnungsadministrator
- Dienstadministrator  
- Benutzeradministrator
- Kennwortadministrator

## Bedarfsabhängiger Administratorzugriff

In der Vergangenheit konnten Sie einen Benutzer über das frühere Azure-Verwaltungsportal oder Windows PowerShell einer Administratorrolle zuweisen. Dadurch wird der Benutzer zum **permanenten Administrator** für diese Rolle, der in der zugewiesenen Rolle stets aktiv bleibt. Azure AD Privileged Identity Management führt das Konzept eines **temporären Administrators** für eine Rolle ein, also einen Benutzer, der für die zugewiesene Rolle einen Aktivierungsprozess abschließen muss. Der Aktivierungsprozess ändert die Zuweisung des Benutzers zu einer Rolle in Azure AD für einen bestimmten Zeitraum, z. B. 8 Stunden, von „Inaktiv“ zu „Aktiv“.

## Aktivieren von Privileged Identity Management für Ihr Verzeichnis

Über das [Azure-Portal](https://portal.azure.com/) können Sie mit der Nutzung von Azure AD Privileged Identity Management beginnen. Azure AD Privileged Identity Management wird im früheren klassischen Portal nicht angezeigt.

>[AZURE.NOTE] Sie müssen ein globaler Administrator mit einem Geschäftskonto (kein Microsoft-Konto) sein, um Azure AD Privileged Identity Management für ein Verzeichnis zu aktivieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Verzeichnisses an.
2. Wenn Ihre Organisation über mehr als ein Verzeichnis verfügt, klicken Sie in der oberen rechten Ecke des Azure-Portals auf Ihren Benutzernamen, und wählen Sie das Verzeichnis aus, in dem Sie Azure AD Privileged Identity Management verwenden möchten.
3. Klicken Sie im linken Navigationsbereich auf das Symbol **Neu**.
4. Wählen Sie **Sicherheit und Identität** aus.
5. Wählen Sie **Azure AD Privileged Identity Management** aus.
6. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf die Schaltfläche **Erstellen**. Das Privileged Identity Management-Dashboard wird geöffnet.

Wenn Sie die erste Person sind, die Azure AD Privileged Identity Management in Ihrem Verzeichnis verwendet, führt Sie der [Sicherheits-Assistent](active-directory-privileged-identity-management-security-wizard.md) durch die erste Zuweisung. Danach sind Sie automatisch der erste **Sicherheitsadministrator** und der **Administrator für privilegierte Rollen** des Verzeichnisses.

Nur ein Administrator für privilegierte Rollen kann die PIM-App verwenden, um den Zugriff für andere Administratoren zu verwalten. Sie können [anderen Benutzern Zugriff auf PIM gewähren, damit diese Verwaltungsaktivitäten ausführen können](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## Privileged Identity Management-Dashboard

Azure AD Privileged Identity Management bietet ein Dashboard, das wichtige Informationen enthält, wie z. B.:

- Anzahl der Benutzer, die jeder privilegierten Rolle zugewiesen sind  
- Anzahl temporärer und permanenter Administratoren
- Zugriffsverlauf jedes Administrators

![PIM-Dashboard – Screenshot][2]

## Verwaltung privilegierter Rollen

Mit Azure AD Privileged Identity Management können Sie die Administratoren verwalten, indem Sie jeder Rolle permanente oder temporäre Administratoren hinzufügen oder diese entfernen.

![Hinzufügen/Entfernen von PIM-Administratoren – Screenshot][3]

## Konfigurieren der Rollenaktivierungseinstellungen

Mithilfe der Rollenaktivierungseinstellungen können Sie die Eigenschaften der Aktivierung der temporären Rolle konfigurieren:

- Dauer des Rollenaktivierungszeitraums
- Benachrichtigung zur Rollenaktivierung
- Informationen, die ein Benutzers während des Rollenaktivierungsprozesses bereitstellen muss  

![PIM-Einstellungen – Administratoraktivierung – Screenshot][4]

## Rollenaktivierung  

Um eine Rolle zu aktivieren, muss ein temporärer Administrator eine zeitgebundene "Aktivierung" für die Rolle anfordern. Die Aktivierung kann über die Option **Meine Rolle aktivieren** in Azure AD Privileged Identity Management angefordert werden.

Ein Administrator, der eine Rolle aktivieren möchte, muss Azure AD Privileged Identity Management im Azure-Portal initialisieren.

Alle Administratortypen können Azure AD Privileged Identity Management zum Aktivieren ihrer Rolle nutzen.

Die Rollenaktivierung ist zeitlich gebunden. In den Rollenaktivierungseinstellungen können Sie die Dauer der Aktivierung und auch die erforderlichen Informationen festlegen, die der Administrator angeben muss, um die Rolle zu aktivieren.

![Anforderung der Rollenaktivierung durch PIM-Administrator – Screenshot][5]

## Verlauf der Rollenaktivierung

Mithilfe von Azure AD Privileged Identity Management können Sie auch Änderungen bei Zuweisungen privilegierter Rollen und den Rollenaktivierungsverlauf nachverfolgen. Dies kann mithilfe der Überwachungsprotokolloptionen erfolgen:

![PIM-Aktivierungsverlauf – Screenshot][6]

## Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png

<!---HONumber=AcomDC_0525_2016-->