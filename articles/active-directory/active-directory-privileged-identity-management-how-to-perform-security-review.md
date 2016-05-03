<properties
   pageTitle="Ausführen einer Sicherheitsüberprüfung | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit der Anwendung Azure AD Privileged Identity Management eine Überprüfung ausführen."
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

# Ausführen einer Sicherheitsüberprüfung in Azure AD Privileged Identity Management

Azure Active Directory (AD) Privileged Identity Management (PIM) vereinfacht die Art und Weise, in der Unternehmen die privilegierten Identitäten und deren Zugriff auf Ressourcen in Azure AD und anderen Microsoft Online Services wie Office 365 oder Microsoft Intune verwalten.

Wenn Ihnen eine Administratorrolle zugewiesen wurde, werden Sie vom Sicherheitsadministrator Ihrer Organisation möglicherweise gebeten, regelmäßig zu überprüfen, ob Sie diese Rolle für Ihre Aufgaben benötigen, und dies zu bestätigen. Sie erhalten möglicherweise eine E-Mail mit einem Link, oder Sie können direkt zum [Azure-Portal](https://portal.azure.com) wechseln. Führen Sie die Schritte in diesem Artikel aus, um die Ihnen zugewiesenen Rollen selbst zu überprüfen.

Wenn Sie Sicherheitsadministrator sind und sich für die Sicherheitsüberprüfungen interessieren, erhalten Sie weitere Informationen unter [Starten einer Sicherheitsüberprüfung](active-directory-privileged-identity-management-how-to-start-security-review.md).

## Hinzufügen der Anwendung Privileged Identity Management

Sie können die Anwendung Azure AD Privileged Identity Management (PIM) im [Azure-Portal](https://portal.azure.com/) verwenden, um die Überprüfung durchzuführen. Wenn sich die Anwendung Azure AD Privileged Identity Management nicht in Ihrem Portal befindet, führen Sie die folgenden Schritte aus, um zu beginnen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, falls Sie dies noch nicht getan haben.
2. Wenn Ihre Organisation über mehr als ein Verzeichnis verfügt, klicken Sie in der oberen rechten Ecke des Azure-Portals auf Ihren Benutzernamen, und wählen Sie das Verzeichnis aus, in dem Sie arbeiten möchten.
3. Wählen Sie im linken Navigationsbereich das Symbol **Neu** aus.
4. Wählen Sie **Sicherheit und Identität** aus dem Menü aus.
5. Wählen Sie **Azure AD Privileged Identity Management** aus.
6. Lassen Sie das Kontrollkästchen **An Dashboard anheften** aktiviert, und klicken Sie dann auf die Schaltfläche **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.


## Genehmigen oder Verweigern des Zugriffs

Der Zugriff wird erst geändert, wenn [die Überprüfung abgeschlossen ist](active-directory-privileged-identity-management-how-to-complete-review.md). Dieser Vorgang besteht lediglich aus dem Erstellen einer Prüfliste für Personen, die den Zugriff für die Rolle möglicherweise ändern. Nachdem mindestens ein Benutzer ausgewählt wurde, werden die Schaltflächen **Zugriff genehmigen** und **Zugriff verweigern** aktiviert.

1. Klicken Sie in der PIM-Anwendung auf **Administratorzugriff überprüfen**. Eine Liste mit Sicherheitsüberprüfungen wird angezeigt.
2. Wählen Sie in der Liste den/die **Benutzer** aus, dessen/deren Zugriff Sie ändern möchten.
3. Klicken Sie für die von Ihnen ausgewählten Benutzer auf **Zugriff genehmigen** oder **Zugriff verweigern**. Im Hauptmenü des Azure-Portals wird eine Benachrichtigung angezeigt, und die ausgewählten Namen in der Überprüfungsliste werden entfernt (Sie können die Namen wieder anzeigen, indem Sie die Filteroptionen ändern). Schließen Sie das Blatt **Azure AD-Rollen überprüfen**.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0420_2016-->