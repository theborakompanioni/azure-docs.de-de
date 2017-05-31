---
title: "Durchführen eines Upgrades auf den Azure AD-Anwendungsproxy | Microsoft-Dokumentation"
description: "Wählen Sie aus, welche Proxylösung am besten für Sie geeignet ist, um ein Upgrade von Microsoft Forefront oder Unified Access Gateway durchzuführen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: e9d5caa4d11012744ce9f26648166371f3aa17ba
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---
# <a name="upgrade-to-azure-ad-proxies-from-microsoft-forefront-or-unified-access-gateway"></a>Durchführen eines Upgrades von Microsoft Forefront oder Unified Access Gateway auf Azure AD-Proxys

In diesem Artikel wird beschrieben, wie Sie von den Microsoft Forefront-Lösungen Threat Management Gateway (TMG) und United Access Gateway (UAG) zum Azure Active Directory-Anwendungsproxy migrieren.

Um detaillierte Informationen zur Migration von Forefront TMG und UAG zum Anwendungsproxy zu erhalten, können Sie [ein Whitepaper von Microsoft zu diesem Thema lesen](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).

## <a name="functionality-details-for-the-conversion"></a>Informationen zur Funktionalität für die Konvertierung

|**TMG-/UAG-Funktion**|**Moderne Lösung**|
|:-----|:-----|
|Selektive HTTP-Veröffentlichung für Browser-Apps|Azure AD-Anwendungsproxy|
|Integration in Active Directory-Verbunddienste (AD FS)|Azure AD-Anwendungsproxy|
|Veröffentlichung umfassender Protokolle (z.B. Citrix, Lync, RDG)|Azure AD-Anwendungsproxy|
|Portal|Azure AD-Zugriffsbereich oder Office 365-App-Startfeld für den Azure AD-Anwendungsproxy|
|Erkennung der Endpunktintegrität|Intune oder System Center|
|SSL-Tunneling|Windows SSL oder VPN|
|Layer 2/3-Firewall|Windows Server|
|Präauthentifizierung für ActiveSync (HTTP-Standardauthentifizierung) und RDG|Keine aktuelle Lösung von Microsoft|
|Web Application Firewall|Keine aktuelle Lösung von Microsoft.|
|Sicheres Webgateway (Weiterleitungsproxy)|Keine aktuelle Lösung von Microsoft.|


## <a name="next-steps"></a>Nächste Schritte

[Blogbeiträge zum Webanwendungsproxy](https://blogs.technet.microsoft.com/applicationproxyblog/tag/web-application-proxy)<br>
[Blog zum Azure AD-Anwendungsproxy](https://blogs.technet.microsoft.com/applicationproxyblog/tag/aad-ap)

