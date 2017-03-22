---
title: Verwenden von Gruppen zum Verwalten des Zugriffs auf Ressourcen in Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Gruppen in Azure Active Directory zum Verwalten des Benutzerzugriffs auf lokale und Cloudanwendungen sowie Ressourcen verwenden.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 714120d0-cdf9-465d-afee-39bef591c6b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: ac3f563828c5fa379f328392a3f5cf7c7932f534
ms.openlocfilehash: 4cde8be3daed2f40b24218726ad8d76455d224ac
ms.lasthandoff: 03/01/2017


---
# <a name="manage-access-to-resources-with-azure-active-directory-groups"></a>Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen
Azure Active Directory (Azure AD) ist eine umfassende Lösung für die Identitäts- und Zugriffsverwaltung, die einen robusten Satz von Funktionen zur Verwaltung des Zugriffs auf lokale und Cloudanwendungen und -ressourcen bereitstellt, einschließlich Microsoft-Onlinediensten wie Office 365 und zahlreicher Microsoft-SaaS-Anwendungen. Dieser Artikel bietet eine allgemeine Übersicht. Wenn Sie jedoch direkt mit der Verwendung von Azure AD-Gruppen beginnen möchten, befolgen Sie die Anweisungen unter [Verwalten von Sicherheitsgruppen in Azure AD](active-directory-accessmanagement-manage-groups.md). Weitere Informationen zum Verwalten von Gruppen in Azure Active Directory mithilfe von PowerShell finden Sie unter [Azure Active Directory-Vorschau-Cmdlets für die Gruppenverwaltung](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

> [!NOTE]
> Um Azure Active Directory verwenden zu können, benötigen Sie ein Azure-Konto. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/pricing/free-trial/).
>
>

Eines der wichtigsten Features in Azure AD ist die Möglichkeit, den Zugriff auf Ressourcen zu verwalten. Diese Ressourcen können Teil des Verzeichnisses sein, wie im Fall von Berechtigungen zum Verwalten von Objekten mithilfe von Rollen im Verzeichnis, oder externe Ressourcen, z. B. SaaS-Anwendungen, Azure-Dienste und SharePoint-Websites oder lokale Ressourcen. Es gibt vier Möglichkeiten, einem Benutzer Zugriffsrechte auf eine Ressource zuzuweisen:

1. Direkte Zuweisung

    Benutzer können einer Ressource direkt vom Besitzer dieser Ressource zugewiesen werden.
2. Gruppenmitgliedschaft

    Eine Gruppe kann einer Ressource durch den Besitzer der Ressource zugewiesen werden, wodurch den Mitgliedern dieser Gruppe Zugriff auf die Ressource gewährt wird. Die Mitgliedschaft der Gruppe kann dann vom Besitzer der Gruppe verwaltet werden. Der Besitzer der Ressource delegiert die Berechtigung, dieser Ressource Benutzer zuzuweisen, an den Besitzer der Gruppe.
3. Regelbasiert

    Der Besitzer der Ressource kann eine Regel verwenden, um auszudrücken, welchen Benutzern Zugriff auf eine Ressource zugewiesen werden soll. Das Ergebnis der Regel hängt von den in der Regel verwendeten Attributen und deren Werten für bestimmte Benutzer ab. So delegiert der Ressourcenbesitzer das Recht zur Verwaltung des Zugriffs auf die Ressource an die autoritative Quelle für die Attribute, die in der Regel verwendet werden. Der Ressourcenbesitzer verwaltet weiterhin die Regel selbst und legt fest, welche Attribute und Werte Zugriff auf die Ressource bereitstellen.
4. Externe Autorität

    Der Zugriff auf eine Ressource stammt aus einer externen Quelle, z.B. einer Gruppe, die mittels einer autoritativen Quelle synchronisiert wird, etwa mittels eines lokalen Verzeichnisses oder einer SaaS-App wie WorkDay. Der Besitzer der Ressource weist der Gruppe Zugriff auf die Ressource zu, und die externe Datenquelle verwaltet die Mitglieder der Gruppe.

   ![Übersicht über das Access Management-Diagramm](./media/active-directory-access-management-groups/access-management-overview.png)

## <a name="watch-a-video-that-explains-access-management"></a>Video zur Erläuterung der Zugriffsverwaltung
Sie können ein kurzes Video anschauen, das nähere Informationen zum Thema bereitstellt.

**Azure AD: Einführung in dynamische Mitgliedschaften für Gruppen**

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]
>
>

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Wie funktioniert die Zugriffsverwaltung in Azure Active Directory?
Im Mittelpunkt der Lösung zur Zugriffsverwaltung von Azure AD steht die Sicherheitsgruppe. Die Verwendung einer Sicherheitsgruppe zum Verwalten des Zugriffs auf Ressourcen ist ein bekanntes Paradigma, mit dem auf flexible und leicht verständliche Weise einer dafür vorgesehenen Gruppe von Benutzern Zugriff auf eine Ressource bereitgestellt werden kann. Der Besitzer der Ressource (oder der Administrator des Verzeichnisses) kann eine Gruppe zuweisen, um bestimmte Zugriffsrechte für seine Ressourcen bereitzustellen. Den Mitgliedern der Gruppe wird Zugriff erteilt, und der Besitzer der Ressource kann das Recht zur Verwaltung der Mitgliederliste einer Gruppe an Dritte delegieren – z.B. einen Abteilungsleiter oder Helpdesk-Administrator.

![Das Azure Active Directory Access Management-Diagramm](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Der Besitzer einer Gruppe kann diese Gruppe auch für Self-Service-Anforderungen zur Verfügung stellen. In diesem Fall kann ein Endbenutzer die Gruppe suchen und finden und eine Beitrittsanfrage stellen. Damit strebt er die Berechtigung zum Zugriff auf die Ressourcen an, die durch die Gruppe verwaltet werden. Der Besitzer der Gruppe kann die Gruppe so einrichten, dass Beitrittsanfragen automatisch genehmigt werden, oder die Genehmigung durch den Besitzer der Gruppe fordern. Wenn ein Benutzer eine Gruppenbeitrittsanfrage stellt, wird die Anfrage an die Besitzer der Gruppe weitergeleitet. Wenn einer der Besitzer die Anfrage genehmigt, wird der anfragende Benutzer benachrichtigt und der Gruppe hinzugefügt. Wenn einer der Besitzer die Anfrage ablehnt, wird der anfragende Benutzer benachrichtigt, jedoch nicht der Gruppe hinzugefügt.

## <a name="getting-started-with-access-management"></a>Erste Schritte mit der Zugriffsverwaltung
Wollen Sie loslegen? Sie sollten einige der grundlegenden Aufgaben testen, die Sie mit Azure AD-Gruppen ausführen können. Verwenden Sie diese Funktionen, um verschiedenen Gruppen von Benutzern spezialisierten Zugriff auf verschiedene Ressourcen in Ihrer Organisation bereitzustellen. Eine Liste der ersten grundlegenden Schritte ist unten aufgeführt.

* [Erstellen einer einfachen Regel zum Konfigurieren von dynamischen Mitgliedschaften für eine Gruppe](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)
* [Verwenden einer Gruppe zum Verwalten des Zugriffs auf SaaS-Anwendungen](active-directory-accessmanagement-group-saasapps.md)
* [Einrichten einer Gruppe für Self-Service durch Endbenutzer](active-directory-accessmanagement-self-service-group-management.md)
* [Synchronisieren einer lokalen Gruppe in Azure mittels Azure AD Connect](active-directory-aadconnect.md)
* [Verwalten von Besitzern einer Gruppe](active-directory-accessmanagement-managing-group-owners.md)

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun die Grundlagen der Zugriffsverwaltung kennen, können Sie sich jetzt mit einigen erweiterten Funktionen vertraut machen, die in Azure Active Directory für die Verwaltung des Zugriffs auf Anwendungen und Ressourcen zur Verfügung stehen.

* [Verwenden von Attributen zum Erstellen erweiterter Regeln](active-directory-accessmanagement-groups-with-advanced-rules.md)
* [Verwalten von Sicherheitsgruppen in Azure AD](active-directory-accessmanagement-manage-groups.md)
* [Einrichten dedizierter Gruppen in Azure AD](active-directory-accessmanagement-dedicated-groups.md)
* [Graph-API-Referenz für Gruppen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)
* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)

