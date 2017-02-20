---
title: "Übergang zum Azure AD-Anwendungsproxy von Microsoft Forefront | Microsoft-Dokumentation"
description: Hier finden Sie grundlegende Informationen zu Azure AD-Anwendungsproxy-Connectors.
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
ms.sourcegitcommit: 9945512d18d66c321c0d24ee1050497d4716fd47
ms.openlocfilehash: 699112846cc1e4e9fc6b04b1b8509152d7aecdef


---
#<a name="transition-to-azure-ad-app-proxy-from-microsoft-forefront"></a>Übergang zum Azure AD-Anwendungsproxy von Microsoft Forefront

In diesem Artikel wird beschrieben, wie Sie von den Microsoft-Lösungen Forefront Threat Management Gateway (TMG) und United Access Gateway (UAG) zu den folgenden Azure AD-Anwendungsproxys migrieren: Webanwendungsproxy (WAP) und Azure AD-Anwendungsproxy (AADAP). 

> [!NOTE]
> Das Feature "Anwendungsproxy" ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
> 
 
Zahlreiche Kunden möchten wissen, wie der Übergang von Forefront UAG und TMG zu den neuen Anwendungsproxys funktioniert. Informationen finden Sie in diesem [Whitepaper](http://download.microsoft.com/download/3/E/3/3E335D93-6DB8-4834-90A8-B86105419F05/Microsoft%20TMG%20and%20UAG%20EOL%20and%20transitioning%20to%20WAP%20and%20AADAP.docx), in dem der Übergang ausführlich beschrieben wird. 
 
## <a name="tmguag-conversion-to-azure-ad-application-proxy-table"></a>Tabelle mit Übergang von TMG/UAG zum Azure AD-Anwendungsproxy
 
|**TMG-/UAG-Funktion**|**Webanwendungsproxy (WAP)/Azure AD-Anwendungsproxy (AADAP)**|
|:-----|:-----|
|Selektive HTTP-Veröffentlichung für Browser-Apps|Verfügbar in WAP unter Windows Server 2012 R2 (in AADAP bereits verfügbar)|
|ADFS-Integration|Verfügbar in WAP unter Windows Server 2012 R2 (in AADAP bereits verfügbar)|
|Veröffentlichung vielfältiger Protokolle (z.B. Citrix, Lync, RDG)|Verfügbar in WAP unter Windows Server 2012 R2 (in AADAP bereits verfügbar)|
|Präauthentifizierung für ActiveSync (HTTP-Standardauthentifizierung) und RDG|Wird in WAP unter Windows Server vNext verfügbar sein (künftig in AADAP verfügbar)|
|Portal|Verwendung von Intune/System Center für WAP (Verwendung des für AADAP verfügbaren AAD-Zugriffsbereichs oder Office 365-App-Startfelds)|
|Integritätserkennung für Endpunkte|Verwendung von Intune/System Center|
|SSL-Tunneling|Verwendung von Windows SSL-/VPN-Funktionen|
|Layer 2/3-Firewall|Verwendung von Windows Server-Funktionen|
|Web Application Firewall|Keine aktuelle Lösung von Microsoft|
|Sicheres Webgateway (Weiterleitungsproxy)|Keine aktuelle Lösung von Microsoft|


## <a name="next-steps"></a>Nächste Schritte

[Blogbeiträge zum Webanwendungsproxy](https://blogs.technet.microsoft.com/applicationproxyblog/tag/web-application-proxy)<br>
[Blogbeiträge zum Anwendungsproxy](https://blogs.technet.microsoft.com/applicationproxyblog/tag/aad-ap)



<!--HONumber=Feb17_HO1-->


