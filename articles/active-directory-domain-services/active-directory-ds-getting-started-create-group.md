---
title: "Azure Active Directory Domain Services: Erstellen der Administratorengruppe für Azure AD-Domänencontroller | Microsoft-Dokumentation"
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
ms.date: 03/15/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 99607cf642bb6767c845ceb6fb4e62b1c15834f9
ms.lasthandoff: 04/12/2017


---
# <a name="get-started-with-azure-active-directory-domain-services"></a>Erste Schritte mit Azure Active Directory Domain Services
Dieser Artikel beschreibt die Konfigurationsaufgaben, die für die Aktivierung von Azure Active Directory Domain Services (Azure AD DS) für Ihren Azure Active Directory-Mandanten (Azure AD) erforderlich sind.

## <a name="task-1-create-the-azure-ad-dc-administrators-group"></a>Aufgabe 1: Erstellen der Administratorengruppe für Azure AD-Domänencontroller
Die erste Aufgabe besteht darin, eine administrative Gruppe in Ihrem Azure AD-Mandanten zu erstellen. Diese spezielle administrative Gruppe heißt *AAD DC Administrators*. Mitgliedern dieser Gruppe werden Administratorrechte auf Computern erteilt, die in die verwaltete Azure Active Directory Domain Services-Domäne eingebunden sind. Auf in die Domäne eingebundenen Computern wird diese Gruppe der Gruppe „Administratoren“ hinzugefügt. Mitglieder dieser Gruppe können außerdem mithilfe von Remotedesktop Remoteverbindungen mit den in die Domäne eingebundenen Computern herstellen.  

> [!NOTE]
> Sie verfügen nicht über Domänenadministrator- oder Unternehmensadministratorberechtigungen in der verwalteten Domäne, die Sie mithilfe von Azure Active Directory Domain Services erstellt haben. In verwalteten Domänen sind diese Berechtigungen für den Dienst reserviert und stehen Benutzern innerhalb des Mandanten nicht zur Verfügung. Sie können jedoch die in dieser Konfigurationsaufgabe verwendete spezielle administrative Gruppe verwenden, um einige privilegierte Vorgänge auszuführen. Zu diesen Vorgängen gehört das Einbinden von Computern in die Domäne, die Mitgliedschaft in der Administratorengruppe auf der Domäne beigetretenen Computern und das Konfigurieren von Gruppenrichtlinien.
>

In dieser Konfigurationsaufgabe erstellen Sie die administrative Gruppe und fügen ihr mindestens einen Verzeichnisbenutzer hinzu. Führen Sie die folgenden Schritte aus, um die administrative Gruppe für Azure Active Directory Domain Services zu erstellen:

1. Wechseln Sie zum [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Wählen Sie im linken Bereich die Schaltfläche **Active Directory** aus.
3. Wählen Sie den Azure AD-Mandanten (Verzeichnis) aus, für den Sie Azure Active Directory Domain Services aktivieren möchten. Sie können für jedes Azure AD-Verzeichnis nur eine Domäne erstellen.

    ![Auswählen des Azure AD-Verzeichnisses](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klicken Sie auf der Seite **preview directory** (Vorschauverzeichnis) auf die Registerkarte **Gruppen**.
5. Klicken Sie im Aufgabenbereich unten im Fenster auf **Gruppe hinzufügen**, um Ihrem Azure AD-Mandanten eine Gruppe hinzuzufügen.

    ![Die Schaltfläche „Gruppe hinzufügen“](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. Erstellen Sie im Dialogfeld **Gruppe hinzufügen** eine Gruppe namens **AAD DC Administrators**, und legen Sie dann **Gruppentyp** auf **Sicherheit** fest.

   > [!WARNING]
   > Sie müssen eine Gruppe mit exakt diesem Namen erstellen, um den Zugriff innerhalb der mit Azure Active Directory Domain Services verwalteten Domäne zu aktivieren.
   >
   >

    ![Das Dialogfeld „Gruppe hinzufügen“](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. Geben Sie im Feld **Beschreibung** eine Beschreibung ein, anhand derer andere erkennen können, dass diese Gruppe administrative Rechte innerhalb von Azure Active Directory Domain Services gewährt.
8. Nachdem Sie die Gruppe erstellt haben, klicken Sie auf den Gruppennamen, um ihre Eigenschaften anzuzeigen. 
9. Klicken Sie unten im Fenster auf die Schaltfläche **Mitglieder hinzufügen**, um dieser Gruppe Benutzer als Mitglieder hinzuzufügen.

    ![Schaltfläche „Gruppenmitglieder hinzufügen“](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. Wählen Sie im Dialogfeld **Mitglieder hinzufügen** die Benutzer aus, die Mitglieder dieser Gruppe werden sollen, und klicken Sie dann auf das Häkchen rechts unten.

    ![Hinzufügen von Benutzern zur Administratorengruppe](./media/active-directory-domain-services-getting-started/add-group-members.png)

## <a name="next-steps"></a>Nächste Schritte
Aufgabe 2: [Erstellen oder Auswählen eines virtuellen Azure-Netzwerks](active-directory-ds-getting-started-vnet.md)
  

