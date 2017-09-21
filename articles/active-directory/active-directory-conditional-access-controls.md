---
title: Steuerelemente beim bedingten Zugriff mit Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Steuerelemente beim bedingten Zugriff mit Azure Active Directory funktionieren.
services: active-directory
keywords: "bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 0f7d847c98e790c542f3a3e666b9a887099a6cbc
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---

# <a name="controls-in-azure-active-directory-conditional-access"></a>Steuerelemente beim bedingten Zugriff mit Azure Active Directory 

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie den Zugriff von autorisierten Benutzern auf Ihre Cloud-Apps steuern. In einer Richtlinie für den bedingten Zugriff definieren Sie die Antwort („do this“) auf eine bestimmte Bedingung („when this happens“). Im Kontext des bedingten Zugriffs gilt: 

- „**When this happens**“ wird als **Bedingungsanweisung ** bezeichnet.
- „**Then do this**“ wird als **Kontrolle** bezeichnet.

![Kontrolle](./media/active-directory-conditional-access-controls/11.png)

Die Kombination aus Bedingungsanweisung und Ihren Kontrollen ergibt eine Richtlinie für den bedingten Zugriff.

![Kontrolle](./media/active-directory-conditional-access-controls/12.png)

Jedes Steuerelement ist entweder eine Anforderung, die von der Person oder dem System erfüllt werden muss, die bzw. das sich anmeldet, oder eine Einschränkung der Aktionen, die der Benutzer nach der Anmeldung durchführen kann. 

Es gibt zwei Arten von Steuerelementen: 

- **Gewährungssteuerelemente**: Zum Steuern des Zugriffs

- **Sitzungssteuerelemente**: Zum Einschränken des Zugriffs innerhalb einer Sitzung

In diesem Thema werden die verschiedenen Steuerelemente beschrieben, die beim bedingten Zugriff mit Azure AD verfügbar sind. 

## <a name="grant-controls"></a>Gewährungssteuerelemente

Mit Gewährungssteuerelementen können Sie entweder den gesamten Zugriff sperren oder den Zugriff mit zusätzlichen Anforderungen gewähren, indem Sie die gewünschten Steuerelemente auswählen. Bei mehreren Steuerelementen können Sie folgenden Anforderungen verwenden:

- Alle ausgewählten Steuerelemente müssen erfüllt werden (*AND*) 
- Ein ausgewähltes Steuerelement muss erfüllt werden (*OR*)

![Kontrolle](./media/active-directory-conditional-access-controls/51.png)



### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Mithilfe dieses Steuerelements können Sie die mehrstufige Authentifizierung für den Zugriff auf die angegebene Cloud-App erforderlich machen. Dieses Steuerelement unterstützt die folgenden Anbieter für die mehrstufige Authentifizierung: 

- Azure Multi-Factor Authentication 

- Ein lokaler Anbieter für die mehrstufige Authentifizierung in Kombination mit Active Directory-Verbunddiensten (AD FS).
 
Die mehrstufige Authentifizierung unterstützt den Schutz Ihrer Ressourcen vor dem Zugriff durch einen nicht autorisierten Benutzer, der möglicherweise Zugriff auf die primären Anmeldeinformationen eines gültigen Benutzers erlangt hat.



### <a name="compliant-device"></a>Kompatibles Gerät

Sie können die Richtlinien für den bedingten Zugriff auf Geräteebene festlegen. Das Ziel einer gerätebasierten Richtlinie für den bedingten Zugriff ist, den Zugriff auf die konfigurierten Ressourcen nur von vertrauenswürdigen Geräten aus zuzulassen. Eine Möglichkeit, zu definieren, was ein vertrauenswürdiges Gerät ist, besteht darin, ein kompatibles Gerät vorauszusetzen. Weitere Informationen finden Sie unter [Einrichten von Richtlinien für den gerätebasierten bedingten Zugriff für Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).

### <a name="domain-joined-device"></a>In eine Domäne eingebundenes Gerät

Ein in eine Domäne eingebundenes Gerät vorauszusetzen, ist eine weitere Option, die Ihnen zum Konfigurieren von Richtlinien für den gerätebasierten bedingten Zugriff zur Verfügung steht. Diese Anforderung bezieht sich auf Windows-Desktops, Laptops und Enterprise-Tablets, die mit einem lokalen Active Directory verknüpft sind. Weitere Informationen finden Sie unter [Einrichten von Richtlinien für den gerätebasierten bedingten Zugriff für Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).





### <a name="approved-client-app"></a>Genehmigte Client-App

Da Ihre Mitarbeiter mobile Geräte sowohl für private als auch für berufliche Zwecke verwenden, sollten Sie die Möglichkeit haben, Unternehmensdaten, auf die mit den Geräten zugegriffen wird, auch dann zu schützen, wenn diese Geräte nicht von Ihnen verwaltet werden.
Sie können [Intune-Richtlinien für den App-Schutz](https://docs.microsoft.com/intune/app-protection-policy) verwenden, um die Daten Ihres Unternehmens unabhängig von der jeweiligen MDM-Lösung (Mobile Device Management, mobile Geräteverwaltung) zu schützen.


Wenn Sie genehmigte Client-Apps verwenden, können Sie die Anforderung festlegen, dass eine Client-App, die auf Ihre Cloud-Apps zugreifen möchte, die [Intune-Richtlinien für den App-Schutz](https://docs.microsoft.com/intune/app-protection-policy) unterstützen muss. Sie können zum Beispiel den Zugriff auf Exchange Online für die Outlook-App einschränken. Eine Richtlinie für den bedingten Zugriff, die genehmigte Client-Apps erfordert, wird auch als [Richtlinie für den App-basierten bedingten Zugriff](active-directory-conditional-access-mam.md) bezeichnet. Eine Liste der unterstützten genehmigten Client-Apps finden Sie unter [Genehmigte Client-App als Voraussetzung](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement).



## <a name="session-controls"></a>Sitzungssteuerelemente

Sitzungssteuerelemente ermöglichen das Einschränken der Benutzeroberfläche innerhalb einer Cloud-App. Die Sitzungssteuerelemente werden von Cloud-Apps erzwungen und verlassen sich auf zusätzliche Informationen über die Sitzung, die der App von Azure AD bereitgestellt werden.

![Kontrolle](./media/active-directory-conditional-access-controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Verwenden von App-erzwungenen Einschränkungen

Sie können dieses Steuerelement verwenden, um von Azure AD anzufordern, die Geräteinformationen an die Cloud-App zu übergeben. Dadurch erfährt die Cloud-App, ob der Benutzer von einem konformen Gerät oder einem in eine Domäne eingebundenen Gerät stammt. Dieses Steuerelement wird derzeit nur mit SharePoint als Cloud-App unterstützt. SharePoint verwendet die Geräteinformationen dazu, Benutzern eine eingeschränkte oder vollständige Benutzeroberfläche zur Verfügung zu stellen, je nach Gerätezustand.
Weitere Informationen zum Anfordern von eingeschränktem Zugriff mit SharePoint finden Sie unter [Steuern des Zugriffs von nicht verwalteten Geräten](https://aka.ms/spolimitedaccessdocs).



## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 

