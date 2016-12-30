---
title: "Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App in der Azure Active Directory-Vorschau | Microsoft Docs"
description: "Deaktivieren einer Unternehmens-App in Azure Active Directory, damit sich Benutzer nicht mehr bei der App anmelden können"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 33d3d50980341e0810b1222ce1199fa8689d95d0


---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App in der Azure Active Directory-Vorschau
In der Azure Active Directory-Vorschau können Sie auf einfache Weise eine Unternehmens-App deaktivieren, damit sich Benutzer nicht mehr bei der App anmelden können. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md)  Sie müssen über die geeigneten Berechtigungen zum Verwalten der Unternehmens-App verfügen. In der aktuellen Vorschau müssen Sie als globaler Administrator für das Verzeichnis konfiguriert sein.

## <a name="how-do-i-disable-user-sign-ins"></a>Wie deaktiviere ich Benutzeranmeldungen?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie auf dem Blatt **Azure Active Directory – *Verzeichnisname*** (d.h. dem Azure AD-Blatt für das Verzeichnis, das Sie verwalten) **Unternehmensanwendungen** aus.
   
    ![Öffnen von Unternehmens-Apps](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. Wählen Sie auf dem Blatt **Unternehmensanwendungen** die Einstellung **Alle Anwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.
5. Wählen Sie auf dem Blatt **Unternehmensanwendungen – Alle Anwendungen** eine App aus.
6. Wählen Sie auf dem Blatt ***App-Name*** (dem Blatt mit dem Namen der ausgewählten App im Titel) die Einstellung **Eigenschaften** aus.
   
    ![Auswählen des Befehls „Alle Anwendungen“](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. Wählen Sie auf dem Blatt ***App-Name*** **– Eigenschaften** für die Option **Aktiviert für die Benutzeranmeldung?** die Einstellung **Nein** aus.
8. Klicken Sie auf **Speichern** .

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](active-directory-groups-view-azure-portal.md)
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Ändern des Namens oder Logos einer Unternehmens-App](active-directory-coreapps-change-app-logo-user-azure-portal.md)




<!--HONumber=Dec16_HO4-->


