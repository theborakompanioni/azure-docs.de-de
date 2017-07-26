---
title: Verwenden der rollenbasierten Zugriffssteuerung in Azure API Management | Microsoft-Dokumentation
description: Informationen zum Verwenden der integrierten Rollen und zum Erstellen benutzerdefinierter Rollen in Azure API Management
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: apimpm
ms.translationtype: Human Translation
ms.sourcegitcommit: e22bd56e0d111add6ab4c08b6cc6e51c364c7f22
ms.openlocfilehash: fa757a591d788f52d759bc24accedd3c55149ae7
ms.contentlocale: de-de
ms.lasthandoff: 05/19/2017


---

# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Verwenden der rollenbasierten Zugriffssteuerung in Azure API Management
Azure API Management basiert auf der rollenbasierten Zugriffssteuerung (RBAC) von Azure, um eine differenzierte Zugriffsverwaltung für API Management-Dienste und -Entitäten (z.B. APIs, Richtlinien) zu ermöglichen. Dieser Artikel bietet Ihnen einen Überblick über die integrierten und benutzerdefinierten Rollen in API Management. Weitere Informationen zur Zugriffsverwaltung im Azure-Portal finden Sie unter [Erste Schritte mit der Zugriffsverwaltung im Azure-Portal](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/).

## <a name="built-in-roles"></a>Integrierte Rollen
API Management bietet zurzeit drei integrierte Rollen, und es werden in naher Zukunft zwei weitere Rollen hinzugefügt. Diese Rollen können in verschiedenen Bereichen zugewiesen werden, darunter Abonnement, Ressourcengruppe und einzelne API Management-Instanz. Beispiel: Wenn einem Benutzer die Leserrolle für den API Management-Dienst auf Ressourcengruppenebene zugewiesen wird, besitzt der Benutzer innerhalb der Ressourcengruppe Lesezugriff für alle Instanzen von API Management. 

Die folgende Tabelle enthält kurze Beschreibungen der integrierten Rollen. Sie können diese Rollen über das Azure-Portal oder andere Tools zuweisen, beispielsweise Azure [PowerShell](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-powershell), die Azure-[Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-azure-cli) oder die [REST-API](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-rest). Informationen zum Zuweisen integrierter Rollen finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/).

| Rolle          | Lesezugriff<sup>[1]</sup> | Schreibzugriff<sup>[2]</sup> | Dienst erstellen, löschen, skalieren, Konfiguration von VPN und benutzerdefinierter Domäne | Zugriff auf das Legacy-Herausgeberportal | Beschreibung
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Mitwirkender des Azure API Management-Diensts | ✓ | ✓  | ✓  | ✓ | Administrator. Besitzt CRUD-Vollzugriff auf API Management-Dienste und -Entitäten (z.B. APIs, Richtlinien). Besitzt Zugriff auf das Legacy-Herausgeberportal. |
| Leser des Azure API Management-Diensts | ✓ | | || Besitzt schreibgeschützten Zugriff auf API Management-Dienste und -Entitäten. |
| Operator des Azure API Management-Diensts | ✓ | | ✓ | | Kann API Management-Dienste, jedoch keine Entitäten verwalten.|
| Editor des Azure API Management-Diensts<sup>*</sup> | ✓ | ✓ | |  | Kann API Management-Entitäten, jedoch keine Dienste verwalten.|
| Azure API Management-Inhalts-Manager<sup>*</sup> | ✓ | | | ✓ | Kann das Entwicklerportal verwalten. Schreibgeschützter Zugriff auf Dienste und Entitäten.|

<sup>[1] Lesezugriff auf API Management-Dienste und -Entitäten (z.B. APIs, Richtlinien)</sup>

<sup>[2] Schreibzugriff auf API Management-Dienste und -Entitäten mit Ausnahme der folgenden Vorgänge: 1) Erstellen, Löschen und Skalieren von Instanzen 2) VPN-Konfiguration 3) Einrichten benutzerdefinierter Domänennamen</sup>

<sup>\* Die Rolle des Dienst-Editors steht zur Verfügung, nachdem die gesamte Administrator-UI vom vorhandenen Herausgeberportal zum Azure-Portal migriert wurde. Die Inhalts-Manager-Rolle ist verfügbar, sobald das Herausgeberportal so umgestaltet wurde, dass darin nur noch Funktionen im Zusammenhang mit der Verwaltung des Entwicklerportals enthalten sind.</sup>  


## <a name="custom-roles"></a>Benutzerdefinierte Rollen
Wenn keine der integrierten Rollen Ihre Anforderungen erfüllt, können benutzerdefinierte Rollen erstellt werden, um eine detailliertere Zugriffsverwaltung für API Management-Entitäten bereitzustellen. Beispielsweise können Sie eine benutzerdefinierte Rolle erstellen, die schreibgeschützten Zugriff auf einen API Management-Dienst, aber nur Schreibzugriff für eine bestimmte API besitzt. Weitere Informationen zu benutzerdefinierten Rollen finden Sie unter [Benutzerdefinierte Rollen in Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles). 

Wenn Sie eine benutzerdefinierte Rolle erstellen, ist es einfacher, mit einer der integrierten Rollen zu beginnen. Bearbeiten Sie die Attribute, und fügen Sie Actions, NotActions oder AssignableScopes hinzu. Speichern Sie die Änderungen anschließend als neue Rolle. Im folgenden Beispiel wird von der Leserrolle für den Azure API Management-Dienst (API Management Service Reader Role) ausgegangen und eine benutzerdefinierte Rolle namens „Calculator API Editor“ erstellt. Die benutzerdefinierte Rolle kann nur einer bestimmten API zugewiesen werden und besitzt aus diesem Grund nur Zugriff auf diese API. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

## <a name="watch-a-video-overview"></a>Anzeigen einer Videoübersicht

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
> 
> 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure
  * [Erste Schritte mit der Zugriffsverwaltung im Azure-Portal](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Benutzerdefinierte Rollen in Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles)

