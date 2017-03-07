---
title: "Azure AD Domain Services: Erstellen der Administratorengruppe für Azure AD-Domänencontroller | Microsoft Docs"
description: "Erste Schritte mit Azure Active Directory-Domänendiensten"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f6160c560b951c1355bf956a392dda5b41ac9cd7
ms.lasthandoff: 02/11/2017


---
# <a name="get-started-with-azure-ad-domain-services"></a>Erste Schritte mit Azure AD Domain Services
Dieser Artikel erläutert die erforderlichen Konfigurationsaufgaben zum Aktivieren der Azure AD Domain Services für Ihren Azure AD-Mandanten.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Aufgabe 1: Erstellen der Administratorengruppe für Azure AD-Domänencontroller
Die erste Aufgabe besteht darin, eine administrative Gruppe in Ihrem Azure Active Directory-Mandanten zu erstellen. Diese spezielle administrative Gruppe heißt **AAD DC Administrators**. Mitgliedern dieser Gruppe werden Administratorrechte auf Computern erteilt, die in die verwaltete Azure Active Directory Domain Services-Domäne eingebunden sind. Auf in die Domäne eingebundenen Computern wird diese Gruppe der Gruppe „Administratoren“ hinzugefügt. Mitglieder dieser Gruppe können außerdem mithilfe von Remotedesktop Remoteverbindungen mit den in die Domäne eingebundenen Computern herstellen.  

> [!NOTE]
> Sie verfügen nicht über Domänenadministrator- oder Unternehmensadministratorberechtigungen in der verwalteten Domäne, die mithilfe der Azure AD Domain Services erstellt wurde. In verwalteten Domänen sind diese Berechtigungen durch den Dienst reserviert und stehen Benutzern innerhalb des Mandanten nicht zur Verfügung. Sie können jedoch die in dieser Konfigurationsaufgabe verwendete spezielle Administratorgruppe verwenden, um einige privilegierte Vorgänge auszuführen. Zu diesen Vorgängen gehört das Einbinden von Computern in die Domäne, die Mitgliedschaft in der Administratorgruppe auf der Domäne beigetretenen Computern, das Konfigurieren von Gruppenrichtlinien usw.
>
>

In dieser Konfigurationsaufgabe erstellen Sie die administrative Gruppe und fügen ihr mindestens einen Verzeichnisbenutzer hinzu. Führen Sie die folgenden Schritte durch, um die Administratorgruppe für die Azure Active Directory Domain Services zu erstellen:

1. Navigieren Sie zum **klassischen Azure-Portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Wählen Sie den Knoten **Active Directory** im linken Bereich aus.
3. Wählen Sie den Azure AD-Mandanten (Verzeichnis) aus, für den Sie die Azure AD Domain Services aktivieren möchten. Sie können für jedes Azure AD-Verzeichnis nur eine Domäne erstellen.

    ![Auswählen des Azure AD-Verzeichnisses](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klicken Sie auf die Registerkarte **Gruppen** .
5. Klicken Sie im Aufgabenbereich am unteren Rand der Seite auf **Gruppe hinzufügen** , um Ihrem Verzeichnis eine Gruppe hinzuzufügen.

    ![Schaltfläche „Gruppe hinzufügen“](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. Erstellen Sie eine Gruppe mit dem Namen **AAD DC Administrators**. Legen Sie für **GRUPPENTYP** die Option **Sicherheit** fest.

   > [!WARNING]
   > Sie müssen eine Gruppe mit exakt diesem Namen erstellen, um den Zugriff innerhalb der mit Azure AD Domain Services verwalteten Domäne zu aktivieren.
   >
   >

    ![Erstellen der Administratorgruppe](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. Fügen Sie eine Beschreibung für diese Gruppe hinzu, damit andere Benutzer innerhalb Ihres Azure AD-Mandanten wissen, dass diese Gruppe zum Erteilen von Administratorberechtigungen innerhalb der Azure Active Directory Domain Services verwendet wird.
8. Nachdem die Gruppe erstellt wurde, klicken Sie auf den Namen der Gruppe, um deren Eigenschaften anzuzeigen. Klicken Sie im unteren Bereich auf die Schaltfläche **Mitglieder hinzufügen** , um Benutzer als Mitglieder dieser Gruppe hinzuzufügen.

    ![Schaltfläche „Gruppenmitglieder hinzufügen“](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
9. Wählen Sie im Dialogfeld **Mitglieder hinzufügen** die Benutzer aus, die Mitglieder dieser Gruppe werden sollen, und wählen Sie das Häkchen aus, wenn Sie fertig sind.

    ![Hinzufügen von Benutzern zur Administratorgruppe](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Aufgabe 2: Erstellen oder Auswählen eines virtuellen Azure-Netzwerks
Die nächste Konfigurationsaufgabe besteht darin, [ein virtuelles Azure-Netzwerk zu erstellen oder auszuwählen](active-directory-ds-getting-started-vnet.md).

