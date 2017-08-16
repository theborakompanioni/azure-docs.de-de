---
title: Bereitstellen von sicherem Remotezugriff auf lokale Apps
description: "Erläutert, wie Sie mit dem Azure AD-Anwendungsproxy sicheren Remotezugriff auf Ihre lokalen Anwendungen bereitstellen können."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 99523f27fe43f07081bd43f5d563e554bda4426f
ms.openlocfilehash: 67f7f5b8d411d11c97a8666d1bfc3c0c5f1174ce
ms.contentlocale: de-de
ms.lasthandoff: 08/05/2017

---

# <a name="how-to-provide-secure-remote-access-to-on-premises-applications"></a>Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen

Heutzutage möchten Mitarbeiter an jedem Ort, zu jeder Zeit und mit jedem Gerät produktiv arbeiten können. Sie möchten auf ihren eigenen Geräten arbeiten, z.B. Tablet-PCs, Smartphones oder Laptops. Und sie erwarten, sowohl auf SaaS-Apps in der Cloud als auch auf lokale Unternehmens-Apps zugreifen zu können. Für die Bereitstellung des Zugriffs auf lokale Anwendungen wurden bisher virtuelle private Netzwerke (VPNs) oder demilitarisierte Zonen (DMZs) eingesetzt. Diese Lösungen sind aber nicht nur komplex und schwer zu schützen, sondern können außerdem nur mit hohem Kostenaufwand eingerichtet und verwaltet werden.

Es gibt eine bessere Möglichkeit!

Moderne Mitarbeiter in einer Arbeitswelt mit dem Motto „mobile-first, cloud-first“ benötigen auch eine moderne Lösung für den Remotezugriff. Der Azure AD-Anwendungsproxy ist ein Feature von Azure Active Directory, das über den Remotezugriff als Dienst verfügt. Dies bedeutet, dass die Bereitstellung, Nutzung und Verwaltung einfach ist.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="what-is-azure-active-directory-application-proxy"></a>Was ist der Azure Active Directory-Anwendungsproxy?
Der Azure AD-Anwendungsproxy ermöglicht das einmalige Anmelden (SSO) und den sicheren Remotezugriff für lokal gehostete Webanwendungen. Einige Apps, die Sie veröffentlichen möchten, können SharePoint-Websites, Outlook Web Access oder beliebige andere Branchenwebanwendungen sein, die Sie einsetzen. Diese lokalen Webanwendungen sind in Azure AD integriert, also in die gleiche Identitäts- und Steuerungsplattform, die auch von O365 verwendet wird. Dadurch können Endbenutzer auf Ihre lokalen Anwendungen auf die gleiche Weise zugreifen wie auf O365 und andere SaaS-Apps, die in Azure AD integriert sind. Dafür müssen Sie weder die Netzwerkinfrastruktur ändern noch VPN verwenden.

## <a name="why-is-application-proxy-a-better-solution"></a>Warum ist Anwendungsproxy eine bessere Lösung?
Der Azure AD-Anwendungsproxy ist eine einfache, sichere und kostengünstige Lösung für den Remotezugriff auf alle lokalen Anwendungen.

Azure AD-Anwendungsproxy ist:

* **Einfach**
   * Sie brauchen Ihre Anwendungen nicht ändern oder aktualisieren, damit Sie mit dem Anwendungsproxy funktionieren. 
   * Ihre Benutzern erhalten einen einheitlichen Authentifizierungsvorgang. Sie können das MyApps-Portal verwenden, um einmaliges Anmelden oder SaaS-Apps in der Cloud sowie Ihre Apps lokal zu erhalten. 
* **Schützen**
   * Wenn Sie Ihre Apps mit dem Azure AD-Anwendungsproxy veröffentlichen, können Sie die umfassenden Autorisierungssteuerungen und Sicherheitsanalysen in Azure nutzen. Sie erhalten Sicherheit auf Cloudebene und Azure-Sicherheitsfeatures wie den bedingten Zugriff sowie die Überprüfung in zwei Schritten.
   * Sie müssen keine eingehenden Verbindungen über Ihre Firewall öffnen, um Benutzern Remotezugriff zu gewähren. 
* **Kosteneffizient**
   * Der Anwendungsproxy funktioniert in der Cloud, und Sie können Zeit und Kosten sparen. Für lokale Lösungen müssen Sie in der Regel DMZs, Edgeserver oder andere komplexe Infrastrukturen einrichten und verwalten.  

## <a name="what-kind-of-applications-work-with-application-proxy"></a>Welche Arten von Anwendungen können mit dem Anwendungsproxy verwendet werden?
Mit dem Azure AD-Anwendungsproxy können Sie auf verschiedene Arten von internen Anwendungen zugreifen:

* Webanwendungen, für die zur Authentifizierung die [integrierte Windows-Authentifizierung](active-directory-application-proxy-sso-using-kcd.md) verwendet wird  
* Webanwendungen mit formularbasiertem oder [headerbasiertem](application-proxy-ping-access.md) Zugriff  
* Web-APIs, die Sie für umfassende Anwendungen auf unterschiedlichen Geräten verfügbar machen möchten  
* Hinter einem [Remotedesktopgateway](application-proxy-publish-remote-desktop.md) gehostete Anwendungen  
* Rich Client-Apps, die in der Active Directory-Authentifizierungsbibliothek (Active Directory Authentication Library (ADAL)) integriert sind

## <a name="how-does-application-proxy-work"></a>Wie funktioniert der Anwendungsproxy?
Es gibt zwei Komponenten, die Sie konfigurieren müssen, damit der Anwendungsproxy funktioniert: ein Connector und ein externer Endpunkt. 

Der Connector ist ein einfacher Agent, der sich auf einem Windows-Server innerhalb Ihres Netzwerks befindet. Der Connector erleichtert den Datenfluss vom Anwendungsproxydienst in der Cloud zu Ihren lokalen Anwendungen. Es werden nur ausgehende Verbindungen verwendet, also müssen Sie keine eingehenden Ports öffnen oder Ressourcen in der DMZ anordnen. Connectors sind zustandslos und rufen alle Informationen nach Bedarf aus der Cloud ab. Weitere Informationen zu Connectors, z.B. wie der Lastenausgleich und die Authentifizierung funktioniert, finden Sie unter [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md). 

Der externe Endpunkt bedeutet, wie Ihre Benutzer Ihre Anwendungen erreichen, während sie sich selbst außerhalb des Netzwerks befinden. Sie können direkt zu einer externen URL wechseln, die Sie bestimmen, oder auf die Anwendung über das MyApps-Portal zugreifen. Wenn Benutzer einen dieser Endpunkte erreichen, authentifizieren sie sich in Azure AD und werden dann über den Connector an die lokale Anwendung geleitet.

 ![Azure AD-Anwendungsproxy – Diagramm](./media/active-directory-application-proxy-get-started/azureappproxxy.png)

1. Der Benutzer greift auf die Anwendung über den Anwendungsproxydienst zu und wird zur Authentifizierung an die Azure AD-Anmeldeseite geleitet.
2. Nach der erfolgreichen Anmeldung wird ein Token generiert und an das Clientgerät gesendet.
3. Der Client sendet das Token an den Anwendungsproxydienst, der den Benutzerprinzipalnamen (UPN) und den Sicherheitsprinzipalnamen (SPN) aus dem Token abruft und die Anforderung dann an den Anwendungsproxyconnector leitet.
4. Wenn Sie das SSO konfiguriert haben, führt der Connector jede weitere erforderliche Authentifizierung im Namen des Benutzers durch.
5. Der Connector sendet die Anforderung an die lokale Anwendung.  
6. Die Antwort wird über den Anwendungsproxydienst und -connector an den Benutzer gesendet.

### <a name="single-sign-on"></a>Einmaliges Anmelden
Der Azure AD-Anwendungsproxy ermöglicht einmaliges Anmelden (SSO) für Anwendungen, die die integrierte Windows-Authentifizierung (IWA) verwenden oder Ansprüche unterstützen. Wenn Ihre Anwendung die integrierte Windows-Authentifizierung verwendet, nimmt der Anwendungsproxy mithilfe der eingeschränkten Kerberos-Delegierung die Identität des Benutzers an, um das einmalige Anmelden zu ermöglichen. Wenn Sie über eine Anwendung verfügen, die Ansprüche unterstützt und Azure Active Directory vertraut, funktioniert das einmalige Anmelden, da der Benutzer bereits über Azure AD authentifiziert wurde.

Weitere Informationen zu Kerberos finden Sie unter [All you want to know about Kerberos Constrained Delegation (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd) (Alles über die eingeschränkte Kerberos-Delegierung, KCD).

### <a name="managing-apps"></a>Verwalten von Apps
Sobald Ihre App mit dem Anwendungsproxy veröffentlicht wurde, können Sie sie wie jede andere Unternehmens-App im Azure-Portal verwalten. Sie können Sicherheitsfunktionen von Azure Active Directory wie den bedingten Zugriff und die Überprüfung in zwei Schritten verwenden, Benutzerberechtigungen steuern und das Branding für Ihre App anpassen. 

## <a name="get-started"></a>Erste Schritte

Bevor Sie den Anwendungsproxy konfigurieren, stellen Sie sicher, dass Sie über eine unterstützte [Azure Active Directory-Edition](https://azure.microsoft.com/pricing/details/active-directory/) sowie ein Azure AD-Verzeichnis verfügen, für das Sie der globale Administrator sind.

Starten mit dem Anwendungsproxy in zwei Schritten:

1. [Aktivieren des Anwendungsproxys und Konfigurieren des Connectors](active-directory-application-proxy-enable.md).    
2. [Veröffentlichen von Anwendungen](active-directory-application-proxy-publish.md) – Verwenden Sie den schnell und einfach einzusetzenden Assistenten, um Ihre lokalen Apps zu veröffentlichen und den Remotezugriff darauf zu ermöglichen.

## <a name="whats-next"></a>Wie geht es weiter?
Sobald Sie Ihre erste App veröffentlicht haben, bietet Ihnen der Anwendungsproxy noch viele weitere Möglichkeiten:

* [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)
* [Veröffentlichen von Anwendungen mit Ihrem eigenen Domänennamen](active-directory-application-proxy-custom-domains.md)
* [Informationen zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)
* [Arbeiten mit vorhandenen lokalen Proxyservern](application-proxy-working-with-proxy-servers.md) 
* [Festlegen einer benutzerdefinierten Startseite](application-proxy-office365-app-launcher.md)

Aktuelle Neuigkeiten und Updates finden Sie im [Blog zum Anwendungsproxy](http://blogs.technet.com/b/applicationproxyblog/)


