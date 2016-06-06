<properties
   pageTitle="Aktivieren oder Deaktivieren einer Rolle | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit der Anwendung Azure Privileged Identity Management Rollen für privilegierte Identitäten aktivieren."
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
   ms.date="05/19/2016"
   ms.author="kgremban"/>

# Aktivieren oder Deaktivieren von Rollen in Azure AD Privileged Identity Management

Azure Active Directory (AD) Privileged Identity Management (PIM) vereinfacht die Art und Weise, in der Unternehmen die privilegierten Identitäten und deren Zugriff auf Ressourcen in Azure AD und anderen Microsoft Online Services wie Office 365 oder Microsoft Intune verwalten.

Dieser Artikel richtet sich an Administratoren, die ihre Rolle in Azure AD Privileged Identity Management aktivieren müssen. Der Artikel erläutert schrittweise, wie Sie eine Rolle aktivieren, wenn Sie die Berechtigungen benötigen, und wie Sie die Rolle wieder deaktivieren, wenn Sie die Berechtigungen nicht mehr benötigen.

Wenn Ihnen eine Administratorrolle zugewiesen wurde, können Sie diese Rolle aktivieren, um Aufgaben durchzuführen, für die diese Rolle erforderlich ist. Wenn Sie beispielsweise Office 365 nur gelegentlich verwalten müssen, werden die Administratoren für privilegierte Rollen in Ihrer Organisation Ihnen keine dauerhafte Administratorrolle zuweisen. Stattdessen werden Sie als Kandidat für die Rollen eines globalen Administrator oder eines Exchange Online-Administrators in Azure AD eingerichtet. Dies bedeutet, dass Sie eine temporäre Rollenzuweisung anfordern können, wenn Sie diese Berechtigungen benötigen, und damit über einen bestimmten Zeitraum administrative Kontrolle für Office 365 besitzen.


## Hinzufügen der Anwendung Privileged Identity Management

Verwenden Sie die Anwendung Azure AD Privileged Identity Management im [Azure-Portal](https://portal.azure.com/), um eine Rollenaktivierung anzufordern, auch wenn Sie in einem anderen Portal oder über PowerShell arbeiten werden. Wenn sich die Anwendung Azure AD Privileged Identity Management nicht in Ihrem Azure-Portal befindet, führen Sie die folgenden Schritte aus, um zu beginnen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, falls Sie dies noch nicht getan haben.
2. Wenn Ihre Organisation über mehr als ein Verzeichnis verfügt, klicken Sie in der oberen rechten Ecke des Azure-Portals auf Ihren Benutzernamen, und wählen Sie das Verzeichnis aus, in dem Sie arbeiten möchten.
3. Klicken Sie im linken Navigationsbereich auf das Symbol **Neu**.
4. Wählen Sie im Menü "Erstellen" **Sicherheit und Identität** aus.
5. Wählen Sie **Azure AD Privileged Identity Management** aus.
6. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf die Schaltfläche **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.

## Aktivieren einer Rolle

Wenn Sie eine Rolle übernehmen müssen, können Sie die Aktivierung über die Schaltfläche **Meine Rolle aktivieren** in der Anwendung Azure AD Privileged Identity Management anfordern.


1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wählen Sie die Kachel „Azure AD Privileged Identity Management“ aus.
2. Wählen Sie **Meine Rolle aktivieren** aus. Eine Liste der Ihnen zugewiesenen Rollen wird angezeigt.
3. Wählen Sie die Rolle aus, die Sie aktivieren möchten.
4. Wählen Sie **Aktivieren** aus. Das Blatt **Rollenaktivierung anfordern** wird angezeigt.
5. Bei einigen Rollen, wie z. B. dem globalen Administrator, ist die Multi-Factor Authentication (MFA) erforderlich, um die Rolle zu aktivieren. Wenn Sie bei der Anmeldung keine MFA verwendet haben, müssen Sie dies nachholen, damit die Rolle aktiviert werden kann.
6. Geben Sie im Textfeld den Grund für die Aktivierungsanforderung ein. Der Administrator für privilegierte Rollen kann Sie möglicherweise auffordern, eine Ticketnummer zur Problembehebung bereitzustellen.
7. Klicken Sie auf **OK**. Die Rolle wird jetzt aktiviert, und die Rollenänderung wird in den Microsoft Online Services sichtbar.

## Deaktivieren einer Rolle

Wenn eine Rolle aktiviert wurde, wird sie nach Erreichen des Zeitlimits deaktiviert.

Wenn Sie vorher mit Ihren Aufgaben fertig sind, können Sie die Rolle in der Anwendung Azure AD Privileged Identity Management auch manuell deaktivieren. Wählen Sie **Meine Rolle aktivieren** aus, suchen Sie die Rolle, die Sie nicht mehr benötigen, und wählen Sie **Deaktivieren** aus.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte

Weitere Informationen über Azure AD Privileged Identity Management finden Sie unter folgenden Links.

[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0525_2016-->