---
title: Konfigurieren des einmaligen Anmeldens bei einer Anwendungsproxyanwendung | Microsoft-Dokumentation
description: Schnelles Konfigurieren des einmaligen Anmeldens bei einer Anwendungsproxyanwendung
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 96d05bd6bdbe0006aa1f8fff7b518f11cebc5d0a
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

<a id="how-to-configure-single-sign-on-to-an-application-proxy-application" class="xliff"></a>

# Konfigurieren des einmaligen Anmeldens bei einer Anwendungsproxyanwendung

Das einmalige Anmelden (Single Sign-On, SSO) gestattet Benutzern den Zugriff auf eine Anwendung, ohne sich mehrfach authentifizieren zu müssen. Es ermöglicht, dass eine einzelne Authentifizierung in der Cloud bei Azure Active Directory erfolgt. Der Dienst oder Connector kann dann die Identität des Benutzers annehmen, um alle weiteren Authentifizierungsaufgaben der Anwendung abzuschließen.

<a id="how-to-configure-single-sign-on" class="xliff"></a>

## Konfigurieren der einmaligen Anmeldung
Stellen Sie zum Konfigurieren des einmaligen Anmeldens zunächst sicher, dass Ihre Anwendung für die Vorauthentifizierung über Azure Active Directory konfiguriert ist. Wechseln Sie zu diesem Zweck zu **Azure Active Directory** -&gt; **Unternehmens-Apps** -&gt; **Alle Anwendungen** -&gt; Ihre Anwendung **-&gt; Anwendungsproxy**. Auf dieser Seite befindet sich das Feld „Vorauthentifizierung“. Stellen Sie sicher, dass hier der Wert „Azure Active Directory“ festgelegt ist. 

Weitere Informationen zu den Methoden zur Vorauthentifizierung finden Sie unter Schritt 4 des [Dokuments zur App-Veröffentlichung](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

   ![Methode zur Vorauthentifizierung im Azure-Portal](./media/application-proxy-config-sso-how-to/app-proxy.png)

<a id="configuring-single-sign-on-modes-for-application-proxy-applications" class="xliff"></a>

## Konfigurieren der Modi für einmaliges Anmelden für Anwendungsproxyanwendungen
Als Nächstes wird der bestimmte Typ des einmaligen Anmeldens konfiguriert. Die Anmeldemethoden sind auf Basis des Authentifizierungstyps klassifiziert, der von der Back-End-Anwendung verwendet wird. App-Proxyanwendungen unterstützten drei Arten von Anmeldungen:

-   **Kennwortbasierte Anmeldung**: Die kennwortbasierte Anmeldung kann für jede Anwendung verwendet werden, die Felder für Benutzername und Kennwort für die Anmeldung verwendet. Entsprechende Konfigurationsschritte finden Sie in unserer [Dokumentation zur Kennwort-SSO-Konfiguration](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#bring-your-own-password-sso-applications).

-   **Integrierte Windows-Authentifizierung**: Für Anwendungen, die die integrierte Windows-Authentifizierung (IWA) verwenden, wird das einmalige Anmelden durch die eingeschränkte Kerberos-Delegierung (KCD) ermöglicht. Dadurch erhalten Anwendungsproxyconnectors in Active Directory die Berechtigung, die Identität von Benutzern anzunehmen und in deren Auftrag Token zu senden und zu empfangen. Informationen zur KCD-Konfiguration finden Sie der [Dokumentation zum einmaligen Anmelden mit KCD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd).

-   **Headerbasierte Anmeldung**: Die headerbasierte Anmeldung wird über eine Partnerschaft ermöglicht und erfordert zusätzliche Konfigurationsschritte. Ausführliche Informationen zur Partnerschaft sowie schrittweise Anweisungen zum Konfigurieren des einmaligen Anmeldens bei einer Anwendung, die Header zur Authentifizierung verwendet, finden Sie in der [Dokumentation zu PingAccess für Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).

Jede dieser Optionen können Sie finden, indem Sie in „Unternehmens-Apps“ zu Ihrer Anwendung wechseln und im linken Menü die Seite **Einmaliges Anmelden** öffnen. Beachten Sie, dass möglicherweise nicht alle Optionen angezeigt werden, wenn Ihre Anwendung im alten Portal erstellt wurde.

Auf dieser Seite befindet sich auch eine zusätzliche Option für einmaliges Anmelden: Anmeldung über Link. Dies wird auch vom Anwendungsproxy unterstützt. Beachten Sie jedoch, dass mit dieser Option nicht das einmalige Anmelden zur Anwendung hinzugefügt wird. Dies bedeutet, dass für die Anwendung das einmalige Anmelden möglicherweise bereits über einen anderen Dienst implementiert ist, z. B. Active Directory-Verbunddienste. 

Mit dieser Option kann einen Administrator einen Link zu einer Anwendung erstellen, zu dem Benutzer zuerst gelangen, wenn sie auf die Anwendung zugreifen. Ist eine Anwendung beispielsweise zum Authentifizieren von Benutzern mit Active Directory Federation Services 2.0 konfiguriert, kann ein Administrator mit der Option „Anmeldung über Link“ einen entsprechenden Link im Zugriffsbereich erstellen.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md)

