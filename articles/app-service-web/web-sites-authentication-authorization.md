---
title: Authentifizieren mit lokaler Active Directory-Instanz in Ihrer Azure-App | Microsoft Docs
description: Erfahren Sie mehr über die verschiedenen Optionen für Branchen-Apps in Azure App Service für die Authentifizierung mit lokaler Active Directory-Instanz.
services: app-service
documentationcenter: ''
author: cephalin
manager: wpickett
editor: jimbe

ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/31/2016
ms.author: cephalin

---
# Authentifizieren mit lokaler Active Directory-Instanz in Ihrer Azure-App
In diesem Artikel erfahren Sie, wie Sie sich mit einer lokalen Active Directory (AD)-Instanz bei [Azure App Service](../app-service/app-service-value-prop-what-is.md) authentifizieren. Eine Azure-App wird zwar in der Cloud gehostet, es stehen aber dennoch Optionen für eine sichere Authentifizierung lokaler AD-Benutzer zur Verfügung.

## Authentifizieren über Azure Active Directory
Ein Azure Active Directory-Mandant kann mit einem lokalen AD-Verzeichnis synchronisiert sein. Dieser Ansatz ermöglicht AD-Benutzer den Zugriff auf Ihre App aus dem Internet und die Authentifizierung mit ihren lokalen Anmeldeinformationen. Darüber hinaus bietet Azure App Service eine [fertige Lösung für diese Methode](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Mit nur wenigen Mausklicks können Sie die Authentifizierung mit einem Mandanten mit Verzeichnissynchronisierung für Ihre Azure-App aktivieren. Dieser Ansatz bietet die folgenden Vorteile:

* Kein Authentifizierungscode in der App erforderlich. Überlassen Sie App Service die Authentifizierung, und konzentrieren Sie sich auf die Bereitstellung von Funktionen in Ihrer App.
* [Azure AD Graph-API](http://msdn.microsoft.com/library/azure/hh974476.aspx) ermöglicht den Zugriff auf Verzeichnisdaten aus Ihrer Azure-App.
* Bereitstellung von SSO für [alle von Azure Active Directory unterstützten Anwendungen](/marketplace/active-directory/), einschließlich Office 365, Dynamics CRM Online, Microsoft Intune sowie Tausender Cloudanwendungen von Drittanbietern.
* Azure Active Directory unterstützt die rollenbasierte Zugriffssteuerung. Sie können das Muster „[Authorize(Roles="X")]“ mit minimalen Änderungen am Code verwenden.

Informationen zum Schreiben einer Azure-Branchen-App, die sich mit Azure Active Directory authentifiziert, finden Sie unter [Erstellen einer Azure-Branchen-App mit Azure Active Directory-Authentifizierung](web-sites-dotnet-lob-application-azure-ad.md).

## Authentifizieren über einen lokalen STS
Bei Verwendung eines lokalen Sicherheitstokendiensts (Secure Token Service, STS) wie Active Directory-Verbunddienste (AD FS) können Sie diesen im Rahmen eines Authentifizierungsverbunds für Ihre Azure-App nutzen. Dieser Ansatz ist optimal, wenn die Unternehmensrichtlinie das Speichern von AD-Daten in Azure verhindert. Beachten Sie jedoch Folgendes:

* Die STS-Topologie muss lokal bereitgestellt werden, was mit Kosten und Verwaltungsaufwand verbunden ist.
* Nur AD FS-Administratoren können [Vertrauensstellungen der vertrauenden Seite und Anspruchsregeln](http://technet.microsoft.com/library/dd807108.aspx) konfigurieren. Diese können die Optionen von Entwicklern einschränken. Andererseits ist es möglich, [Ansprüche](http://technet.microsoft.com/library/ee913571.aspx) anwendungsspezifisch zu verwalten und anzupassen.
* Für den Zugriff auf lokale AD-Daten über die Unternehmensfirewall ist eine separate Lösung erforderlich.

Informationen zum Schreiben einer Azure-Branchen-App, die sich mit einem lokalen STS authentifiziert, finden Sie unter [Erstellen einer Azure-Branchen-App mit AD FS-Authentifizierung](web-sites-dotnet-lob-application-adfs.md).

<!---HONumber=AcomDC_0831_2016-->