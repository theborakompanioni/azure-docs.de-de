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
ms.date: 01/27/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: c887adaae811972efd7fcea86eaa67a899f3f4c2
ms.openlocfilehash: 542dd7df7e0b887298522f29cb597f1df73709cb
ms.lasthandoff: 02/27/2017


---
# <a name="transition-to-azure-ad-application-proxies-from-microsoft-forefront"></a>Migrieren von Microsoft Forefront zu Azure AD-Anwendungsproxys

In diesem Artikel wird beschrieben, wie Sie von den Microsoft Forefront-Lösungen Threat Management Gateway (TMG) und United Access Gateway (UAG) zu den folgenden Azure Active Directory-Anwendungsproxys migrieren: Webanwendungsproxy und Azure AD-Anwendungsproxy.

> [!NOTE]
> Der Azure AD-Anwendungsproxy ist ein Feature, das nur verfügbar ist, wenn Sie auf die Premium- oder Basic-Edition von Azure AD aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).


Um detaillierte Informationen zur Migration von Forefront TMG und UAG zu den neuen Anwendungsproxys zu erhalten, können Sie [ein Whitepaper von Microsoft zu diesem Thema herunterladen](http://download.microsoft.com/download/3/E/3/3E335D93-6DB8-4834-90A8-B86105419F05/Microsoft%20TMG%20and%20UAG%20EOL%20and%20transitioning%20to%20WAP%20and%20AADAP.docx).

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

