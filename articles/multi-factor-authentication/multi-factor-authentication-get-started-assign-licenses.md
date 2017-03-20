---
title: "Zuweisen von Lizenzen für Azure MFA | Microsoft-Dokumentation"
description: "Erfahren Sie wie Sie Benutzerlizenzen für Microsoft Azure Multi-Factor Authentication zuweisen."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 514ef423-8ee6-4987-8a4e-80d5dc394cf9
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/13/2017
ms.author: kgremban
ROBOTS: NOINDEX
translationtype: Human Translation
ms.sourcegitcommit: 4572913c2bc732c31f38021f9d3ccc34417de875
ms.openlocfilehash: 536968a803e14019c42e7beed4c948ab760cf020
ms.lasthandoff: 02/15/2017

---
# <a name="assigning-an-azure-mfa-azure-ad-premium-or-enterprise-mobility-license-to-users"></a>Zuweisen einer Azure MFA-, Azure AD Premium- oder Enterprise Mobility-Lizenz zu Benutzern
Wenn Sie Azure MFA-, Azure AD Premium- oder Enterprise Mobility Suite-Lizenzen erworben haben, müssen Sie keinen Multi-Factor Auth-Anbieter erstellen. Nachdem Sie Ihren Benutzern die Lizenzen zugewiesen haben, können Sie sie für MFA aktivieren.

## <a name="to-assign-a-license"></a>So weisen Sie eine Lizenz zu
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Doppelklicken Sie auf der Seite „Active Directory“ auf das Verzeichnis mit den Benutzern, die Sie aktivieren möchten.
4. Wählen Sie oben auf der Seite des Verzeichnisses die Option **Lizenzen**aus.
   ![Lizenzen zuweisen](./media/multi-factor-authentication-get-started-assign-licenses/assign1.png)
5. Wählen Sie auf der Seite mit den Lizenzen die Option **Azure Multi-Factor Authentication**, **Active Directory Premium** oder **Enterprise Mobility Suite** aus.  Bei nur einer Lizenz ist diese automatisch ausgewählt.
6. Klicken Sie unten auf der Seite auf **Zuweisen**.
   ![Lizenzen zuweisen](./media/multi-factor-authentication-get-started-assign-licenses/assign3.png)
7. Klicken Sie im angezeigten Feld neben die Benutzer oder Gruppen, denen Sie Lizenzen zuweisen möchten.  Daraufhin wird ein grünes Häkchen angezeigt.
8. Klicken Sie auf das Häkchen, um die Änderungen zu speichern.
   ![Lizenzen zuweisen](./media/multi-factor-authentication-get-started-assign-licenses/assign4.png)
9. Anschließend erscheint eine Nachricht, die angibt, wie viele Lizenzen zugewiesen wurden und wie viele fehlgeschlagen sind.  Klicken Sie auf **OK**.
   ![Lizenzen zuweisen](./media/multi-factor-authentication-get-started-assign-licenses/assign5.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Was ist Microsoft Azure Active Directory-Lizenzierung?](../active-directory/active-directory-licensing-what-is.md)
