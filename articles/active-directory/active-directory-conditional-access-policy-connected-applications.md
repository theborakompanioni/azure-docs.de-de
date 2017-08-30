---
title: "Konfigurieren des gerätebasierten bedingten Zugriffs für Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Richtlinien für den gerätebasierten bedingten Zugriff für Azure Active Directory konfigurieren."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: a26c40351c6b982fd90acb4bf06220ef3f79f399
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Konfigurieren von Richtlinien für den gerätebasierten bedingten Zugriff für Azure Active Directory

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie präzise steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen können. Sie können z.B. den Zugriff auf bestimmte Ressourcen auf vertrauenswürdige Geräte beschränken. Eine Richtlinie für den bedingten Zugriff, die ein vertrauenswürdiges Gerät erfordert, wird auch als Richtlinie für den gerätebasierten bedingten Zugriff bezeichnet.

Dieses Thema bietet Informationen zum Konfigurieren von Richtlinien für den gerätebasierten bedingten Zugriff für mit Azure AD verbundene Anwendungen. 


## <a name="before-you-begin"></a>Voraussetzungen

Beim gerätebasierten bedingten Zugriff werden der **bedingte Zugriff von Azure AD** und die **Geräteverwaltung von Azure AD** miteinander kombiniert. Wenn Sie mit einem dieser Bereiche noch nicht vertraut sind, sollten Sie zunächst die folgenden Themen lesen:

- **[Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md):** Dieses Thema bietet eine konzeptionelle Übersicht über den bedingten Zugriff und die zugehörige Terminologie.

- **[Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md):** Dieses Thema bietet eine Übersicht über die verschiedenen Optionen zum Verbinden von Geräten mit Azure AD. 


## <a name="trusted-devices"></a>Vertrauenswürdige Geräte

In einer Welt, in der Mobilität und die Cloud an erster Stelle stehen, ermöglicht Azure Active Directory das einmalige Anmelden an Geräten, Apps und Diensten an jedem Ort. Bei bestimmten Ressourcen in Ihrer Umgebung ist das Beschränken des Zugriffs auf die richtigen Benutzer möglicherweise nicht ausreichend. Die richtigen Benutzer müssen möglicherweise auch vertrauenswürdige Geräte verwenden, um auf eine Ressource zuzugreifen. Sie können in Ihrer Umgebung basierend auf den folgenden Komponenten festlegen, was ein vertrauenswürdiges Gerät ist:

- Die [Geräteplattformen](active-directory-conditional-access-azure-portal.md#device-platforms) auf einem Gerät
- Ob ein Gerät konform ist
- Ob ein Gerät einer Domäne angehört 

Die [Geräteplattformen](active-directory-conditional-access-azure-portal.md#device-platforms) sind durch das Betriebssystem gekennzeichnet, das auf dem Gerät ausgeführt wird. Sie können den Zugriff auf bestimmte Ressourcen in der Richtlinie für den gerätebasierten bedingten Zugriff auf bestimmte Plattformen beschränken.



Sie können in einer Richtlinie für den gerätebasierten bedingten Zugriff erzwingen, dass vertrauenswürdige Geräte als konform gekennzeichnet sein müssen.

![Cloud-Apps](./media/active-directory-conditional-access-policy-connected-applications/24.png)

Geräte können durch folgende Instanzen im Verzeichnis als konform gekennzeichnet werden:

- Intune 
- Ein Drittanbieter-Verwaltungssystem für mobile Geräte, das in Azure AD integriert ist  

Nur Geräte, die mit Azure AD verbunden sind, können als konform gekennzeichnet werden. Für das Verbinden eines Geräts mit Azure Active Directory haben Sie die folgenden Möglichkeiten: 

- Bei Azure AD registriert
- In Azure AD eingebunden
- Hybrid in Azure AD eingebunden

    ![Cloud-Apps](./media/active-directory-conditional-access-policy-connected-applications/26.png)

Wenn Sie eine lokale Active Directory-Instanz (AD) verwenden, können Sie Geräte, die nicht mit Azure AD verbunden, aber in Ihr AD eingebunden sind, als vertrauenswürdig kennzeichnen.

![Cloud-Apps](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a>Nächste Schritte

Bevor Sie eine Richtlinie für den gerätebasierten bedingten Zugriff in Ihrer Umgebung konfigurieren, sollten Sie die [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) kennen.


