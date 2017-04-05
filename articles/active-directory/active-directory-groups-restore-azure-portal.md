---
title: "Wiederherstellen einer gelöschten Office 365-Gruppe in der Azure Active Directory-Vorschau | Microsoft-Dokumentation"
description: "Vorgehensweise beim Wiederherstellen einer gelöschten Gruppe, Anzeigen von wiederherstellbaren Gruppen und dauerhaften Löschen einer Gruppe in Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: cc177695f73805940f6a0229c94e391bf3899f09
ms.lasthandoff: 03/29/2017


---

# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Wiederherstellen einer gelöschten Office 365-Gruppe in der Azure Active Directory-Vorschau

Wenn Sie eine Office 365-Gruppe in der Vorschau von Azure Active Directory (Azure AD) löschen, wird die gelöschte Gruppe beibehalten, ist aber 30 Tage nach dem Löschdatum nicht sichtbar. Damit soll sichergestellt werden, dass die Gruppe und dessen Inhalte bei Bedarf wiederhergestellt werden können. Diese Funktionalität ist ausschließlich auf Office 365-Gruppen in Azure AD beschränkt. Sie ist nicht für Sicherheits- und Verteilergruppen verfügbar.

Die Berechtigungen, die zum Wiederherstellen einer Gruppe erforderlich sind, können Folgende umfassen:

Rolle  | Berechtigungen 
--------- | ---------
Unternehmensadministrator, Partnersupport der Ebene 2 und Intune-Dienstadministratoren | Können eine beliebige gelöschte Office 365-Gruppe wiederherstellen 
Benutzerkontoadministrator und Partnersupport der Ebene 1 | Können eine beliebige gelöschte Office 365-Gruppe wiederherstellen, außer Gruppen, die der Rolle „Unternehmensadministrator“ zugewiesen sind 
Benutzer | Können eine beliebige gelöschte Office 365-Gruppe wiederherstellen, die sich in dessen Besitz befand 


## <a name="how-to-view-deleted-office-365-groups-that-are-available-to-restore"></a>Vorgehensweise beim Anzeigen gelöschter Office 365-Gruppen, die zur Wiederherstellung zur Verfügung stehen
Mit den folgenden Cmdlets können gelöschte Gruppen angezeigt werden, um sicherzustellen, dass die gewünschten Gruppen noch nicht endgültig gelöscht wurden. Diese Cmdlets gehören zum [Azure Active Directory PowerShell V2-Vorschaumodul](https://www.powershellgallery.com/packages/AzureADPreview) (Download-Link). Weitere Informationen zu diesem Modul finden Sie im Artikel [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/).

1.    Führen Sie das folgende Cmdlet aus, um alle gelöschten Office 365-Gruppen in Ihrem Mandanten, die noch zur Wiederherstellung verfügbar sind, anzuzeigen.
  ```
  Get-AzureADMSDeletedGroup
  ```

2.    Wenn Sie die ObjectID einer bestimmten Gruppe kennen (und Sie diese über das Cmdlet in Schritt 1 abrufen können), führen Sie alternativ das folgende Cmdlet aus, um sicherzustellen, dass die jeweilige gelöschte Gruppe noch nicht endgültig gelöscht wurde.
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-to-restore-an-office-365-group"></a>Vorgehensweise beim Wiederherstellen einer Office 365-Gruppe
Nachdem Sie sichergestellt haben, dass die Gruppe noch zur Wiederherstellung verfügbar ist, stellen Sie die gelöschte Gruppe über eine der folgenden Methoden wieder her. Wenn die Gruppe Dokumente, SharePoint-Websites oder andere beständige Objekte enthält, kann es bis zu 24 Stunden dauern, bis eine Gruppe und dessen Inhalte vollständig wiederhergestellt werden.

1.    Führen Sie das folgende Cmdlet aus, um die Gruppe und dessen Inhalte wiederherzustellen.
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

Alternativ können Sie das folgende Cmdlet ausführen, um die gelöschte Gruppe dauerhaft zu löschen.
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a>Wie finde ich heraus, ob es funktioniert hat?
Um sicherzustellen, dass Sie eine Office 365-Gruppe erfolgreich wiederhergestellt haben, führen Sie das Cmdlet `Get-AzureADGroup –ObjectId <objectId>` aus, um Informationen zur Gruppe anzuzeigen. Nach Durchführung der Wiederherstellungsanforderung:
- Die Gruppe wird in der linken Navigationsleiste in Exchange angezeigt.
- Der Plan für die Gruppe wird in Planner angezeigt.
- Alle SharePoint-Websites und deren Inhalte sind verfügbar.
- Die Gruppe kann über einen der Exchange-Endpunkte und andere Office 365-Workloads, die Office 365-Gruppen unterstützen, aufgerufen werden.


## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory-Gruppen.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)
