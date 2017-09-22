---
title: "Nächste Schritte für die Zugriffsverwaltung mithilfe von Gruppen | Microsoft-Dokumentation"
description: Erweiterte Vorgehensweisen zum Verwalten von Sicherheitsgruppen und Verwenden dieser Gruppen zum Verwalten des Zugriffs auf eine Ressource.
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
ms.date: 09/12/2017
ms.author: curtand
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: e1b88f7d43ba29589e5f1f4a8036c716ae984ae4
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="managing-owners-for-a-group"></a>Verwalten von Besitzern einer Gruppe
Wenn ein Ressourcenbesitzer einer Ressource den Zugriff auf eine Azure AD-Gruppe zugewiesen hat, wird die Mitgliedschaft der Gruppe durch den Besitzer der Gruppe verwaltet. Tatsächlich delegiert der Besitzer der Ressource die Berechtigung, dieser Ressource Benutzer zuzuweisen, an den Besitzer der Gruppe.

## <a name="add-an-owner-to-a-group"></a>Hinzufügen eines Besitzers zu einer Gruppe

1. Wählen Sie im [Azure AD Admin Center](https://aad.portal.azure.com) die Option **Benutzer und Gruppen**.
2. Wählen Sie **Alle Gruppen** aus, und öffnen Sie dann die Gruppe, der Besitzer hinzugefügt werden sollen.
3. Wählen Sie **Besitzer hinzufügen**aus.
4. Wählen Sie auf der Seite **Besitzer hinzufügen** den Benutzer aus, den Sie als Besitzer dieser Gruppe hinzufügen möchten, und stellen Sie sicher, dass dieser Name dem Bereich **Ausgewählt** hinzugefügt wird.

## <a name="remove-an-owner-from-a-group"></a>Entfernen eines Besitzers aus einer Gruppe

1. Wählen Sie im [Azure AD Admin Center](https://aad.portal.azure.com) die Option **Benutzer und Gruppen**.
2. Wählen Sie **Alle Gruppen**, und öffnen Sie dann die Gruppe, aus der Sie Besitzer entfernen möchten.
3. Wählen Sie die Registerkarte **Besitzer** aus.
4. Wählen Sie den Besitzer aus, der aus der Gruppe entfernt werden soll, und wählen Sie dann **Entfernen**aus.

## <a name="additional-information"></a>Zusätzliche Informationen
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

