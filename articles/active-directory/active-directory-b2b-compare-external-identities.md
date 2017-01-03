---
title: "Vergleich von Funktionen zum Verwalten externer Identitäten mithilfe von Azure Active Directory | Microsoft Docs"
description: "Vergleicht Azure Active Directory B2B-Zusammenarbeit, B2C und die mehrinstanzenfähige App zur Unterstützung der Authentifizierung und der Autorisierung für externe Identitäten"
services: active-directory
documentationcenter: 
author: arvindsuthar
manager: cliffdi
editor: 
tags: 
ms.assetid: 749b2e3a-2b8e-45ba-9f2a-6ca56eb1053b
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/24/2016
ms.author: asuthar
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: a9a7c0a9adea7fe4fccaa4ec9df0b077cd1e7382


---
# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Vergleich von Funktionen zum Verwalten externer Identitäten mithilfe von Active Directory
Zusätzlich zur Verwaltung des mobilen Zugriffs von Mitarbeitern auf SaaS-Apps kann Ihr Unternehmen mithilfe von Azure Active Directory (Azure AD) auch Ressourcen für Geschäftspartner freigeben und Anwendungen für Unternehmen und Kunden bereitstellen.

## <a name="developing-applications-for-businesses"></a>Anwendungsentwicklung für Unternehmen
Bieten Sie einen Dienst oder eine Anwendung, wie z. B. einen Lohnbuchhaltungsdienst, für Unternehmen an? Azure AD bietet die Identitätsplattform, mithilfe derer Sie Anwendungen für Millionen von Unternehmen erstellen können, die Azure AD als Teil der Bereitstellung von Office 365 oder anderer Unternehmensdienste konfiguriert haben.

**Beispiel:** Ein Pharmahändler verkauft Medizinprodukte und Informationssysteme an Unternehmen in der Gesundheitsbranche. Er benötigt eine Analyseanwendung für Arztpraxen und möchte, dass die Kunden ihre eigenen Identitäten verwalten können. Daher hat sich das Unternehmen für Azure AD als Identitätsplattform für eine Anwendung zur Arztpraxenverwaltung entschieden, wobei den Kunden bei der Anmeldung bei Bedarf Azure AD-Identitäten zur Verfügung gestellt werden. Weitere Informationen finden Sie im [Entwicklerhandbuch zu Azure Active Directory](active-directory-developers-guide.md).

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Zugriff von Geschäftspartnern auf Ihre Unternehmensressourcen
Benötigen Ihre Geschäftspartner oder andere Personen außerhalb Ihres Unternehmens Zugriff auf Ihre Unternehmensressourcen, wie z. B. eine SharePoint-Website oder Ihr ERP-System? Azure AD ermöglicht es Administratoren, externen Benutzern (die möglicherweise in Azure AD vorhanden sind) über die einmalige Anmeldung den Zugriff auf Unternehmensanwendungen zu gewähren. Dies erhöht die Sicherheit, da die Benutzer keinen Zugriff mehr haben, wenn sie das Partnerunternehmen verlassen, während Sie die Zugriffsrichtlinien innerhalb Ihres Unternehmens kontrollieren. Dies vereinfacht auch die Verwaltung, da Sie weder Verzeichnisse externer Partner noch Partnerbeziehungen verwalten müssen.

**Beispiel:** Ein Fotounternehmen bietet Einzelhändlern Dienstleistungen für Fotoprodukte an und verfügt über das größte Vertriebsnetz von Fotostationen. Es hat sich für Azure AD entschieden, damit Tausende von Benutzern bei seinen Einzelhandelspartnern mit ihren Anmeldeinformationen die neuesten Marketingmaterialien herunterladen und im Lieferantenextranet des Unternehmens Bildbearbeitungszubehör bestellen können. Weitere Informationen finden Sie unter [Azure AD B2B-Zusammenarbeit](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Anwendungsentwicklung für Verbraucher
Möchten Sie sicher und kostengünstig Onlineanwendungen, wie z. B. Ihr Warenverzeichnis, für Millionen von Kunden veröffentlichen? Azure AD bietet eine Plattform, die den Nutzern Ihrer Anwendung eine Anmeldung über soziale Netzwerke oder über Benutzername und Kennwort sowie die Self-Service-Registrierung und Self-Service-Kennwortzurücksetzung erlaubt. Dies verbessert die Benutzerfreundlichkeit und verringert gleichzeitig den Aufwand für Ihre Entwickler.

**Beispiel:** Das weltweit größte Franchiseunternehmen in der Sportbranche möchte direkt mit seinen 450 Mio. Fans in Kontakt treten.\# Dafür hat es eine mobile App erstellt, bei der Azure AD für die Benutzerauthentifizierung und die Profilspeicherung genutzt wird. Über soziale Netzwerke wie etwa Facebook können sich die Fans einfacher registrieren und anmelden. Dies ist alternativ aber auch über Benutzername und Kennwort möglich, und die Benutzer können die App somit reibungslos auf iOS-, Android- und Windows-Smartphones verwenden. Der Programmieraufwand wurde dadurch verringert, dass die App auf der Azure AD-Plattform aufbaut und gleichzeitig ein individuelles Branding für das Franchiseunternehmen ermöglicht und Bedenken in Bezug auf Sicherheit, Datenschutz und Skalierbarkeit ausräumt. Weitere Informationen finden Sie im [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Vergleich von Azure AD-Funktionen
Azure AD-Funktionen bieten eine Lösung für jedes der in diesem Artikel beschriebenen Szenarien. In der folgenden Tabelle finden Sie einen Überblick darüber, welche Funktionen für Sie am relevantesten sind:

| **Dieses Produkt wäre etwas für Sie...** | [Mehrinstanzenfähige Azure AD-SaaS-App](active-directory-developers-guide.md) | [Azure AD B2B-Zusammenarbeit](active-directory-b2b-what-is-azure-ad-b2b.md) | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/) |
| --- | --- | --- | --- |
| **Wenn Sie Folgendes bereitstellen möchten...** |Einen Dienst für Unternehmen |Partnerzugriff auf meine Apps |Einen Dienst für Kunden |
| **Wenn Sie diesem Unternehmen ähneln...** |Pharmahändler |Fotounternehmen |Franchiseunternehmen in der Sportbranche |
| **Wenn Sie die App für Folgendes bereitstellen...** |Arztpraxenverwaltung |Lieferantenextranet |Fußballfans |
| **Zielgruppe...** |Arztpraxen |Zugelassene Geschäftspartner |Jeder Benutzer mit einer E-Mail-Adresse |
| **Zugriff ist möglich, wenn...** |Der Administrator des Kunden zustimmt |Auf Einladung Ihres Administrators |Der Kunde sich registriert |




<!--HONumber=Jan17_HO1-->


