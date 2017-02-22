---
title: Ressourcen, Rollen und Zugriffssteuerung in Application Insights
description: "&quot;Besitzer&quot;, &quot;Mitwirkende&quot; und &quot;Leser&quot; für die gewonnenen Unternehmensinformationen."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/07/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1520fde6b60546e408772e04488e8a530a9c1344


---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Ressourcen, Rollen und Zugriffssteuerung in Application Insights
Anhand einer [rollenbasierten Zugriffssteuerung in Microsoft Azure](../active-directory/role-based-access-control-configure.md) können Sie steuern, wer in Visual Studio [Application Insights][start] über Lese- und Aktualisierungszugriff auf Ihre Daten verfügt.

> [!IMPORTANT]
> Weisen Sie Benutzern den Zugriff in der **Ressourcengruppe oder dem Abonnement** zu, dem Ihre Anwendungsressource angehört – nicht in der Ressource selbst. Weisen Sie die Rolle **Mitwirkender der Application Insights-Komponente** zu. Dadurch wird eine einheitliche Steuerung des Zugriffs auf Webtests und Warnungen zusammen mit Ihrer Anwendungsressource sichergestellt. [Weitere Informationen](#access).
> 
> 

## <a name="resources-groups-and-subscriptions"></a>Ressourcen, Gruppen und Abonnements
Zunächst einige Definitionen:

* **Ressource** – Eine Instanz eines Microsoft Azure-Diensts. Die Application Insights-Ressource erfasst, analysiert und zeigt die Telemetriedaten an, die von der Anwendung gesendet werden.  Andere Arten von Azure-Ressourcen sind Web-Apps, Datenbanken und virtuelle Computer.
  
    Um alle Ressourcen anzuzeigen, wechseln Sie zum [Azure-Portal][portal], melden Sie sich an, und klicken Sie auf „Durchsuchen“.
  
    ![Wählen Sie "Durchsuchen", und wählen Sie dann entweder "Alle", oder filtern Sie nach Application Insights.](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Ressourcengruppe**][Gruppe]: Jede Ressource gehört zu einer Gruppe. Eine Gruppe ist eine bequeme Möglichkeit, verwandte Ressourcen insbesondere für die Zugriffssteuerung zu verwalten. Beispielsweise könnten Sie eine Web-App, eine Application Insights-Ressource zur Überwachung der App sowie eine Storage-Ressource zur Aufbewahrung exportierter Daten in eine Ressourcengruppe aufnehmen.

    ![Wählen Sie "Durchsuchen", "Ressourcengruppen" und dann eine Gruppe aus.](./media/app-insights-resources-roles-access-control/11-group.png)

* [**Abonnement**](https://manage.windowsazure.com): Um Application Insights oder sonstige Azure-Ressourcen zu verwenden, müssen Sie ein Azure-Abonnement abschließen. Jede Ressourcengruppe gehört zu einem Azure-Abonnement, in dem Sie Ihr Preispaket und, wenn es sich um ein Abonnement für die Organisation handelt, die Mitglieder und deren Zugriffsberechtigungen auswählen.
* [**Microsoft-Konto**][Konto]: Benutzername und Kennwort für Ihre Anmeldung an Ihre Microsoft Azur-Abonnements, an XBox Live, Outlook.com und sonstige Microsoft-Dienste.

## <a name="a-nameaccessa-control-access-in-the-resource-group"></a><a name="access"></a> Steuern des Zugriffs in der Ressourcengruppe
Es ist wichtig zu wissen, dass es zusätzlich zu den Ressource, die Sie für Ihre Anwendung erstellt haben, auch separate, ausgeblendete Ressourcen für Warnungen und Webtests gibt. Sie sind derselben [Ressourcengruppe](#resource-group) zugeordnet wie Ihre Anwendung. Möglicherweise haben Sie auch andere Azure-Dienste dort aufgenommen, z. B. Websites oder Speicher.

![Ressourcen in Application Insights](./media/app-insights-resources-roles-access-control/00-resources.png)

Zum Steuern des Zugriffs auf diese Ressourcen empfiehlt es sich daher, folgendermaßen vorzugehen:

* Steuern Sie den Zugriff auf **Ressourcengruppen- oder Abonnementebene** .
* Weisen Sie Benutzern die Rolle **Mitwirkender der Application Insights-Komponente** zu. Dadurch können sie Webtests, Warnungen und Application Insights-Ressourcen bearbeiten, ohne dass Zugriff auf alle anderen Dienste in der Gruppe erteilt wird.

## <a name="to-provide-access-to-another-user"></a>So erteilen Sie einem anderen Benutzer Zugriffsberechtigungen
Sie benötigen Eigentümerrechte für das Abonnement oder die Ressourcengruppe.

Der Benutzer muss über ein [Microsoft-Konto][Konto] oder über Zugriff auf sein [Microsoft-Organisationskonto](../active-directory/sign-up-organization.md) verfügen. Sie können den Zugriff an Einzelpersonen und auch an Benutzergruppen erteilen, die in Azure Active Directory definiert sind.

#### <a name="navigate-to-the-resource-group"></a>Navigieren zur Ressourcengruppe
Fügen Sie den Benutzer dort hinzu.

![Öffnen Sie auf dem Ressourcenblatt in Ihrer Anwendung "Essentials", öffnen Sie die Ressourcengruppe, und wählen Sie dort "Einstellungen/Benutzer". Klicken Sie auf "Hinzufügen".](./media/app-insights-resources-roles-access-control/01-add-user.png)

Sie können auch zur nächsthöheren Ebene wechseln und den Benutzer zum Abonnement hinzufügen.

#### <a name="select-a-role"></a>Auswählen einer Rolle
![Wählen Sie eine Rolle für den neuen Benutzer.](./media/app-insights-resources-roles-access-control/03-role.png)

| Rolle | In der Ressourcengruppe |
| --- | --- |
| Besitzer |Kann alle Elemente ändern, einschließlich des Benutzerzugriffs |
| Mitwirkender |Kann alle Elemente bearbeiten, einschließlich aller Ressourcen |
| Mitwirkender der Application Insights-Komponente |Kann Application Insights-Ressourcen, Webtests und Warnungen bearbeiten. |
| Leser |Kann Inhalte anzeigen, aber nicht ändern. |

Zum "Bearbeiten" gehört das Erstellen, Löschen und Aktualisieren von:

* Ressourcen
* Webtests
* Warnungen
* Fortlaufendem Export

#### <a name="select-the-user"></a>Auswählen des Benutzers
![Geben Sie die E-Mail-Adresse eines neuen Benutzers ein. Auswählen des Benutzers](./media/app-insights-resources-roles-access-control/04-user.png)

Wenn der gewünschte Benutzer nicht im Verzeichnis enthalten ist, können Sie jeden Benutzer mit einem Microsoft-Konto einladen.
(Wenn Benutzer Dienste wie Outlook.com, OneDrive, Windows Phone oder XBox Live verwenden, besitzen sie ein Microsoft-Konto.)

## <a name="users-and-roles"></a>Benutzer und Rollen
* [Rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[Konto]: https://account.microsoft.com
[Gruppe]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md



<!--HONumber=Nov16_HO3-->


