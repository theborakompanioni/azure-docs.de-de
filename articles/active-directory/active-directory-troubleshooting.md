---
title: 'Troubleshooting: ''Active Directory'' item is missing or not available | Microsoft Docs'
description: "Vorgehensweise, wenn das Active Directory-Menüelement nicht im Azure-Verwaltungsportal angezeigt wird."
services: active-directory
documentationcenter: na
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 3383020d-6397-43ea-b7aa-c6a9d6a1e3df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/07/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9c52e9257272c5b0c55c488d2c7ca6fef500260b


---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Problembehandlung: Element "Active Directory" fehlt oder ist nicht verfügbar
Viele der Anweisungen für die Verwendung von Azure Active Directory-Funktionen und -Diensten beginnen mit der Aufforderung, das Azure-Verwaltungsportal aufzurufen und auf **Active Directory**zu klicken. Aber was tun Sie, wenn die Active Directory-Erweiterung oder das entsprechende Menüelement nicht angezeigt wird oder als **Nicht verfügbar**markiert ist? Dieses Thema soll Ihnen helfen. Es beschreibt die Umstände, unter denen **Active Directory** nicht angezeigt wird oder nicht verfügbar ist, und mögliche Abhilfen.

## <a name="active-directory-is-missing"></a>Active Directory fehlt
In der Regel wird ein Element **Active Directory** im linken Navigationsmenü angezeigt. Die Anweisungen in Azure Active Directory-Prozeduren setzen voraus, dass dieses Element in der Ansicht vorhanden ist.

![Screenshot: Active Directory in Azure](./media/active-directory-troubleshooting/typical-view.png)

Das Active Directory-Element wird im linken Navigationsmenü angezeigt, wenn eine der folgenden Bedingungen zutrifft. Andernfalls wird das Element nicht angezeigt.

* Der aktuelle Benutzer ist mit einem Microsoft-Konto (früher Windows Live ID) angemeldet.
  
    OR
* Der Azure-Mandant hat ein Verzeichnis, und das aktuelle Konto ist ein Verzeichnisadministrator.
  
    OR
* Der Azure-Mandant hat mindestens einen Azure AD Access Control-Namespace (ACS). Weitere Informationen finden Sie unter [Access Control-Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).
  
    OR
* Der Azure-Mandant hat mindestens einen Azure Multi-Factor Authentication-Anbieter. Weitere Informationen finden Sie unter [Verwalten von Azure Multi-Factor Authentication-Anbietern](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

Um einen Access Control-Namespace oder einen Multi-Factor Authentication-Anbieter zu erstellen, klicken Sie auf **+NEU** > **App Services** > **Active Directory**.

Um Administratorrechte für ein Verzeichnis zu erhalten, muss ein Administrator Ihrem Konto eine Administratorrolle zuweisen. Weitere Informationen finden Sie unter [Zuweisen von Administratorrollen](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Active Directory ist nicht verfügbar
Beim Klicken auf **+NEU** > **App Services** wird ein **Active Directory**-Element angezeigt. Konkret wird das Active Directory-Element angezeigt, wenn eine der Active Directory-Funktionen wie Verzeichnis, Access Control oder Multi-Factor Authentication-Anbieter für den aktuellen Benutzer verfügbar ist.

Während die Seite geladen wird, wird das Element jedoch abgeblendet dargestellt und ist mit **Nicht verfügbar**markiert. Dies ist ein vorübergehender Zustand. Wenn Sie ein paar Sekunden warten, ist das Element verfügbar. Wenn der Zustand andauert, löst häufig das Aktualisieren die Webseite das Problem.

![Screenshot: Active Directory ist nicht verfügbar](./media/active-directory-troubleshooting/not-available.png)




<!--HONumber=Nov16_HO3-->


