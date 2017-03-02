---
title: "Erstellen von Zugriffsberichten – Azure RBAC | Microsoft Docs"
description: "Erstellen Sie einen Bericht, in dem alle Änderungen am Zugriff auf Ihre Azure-Abonnements mit rollenbasierter Zugriffssteuerung von Azure in den letzten 90 Tagen aufgeführt sind."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: bb894c38de63d0eac4066eeabaed7ba791021cc4
ms.openlocfilehash: bb15538a37fd610207c77fe2cf89fbfa16bbba11
ms.lasthandoff: 02/18/2017


---
# <a name="create-an-access-change-history-report"></a>Erstellen eines Verlaufsberichts zu Zugriffsänderungen
Jedes Mal, wenn ein Benutzer in Ihren Abonnement Zugriff gewährt bzw. widerruft, werden die Änderungen in Azure-Ereignissen protokolliert. Sie können Verlaufsberichte zu Zugriffsänderungen erstellen, um alle Änderungen der letzten 90 Tage anzuzeigen.

## <a name="create-a-report-with-azure-powershell"></a>Erstellen eines Berichts mit Azure PowerShell
Verwenden Sie zum Erstellen eines Verlaufsberichts zu Zugriffsänderungen in PowerShell den `Get-AzureRMAuthorizationChangeLog` -Befehl. Weitere Informationen zu diesem Cmdlet finden Sie im [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Wenn Sie diesen Befehl aufrufen, können Sie angeben, welche Eigenschaft der Zuweisungen aufgeführt werden soll, u.a.:

| Eigenschaft | Beschreibung |
| --- | --- |
| **Aktion** |Gibt an, ob der Zugriff erteilt oder widerrufen wurde. |
| **Caller** |Der für die Zugriffsänderung verantwortliche Besitzer |
| **Date** |Das Datum und die Uhrzeit, an dem bzw. zu der der Zugriff geändert wurde |
| **DirectoryName** |Das Azure Active Directory-Verzeichnis |
| **PrincipalName** |Der Name des Benutzers, der Gruppe oder der Anwendung |
| **PrincipalType** |Gibt an, ob die Zuordnung für einen Benutzer, eine Gruppe oder eine Anwendung erfolgt ist. |
| **RoleId** |Die GUID der Rolle, die erteilt oder widerrufen wurde |
| **RoleName** |Die Rolle, die erteilt oder widerrufen wurde |
| **ScopeName** |Der Name des Abonnements, der Ressourcengruppe oder der Ressource |
| **ScopeType** |Gibt an, ob die Zuweisung im Abonnement-, Ressourcengruppen- oder Ressourcenbereich erfolgt ist. |
| **SubscriptionId** |Die GUID des Azure-Abonnements |
| **SubscriptionName** |Der Name des Azure-Abonnements |

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


