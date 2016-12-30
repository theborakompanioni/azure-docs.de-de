---
title: "Häufig gestellte Fragen zum bedingten Zugriff mit Azure Active Directory | Microsoft Docs"
description: "Häufig gestellte Fragen zum bedingten Zugriff  "
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 3980b99206c9f0c81ba3d03778777878e25db7c8


---
# <a name="azure-active-directory-conditional-access-faq"></a>Häufig gestellte Fragen zum bedingten Zugriff mit Azure Active Directory
## <a name="which-applications-work-with-conditional-access-policies"></a>Welche Anwendungen arbeiten mit Richtlinien für den bedingten Zugriff?
**A** : Informationen dazu finden Sie im Thema [Bedingter Zugriff – unterstützte Anwendungen](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Werden Richtlinien für den bedingten Zugriff für B2B-Zusammenarbeits- und Gastbenutzer erzwungen?
**A** : Richtlinien werden für B2B-Zusammenarbeitsbenutzer erzwungen. In einigen Fällen erfüllt ein Benutzer möglicherweise die Richtlinienanforderungen nicht, beispielsweise dann, wenn eine Organisation keine Multi-Factor Authentication unterstützt. 

Die Richtlinie wird zurzeit für SharePoint-Gastbenutzer nicht erzwungen. Die Gastbeziehung wird innerhalb von SharePoint verwaltet. Gastbenutzerkonten unterliegen daher keinen Zugriffsrichtlinien auf dem Authentifizierungsserver. Der Gastzugriff kann auf SharePoint verwaltet werden.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Gelten SharePoint Online-Richtlinien auch für OneDrive for Business?
**A:** Ja.

## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Warum kann ich keine Richtlinien für Clientanwendungen wie Word oder Outlook festlegen?
**A:** Eine Richtlinie für den bedingten Zugriff legt Anforderungen für den Zugriff auf einen Dienst fest und wird erzwungen, wenn die Authentifizierung für diesen Dienst erfolgt. Die Richtlinie wird also nicht direkt in einer Clientanwendung festgelegt, sondern wird beim Aufrufen eines Diensts angewendet. Eine für SharePoint festgelegte Richtlinie wird beispielsweise auf Clients angewendet, die SharePoint aufrufen. Eine für Exchange festgelegte Richtlinie wird auf Outlook angewendet.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Gilt eine Richtlinie für bedingten Zugriff für Dienstkonten?
**A:** Richtlinien für bedingten Zugriff gelten für alle Benutzerkonten. Dies schließt Benutzerkonten ein, die als Dienstkonten verwendet werden. In vielen Fällen kann ein unbeaufsichtigt ausgeführtes Dienstkonto keine Richtlinie erfüllen, z. B. wenn eine MFA erforderlich ist. In diesen Fällen können Dienstkonten über die Einstellungen zur Verwaltung der Richtlinie für bedingten Zugriff von einer Richtlinie ausgeschlossen werden. Hier erfahren Sie mehr über das Anwenden einer Richtlinie für Benutzer.




<!--HONumber=Dec16_HO4-->


