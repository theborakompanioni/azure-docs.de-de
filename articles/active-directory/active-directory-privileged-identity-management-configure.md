---
title: Konfigurieren von Azure AD Privileged Identity Management | Microsoft-Dokumentation
description: "Ein Thema, in dem erläutert wird, was Azure AD Privileged Identity Management ist und wie Sie damit die Sicherheit in Ihrer Cloud erhöhen."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: c548ed2e-06e3-4eaf-a63d-0f02ee72da25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 44867f16acb620fd29957eae90311ef6c8fde559
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---

# <a name="what-is-azure-ad-privileged-identity-management"></a>Was ist Azure AD Privileged Identity Management?
Mit Azure Active Directory (AD) Privileged Identity Management können Sie den Zugriff innerhalb Ihrer Organisation verwalten, steuern und überwachen. Dazu zählt der Zugriff auf Ressourcen in Azure AD und anderen Microsoft-Onlinediensten wie Office 365 oder Microsoft Intune.  

> [!NOTE]
> Privileged Identity Management ist für Ihre gesamte Organisation verfügbar, wenn Sie Ihre Administratoren mit der Premium P2-Edition von Azure Active Directory lizenzieren. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

Organisationen möchten die Anzahl der Personen minimieren, die auf sichere Informationen oder Ressourcen zugreifen können, da dadurch das Risiko reduziert wird, dass ein böswilligen Benutzers Zugriff hat. Benutzer müssen jedoch immer noch in der Lage sein, in Azure-, Office 365- oder SaaS-Apps privilegierte Vorgänge auszuführen. Organisationen gewähren Benutzern privilegierten Zugriff in Azure AD, ohne zu überwachen, was Benutzer mit ihren Administratorberechtigungen machen. Mit Azure AD Privileged Identity Management können Sie dieses Risiko in den Griff bekommen.  

Azure AD Privileged Identity Management ermöglicht Ihnen Folgendes:  

* Ermitteln, welche Benutzer Azure AD-Administratoren sind
* Aktivieren von bedarfsgesteuertem Just-In-Time-Administratorzugriff auf Microsoft Online Services wie z. B. Office 365 und Intune
* Abrufen von Berichten zum Administratorzugriffsverlauf und zu Änderungen bei Administratorzuweisungen
* Aktivieren von Benachrichtigungen zum Zugriff auf eine privilegierte Rolle
* Erfordern der Genehmigung zum Aktivieren (Vorschauversion)

Azure AD Privileged Identity Management kann u.a. die folgenden integrierten Azure AD-Organisationsrollen verwalten:  

* Globaler Administrator
* Abrechnungsadministrator
* Dienstadministrator  
* Benutzeradministrator
* Kennwortadministrator

## <a name="just-in-time-administrator-access"></a>Bedarfsabhängiger Administratorzugriff
In der Vergangenheit konnten Sie einen Benutzer über das klassische Azure-Portal oder Windows PowerShell einer Administratorrolle zuweisen. Dadurch wird der Benutzer zum **permanenten Administrator**, der in seiner zugewiesenen Rolle stets aktiv bleibt. Mit Azure AD Privileged Identity Management wird das Konzept **berechtigter Administratoren** eingeführt. Berechtigte Administratoren sollten Benutzer sein, die ab und an privilegierten Zugriff benötigen, aber nicht jeden Tag. Die Rolle ist inaktiv, bis der Benutzer Zugriff benötigt. Dann wird eine Aktivierung ausgeführt, und der Benutzer wird für einen zuvor festgelegten Zeitraum zu einem aktiven Administrator.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Aktivieren von Privileged Identity Management für Ihr Verzeichnis
Im [Azure-Portal](https://portal.azure.com/)können Sie mit der Nutzung von Azure AD Privileged Identity Management beginnen.

> [!NOTE]
> Sie müssen ein globaler Administrator mit einem Geschäftskonto (wie @yourdomain.com) und keinem Microsoft-Konto (wie @outlook.com) sein, um Azure AD Privileged Identity Management für ein Verzeichnis zu aktivieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Verzeichnisses an.
2. Wenn Ihre Organisation über mehr als ein Verzeichnis verfügt, klicken Sie in der oberen rechten Ecke des Azure-Portals auf Ihren Benutzernamen. Wählen Sie das Verzeichnis aus, in dem Sie Azure AD Privileged Identity Management verwenden möchten.
3. Wählen Sie **Weitere Dienste** aus, und verwenden Sie das Textfeld „Filter“, um nach **Azure AD Privileged Identity Management** zu suchen.
4. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.

Wenn Sie die erste Person sind, die Azure AD Privileged Identity Management in Ihrem Verzeichnis verwendet, führt Sie der [Sicherheits-Assistent](active-directory-privileged-identity-management-security-wizard.md) durch die erste Zuweisung. Danach sind Sie automatisch der erste **Sicherheitsadministrator** und der **Administrator für privilegierte Rollen** des Verzeichnisses.

Nur ein Administrator für privilegierte Rollen kann den Zugriff für andere Administratoren verwalten. Sie können [anderen Benutzern Zugriff auf PIM gewähren, damit diese Verwaltungsaktivitäten ausführen können](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management-Administator-Dashboard
Azure AD Privileged Identity Manager bietet ein Administratoren-Dashboard, das wichtige Informationen enthält, wie etwa:

* Warnungen, die auf Möglichkeiten zur Erhöhung der Sicherheit hinweisen
* Anzahl der Benutzer, die jeder privilegierten Rolle zugewiesen sind  
* Anzahl berechtigter und permanenter Administratoren
* Ein Diagramm der Aktivierungen privilegierter Rollen in Ihrem Verzeichnis

![PIM-Dashboard – Screenshot][2]

## <a name="privileged-role-management"></a>Verwaltung privilegierter Rollen
Mit Azure AD Privileged Identity Management können Sie die Administratoren verwalten, indem Sie jeder Rolle permanente oder berechtigte Administratoren hinzufügen oder diese entfernen.

![Hinzufügen/Entfernen von PIM-Administratoren – Screenshot][3]

## <a name="configure-the-role-activation-settings"></a>Konfigurieren der Rollenaktivierungseinstellungen
Mithilfe der [Rolleneinstellungen](active-directory-privileged-identity-management-how-to-change-default-settings.md) können Sie die Eigenschaften der Aktivierung der berechtigten Rolle konfigurieren:

* Dauer des Rollenaktivierungszeitraums
* Benachrichtigung zur Rollenaktivierung
* Informationen, die ein Benutzers während des Rollenaktivierungsprozesses bereitstellen muss
* Dienstticket oder Vorfallnummer
* [Anforderungen an den Genehmigungsworkflow – Vorschauversion](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![PIM-Einstellungen – Administratoraktivierung – Screenshot][4]

Beachten Sie, dass in der Abbildung die Schaltflächen für **Multi-Factor Authentication** deaktiviert sind. Für bestimmte, sehr privilegierte Rollen ist MFA für erhöhten Schutz erforderlich.

## <a name="role-activation"></a>Rollenaktivierung
Um eine [Rolle zu aktivieren](active-directory-privileged-identity-management-how-to-activate-role.md), fordert ein berechtigter Administrator eine zeitgebundene „Aktivierung“ für die Rolle an. Die Aktivierung kann über die Option **Meine Rolle aktivieren** in Azure AD Privileged Identity Management angefordert werden.

Ein Administrator, der eine Rolle aktivieren möchte, muss Azure AD Privileged Identity Management im Azure-Portal initialisieren.

Die Rollenaktivierung ist anpassbar. In den PIM-Einstellungen können Sie die Dauer der Aktivierung sowie die Informationen festlegen, die der Administrator angeben muss, um die Rolle zu aktivieren.

![Anforderung der Rollenaktivierung durch PIM-Administrator – Screenshot][5]

## <a name="review-role-activity"></a>Überprüfen der Rollenaktivität
Ihnen stehen zwei Möglichkeiten zur Verfügung, um nachzuverfolgen, wie Ihre Mitarbeiter und Administratoren privilegierte Rollen nutzen. Die erste Option verwendet den [Verlauf der Verzeichnisrollenüberwachung](active-directory-privileged-identity-management-how-to-use-audit-log.md). Der Überwachungsverlauf protokolliert Änderungen bei Zuweisungen privilegierter Rollen und im Verlauf der Rollenaktivierung.

![PIM-Aktivierungsverlauf – Screenshot][6]

Die zweite Option besteht darin, regelmäßige [Zugriffsüberprüfungen](active-directory-privileged-identity-management-how-to-start-security-review.md)einzurichten. Diese Zugriffsüberprüfungen können von einem zugewiesenen Prüfer (wie etwa einem Teamleiter) durchgeführt werden, oder die Mitarbeiter führen die Überprüfung selbst durch. Dies ist die beste Möglichkeit, um zu prüfen, wer noch Zugriff benötigt und wer nicht mehr.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM bei Abonnementablauf
Vor dem Erreichen der allgemeinen Verfügbarkeit war Azure AD PIM als Vorschauversion verfügbar, und für die Vorschau auf Azure AD PIM wurden keine Lizenzüberprüfungen für einen Mandanten durchgeführt.  Jetzt, da Azure AD PIM allgemein verfügbar ist, müssen den Administratoren des Mandanten Test- oder kostenpflichtige Lizenzen zugewiesen werden, damit PIM weiterhin verwendet werden kann.  Wenn Ihr Unternehmen Azure AD Premium P2 nicht erwirbt, oder Ihre Testversion abläuft, werden fast alle Funktionen von Azure AD PIM nicht mehr in Ihrem Mandanten verfügbar sein.  Weitere Informationen finden Sie in den [Abonnementanforderungen von Azure AD PIM](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png

