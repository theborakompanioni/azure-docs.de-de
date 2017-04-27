---
title: "Registrieren für Azure mit einem Office 365-Konto | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie mit einem Office 365-Konto ein Azure-Abonnement erstellen.
services: 
documentationcenter: 
author: JiangChen79
manager: vikdesai
editor: 
tags: billing,top-support-issue
ms.assetid: 129cdf7a-2165-483d-83e4-8f11f0fa7f8b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: cjiang
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: d62ec023605573e4b0804da00c75f65c8dea5173
ms.lasthandoff: 04/12/2017


---
# <a name="sign-up-for-an-azure-subscription-with-your-office-365-account"></a>Registrieren für ein Azure-Abonnement mit Ihrem Office 365-Konto
Wenn Sie über ein Office 365-Abonnement verfügen, können Sie mit Ihrem Office 365-Konto ein Azure-Abonnement erstellen. Sie können sich ohne Azure-Abonnement beim [Azure-Portal](https://portal.azure.com/) anmelden, indem Sie Ihren Office 365-Benutzernamen und das zugehörige Kennwort verwenden. Wenn Sie jedoch virtuelle Computer einrichten oder andere Azure-Dienste verwenden möchten, müssen Sie für Ihr Konto ein Azure-Abonnement erstellen. Wenn Sie einer Organisation angehören, die Azure-Dienste verwenden möchten, fügen Sie dem Abonnement nach dem Erstellen weitere Benutzer aus Ihrer Organisation hinzu.  

Wenn Sie bereits über ein Office 365-Konto und ein Azure-Abonnement verfügen, finden Sie weitere Informationen unter [Zuordnen eines Office 365-Mandanten zu einem Azure-Abonnement](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-azure-subscription-by-using-your-office-365-account"></a>Erstellen eines Azure-Abonnements mit Ihrem Office 365-Konto

Zur Registrierung für Azure verwenden Sie den Link für das kostenlose Azure-Abonnement, und melden Sie sich mit Ihrem Office 365-Benutzernamen und -Kennwort bei Azure an. 

1. Wechseln Sie zu [Azure.com](https://azure.microsoft.com/). 
2. Klicken Sie auf **Kostenlos einsteigen**.
3. Melden Sie sich mit Ihrem Office 365-Benutzernamen und -Kennwort an. Das Konto, das Sie verwenden, muss nicht über Administratorberechtigungen verfügen. Wenn Sie mehrere Office 365-Konten haben, stellen Sie sicher, dass Sie die Anmeldeinformationen für das Office 365-Konto verwenden, das Ihrem Azure-Abonnement zugeordnet werden soll. 

   ![Screenshot der Anmeldeseite.](./media/billing-use-existing-office-365-account-azure-subscription/billing-sign-in-with-office-365-account.png)

4. Geben Sie die erforderlichen Informationen ein, und schließen Sie den Registrierungsvorgang ab.

    ![Screenshot des Registrierungsformulars.](./media/billing-use-existing-office-365-account-azure-subscription/billing-azure-sign-up-fill-information.png)

- Wenn Sie dem Azure-Abonnement weitere Benutzer aus Ihrer Organisation hinzufügen möchten, finden Sie weitere Informationen unter [Erste Schritte mit der Zugriffsverwaltung im Azure-Portal](../active-directory/role-based-access-control-what-is.md). 
- Wenn Sie einen Administrator hinzufügen möchten, der Sie bei der Verwaltung des Abonnements unterstützt, finden Sie weitere Informationen unter [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](billing-add-change-azure-subscription-administrator.md).

## <a id="more-about-subs">Weitere Informationen zu Azure- und Office 365-Abonnements</a>
Office 365 und Azure verwenden den Azure AD-Dienst zum Verwalten von Benutzern und Abonnements. Das Azure-Verzeichnis funktioniert wie ein Container, in dem Sie Benutzer und Abonnements gruppieren können. Damit Sie für Ihre Azure- und Office 365-Abonnements die gleichen Benutzerkonten verwenden können, müssen Sie sicherstellen, dass die Abonnements im gleichen Verzeichnis erstellt werden. Berücksichtigen Sie dabei Folgendes:

* Ein Abonnement wird in einem Verzeichnis erstellt.
* Benutzer gehören Verzeichnissen an.
* Ein Abonnement wird in dem Verzeichnis des Benutzers erstellt, der das Abonnement erstellt. Das heißt, Ihr Office 365-Abonnement ist mit dem gleichen Konto verknüpft wie Ihr Azure-Abonnement.
* Azure-Abonnements gehören einzelnen Benutzern im Verzeichnis.
* Office 365-Abonnements gehören dem Verzeichnis selbst. Benutzer mit den richtigen Berechtigungen im Verzeichnis können diese Abonnements verwalten.

![Screenshot der Beziehung zwischen dem Verzeichnis, Benutzern und Abonnements.](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Weitere Informationen finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen. 
