---
title: "Ändern des Namens oder Logos einer Unternehmens-App in Azure Active Directory | Microsoft-Dokumentation"
description: "Ändern des Namens oder des Logos einer Unternehmens-App in der Azure Active Directory-Vorschau"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 3e44e876dcbac704a9809ae5b3957bf94be21c48
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Ändern des Namens oder Logos einer Unternehmens-App in Azure Active Directory
In Azure Active Directory (Azure AD) können Sie auf einfache Weise den Namen oder das Logo für eine benutzerdefinierte Unternehmensanwendung ändern. Sie benötigen die erforderlichen Berechtigungen, um diese Änderungen vorzunehmen, und Sie müssen der Ersteller der benutzerdefinierten App sein.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Wie ändere ich den Namen oder das Logo einer Unternehmens-App?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie auf dem Blatt **Azure Active Directory – *Verzeichnisname*** (d.h. dem Azure AD-Blatt für das Verzeichnis, das Sie verwalten) **Unternehmensanwendungen** aus.

    ![Öffnen von Unternehmens-Apps](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. Wählen Sie auf dem Blatt **Unternehmensanwendungen** die Einstellung **Alle Anwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.
5. Wählen Sie auf dem Blatt **Unternehmensanwendungen – Alle Anwendungen** eine App aus.
6. Wählen Sie auf dem Blatt ***App-Name*** (dem Blatt mit dem Namen der ausgewählten App im Titel) die Einstellung **Eigenschaften** aus.

    ![Auswählen des Befehls „Eigenschaften“](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. Suchen Sie auf dem Blatt ***App-Name*** **-Eigenschaften** nach einer Datei, die Sie als neues Logo verwenden möchten, oder bearbeiten Sie den App-Namen (oder beides).

    ![Befehl zum Ändern des App-Logos oder des Namens](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. Klicken Sie auf **Speichern** .

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](active-directory-groups-view-azure-portal.md)
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App](active-directory-coreapps-disable-app-azure-portal.md)

