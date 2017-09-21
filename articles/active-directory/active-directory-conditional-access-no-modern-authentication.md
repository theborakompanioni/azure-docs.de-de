---
title: "Einrichten von SharePoint Online und Exchange Online für bedingten Zugriff mit Azure Active Directory | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie SharePoint Online und Exchange Online für den bedingten Zugriff mit Azure Active Directory einrichten."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 5d09021627ca60a94cb1984b29b8dc7913d093d6
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="set-up-sharepoint-online-and-exchange-online-for-azure-active-directory-conditional-access"></a>Einrichten von SharePoint Online und Exchange Online für bedingten Zugriff mit Azure Active Directory 

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie den Zugriff von Benutzern auf Ihre Cloud-Apps steuern. Wenn Sie den Zugriff auf SharePoint Online und Exchange Online mithilfe des bedingten Zugriffs steuern möchten, müssen Sie folgende Vorgänge ausführen:

- Überprüfen, ob Ihr Szenario mit bedingtem Zugriff unterstützt wird
- Verhindern, dass Client-Apps die Erzwingung der Richtlinien für den bedingten Zugriff umgehen.   

In diesem Artikel wird erläutert, wie Sie bei beiden Vorgängen vorgehen müssen.


## <a name="what-you-need-to-know"></a>Wichtige Informationen

Mithilfe des bedingten Zugriffs mit Azure AD können Sie Cloud-Apps schützen, wenn ein Authentifizierungsversuch aus diesen Quellen erfolgt:

- Aus einem Webbrowser

- Aus einer Client-App, die [moderne Authentifizierung](https://support.office.com/article/Using-Office-365-modern-authentication-with-Office-clients-776c0036-66fd-41cb-8928-5495c0f9168a) nutzt

- Exchange ActiveSync 

Einige Cloud-Apps unterstützen auch ältere Authentifizierungsprotokolle. Dies gilt beispielsweise auch für SharePoint Online und Exchange Online. Wenn eine Client-App mit einem älteren Authentifizierungsprotokoll auf eine Cloud-App zugreifen kann, kann Azure AD für diesen Zugriffsversuch keine Richtlinie für den bedingten Zugriff erzwingen. Um zu verhindern, dass eine Client-App die Erzwingung von Richtlinien umgeht, sollten Sie überprüfen, ob es möglich ist, für die betroffenen Cloud-Apps nur die moderne Authentifizierung zu aktivieren. 

Beispiele für Client-Apps, auf die der bedingte Zugriff nicht zutrifft:

- Office 2010 und frühere Versionen

- Office 2013, wenn die moderne Authentifizierung nicht aktiviert ist



 
## <a name="control-access-to-sharepoint-online"></a>Steuern des Zugriffs auf SharePoint Online

Zusätzlich zur modernen Authentifizierung unterstützt SharePoint Online auch ältere Authentifizierungsmethoden. Wenn die älteren Authentifizierungsprotokolle aktiviert sind, werden die Richtlinien für den bedingten Zugriff auf SharePoint nicht für Clients erzwungen, die keine moderne Authentifizierung nutzen.

Sie können ältere Authentifizierungsprotokolle für den SharePoint-Zugriff mithilfe des Cmdlets **[Set-SPOTenant](https://technet.microsoft.com/library/fp161390.aspx)** deaktivieren: 

    Set-SPOTenant -LegacyAuthProtocolsEnabled $false

## <a name="control-access-to-exchange-online"></a>Steuern des Zugriffs auf Exchange Online

Wenn Sie Richtlinien für den bedingten Zugriff auf Exchange Online einrichten, müssen Sie Folgendes überprüfen:

- Exchange ActiveSync

- Ältere Authentifizierungsprotokolle



### <a name="exchange-activesync"></a>Exchange ActiveSync

Exchange Active Sync unterstützt zwar die moderne Authentifizierung, es gibt jedoch einige Einschränkungen hinsichtlich der Unterstützung von Szenarien mit bedingtem Zugriff:

- Sie können nur die Bedingung für Geräteplattformen konfigurieren.  

    ![Geräteplattformen](./media/active-directory-conditional-access-no-modern-authentication/05.png)

- Das Festlegen der Anforderung der mehrstufigen Authentifizierung wird nicht unterstützt.  

    ![Bedingter Zugriff](./media/active-directory-conditional-access-no-modern-authentication/01.png)

Um den Zugriff auf Exchange Online aus Exchange ActiveSync effektiv schützen, können Sie wie folgt vorgehen:

- Konfigurieren Sie eine unterstützte Richtlinie für den bedingten Zugriff, indem Sie die folgenden Schritte ausführen:

    a. Wählen Sie nur **Office 365 Exchange Online** als Cloud-App aus.  

    ![Bedingter Zugriff](./media/active-directory-conditional-access-no-modern-authentication/04.png)

    b. Wählen Sie **Exchange Active Sync** als **Client-App** aus, und wählen Sie anschließend **Richtlinie nur auf unterstützte Plattformen anwenden** aus.  

    ![Geräteplattformen](./media/active-directory-conditional-access-no-modern-authentication/03.png)

- Blockieren Sie Exchange ActiveSync mithilfe der Regeln der Active Directory-Verbunddienste (AD FS).

        @RuleName = "Block Exchange ActiveSync"
        c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
        => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "false");




### <a name="legacy-authentication-protocols"></a>Ältere Authentifizierungsprotokolle

Zusätzlich zur modernen Authentifizierung unterstützt Exchange Online auch ältere Authentifizierungsmethoden. Wenn ältere Authentifizierungsprotokolle aktiviert sind, werden die Richtlinien für den bedingten Zugriff auf Exchange Online nicht für Clients erzwungen, die keine moderne Authentifizierung nutzen.

Sie können ältere Authentifizierungsprotokolle für Exchange Online deaktivieren, indem Sie AD FS-Regeln festlegen. Dadurch wird der Zugriff blockiert von:

- Älteren Office-Clients wie Office 2013, für die keine moderne Authentifizierung aktiviert ist 

- Früheren Versionen von Office


## <a name="set-up-ad-fs-rules"></a>Einrichten von AD FS-Regeln

Mithilfe der folgenden Regeln für Ausstellungsautorisierung können Sie Datenverkehr auf der AD FS-Ebene aktivieren oder blockieren. 

### <a name="block-legacy-traffic-from-the-extranet"></a>Blockieren von Legacy-Datenverkehr aus dem Extranet

Durch Anwenden der folgenden drei Regeln: 

- Aktivieren Sie den Zugriff für:
    - Exchange ActiveSync-Datenverkehr
    - Browser-Datenverkehr
    - Datenverkehr bei der modernen Authentifizierung
- Blockieren Sie den Zugriff für: 
    - Ältere Client-Apps aus dem Extranet

**Regel 1:**

    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**Regel 2:**

    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**Regel 3:**

    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

### <a name="block-legacy-traffic-from-anywhere"></a>Blockieren von Legacy-Datenverkehr aus beliebigen Quellen

Durch Anwenden der folgenden drei Regeln:

- Aktivieren Sie den Zugriff für: 
    - Exchange ActiveSync-Datenverkehr
    - Browser-Datenverkehr
    - Datenverkehr bei der modernen Authentifizierung
- Blockieren Sie den Zugriff für: 
    - Ältere Apps von beliebigen Standorten

##### <a name="rule-1"></a>Regel 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Regel 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Regel 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access.md).





