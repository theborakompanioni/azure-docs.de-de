---
title: "Starten einer Zugriffsüberprüfung | Microsoft Docs"
description: "Erfahren Sie, wie Sie mit der Anwendung Azure AD Privileged Identity Management eine Zugriffsüberprüfung für privilegierte Identitäten erstellen."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 3e52b731-55f4-4c8a-ba87-9fd34033f52f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim
ms.translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8c9bac62a92947ea888f1ccdbd300f5b40cb3e07
ms.contentlocale: de-de
ms.lasthandoff: 12/28/2016

---
# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a>Starten einer Zugriffsüberprüfung in Azure AD Privileged Identity Management
Rollenzuweisungen „veralten“, wenn Benutzer über privilegierten Zugriff verfügen, den sie nicht mehr benötigen. Um die Risiken im Zusammenhang mit diesen „veralteten“ Rollenzuweisungen zu verringern, sollten Administratoren für privilegierte Rollen die den Benutzern zugewiesenen Rollen regelmäßig überprüfen. Dieses Dokument beschreibt die Schritte zum Starten einer Zugriffsüberprüfung in Azure AD Privileged Identity Management (PIM).

## <a name="start-an-access-review"></a>Starten einer Zugriffsüberprüfung
> [!NOTE]
> Wenn Sie die PIM-Anwendung noch nicht zu Ihrem Dashboard im Azure-Portal hinzugefügt haben, finden Sie die notwendigen Schritte unter [Erste Schritte mit Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)
> 
> 

Eine Zugriffsüberprüfung kann auf der Hauptseite der PIM-Anwendung auf drei Arten gestartet werden:

* **Zugriffsüberprüfung** > **Hinzufügen**
* Die Schaltflächen **Rollen** > **Überprüfen**
* Wählen Sie in der Rollenliste die zu überprüfende Rolle aus > Schaltfläche **Überprüfen**.

Wenn Sie auf die Schaltfläche **Überprüfen** klicken, wird das Blatt **Zugriffsüberprüfung starten** angezeigt. Auf diesem Blatt konfigurieren Sie die Überprüfung mit einem Namen und einem Zeitlimit, wählen eine zu überprüfende Rolle aus und entscheiden, wer die Überprüfung durchführen soll.

![Zugriffsüberprüfung starten – Screenshot][1]

### <a name="configure-the-review"></a>Konfigurieren der Überprüfung
Um eine Zugriffsüberprüfung zu erstellen, müssen Sie ihr einen Namen geben und ein Start- und Enddatum festlegen.

![Überprüfung konfigurieren – Screenshot][2]

Wählen Sie eine ausreichende Länge für die Überprüfung aus, damit Benutzer sie abschließen können. Wenn Sie vor dem Enddatum fertig sind, können Sie die Überprüfung frühzeitig beenden.

### <a name="choose-a-role-to-review"></a>Auswählen einer zu überprüfenden Rolle
Jede Überprüfung konzentriert sich nur auf eine Rolle. Wenn Sie die Zugriffsüberprüfung nicht über das Blatt einer bestimmten Rolle gestartet haben, müssen Sie eine Rolle auswählen.

1. Navigieren Sie zu **Rollenmitgliedschaft überprüfen**
   
    ![Rollenmitgliedschaft überprüfen – Screenshot][3]
2. Wählen Sie eine Rolle aus der Liste aus.

### <a name="decide-who-will-perform-the-review"></a>Entscheiden, wer die Überprüfung durchführen soll
Bei der Durchführung einer Überprüfung gibt es drei Möglichkeiten: Sie können die Überprüfungsdurchführung einer anderen Person zuweisen, Sie können die Überprüfung selbst durchführen, oder Sie können festlegen, dass jeder Benutzer seinen Zugriff selbst überprüfen soll.

1. Navigieren Sie zu **Prüfer auswählen**
   
    ![Prüfer auswählen – Screenshot][4]
2. Wählen Sie eine der Optionen aus:
   
   * **Prüfer auswählen**: Verwenden Sie diese Option, wenn Sie nicht wissen, wer Zugriff benötigt. Mit dieser Option können Sie die Überprüfungsdurchführung einem Ressourcenbesitzer oder Gruppen-Manager zuweisen.
   * **Ich**: Diese Option ist hilfreich, wenn Sie sich vorab mit der Funktionsweise von Zugriffsüberprüfungen auseinandersetzen oder eine Überprüfung für Personen durchführen möchten, die die Überprüfung nicht selbst durchführen können.
   * **Selbstüberprüfung durch Mitglieder**: Mit dieser Option können Benutzer ihre Rollenzuweisungen selbst überprüfen.

### <a name="start-the-review"></a>Starten der Überprüfung
Sie haben die Option, von den Benutzern die Angabe eines Grunds anzufordern, wenn sie ihren Zugriff genehmigen. Fügen Sie ggf. eine Beschreibung der Überprüfung hinzu, und wählen Sie **Starten** aus.

Informieren Sie die Benutzer über die anstehende Zugriffsüberprüfung, und zeigen Sie ihnen, [wie eine Zugriffsüberprüfung durchgeführt wird](active-directory-privileged-identity-management-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung
Sie können den Fortschritt der Überprüfungen durch die Prüfer im Azure AD PIM-Dashboard im Abschnitt mit den Zugriffsüberprüfungen nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](active-directory-privileged-identity-management-how-to-complete-review.md)ist.

Bis der Überprüfungszeitraum beendet ist, können Sie die Benutzer daran erinnern, die Überprüfung abzuschließen, oder die Überprüfung vorzeitig im Abschnitt mit den Zugriffsüberprüfungen beenden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a>Inhaltsverzeichnis zu PIM
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png

