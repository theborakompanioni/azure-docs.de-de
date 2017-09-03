---
title: Beziehung zwischen Azure-Abonnements und Azure Active Directory | Microsoft Docs
description: "Enthält Informationen zum Anmelden bei Microsoft Azure und verwandten Aspekten, z.B. zur Beziehung zwischen einem Azure-Abonnement und Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: b2f8cc774b6cc245a4000e5c6924a8bb817707bc
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>Beziehung zwischen Azure-Abonnements und Azure Active Directory
Dieser Artikel enthält Informationen zur Beziehung zwischen einem Azure-Abonnement und Azure Active Directory (Azure AD).

## <a name="your-azure-subscriptions-relationship-to-azure-ad"></a>Beziehung zwischen Ihrem Azure-Abonnement und Azure AD
Ihr Azure-Abonnement hat eine Vertrauensbeziehung mit Azure AD (Vertrauensstellung). Das bedeutet, es vertraut dem Verzeichnis bei der Authentifizierung von Benutzern, Diensten und Geräten. Mehrere Abonnements können dem gleichen Verzeichnis vertrauen, jedes Abonnement vertraut jedoch nur einem einzelnen Verzeichnis. 

Die Vertrauensstellung zwischen einem Abonnement und einem Verzeichnis unterscheidet sich von der Beziehung zwischen einem Abonnement und anderen Ressourcen in Azure (Websites, Datenbanken usw.). Mit dem Ablauf eines Abonnements endet auch der Zugriff auf die anderen Ressourcen, die dem Abonnement zugeordnet sind. Das Verzeichnis verbleibt jedoch in Azure, und Sie können ihm ein anderes Abonnement zuordnen und weiterhin die Benutzer des Verzeichnisses verwalten.

![Zuordnungsdiagramm für Abonnements](./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png)

Die Funktionsweise von Azure AD unterscheidet sich von der Funktionsweise der anderen Dienste in Ihrem Azure-Abonnement. Andere Azure-Dienste sind dem Azure-Abonnement untergeordnet. Die in Azure AD angezeigten Inhalte variieren jedoch nicht nach Abonnement. Der Zugriff auf Verzeichnisse wird auf der Grundlage des angemeldeten Benutzers gewährt.

Alle Benutzer verfügen über ein einzelnes Basisverzeichnis für die Authentifizierung, können aber auch Gäste in anderen Verzeichnissen sein. In Azure AD sind für Sie nur Verzeichnisse sichtbar, denen Ihr Benutzerkonto angehört. Ein Verzeichnis kann auch mit dem lokalen Active Directory synchronisiert werden.

## <a name="azure-ad-and-cloud-service-subscriptions"></a>Azure AD und Clouddienstabonnements
Azure AD bietet die wichtigsten Unternehmensverzeichnis- und Verwaltungsfunktionen für die meisten Clouddienste von Microsoft, einschließlich:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Den Azure AD-Dienst erhalten Sie kostenlos, wenn Sie sich für einen dieser Clouddienste von Microsoft registrieren. Wenn Sie einem Azure AD-Verzeichnis ein weiteres Azure-Abonnement hinzufügen möchten, müssen Sie dazu mit einem Microsoft-Konto angemeldet sein. Wenn Sie sich also beispielsweise unter Verwendung eines Microsoft-Kontos für Azure und unter Verwendung eines Geschäfts-, Schul- oder Unikontos für einen anderen Microsoft-Clouddienst registrieren, verfügen Sie über zwei Instanzen von Azure AD:
1. Das Standardverzeichnis für Ihr Azure-Abonnement. Wenn Sie mit einem Microsoft-Konto angemeldet sind, können Sie diesem Verzeichnis oder anderen Verzeichnissen, die Sie erstellen, ein weiteres Azure-Abonnement hinzufügen, da Sie von Azure authentifiziert werden können.
2. Das Basisverzeichnis für Ihr Geschäfts-, Schul- oder Unikonto. Wenn Sie bei Azure mit einem Geschäfts-, Schul- oder Unikonto angemeldet sind, können Sie einem vorhandenen Verzeichnis kein Azure-Abonnement hinzufügen, da Ihr Geschäfts-, Schul- oder Unikonto nicht direkt von Azure authentifiziert werden kann. 
 
Weitere Informationen zum Ändern von Administratoren für ein Azure-Abonnement finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../billing/billing-subscription-transfer.md).

## <a name="suggestions-to-manage-both-a-subscription-and-a-directory"></a>Vorschläge für die Verwaltung eines Abonnements und eines Verzeichnisses
Die Administratorrollen für ein Azure-Abonnement verwalten Ressourcen, die an das Azure-Abonnement gebunden sind. In diesem Abschnitt werden die Unterschiede zwischen Azure-Abonnementadministratoren und Azure AD-Verzeichnisadministratoren erläutert. Informationen zu Administratorrollen sowie weitere Vorschläge für die Verwaltung Ihres Abonnements mithilfe dieser Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md).

Standardmäßig wird Ihnen die Dienstadministratorrolle zugewiesen, wenn Sie sich registrieren. Wenn sich andere Benutzer anmelden und mithilfe des gleichen Abonnements auf die Dienste zugreifen müssen, können Sie diese als Co-Administratoren hinzufügen. 

Azure AD weist eine andere Gruppe von Administratorrollen zum Verwalten der Features für Verzeichnisse und Identitäten. Der globale Administrator eines Verzeichnisses kann z. B. dem Verzeichnis Benutzer hinzufügen oder für Benutzer die Multi-Factor Authentication verlangen. Ein Benutzer, der ein Verzeichnis erstellt, wird der globalen Administratorrolle zugewiesen und kann anderen Benutzern Administratorrollen zuweisen. Die Azure AD-Administratorrollen werden auch von anderen Diensten (z. B. Office 365 und Microsoft Intune) genutzt. 

Azure-Abonnementadministratoren und Azure AD-Verzeichnisadministratoren sind zwei unterschiedliche Rollen. 
* Azure-Abonnementadministratoren können Ressourcen in Azure verwalten und Azure AD im Azure-Portal verwenden (da auch das Azure-Portal eine Azure-Ressource ist). 
* Verzeichnisadministratoren können Eigenschaften nur im Azure AD-Verzeichnis verwalten.

Eine Person kann beiden Rollen angehören, dies ist jedoch nicht zwingend erforderlich. Ein Benutzer kann der globalen Verzeichnisadministratorrolle, aber nicht als Dienstadministrator oder Co-Administrator eines Azure-Abonnements zugewiesen sein. Ein Benutzer ohne Administratorberechtigungen für das Abonnement kann sich zwar beim Azure-Portal anmelden, die Verzeichnisse für das Abonnement aber nicht über das Portal verwalten. Ein solcher Benutzer kann Verzeichnisse mit anderen Tools verwalten (beispielsweise mit Azure AD PowerShell oder Office 365 Admin Center).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Ändern von Administratoren für ein Azure-Abonnement finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../billing/billing-subscription-transfer.md).
* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](active-directory-understanding-resource-access.md)
* Informationen zum Zuweisen von Rollen in Azure AD finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG

