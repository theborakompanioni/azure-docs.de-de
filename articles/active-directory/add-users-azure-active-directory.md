---
title: "Hinzufügen neuer Benutzer in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie neue Benutzer in Azure Active Directory hinzugefügt werden."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 13a7d2d3b991206c45e66872b590bc27a224eead
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="quickstart-add-new-users-to-azure-active-directory"></a>Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory
In diesem Artikel wird erläutert, wie neue Benutzer in Ihrer Organisation über das Azure-Portal oder durch Synchronisierung der lokalen Windows Server AD-Benutzerkontodaten nacheinander in Azure Active Directory (Azure AD) hinzugefügt werden. 

## <a name="add-cloud-based-users"></a>Hinzufügen von cloudbasierten Benutzern
1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Azure Active Directory** und dann **Benutzer und Gruppen** aus.
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Alle Benutzer** und dann **Neuer Benutzer** aus.
   ![Auswählen des Befehls „Hinzufügen“](./media/add-users-azure-active-directory/add-user.png)
4. Geben Sie Details zum Benutzer ein, beispielsweise **Name** und **Benutzername**. Der Domänennamensteil des Benutzernamens muss entweder der anfängliche Standarddomänenname [Domänenname].onmicrosoft.com oder ein überprüfter, nicht im Verbund konfigurierter [benutzerdefinierter Domänenname](add-custom-domain.md) wie z.B. contoso.com sein.
5. Kopieren oder notieren Sie sich das generierte Benutzerkennwort, damit sie es nach Abschluss des Vorgangs dem Benutzer mitteilen können.
6. Optional können Sie die Informationen auf dem Blatt **Profil**, dem Blatt **Gruppen** oder dem Blatt **Verzeichnisrolle** für den Benutzer ausfüllen. Weitere Informationen zu Benutzer- und Administratorrollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md).
7. Wählen Sie auf dem Blatt **Benutzer** die Option **Erstellen** aus.
8. Sorgen Sie für eine sichere Übermittlung des generierten Kennworts an den neuen Benutzer, damit sich dieser anmelden kann.

> [!TIP]
> Sie können auch Benutzerkontodaten über das lokale Windows Server AD synchronisieren. Die Identitätslösungen von Microsoft decken sowohl lokale als auch cloudbasierte Funktionen ab, und es wird eine einzelne Benutzeridentität für die standortunabhängige Authentifizierung und Autorisierung gegenüber allen Ressourcen erstellt. Wir bezeichnen dies als Hybrid-Identität. Mithilfe von [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) können Ihre lokalen Verzeichnisse bei Szenarien mit Hybrididentitäten in Azure Active Directory integriert werden. Dadurch können Sie für Ihre Benutzer eine einzige Identität für in Azure AD integrierte in Office 365-, Azure- und SaaS-Anwendungen bereitstellen. 

## <a name="delete-users-from-azure-ad"></a>Löschen von Benutzern aus Azure AD
1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Benutzer und Gruppen**.
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** den zu löschenden Benutzer in der Liste aus. 
4. Wählen Sie auf dem Blatt für den ausgewählten Benutzer die Option **Übersicht** aus, und klicken Sie auf der Befehlsleiste auf **Löschen**.
   ![Auswählen des Befehls „Hinzufügen“](./media/add-users-azure-active-directory/delete-user.png)


### <a name="learn-more"></a>Weitere Informationen 
* [Hinzufügen eines externen Benutzers](active-directory-users-create-external-azure-portal.md)

* [Zuweisen eines Benutzers zu einer Rolle in Azure AD](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie gelernt, wie Sie neue Benutzer in Azure AD Premium hinzufügen. 

Über den folgenden Link können Sie im Azure-Portal einen neuen Benutzer in Azure AD erstellen.

> [!div class="nextstepaction"]
> [Hinzufügen von Benutzern zu Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 
