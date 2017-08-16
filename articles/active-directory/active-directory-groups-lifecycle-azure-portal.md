---
title: Vorschau zum Ablaufen von Office 365-Gruppen in Azure Active Directory | Microsoft-Dokumentation
description: "Einrichten des Ablaufs für Office 365-Gruppen in Azure Active Directory (Vorschau)"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: f4efac7b74620e68e727292e02a5a904b03d6e2f
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---

# <a name="configure-office-365-groups-expiration-preview"></a>Konfigurieren des Ablaufs von Office 365-Gruppen (Vorschau)

Sie können jetzt den Lebenszyklus von Office 365-Gruppen durch Festlegen des Ablaufs für beliebige Office 365-Gruppen verwalten, die Sie auswählen. Sobald dieser Ablauf festgelegt ist, werden Besitzer dieser Gruppen aufgefordert, ihre Gruppen zu verlängern, wenn sie die Gruppen noch benötigen. Eine Office 365-Gruppe, die nicht verlängert wird, wird gelöscht. Eine Office 365-Gruppe, die gelöscht wurde, kann innerhalb von 30 Tagen von den Gruppenbesitzern oder vom Administrator wiederhergestellt werden.  


> [!NOTE]
> Sie können die Ablaufzeit nur für Office 365-Gruppen festlegen.
>
> Die Einstellung des Ablaufs für O365-Gruppen erfordert, dass folgenden Benutzern eine Azure AD Premium-Lizenz zugewiesen wurde:
>   - Dem Administrator, der die Ablaufeinstellungen für den Mandanten konfiguriert
>   - Allen Mitglieder der für diese Einstellung ausgewählten Gruppen

## <a name="set-office-365-groups-expiration"></a>Einrichten des Office 365-Gruppenablaufs

1. Öffnen Sie das [Azure AD Admin Center](https://aad.portal.azure.com) mit dem Konto eines globalen Administrators in Ihrem Azure AD-Mandanten.

2. Öffnen Sie Azure AD, und wählen Sie **Benutzer und Gruppen**.

3. Wählen Sie **Gruppeneinstellungen**, und wählen Sie dann **Ablauf**, um die Ablaufeinstellungen zu öffnen.
  
  ![Blatt „Ablauf“](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Auf dem Blatt **Ablauf** können Sie:

  * Die Gruppenlebensdauer in Tagen festlegen. Sie könnten einen der vordefinierten Werte oder einen benutzerdefinierten Wert wählen (mindestens 31 Tage). 
  * Geben Sie eine E-Mail-Adresse an, an die die Verlängerungs- und Ablaufbenachrichtigungen gesendet werden sollten, wenn eine Gruppe keinen Besitzer hat. 
  * Wählen Sie, welche Office 365-Gruppen ablaufen. Sie können den Ablauf für **Alle** Office 365-Gruppen aktivieren, Sie können eine Auswahl aus den Office 365-Gruppen treffen, oder Sie wählen **Keine**, um den Ablauf für alle Gruppen zu deaktivieren.
  * Speichern Sie die Einstellungen durch Auswahl von **Speichern**.

Anweisungen zum Herunterladen und Installieren des Microsoft PowerShell-Moduls zum Konfigurieren des Ablaufs für Office 365-Gruppen über PowerShell finden Sie unter [Azure Active Directory V2 PowerShell Module – Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) (Azure Active Directory V2 PowerShell-Modul – öffentliche Preview-Version 2.0.0.137).

E-Mail-Benachrichtigungen, wie diese werden an die Office 365-Gruppenbesitzer 30 Tage, 15 Tage und 1 Tag vor Ablauf der Gruppe gesendet.

![E-Mail-Benachrichtigung zum Ablauf](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Der Besitzer der Gruppe kann dann **Gruppe verlängern** auswählen, um die Office 365-Gruppe zu verlängern, oder **Zur Gruppe wechseln**, um die Mitglieder und andere Details der Gruppe anzuzeigen.

Wenn eine Gruppe abläuft, wird die Gruppe einen Tag nach dem Ablaufdatum gelöscht. Eine E-Mail-Benachrichtigung wie diese wird an die Office 365-Gruppenbesitzer gesendet, um sie über den Ablauf und die nachfolgende Löschung ihrer Office 365-Gruppe zu informieren.

![E-Mail-Benachrichtigung zur Gruppenlöschung](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Die Gruppe kann durch Auswahl von **Gruppe wiederherstellen** oder mithilfe von PowerShell-Cmdlets wiederhergestellt werden, wie beschrieben in [Wiederherstellen einer gelöschten Office 365-Gruppe in der Azure Active Directory-Vorschau] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).
    
Wenn die Gruppe, die Sie wiederherstellen, Dokumente, SharePoint-Websites oder andere beständige Objekte enthält, kann es bis zu 24 Stunden dauern, bis die Gruppe und deren Inhalte vollständig wiederhergestellt werden.

> [!NOTE]
> * Bei der Bereitstellung von Ablaufeinstellungen gibt es möglicherweise Gruppen, die älter sind als das Ablauffenster. Diese Gruppen werden nicht sofort gelöscht, jedoch auf 30 Tage bis zum Ablaufdatum festgelegt. Die erste Verlängerungsbenachrichtigung wird innerhalb eines Tages gesendet. Beispielsweise wurde Gruppe A vor 400 Tagen erstellt, und das Ablaufintervall ist auf 180 Tage festgelegt. Wenn Sie Ablaufeinstellungen anwenden, hat Gruppe A 30 Tage, bevor sie gelöscht wird, es sei denn, der Besitzer verlängert sie.
> * Wenn eine dynamische Gruppe gelöscht und wiederhergestellt wird, gilt sie als neue Gruppe und wird der Regel entsprechend erneut aufgefüllt. Dieser Vorgang kann bis zu 24 Stunden dauern.

## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Azure AD-Gruppen.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)

