---
title: Migrieren von Microsoft Forefront zu Azure AD-Anwendungsproxys | Microsoft-Dokumentation
description: "Dieser Artikel erläutert die Grundlagen der Migration von den Microsoft Forefront TMG- und UAG-Lösungen zu Azure Active Directory-Anwendungsproxys."
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
ms.date: 04/17/2017
ms.author: kgremban
ROBOTS: NOINDEX
translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: 6e95e9abac988ae54a401927a92bdb397dd63eed
ms.lasthandoff: 04/21/2017

---
# <a name="transition-to-azure-ad-application-proxies-from-microsoft-forefront"></a>Migrieren von Microsoft Forefront zu Azure AD-Anwendungsproxys

In diesem Artikel wird beschrieben, wie Sie von den Microsoft Forefront-Lösungen Threat Management Gateway (TMG) und United Access Gateway (UAG) zum Azure Active Directory-Anwendungsproxy migrieren.

Um detaillierte Informationen zur Migration von Forefront TMG und UAG zum Anwendungsproxy zu erhalten, können Sie [ein Whitepaper von Microsoft zu diesem Thema lesen](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).

## <a name="functionality-details-for-the-conversion"></a>Informationen zur Funktionalität für die Konvertierung

|**TMG-/UAG-Funktion**|**Webanwendungsproxy/Azure AD-Anwendungsproxy**|
|:-----|:-----|
|Selektive HTTP-Veröffentlichung für Browser-Apps|Verfügbar im Webanwendungsproxy in Windows Server 2012 R2. Bereits heute im Azure AD-Anwendungsproxy verfügbar.|
|Integration in Active Directory-Verbunddienste (AD FS)|Verfügbar im Webanwendungsproxy in Windows Server 2012 R2. Bereits heute im Azure AD-Anwendungsproxy verfügbar.|
|Veröffentlichung umfassender Protokolle (z.B. Citrix, Lync, RDG)|Verfügbar im Webanwendungsproxy in Windows Server 2012 R2. Bereits heute im Azure AD-Anwendungsproxy verfügbar.|
|Präauthentifizierung für ActiveSync (HTTP-Standardauthentifizierung) und RDG|Derzeit im Webanwendungsproxy oder Azure AD-Anwendungsproxy nicht verfügbar.|
|Portal|Verwenden Sie Intune oder System Center für den Webanwendungsproxy. Verwenden Sie den Azure AD-Zugriffsbereich oder das Office 365-App-Startfeld für den Azure AD-Anwendungsproxy.|
|Erkennung der Endpunktintegrität|Verwenden Sie Intune oder System Center.|
|SSL-Tunneling|Verwenden Sie Windows SSL- oder VPN-Funktionen.|
|Layer 2/3-Firewall|Verwenden Sie Windows Server-Funktionen.|
|Web Application Firewall|Keine aktuelle Lösung von Microsoft.|
|Sicheres Webgateway (Weiterleitungsproxy)|Keine aktuelle Lösung von Microsoft.|


## <a name="next-steps"></a>Nächste Schritte

[Blogbeiträge zum Webanwendungsproxy](https://blogs.technet.microsoft.com/applicationproxyblog/tag/web-application-proxy)<br>
[Blog zum Azure AD-Anwendungsproxy](https://blogs.technet.microsoft.com/applicationproxyblog/tag/aad-ap)

