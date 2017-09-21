---
title: "Best Practices für den bedingten Zugriff in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, was Sie wissen sollten und was Sie beim Konfigurieren von Richtlinien für den bedingten Zugriff vermeiden sollten."
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
ms.sourcegitcommit: f2ac16c2f514aaa7e3f90fdf0d0b6d2912ef8485
ms.openlocfilehash: fedc72f8fe1ada9a991d417cc77b8ca659589f55
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Best Practices für den bedingten Zugriff in Azure Active Directory

In diesem Thema erfahren Sie, was Sie wissen sollten und was Sie beim Konfigurieren von Richtlinien für den bedingten Zugriff vermeiden sollten. Bevor Sie dieses Thema lesen, sollten Sie sich mit den Konzepten und der Terminologie vertraut machen, die in [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md) beschrieben werden.

## <a name="what-you-should-know"></a>Wichtige Informationen

### <a name="whats-required-to-make-a-policy-work"></a>Was ist erforderlich, damit eine Richtlinie funktioniert?

Wenn Sie eine neue Richtlinie erstellen, wurden keine Benutzer, Gruppen, Anwendungen oder Zugriffssteuerungen ausgewählt.

![Cloud-Apps](./media/active-directory-conditional-access-best-practices/02.png)


Damit Ihre Richtlinie funktioniert, müssen Sie Folgendes konfigurieren:


|Was           | Vorgehensweise                                  | Warum|
|:--            | :--                                  | :-- |
|**Cloud-Apps** |Sie müssen eine oder mehrere Apps auswählen.  | Das Ziel einer Richtlinie für bedingten Zugriff liegt darin, es Ihnen zu ermöglichen, genau festzulegen, wie autorisierte Benutzer auf Ihre Apps zugreifen können.|
| **Benutzer und Gruppen** | Sie müssen mindestens einen Benutzer oder eine Gruppe auswählen, die autorisiert ist, auf die ausgewählten Cloud-Apps zuzugreifen. | Eine Richtlinie für den bedingten Zugriff, der keine Benutzer und Gruppen zugewiesen sind, wird niemals angewendet. |
| **Steuerelemente** | Wählen Sie mindestens eine Zugriffssteuerung aus. | Ihr Richtlinienprozessor muss wissen, was er zu tun hat, wenn die Bedingungen erfüllt sind.|


Zusätzlich zu diesen grundlegenden Anforderungen sollten Sie in vielen Fällen auch eine Bedingung konfigurieren. Während eine Richtlinie auch ohne eine konfigurierte Bedingung funktionieren würde, sind die Bedingungen ausschlaggebend für die Optimierung des Zugriffs auf Ihre Apps.


![Cloud-Apps](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Wie werden Zuweisungen ausgewertet?

Alle Zuweisungen sind logisch per **UND**-Operator verbunden. Wenn Sie mehr als eine Zuweisung konfiguriert haben, müssen die Bedingungen aller Zuweisungen erfüllt sein, damit die Richtlinie ausgelöst wird.  

Falls Sie eine Standortbedingung konfigurieren müssen, die für alle Verbindungen von außerhalb des Organisationsnetzwerks gelten, können Sie dies wie folgt erreichen:

- Einschließen: **All locations** (Alle Standorte)
- Ausschließen: **All trusted IPs** (Alle vertrauenswürdigen IPs)

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Was passiert, wenn Sie im klassischen Azure-Portal und im Azure-Portal Richtlinien konfiguriert haben?  
Beide Richtlinien werden von Azure Active Directory erzwungen, und ein Benutzer erhält nur dann Zugriff, wenn alle Anforderungen erfüllt sind.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Was passiert, wenn Sie im Intune Silverlight-Portal und im Azure-Portal über Richtlinien verfügen?
Beide Richtlinien werden von Azure Active Directory erzwungen, und ein Benutzer erhält nur dann Zugriff, wenn alle Anforderungen erfüllt sind.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Was passiert, wenn ich mehrere Richtlinien für denselben Benutzer konfiguriert habe?  
Bei jeder Anmeldung werden von Azure Active Directory alle Richtlinien ausgewertet, und es wird sichergestellt, dass alle Anforderungen erfüllt sind, bevor dem Benutzer der Zugriff gewährt wird.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Funktioniert der bedingte Zugriff mit Exchange ActiveSync?

Ja, Sie können Exchange ActiveSync in einer Richtlinie für den bedingten Zugriff verwenden.


## <a name="what-you-should-avoid-doing"></a>Das sollten Sie vermeiden

Das Framework für bedingten Zugriff bietet Ihnen mehr Flexibilität bei der Konfiguration. Mehr Flexibilität bedeutet jedoch auch, dass Sie jede Konfigurationsrichtlinie vor dem Freigeben sorgfältig prüfen sollten, um unerwünschte Ergebnisse zu vermeiden. Achten Sie in diesem Fall besonders auf Zuweisungen, die sich auf komplette Sätze auswirken, z.B. **alle Benutzer/Gruppen/Cloud-Apps**.

Vermeiden Sie in Ihrer Umgebung die folgenden Konfigurationen:


**Für alle Benutzer, alle Cloud-Apps:**

- **Zugriff blockieren:** Diese Konfiguration blockiert Ihre gesamte Organisation, was in keinem Fall wünschenswert ist.

- **Erfordert kompatibles Gerät:** Für Benutzer, die ihre Geräte noch nicht registriert haben, blockiert diese Richtlinie den gesamten Zugriff, einschließlich des Zugriffs auf das Intune-Portal. Wenn Sie ein Administrator ohne registriertes Gerät sind, verhindert diese Richtlinie auch, dass Sie in das Azure-Portal zurückkehren und die Richtlinie ändern können.

- **Erfordert Domänenbeitritt:** Diese Richtlinie blockiert potenziell den Zugriff für alle Benutzer in Ihrer Organisation, wenn Sie noch nicht über in die Domäne eingebundene Geräte verfügen.


**Für alle Benutzer, alle Cloud-Apps, alle Geräteplattformen:**

- **Zugriff blockieren:** Diese Konfiguration blockiert Ihre gesamte Organisation, was in keinem Fall wünschenswert ist.



## <a name="policy-migration"></a>Richtlinienmigration

Falls Sie Richtlinien im klassischen Azure-Portal konfiguriert haben, sollten Sie diese aus folgenden Gründen migrieren:


- Ein Benutzer, der sowohl in einer Richtlinie des klassischen Azure-Portals als auch in einer Richtlinie des Azure-Portals enthalten ist, muss die Anforderungen beider Richtlinien erfüllen. 

- Wenn Sie Ihre vorhandenen Richtlinien nicht migrieren, können Sie keine Richtlinien für die Zugriffsgewährung implementieren.


### <a name="migration-from-the-azure-classic-portal"></a>Migration aus dem klassischen Azure-Portal

Szenario: 

- In Ihrem [klassischen Azure-Portal](https://manage.windowsazure.com) ist Folgendes konfiguriert:

    - SharePoint Online

    ![Bedingter Zugriff](./media/active-directory-conditional-access-best-practices/14.png)

    - Eine gerätebasierte Richtlinie zum bedingten Zugriff

    ![Bedingter Zugriff](./media/active-directory-conditional-access-best-practices/15.png)

- Sie möchten im Azure-Portal eine Richtlinie zum bedingten Zugriff mit mobiler Anwendungsverwaltung konfigurieren. 
 

#### <a name="configuration"></a>Konfiguration 

- Überprüfen Sie Ihre gerätebasierten Richtlinien zum bedingten Zugriff.

- Migrieren Sie sie zum Azure-Portal. 

- Fügen Sie Richtlinien zum bedingten Zugriff mit mobiler Anwendungsverwaltung hinzu.


### <a name="migrating-from-intune"></a>Migrieren aus Intune 

Szenario:

- In [Intune](https://portal.azure.com/#blade/Microsoft_Intune/SummaryBlade ) ist eine Richtlinie zum bedingten Zugriff mit mobiler Anwendungsverwaltung für Exchange Online oder SharePoint Online konfiguriert.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-best-practices/15.png)

- Sie möchten im Azure-Portal zur Verwendung von bedingtem Zugriff mit mobiler Anwendungsverwaltung migrieren.


#### <a name="configuration"></a>Konfiguration 
 
- Überprüfen Sie Ihre gerätebasierten Richtlinien zum bedingten Zugriff.

- Migrieren Sie sie zum Azure-Portal. 

- Überprüfen Sie Ihre Richtlinien zum bedingten Zugriff mit mobiler Anwendungsverwaltung, die in Intune für Exchange Online oder SharePoint Online konfiguriert sind.

- Geben Sie zusätzlich zur gerätebasierten Kontrolle an, dass auch genehmigte Anwendungen erforderlich** **sind. 
 

### <a name="migrating-from-the-azure-classic-portal-and-intune"></a>Migrieren aus dem klassischen Azure-Portal und aus Intune

Szenario:

- Folgendes ist konfiguriert:

    - **Klassisches Azure-Portal:** gerätebasierter bedingter Zugriff 

    - **Intune:** Richtlinien zum bedingten Zugriff mit mobiler Anwendungsverwaltung 
    
- Sie möchten beide Richtlinien im Azure-Portal zur Verwendung von Richtlinien zum bedingtem Zugriff mit mobiler Anwendungsverwaltung migrieren.


#### <a name="configuration"></a>Konfiguration

- Überprüfen Sie Ihre gerätebasierten Richtlinien zum bedingten Zugriff.

- Migrieren Sie sie zum Azure-Portal. 

- Überprüfen Sie Ihre Richtlinie zum bedingten Zugriff mit mobiler Anwendungsverwaltung, die in Intune für Exchange Online oder SharePoint Online konfiguriert ist.

- Geben Sie zusätzlich zur gerätebasierten Kontrolle an, dass auch genehmigte Anwendungen erforderlich** **sind. 












## <a name="common-scenarios"></a>Gängige Szenarien

### <a name="requiring-multi-factor-authentication-for-apps"></a>Erzwingen von Multi-Factor Authentication für Apps

Viele Umgebungen verfügen über Apps, für die eine höhere Schutzebene als für andere Apps erforderlich ist.
Dies gilt beispielsweise für Apps, für die Zugriff auf sensible Daten besteht.
Wenn Sie diesen Apps eine weitere Schutzebene hinzufügen möchten, können Sie eine Richtlinie für den bedingten Zugriff konfigurieren, für die eine Multi-Factor Authentication erforderlich ist, wenn Benutzer auf diese Apps zugreifen.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Erzwingen der Multi-Factor Authentication für den Zugriff aus nicht vertrauenswürdigen Netzwerken

Dieses Szenario ähnelt dem vorherigen Szenario, da hiermit eine Anforderung für die Multi-Factor Authentication hinzugefügt wird.
Der Hauptunterschied ist aber die Bedingung für diese Anforderung.  
Während der Schwerpunkt des vorherigen Szenarios auf Apps mit Zugriff auf sensible Daten lag, geht es in diesem Szenario nun um vertrauenswürdige Standorte.  
Anders ausgedrückt: Sie können beispielsweise die Multi-Factor Authentication erzwingen, wenn auf eine App von einem Benutzer über ein Netzwerk zugegriffen wird, das für Sie nicht vertrauenswürdig ist.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Nur mit vertrauenswürdigen Geräten kann auf Office 365-Dienste zugegriffen werden

Wenn Sie Intune in Ihrer Umgebung verwenden, können Sie sofort damit beginnen, die Oberfläche für Richtlinien für den bedingten Zugriff in der Azure-Konsole zu verwenden.

Viele Intune-Kunden nutzen den bedingten Zugriff, um sicherzustellen, dass nur vertrauenswürdige Geräte auf Office 365-Dienste zugreifen können. Dies bedeutet, dass mobile Geräte bei Intune registriert werden und die Anforderungen der Konformitätsrichtlinien erfüllen und dass Windows-PCs in eine lokale Domäne eingebunden werden. Eine wichtige Verbesserung besteht darin, dass Sie eine Richtlinie nicht für jeden Office 365-Dienst neu festlegen müssen.  Konfigurieren Sie die Cloud-Apps beim Erstellen einer neuen Richtlinie so, dass alle O365-Apps eingebunden sind, die Sie per bedingtem Zugriff schützen möchten.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

