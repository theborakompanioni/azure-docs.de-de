---
title: "Azure AD Connect: Wenn Sie bereits über Azure AD verfügen | Microsoft-Dokumentation"
description: Dieses Thema beschreibt die Verwendung von Connect, wenn ein Azure AD-Mandant vorhanden ist.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: f59028a2f909914222236f3b3575afd0949b4277
ms.openlocfilehash: c89e206462856d25a81729e7028065ac1cd13ef3
ms.lasthandoff: 02/23/2017

---

# <a name="azure-ad-connect-when-you-have-an-existent-tenant"></a>Azure AD Connect mit vorhandenem Mandanten
Bei den meisten Themen zur Verwendung von Azure AD Connect wird davon ausgegangen, dass Sie mit einem neuen Azure AD-Mandanten starten und dass es keine weiteren Benutzer oder Objekte gibt. Wenn Sie jedoch bereits über einen Azure AD-Mandanten verfügen, der mit Benutzern und anderen Objekten aufgefüllt ist, und jetzt Connect verwenden möchten, eignet sich dieses Thema perfekt für Sie.

## <a name="the-basics"></a>Grundlagen
Ein Objekt in Azure AD wird entweder in der Cloud (Azure AD) oder lokal verwaltet. Bei nur einem Objekt ist es nicht möglich, einige Attribute lokal und andere Attribute in Azure AD zu verwalten. Jedes Objekt verfügt über ein Flag, das angibt, wo das Objekt verwaltet wird.

Sie können einige Benutzer lokal und andere in der Cloud verwalten. Ein häufiges Szenario für diese Konfiguration ist eine Organisation, die sowohl über Buchhaltungs- als auch Vertriebsmitarbeiter verfügt. Die Buchhaltungsmitarbeiter haben ein lokales AD-Konto, aber die Vertriebsmitarbeiter nicht – sie haben ein Konto in Azure AD. Daher müssen Sie einige Benutzer lokal und einige in Azure AD verwalten.

Wenn Sie damit begonnen haben, Benutzer in Azure AD zu verwalten, die sich auch im lokalen Azure Directory befinden, und später Connect verwenden möchten, sind einige weitere Aspekte zu berücksichtigen.

## <a name="sync-with-existing-users-in-azure-ad"></a>Synchronisieren mit vorhandenen Benutzern in Azure AD
Wenn Sie Azure AD Connect installieren und mit der Synchronisierung beginnen, überprüft der Azure AD-Synchronisierungsdienst jedes neue Objekt (in Azure AD) und versucht, ein entsprechendes vorhandenes Objekt zu finden. Für diesen Prozess werden drei Attribute verwendet: **userPrincipalName**, **proxyAddresses** und **sourceAnchor**/**immutableID**. Eine Übereinstimmung bei **userPrincipalName** und **proxyAddresses** wird als **Soft Match** bezeichnet. Eine Übereinstimmung bei **sourceAnchor** wird als **Hard Match** bezeichnet. Für das Attribut **proxyAddresses** wird zur Auswertung nur der Wert mit **SMTP:** verwendet – dies ist die primäre E-Mail-Adresse.

Die Übereinstimmung wird nur für neue, aus Connect eingehende Objekte ausgewertet. Wenn Sie ein vorhandenes Objekt so ändern, dass es einem dieser Attribute entspricht, wird stattdessen ein Fehler angezeigt.

Wenn Azure AD ein Objekt findet, dessen Attributwerte die gleichen sind wie für ein aus Connect eingehendes Objekt, und das Objekt bereits in Azure AD vorhanden ist, wird das Objekt in Azure AD von Connect übernommen. Das zuvor in der Cloud verwaltete Objekt wird als „lokal verwaltet“ gekennzeichnet. Alle Attribute in Azure AD mit einem Wert im lokalen Active Directory werden mit dem lokalen Wert überschrieben. Ausgenommen hiervon sind Attribute, die lokal den Wert **NULL** aufweisen. In diesem Fall bleibt der Wert in Azure AD erhalten, Sie können das Attribut jedoch weiterhin nur lokal in einen anderen Wert ändern.

> [!WARNING]
> Da alle Attribute in Azure AD durch ihre lokalen Werte überschrieben werden, stellen Sie sicher, dass Ihre lokalen Daten richtig und vollständig sind. Ein Beispiel: Wenn Sie E-Mail-Adressen nur in Office 365 verwaltet und im lokalen AD DS nicht aktualisiert haben, gehen alle Werte aus Azure AD/Office 365 verloren, die nicht im AD DS vorhanden sind.

> [!IMPORTANT]
> Wenn Sie die Kennwortsynchronisierung verwenden – die bei Expresseinstellungen immer zum Einsatz kommt –, wird das Kennwort in Azure AD durch das Kennwort im lokalen Active Directory überschrieben. Wenn Ihre Benutzer daran gewöhnt sind, verschiedene Kennwörter zu verwenden, müssen Sie sie darüber informieren, dass sie ihr lokales Kennwort verwenden müssen, nachdem Sie Connect installiert haben.

Die Warnungen und Informationen im vorherigen Abschnitt müssen in Ihrer Planung berücksichtigt werden. Wenn Sie Änderungen in Azure AD vorgenommen haben, die nicht im lokalen AD DS übernommen wurden, müssen Sie planen, wie der AD DS mit den aktualisierten Werten aufgefüllt werden soll, bevor Sie Ihre Objekte mithilfe von Azure AD Connect synchronisieren.

Wenn Sie Ihre Objekte mit einem Soft Match abgeglichen haben, wird dem Objekt in Azure AD ein **sourceAnchor** hinzugefügt, sodass später ein Hard Match verwendet werden kann.

### <a name="hard-match-vs-soft-match"></a>Hard Match im Vergleich zu Soft Match
Bei einer neuen Connect-Installation gibt es keinen praktischen Unterschied zwischen einem Soft Match und einem Hard Match. Der Unterschied kommt bei einer Notfallwiederherstellung zum Tragen. Wenn der Server mit Azure AD Connect nicht mehr vorhanden ist, können Sie eine neue Instanz installieren, ohne dass Daten verloren gehen. Während der Erstinstallation wird ein Objekt mit einem sourceAnchor-Parameter an Connect gesendet. Die Übereinstimmung kann dann vom Client (Azure AD Connect) ausgewertet werden – dies ist erheblich schneller als das gleiche Vorgehen in Azure AD. Ein Hard Match wird sowohl von Connect als auch von Azure AD ausgewertet. Ein Soft Match wird nur von Azure AD ausgewertet.

### <a name="other-objects-than-users"></a>Andere Objekte als Benutzer
Benutzer verfügen üblicherweise sowohl über ein userPrincipalName- als auch ein proxyAddresses-Attribut, sodass der Abgleich problemlos erfolgen kann. Andere Objekte jedoch, wie z.B. Sicherheitsgruppen, verfügen nicht über diese Attribute. In diesem Fall können Sie ein Hard Match nur anhand des sourceAnchor-Attributs erzielen. Der sourceAnchor ist immer die über Base64 konvertierte lokale **objectGUID**, daher müssen Sie den Wert in Azure AD aktualisieren, wenn Sie zwei Objekte benötigen, die abgeglichen werden sollen. Das sourceAnchor/immutableID-Attribut kann nur mithilfe von PowerShell aktualisiert werden, nicht über die Portale.

## <a name="create-a-new-on-premises-active-directory-from-data-in-azure-ad"></a>Erstellen eines neuen lokalen Active Directory aus Daten in Azure AD
Einige Kunden beginnen mit einer reinen Cloudlösung mit Azure AD und verfügen nicht über ein lokales Active Directory. Später möchten diese Kunden ihre lokalen Ressourcen nutzen und ein lokales Active Directory basierend auf den Azure AD-Daten aufbauen. Für dieses Szenario kann Azure AD Connect nicht eingesetzt werden. Connect erstellt keine lokalen Benutzer und bietet keine Möglichkeit, lokale Kennwörter auf die gleichen Kennwörter festzulegen, die in Azure AD verwendet werden.

Wenn der einzige Grund, aus dem Sie ein lokales Active Directory planen, die Unterstützung von branchenspezifischen Apps ist, sollten Sie stattdessen die Verwendung von [Azure AD Domain Services](../../active-directory-domain-services/index.md) erwägen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

