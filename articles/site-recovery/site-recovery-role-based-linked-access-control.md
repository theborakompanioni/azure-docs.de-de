---
title: Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie die rollenbasierte Zugriffssteuerung (RBAC) zum Verwalten von Azure Site Recovery-Bereitstellungen an- und verwendet wird.
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: manayar
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 08a4d0fa673a37c61e57daed66ab6768e0276ca8
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="use-role-based-access-control-to-manage-azure-site-recovery-deployments"></a>Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Site Recovery-Bereitstellungen

Die rollenbasierte Access Control in Azure (RBAC) ermöglicht eine präzise Zugriffsverwaltung für Azure. Mithilfe der rollenbasierten Zugriffssteuerung können Sie Zuständigkeiten in Ihrem Team aufteilen und Benutzern nur die jeweils spezifischen Zugriffsrechte zum Ausführen bestimmter Aufträge zuweisen.

Azure umfasst verschiedene integrierte Rollen zum Steuern von Vorgängen zur Ressourcenverwaltung. Erfahren Sie mehr über [integrierte Rollen von Azure RBAC](../active-directory/role-based-access-built-in-roles.md).

Wenn Sie Ihre eigenen Rollen definieren möchten, um eine noch bessere Kontrolle zu haben, helfen Ihnen die Informationen zum [Erstellen von benutzerdefinierten Rollen](../active-directory/role-based-access-control-custom-roles.md) in Azure weiter.

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Erforderliche Berechtigungen zum Aktivieren der Replikation für neue virtuelle Computer
Wenn ein neuer virtueller Computer in Azure mithilfe von Azure Site Recovery repliziert wird, werden die Zugriffsebenen des zugeordneten Benutzers überprüft, um sicherzustellen, dass der Benutzer über die erforderlichen Berechtigungen zum Verwenden der in Site Recovery bereitgestellten Azure-Ressourcen verfügt.

Zum Aktivieren der Replikation für einen neuen virtuellen Computer muss ein Benutzer über folgende Berechtigungen verfügen:
* Berechtigung zum Erstellen eines virtuellen Computers in der ausgewählten Ressourcengruppe
* Berechtigung zum Erstellen eines virtuellen Computers im ausgewählten virtuellen Netzwerk
* Berechtigung zum Schreiben in das ausgewählte Speicherkonto

Ein Benutzer benötigt die folgenden Berechtigungen, um die Replikation eines neuen virtuellen Computers abschließen zu können.

> [!IMPORTANT]
>Stellen Sie sicher, dass die relevanten Berechtigungen entsprechend dem für die Ressourcenbereitstellung verwendeten Bereitstellungsmodell (Resource Manager-Modell oder klassisches Modell) hinzugefügt werden.

| **Ressourcentyp** | **Bereitstellungsmodell** | **Berechtigung** |
| --- | --- | --- |
| Compute | Ressourcen-Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Klassisch | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Netzwerk | Ressourcen-Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Klassisch | Microsoft.ClassicNetwork/virtualNetworks/read |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Speicher | Ressourcen-Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Klassisch | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Ressourcengruppe | Ressourcen-Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

Sie können auch jeweils die [integrierte Rolle](../active-directory/role-based-access-built-in-roles.md) „Mitwirkender für virtuelle Computer“ oder „Mitwirkender für klassische virtuelle Computer“ für das Resource Manager- bzw. das klassische Bereitstellungsmodell verwenden.

## <a name="next-steps"></a>Nächste Schritte
* [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md): Erste Schritte mit RBAC im Azure-Portal.
* Informationen zur Zugriffsverwaltung mit:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure-Befehlszeilenschnittstelle](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST-API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Problembehandlung bei rollenbasierter Zugriffssteuerung:](../active-directory/role-based-access-control-troubleshooting.md)Sehen Sie sich Vorschläge zur Behebung häufig auftretender Probleme an.

