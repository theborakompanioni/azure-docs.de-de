<properties
   pageTitle="Erste Schritte mit Azure AD Privileged Identity Management | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit der Anwendung Azure Active Directory Privileged Identity Management privilegierte Identitäten im Azure-Portal verwalten."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="04/15/2016"
   ms.author="kgremban"/>

# Erste Schritte mit Azure AD Privileged Identity Management


Mithilfe von Azure Active Directory (AD) Privileged Identity Management (PIM) können Sie Ihre privilegierten Identitäten und deren Zugriff auf Ressourcen in Azure AD und anderen Microsoft Online Services wie Office 365 oder Microsoft Intune verwalten, steuern und überwachen.

Dieser Artikel beschreibt, wie Sie die Azure AD PIM-App zu Ihrem Azure-Portaldashboard hinzufügen.

## Hinzufügen der Anwendung Privileged Identity Management

Bevor Sie Azure AD Privileged Identity Management verwenden können, müssen Sie die Anwendung zum Dashboard in Ihrem Azure-Portal hinzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Verzeichnisses an.
2. Wenn Ihre Organisation über mehr als ein Verzeichnis verfügt, klicken Sie in der oberen rechten Ecke des Azure-Portals auf Ihren Benutzernamen, und wählen Sie das Verzeichnis aus, in dem Sie PIM verwenden möchten.
3. Wählen Sie im linken Navigationsbereich das Symbol **Neu** aus.
4. Wählen Sie **Sicherheit und Identität** aus.
5. Wählen Sie **Azure AD Privileged Identity Management** aus.
6. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf die Schaltfläche **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.


Wenn Sie die erste Person sind, die Azure AD Privileged Identity Management in Ihrem Verzeichnis verwendet, führt Sie der [Sicherheits-Assistent](active-directory-privileged-identity-management-security-wizard.md) durch die erste Zuweisung. Danach sind Sie automatisch der erste **Sicherheitsadministrator** des Verzeichnisses. Nur ein Sicherheitsadministrator kann auf diese Anwendung zugreifen, um den Zugriff für andere Administratoren zu verwalten.

Andernfalls, wenn Ihnen durch einen anderen Sicherheitsadministrator eine oder mehrere Rollen zugewiesen wurden, können Sie auswählen, welche Rolle Sie aktivieren möchten. Wenn Sie selbst Sicherheitsadministrator sind, wird eine Option angezeigt, mit der Sie **Identitäten verwalten** können.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte

Die Übersicht über [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) enthält weitere Details zur Verwaltung des administrativen Zugriffs in Ihrer Organisation.

[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0420_2016-->