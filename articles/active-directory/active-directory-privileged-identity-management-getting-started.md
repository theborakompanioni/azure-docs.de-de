---
title: Erste Schritte mit Azure AD Privileged Identity Management | Microsoft Docs
description: "Erfahren Sie, wie Sie mit der Anwendung Azure Active Directory Privileged Identity Management privilegierte Identitäten im Azure-Portal verwalten."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 17cdff033cc3dbb199d11c3b8ac1acbc92499877
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017

---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Erste Schritte mit Azure AD Privileged Identity Management
Mit Azure Active Directory (AD) Privileged Identity Management können Sie den Zugriff innerhalb Ihrer Organisation verwalten, steuern und überwachen. Dazu zählt der Zugriff auf Ressourcen in Azure AD und anderen Microsoft-Onlinediensten wie Office 365 oder Microsoft Intune.

Dieser Artikel beschreibt, wie Sie die Azure AD PIM-App zu Ihrem Azure-Portaldashboard hinzufügen.

## <a name="add-the-privileged-identity-management-application"></a>Hinzufügen der Anwendung Privileged Identity Management
Bevor Sie Azure AD Privileged Identity Management verwenden können, müssen Sie die Anwendung zum Dashboard in Ihrem Azure-Portal hinzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Verzeichnisses an.
2. Wenn Ihre Organisation über mehr als ein Verzeichnis verfügt, klicken Sie in der oberen rechten Ecke des Azure-Portals auf Ihren Benutzernamen. Wählen Sie das Verzeichnis aus, in dem Sie PIM verwenden möchten.
3. Wählen Sie **Weitere Dienste** aus, und verwenden Sie das Textfeld „Filter“, um nach **Azure AD Privileged Identity Management** zu suchen.
4. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.

Wenn Sie die erste Person sind, die Azure AD Privileged Identity Management in Ihrem Verzeichnis verwendet, werden Ihnen automatisch die Rollen **Sicherheitsadministrator** und **Administrator für privilegierte Rollen** im Verzeichnis zugewiesen. Nur Administratoren für privilegierte Rollen können Rollenzuweisungen von Benutzern verwalten. Darüber hinaus können Sie den [Sicherheits-Assistenten](active-directory-privileged-identity-management-security-wizard.md) ausführen. Dieser Assistent führt Sie durch die Erstermittlung und -zuweisung.

## <a name="navigate-to-your-tasks"></a>Navigieren zu Ihren Aufgaben
Nachdem Azure AD Privileged Identity Management eingerichtet wurde, wird beim Öffnen der Anwendung immer das Blatt für die Navigation angezeigt. Verwenden Sie dieses Blatt für Ihre Aufgaben der Identitätsverwaltung.

![Aufgaben der obersten Ebene für PIM – Screenshot](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

* Über **Meine Rollen** gelangen Sie zu einer Liste mit Rollen, die Ihnen zugewiesen sind. In diesem Abschnitt aktivieren Sie alle Rollen, zu deren Nutzung Sie berechtigt sind.
* **Anforderungen genehmigen** (Vorschau) zeigt eine Liste mit ausstehenden Aktivierungsanforderungen von Benutzern in Ihrem Verzeichnis. [Weitere Informationen.](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
* **Ausstehende Anforderungen** (Vorschau) zeigt alle aktuellen Anforderungen an, die noch aktiviert werden müssen.
* Über **Zugriff überprüfen** gelangen Sie zu ausstehenden Zugriffsüberprüfungen, die durchgeführt werden müssen. Dabei spielt es keine Rolle, ob Sie den Zugriff für sich selbst oder für eine andere Person überprüfen.
* Bei **Azure AD-Verzeichnisrollen** (unter dem Abschnitt „Verwalten“) handelt es sich um das Dashboard für Administratoren für privilegierte Rollen. Es dient unter anderem zum Verwalten von Rollenzuweisungen, Ändern der Einstellungen für die Rollenaktivierung und zum Starten von Zugriffsüberprüfungen. Die Optionen in diesem Dashboard sind für alle Benutzer deaktiviert, die keine privilegierten Rollenadministratoren sind.

## <a name="next-steps"></a>Nächste Schritte
Die Übersicht über [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) enthält weitere Details zur Verwaltung des administrativen Zugriffs in Ihrer Organisation.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

