---
title: "Erstellen von Zugriffsberichten – Azure RBAC | Microsoft Docs"
description: "Erstellen Sie einen Bericht, in dem alle Änderungen am Zugriff auf Ihre Azure-Abonnements mit rollenbasierter Zugriffssteuerung von Azure in den letzten 90 Tagen aufgeführt sind."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: kgremban
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 43ddeebfea4c914b8377d3363ba3d0c12db0adca
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="create-an-access-report-for-role-based-access-control"></a>Erstellen eines Zugriffsberichts für die rollenbasierte Zugriffssteuerung
Jedes Mal, wenn ein Benutzer in Ihren Abonnement Zugriff gewährt bzw. widerruft, werden die Änderungen in Azure-Ereignissen protokolliert. Sie können Verlaufsberichte zu Zugriffsänderungen erstellen, um alle Änderungen der letzten 90 Tage anzuzeigen.

## <a name="create-a-report-with-azure-powershell"></a>Erstellen eines Berichts mit Azure PowerShell
Verwenden Sie zum Erstellen eines Verlaufsberichts zu Zugriffsänderungen in PowerShell den Befehl [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog).

Wenn Sie diesen Befehl aufrufen, können Sie angeben, welche Eigenschaft der Zuweisungen aufgeführt werden soll, u.a.:

| Eigenschaft | Beschreibung |
| --- | --- |
| **Aktion** |Gibt an, ob der Zugriff erteilt oder widerrufen wurde. |
| **Caller** |Der für die Zugriffsänderung verantwortliche Besitzer |
| **PrincipalId** | Der eindeutige Bezeichner des Benutzers, der Gruppe oder der Anwendung, dem bzw. der die Rolle zugewiesen wurde |
| **PrincipalName** |Der Name des Benutzers, der Gruppe oder der Anwendung |
| **PrincipalType** |Gibt an, ob die Zuordnung für einen Benutzer, eine Gruppe oder eine Anwendung erfolgt ist. |
| **RoleDefinitionId** |Die GUID der Rolle, die erteilt oder widerrufen wurde |
| **RoleName** |Die Rolle, die erteilt oder widerrufen wurde |
| **Umfang** | Der eindeutige Bezeichner des Abonnements, der Ressourcengruppe oder der Ressource, für das bzw. die die Zuordnung gilt | 
| **ScopeName** |Der Name des Abonnements, der Ressourcengruppe oder der Ressource |
| **ScopeType** |Gibt an, ob die Zuweisung im Abonnement-, Ressourcengruppen- oder Ressourcenbereich erfolgt ist. |
| **Timestamp** |Das Datum und die Uhrzeit, an dem bzw. zu der der Zugriff geändert wurde |

Mit dem folgenden Beispielbefehl werden alle Zugriffsänderungen im Abonnement für die letzten sieben Tage aufgeführt:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog – Screenshot](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Erstellen eines Berichts über die Azure-Befehlszeilenschnittstelle
Verwenden Sie zum Erstellen eines Verlaufsberichts zu Zugriffsänderungen in der Azure-Befehlszeilenschnittstelle den `azure role assignment changelog list` -Befehl.

## <a name="export-to-a-spreadsheet"></a>Exportieren in eine Kalkulationstabelle
Wenn Sie den Bericht speichern oder die Daten bearbeiten möchten, exportieren Sie die Zugriffsänderungen in eine CSV-Datei. Sie können dann den Bericht in einer Kalkulationstabelle zur Überprüfung anzeigen.

![Änderungsprotokoll als Arbeitsblatt – Screenshot](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a>Nächste Schritte
* Arbeiten mit [benutzerdefinierten Rollen in Azure RBAC](role-based-access-control-custom-roles.md)
* Erfahren Sie, wie Sie [Azure RBAC mit PowerShell](role-based-access-control-manage-access-powershell.md) verwalten.


