---
title: Verwenden von Azure RemoteApp mit Office 365-Benutzerkonten | Microsoft Docs
description: "Informationen zum Verwenden von Azure RemoteApp mit meinen Office 365-Benutzerkonten"
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: aba70fd3-60b0-4b44-9321-1ab18760ccd5
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 0af5a4e2139a202c7f62f48c7a7e8552457ae76d
ms.openlocfilehash: 8744c5a133738fc60b86e7abd4b0d4b1cb28314c


---
# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Verwenden von Azure RemoteApp mit Office 365-Benutzerkonten
> [!IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Wenn Sie über ein Office 365-Abonnement verfügen, haben Sie ein Azure Active Directory, in dem Ihre Benutzernamen und Kennwörter für den Zugriff auf Office 365-Dienste gespeichert werden. Wenn Ihre Benutzer z. B. Office 365 ProPlus aktivieren, authentifizieren sie sich bei Azure AD, um das Vorhandensein von Lizenzen zu überprüfen. Die meisten Kunden möchten das gleiche Verzeichnis mit Azure RemoteApp verwenden.

Bei der Bereitstellung von Azure RemoteApp verwenden Sie wahrscheinlich ein Azure-Abonnement, das mit einem anderen Azure AD verknüpft ist. Um Ihr Office 365-Verzeichnis verwenden zu können, müssen Sie das Azure-Abonnement in das betreffende Verzeichnis verschieben.

Informationen zum Bereitstellen von Office 365-Clientanwendungen finden Sie unter [Verwenden Ihres Office 365-Abonnements mit Azure RemoteApp](remoteapp-officesubscription.md).

## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Phase 1: Registrieren Sie Ihr kostenloses Abonnement für Office 365 Azure Active Directory.
Wenn Sie das klassische Azure-Portal verwenden, gehen Sie anhand der Schritte unter [Registrieren Ihres kostenlosen Azure Active Directory-Abonnements](https://technet.microsoft.com/library/dn832618.aspx) vor, um Administratorzugriff auf Ihr Azure AD über das Azure-Verwaltungsportal zu erhalten. Als Ergebnis dieses Prozesses sollten Sie sich beim Azure-Portal anmelden können und dort Ihr Verzeichnis sehen – an diesem Punkt wird nicht viel mehr angezeigt, da sich das vollständige Azure-Abonnement, das Sie mit Azure RemoteApp verwenden, in einem anderen Verzeichnis befindet.

Merken Sie sich den Namen und das Kennwort des Administratorkontos, das Sie in diesem Schritt erstellt haben – sie werden in Phase 2 benötigt.

Wenn Sie das Azure-Portal verwenden, lesen Sie [How to register and activate a free Azure Active Directory using Office 365 portal](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/)(Registrieren und Aktivieren eines kostenlosen Azure Active Directory mit dem Office 365-Portal).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Phase 2: Ändern Sie das Azure AD, das mit Ihrem Azure-Abonnement verknüpft ist.
Wir werden Ihr Azure-Abonnement von ihrem aktuellen Verzeichnis in das Office 365-Verzeichnis ändern, mit dem wir in Phase 1 gearbeitet haben.

Befolgen Sie die Anweisungen in [Ändern des Azure Active Directory-Mandanten in Azure RemoteApp](remoteapp-changetenant.md). Achten Sie besonders auf die folgenden Schritte:

* Schritt 1: Wenn Sie Azure RemoteApp (ARA) in diesem Abonnement bereitgestellt haben, stellen Sie sicher, dass Sie zuerst alle Azure AD-Benutzerkonten aus allen ARA-Sammlungen entfernen, bevor Sie versuchen, etwas anderes zu tun. Alternativ können Sie erwägen, alle vorhandenen Sammlungen zu löschen.
* Schritt 2: Dies ist ein wichtiger Schritt. Sie müssen ein Microsoft-Konto (z.B. @outlook.com)) als Dienstadministrator des Abonnements verwenden, weil keine Benutzerkonten aus dem vorhandenen Azure AD dem Abonnement angefügt sein dürfen – andernfalls können wir es nicht in ein anderes Azure AD verschieben.
* Schritt 4: Wenn Sie ein vorhandenes Verzeichnis hinzufügen, fordert das System Sie auf, sich mit dem Administratorkonto für dieses Verzeichnis anzumelden. Stellen Sie sicher, dass Sie das Administratorkonto aus Phase 1 verwenden.
* Schritt 5: Ändern Sie das übergeordnete Verzeichnis des Abonnements in Ihr Office 365-Verzeichnis. Das Endergebnis sollte sein, dass Ihr Abonnement unter „Einstellungen > Abonnements“ das Office 365-Verzeichnis auflistet. 
  ![Ändern des übergeordneten Verzeichnisses des Abonnements](./media/remoteapp-o365user/settings.png)

An diesem Punkt ist Ihr Azure RemoteApp-Abonnement Ihrem Office 365 Azure AD zugeordnet. Sie können die vorhandenen Office 365-Benutzerkonten mit Azure RemoteApp verwenden!




<!--HONumber=Dec16_HO2-->


