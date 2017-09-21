---
title: Verwalten von Gruppenbesitzern in Azure Active Directory | Microsoft-Dokumentation
description: Verwalten von Gruppenbesitzern und Verwenden dieser Gruppen zum Verwalten des Zugriffs auf eine Ressource.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 44350a3c-8ea1-4da1-aaac-7fc53933dd21
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2017
ms.author: curtand
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 70be82fdd673c4f245e306b1e1cfdcd94d4dac53
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="managing-owners-for-a-group"></a>Verwalten von Besitzern einer Gruppe
Nachdem ein Ressourcenbesitzer einer Azure AD-Gruppe den Zugriff auf eine Ressource zugewiesen hat, wird die Mitgliedschaft der Gruppe durch den Besitzer der Gruppe verwaltet. Tatsächlich delegiert der Besitzer der Ressource die Berechtigung, dieser Ressource Benutzer zuzuweisen, an den Besitzer der Gruppe.

## <a name="add-an-owner-to-a-group"></a>Hinzufügen eines Besitzers zu einer Gruppe

1. Wählen Sie im [Azure AD Admin Center](https://aad.portal.azure.com) die Option **Benutzer und Gruppen**.
2. Wählen Sie **Alle Gruppen** aus, und öffnen Sie dann die Gruppe, der Besitzer hinzugefügt werden sollen.
3. Wählen Sie **Besitzer** und dann **Besitzer hinzufügen**.
4. Wählen Sie auf der Seite **Besitzer hinzufügen** den Benutzer aus, den Sie als Besitzer dieser Gruppe hinzufügen möchten. Klicken oder tippen Sie dann auf **Auswählen**. 

## <a name="remove-an-owner-from-a-group"></a>Entfernen eines Besitzers aus einer Gruppe

1. Wählen Sie im [Azure AD Admin Center](https://aad.portal.azure.com) die Option **Benutzer und Gruppen**.
2. Wählen Sie **Alle Gruppen**, und öffnen Sie dann die Gruppe, aus der Sie Besitzer entfernen möchten.
3. Wählen Sie **Besitzer**, und wählen Sie den Besitzer aus, den Sie aus dieser Gruppe entfernen möchten. Klicken oder tippen Sie dann auf **Auswählen**.
4. Wählen Sie im geöffneten Bereich für den ausgewählten Besitzer **Entfernen**.

## <a name="additional-information"></a>Zusätzliche Informationen
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory-Gruppen.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)

