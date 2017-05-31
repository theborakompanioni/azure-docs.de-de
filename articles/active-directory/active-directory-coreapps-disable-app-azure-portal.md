---
title: "Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App in Azure Active Directory | Microsoft-Dokumentation"
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
ms.date: 05/04/2017
ms.author: curtand
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 5d27046370eada0c371c94fb573fa1bcf536f7cf
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App in Azure Active Directory
In Azure Active Directory (Azure AD) können Sie auf einfache Weise eine Unternehmens-App deaktivieren, damit sich Benutzer nicht mehr bei der App anmelden können. Sie benötigen die entsprechenden Berechtigungen zum Verwalten der Unternehmens-App, und Sie müssen globaler Administrator für das Verzeichnis sein.

## <a name="how-do-i-disable-user-sign-ins"></a>Wie deaktiviere ich Benutzeranmeldungen?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie auf dem Blatt **Azure Active Directory** -  ***Verzeichnisname*** (d.h. dem Azure AD-Blatt für das Verzeichnis, das Sie verwalten) **Unternehmensanwendungen** aus.

    ![Öffnen von Unternehmens-Apps](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. Wählen Sie auf dem Blatt **Unternehmensanwendungen** die Einstellung **Alle Anwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.
5. Wählen Sie auf dem Blatt **Unternehmensanwendungen – Alle Anwendungen** eine App aus.
6. Wählen Sie auf dem Blatt ***App-Name*** (dem Blatt mit dem Namen der ausgewählten App im Titel) die Einstellung **Eigenschaften** aus.

    ![Auswählen des Befehls „Alle Anwendungen“](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. Wählen Sie auf dem Blatt ***App-Name*** - **Eigenschaften** für die Option **Aktiviert für die Benutzeranmeldung?** die Einstellung **Nein** aus.
8. Klicken Sie auf **Speichern** .

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](active-directory-groups-view-azure-portal.md)
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Ändern des Namens oder Logos einer Unternehmens-App](active-directory-coreapps-change-app-logo-user-azure-portal.md)

