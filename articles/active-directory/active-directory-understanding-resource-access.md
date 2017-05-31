---
title: Grundlegendes zum Zugriff auf Ressourcen in Azure | Microsoft Docs
description: "In diesem Thema werden Konzepte zum Einsatz von Abonnementadministratoren erklärt, um den Zugriff auf Ressourcen im gesamten Azure-Portal zu steuern."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 174f1706-b959-4230-9a75-bf651227ebf6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: d9143c1ec2075f71659e8deaadc93d1972ea9a85
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="understanding-resource-access-in-azure"></a>Grundlegendes zum Zugriff auf Ressourcen in Azure
> [!NOTE]
> In diesem Thema werden Konzepte zum Einsatz von Abonnementadministratoren erklärt, um den Zugriff auf Ressourcen im ganzen Azure-Portal zu steuern. Als Alternative bietet das Azure-Vorschauportal die [rollenbasierte Zugriffssteuerung](role-based-access-control-configure.md) für eine detailliertere Verwaltung der Azure-Ressourcen.
> 
> 

Im Oktober 2013 wurden das klassische Azure-Portal und die Dienstverwaltungs-APIs in Azure Active Directory integriert, um die Benutzerfreundlichkeit bei der Verwaltung des Azure-Ressourcenzugriffs zu verbessern. Azure Active Directory bietet bereits leistungsstarke Funktionen wie Benutzerverwaltung, lokale Verzeichnissynchronisierung, mehrstufige Authentifizierung und Anwendungszugriffssteuerung. Diese Funktionen sollten natürlich auch für die allgemeine Verwaltung von Azure-Ressourcen zur Verfügung stehen.

Die Zugriffssteuerung in Azure unterliegt zunächst den Abrechnungsaspekten. Der Besitzer eines Azure-Kontos, auf das über das [Azure Accounts Center](https://account.windowsazure.com/subscriptions) zugegriffen wird, ist der Kontoadministrator (Account Administrator, AA). Abonnements dienen nicht nur als Container für die Abrechnung, sondern auch als Sicherheitsgrenze: Jedes Abonnement verfügt über einen Dienstadministrator (SA), der Azure-Ressourcen für dieses Abonnement mit dem [klassischen Azure-Portal](https://manage.windowsazure.com/) hinzufügen, entfernen und ändern kann. Der standardmäßige Dienstadministrator eines neuen Abonnements ist der Kontoadministrator. Der Kontoadministrator kann den Dienstadministrator jedoch über das Azure Accounts Center ändern.

<br><br>![Azure-Konten][1]

Außerdem sind Abonnements einem Verzeichnis zugeordnet. Durch das Verzeichnis wird eine Gruppe von Benutzern definiert. Diese können Benutzer aus der Organisation (Unternehmen oder Schule) sein, die das Verzeichnis erstellt hat, oder externe Benutzer (also Microsoft-Konten). Auf Abonnements kann über eine Teilmenge dieser Verzeichnisbenutzer zugegriffen werden, die entweder als Dienstadministrator (SA) oder Co-Administrator (CA) zugewiesen wurden. Die einzige Ausnahme besteht darin, dass Microsoft-Konten (früher Windows Live ID) – aus rechtlichen Gründen – als SA oder CA zugewiesen werden können, ohne im Verzeichnis vorhanden zu sein.

<br><br>![Zugriffssteuerung in Azure][2]

Mit Funktionen im klassischen Azure-Portal können SAs, die mit einem Microsoft-Konto angemeldet sind, das einem Abonnement zugeordnete Verzeichnis ändern. Hierfür wird der Befehl **Verzeichnis bearbeiten** auf der Seite **Abonnements** unter **Einstellungen** verwendet. Beachten Sie, dass sich dieser Vorgang auf die Zugriffssteuerung dieses Abonnements auswirkt.

> [!NOTE]
> Der Befehl **Verzeichnis bearbeiten** im klassischen Azure-Portal steht nicht für Benutzer zur Verfügung, die über ein Geschäfts- oder Schulkonto angemeldet sind, weil sich diese Konten nur an dem Verzeichnis anmelden können, zu dem sie gehören.
> 
> 

<br><br>![Einfache Benutzeranmeldung − Ablauf][3]

Im einfachen Fall erzwingt eine Organisation (etwa Contoso), dass die Abrechnung und die Zugriffssteuerung für dieselbe Gruppe von Abonnements erfolgen. Dies bedeutet, dass das Verzeichnis Abonnements zugeordnet ist, die sich im Besitz eines einzigen Azure-Kontos befinden. Nach der erfolgreichen Anmeldung beim klassischen Azure-Portal sehen Benutzer zwei Ressourcenauflistungen (in der Abbildung oben orangefarben dargestellt):

* Verzeichnisse, in denen ihr Benutzerkonto enthalten ist (aus dem Verzeichnis selbst oder als Fremdprinzipal hinzugefügt). Da das zum Anmelden verwendete Verzeichnis für diese Berechnung nicht relevant ist, werden Ihre Verzeichnisse immer angezeigt, unabhängig davon, wo Sie sich angemeldet haben.
* Ressourcen als Teil von Abonnements, die dem für die Anmeldung verwendeten Verzeichnis zugeordnet sind UND auf das der Benutzer (als SA oder CA) zugreifen kann.

<br><br>![Benutzer mit mehreren Abonnements und Verzeichnissen][4]

Benutzer mit Abonnements, die mehrere Verzeichnisse umfassen, können den aktuellen Kontext des klassischen Azure-Portals mithilfe des Abonnementfilters wechseln. Im Hintergrund führt dies zu einer getrennten Anmeldung bei einem anderen Verzeichnis, allerdings ist durch einmaliges Anmelden (SSO) eine nahtlose Anmeldung gewährleistet.

Vorgänge wie das Verschieben von Ressourcen zwischen Abonnements können sich aufgrund dieser Einzelverzeichnisansicht von Abonnements schwieriger gestalten. Um die Ressourcen zu übertragen, müssen Sie die Abonnements u.U. zuerst mit dem Befehl **Verzeichnis bearbeiten** unter **Einstellungen** auf der Seite „Abonnements“ demselben Verzeichnis zuordnen.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Ändern von Administratoren für ein Azure-Abonnement finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen](../billing/billing-add-change-azure-subscription-administrator.md)
* Weitere Informationen zur Beziehung zwischen Azure Active Directory und Ihrem Azure-Abonnement finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* Informationen zum Zuweisen von Rollen in Azure AD finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png

