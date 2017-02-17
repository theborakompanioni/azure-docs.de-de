---
title: "Privileged Identity Management-Abonnements – Azure | Microsoft-Dokumentation"
description: "Erläutert die Abonnement- und Lizenzierungsanforderungen für die Verwaltung und Verwendung von Azure AD Privileged Identity Management im Mandanten."
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: mwahl
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2017
ms.author: barclayn
translationtype: Human Translation
ms.sourcegitcommit: 9364a1449ba17568c82832bc1e97d40febbb30ab
ms.openlocfilehash: c6aea0b7280ad8f1365c04203c78a1499ba0aa58


---

# <a name="azure-active-directory-privileged-identity-management-subscription-requirements"></a>Abonnementanforderungen für Azure Active Directory Privileged Identity Management

Azure AD Privileged Identity Management ist nur mit der Premium P2-Edition von Azure AD verfügbar. Weitere Informationen zu anderen Features von P2 und eine Gegenüberstellung zu Premium P1 finden Sie unter [Azure Active Directory-Editionen](../active-directory-editions.md).

>[!NOTE]
In der Preview-Version von Azure Active Directory (Azure AD) Privileged Identity Management wurden keine Lizenzüberprüfungen ausgeführt, wenn Mandanten den Dienst testen wollten.  Jetzt, da Azure AD Privileged Identity Management allgemein verfügbar ist, muss der Mandant ab Dezember 2016 über ein Testabonnement oder ein kostenpflichtiges Abonnement verfügen, um Privileged Identity Management weiterhin verwenden zu können.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Bestätigen Ihres Test- oder kostenpflichtigen Abonnements

Wenn Sie nicht sicher sind, ob Ihre Organisation ein Testabonnement verwendet oder ein kostenpflichtiges Abonnement erworben hat, können Sie mithilfe der Befehle im Azure Active Directory-Modul für Windows PowerShell V1 überprüfen, ob Ihr Mandant über ein Abonnement verfügt. 
1. Öffnen Sie ein PowerShell-Fenster.
2. Geben Sie `Connect-MsolService` ein, um sich im Mandanten als Benutzer zu authentifizieren.
3. Geben Sie `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status` ein.

Dieser Befehl ruft eine Liste der Abonnements im Mandanten ab. Werden keine Zeilen zurückgegeben, müssen Sie ein Azure AD Premium P2-Testabonnement, ein Azure AD Premium P2-Abonnement oder ein EMS E5-Abonnement erwerben, um Azure AD Privileged Identity Management verwenden zu können.  Informationen zum Beziehen eines Testabonnements und zum Einstieg in Azure AD Privileged Identity Management finden Sie unter [Erste Schritte mit Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

Wenn dieser Befehl eine Zeile zurückgibt, in „SkuPartNumber“ den Wert „AAD_PREMIUM_P2“ oder „EMSPREMIUM“ und „IsTrial“ den Wert „True“ aufweist, bedeutet dies, dass ein Testabonnement von Azure AD Premium P2 im Mandanten vorhanden ist.  Wenn der Abonnementstatus nicht aktiviert ist, und Sie über kein Azure AD Premium P2- oder EMS E5-Abonnement verfügen, müssen Sie ein Azure AD Premium P2- oder ein EMS E5-Abonnement erwerben, um Azure AD Privileged Identity Management weiterhin verwenden zu können.

Azure AD Premium P2ist über ein [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), das [Open License-Volumenlizenzprogramm](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx) und das [Cloud Solution Providers-Programm](https://partner.microsoft.com/en-US/cloud-solution-provider) erhältlich. Azure- und Office 365-Abonnenten können Azure AD Premium P2 auch online erwerben.  Weitere Informationen zu den Preisen von Azure AD Premium und zur Onlinebestellung finden Sie unter [Azure Active Directory-Preise](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>Azure AD Privileged Identity Management ist im Mandanten nicht vorhanden

Azure AD Privileged Identity Management ist in folgenden Fällen im Mandanten nicht mehr verfügbar:
- Ihre Organisation hat Azure AD Privileged Identity Management in der Preview-Version verwendet und hat kein Azure AD Premium P2- oder EMS E5-Abonnement erworben.
- Ihre Organisation hat ein Azure AD Premium P2- oder EMS E5-Testabonnement verwendet, das abgelaufen ist.
- Ihre Organisation hat ein Abonnement erworben, das abgelaufen ist.

Wenn ein Azure AD Premium P2-oder EMS E5-Abonnement abläuft oder eine Organisation Azure AD Privileged Identity Management in der Preview-Version verwendet hat und kein Azure AD Premium P2- oder EMS E5-Abonnement erwirbt, geschieht Folgendes:

- Permanente Rollenzuweisungen für Azure AD-Rollen sind nicht betroffen.
- Die Azure AD Privileged Identity Management-Erweiterung im Azure-Portal sowie die Graph-API-Cmdlets und PowerShell-Schnittstellen von Azure AD Privileged Identity Management sind für Benutzer nicht mehr verfügbar, um privilegierte Rollen zu aktivieren, den privilegierten Zugriff zu verwalten oder Zugriffsüberprüfungen privilegierter Rollen auszuführen.
- Berechtigte Rollenzuweisungen von Azure AD-Rollen werden entfernt, da Benutzer keine privilegierten Rollen mehr aktivieren können.
- Alle laufenden Zugriffsüberprüfungen von Azure AD-Rollen enden, und Azure AD Privileged Identity Management-Konfigurationseinstellungen werden entfernt.
- Azure AD Privileged Identity Management sendet keine E-Mails mehr zu Rollenzuweisungsänderungen.

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md)
- [Rollen in Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-roles.md)



<!--HONumber=Feb17_HO3-->


