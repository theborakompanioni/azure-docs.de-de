---
title: Bereitstellen von sicherem Remotezugriff auf lokale Apps
description: "Erläutert, wie Sie mit dem Azure AD-Anwendungsproxy sicheren Remotezugriff auf Ihre lokalen Anwendungen bereitstellen können."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2e7815702f2d2f4ce935826c4769838727a83696
ms.openlocfilehash: 7d1be1dea6ed4ecda196743f592456a5b977e9b0


---
# <a name="how-to-provide-secure-remote-access-to-on-premises-applications"></a>Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen
> [!NOTE]
> Das Feature "Anwendungsproxy" ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
> 
> 

Heutzutage möchten Mitarbeiter an jedem Ort, zu jeder Zeit und mit jedem Gerät produktiv arbeiten können. Sie möchten auf ihren eigenen Geräten arbeiten, z.B. Tablet-PCs, Smartphones oder Laptops. Und sie erwarten, sowohl auf SaaS-Apps in der Cloud als auch auf lokale Unternehmens-Apps zugreifen zu können. Für die Bereitstellung des Zugriffs auf lokale Anwendungen wurden bisher virtuelle private Netzwerke (VPNs), demilitarisierte Zonen (DMZs) oder lokale Reverseproxys eingesetzt. Diese Lösungen sind aber nicht nur komplex und schwer zu schützen, sondern können außerdem nur mit hohem Kostenaufwand eingerichtet und verwaltet werden.

Es gibt eine bessere Möglichkeit!

Moderne Mitarbeiter in einer Arbeitswelt mit dem Motto „mobile-first, cloud-first“ benötigen auch eine moderne Lösung für den Remotezugriff. Der Azure AD-Anwendungsproxy ist ein Feature des Azure Active Directory Premium-Angebots und verfügt über den Remotezugriff als Dienst. Dies bedeutet, dass die Bereitstellung, Nutzung und Verwaltung einfach ist.

## <a name="what-is-azure-active-directory-application-proxy"></a>Was ist der Azure Active Directory-Anwendungsproxy?
Der Azure AD-Anwendungsproxy ermöglicht das einmalige Anmelden (SSO) und den sicheren Remotezugriff für lokal gehostete Webanwendungen. Dies können SharePoint-Websites, Outlook Web Access oder beliebige andere Branchenwebanwendungen sein, die Sie einsetzen. Diese lokalen Webanwendungen sind in Azure AD integriert, also in die gleiche Identitäts- und Steuerungsplattform, die auch von O365 verwendet wird. Dadurch können Endbenutzer auf Ihre lokalen Anwendungen auf die gleiche Weise zugreifen wie auf O365 und andere SaaS-Apps, die in Azure AD integriert sind. Dafür müssen Sie weder die Netzwerkinfrastruktur ändern noch VPN verwenden.

## <a name="why-is-this-a-better-solution"></a>Warum ist dies eine bessere Lösung?
Der Azure AD-Anwendungsproxy ist eine einfache, sichere und kostengünstige Lösung für den Remotezugriff auf alle lokalen Anwendungen.

Azure AD-Anwendungsproxy:  

* Funktioniert in der Cloud, und Sie können Zeit und Kosten sparen. Für lokale Lösungen müssen Sie DMZs, Edgeserver oder andere komplexe Infrastrukturen einrichten und verwalten.  
* Sie sind einfacher einzurichten und zu schützen als lokale Lösungen, da Sie keine eingehenden Verbindungen über Ihre Firewall öffnen müssen.  
* Bietet ein hohes Maß an Sicherheit. Wenn Sie Ihre Apps mit dem Azure AD-Anwendungsproxy veröffentlichen, können Sie die umfassenden Autorisierungssteuerungen und Sicherheitsanalysen in Azure nutzen. Dies bedeutet, dass Sie erweiterte Sicherheitsfunktionen für alle vorhandenen Apps erhalten, ohne Apps dafür ändern zu müssen.  
* Ermöglicht Ihren Benutzern einen einheitlichen Authentifizierungsvorgang. Mit dem einmaligen Anmelden können Ihre Benutzer leicht und problemlos mit nur einem Kennwort auf alle Apps zugreifen, die sie zum produktiven Arbeiten benötigen.  

## <a name="what-kind-of-applications-work-with-azure-ad-application-proxy"></a>Welche Arten von Anwendungen können mit dem Azure AD-Anwendungsproxy verwendet werden?
Mit dem Azure AD-Anwendungsproxy können Sie auf verschiedene Arten von internen Anwendungen zugreifen:

* Webanwendungen, für die zur Authentifizierung die integrierte Windows-Authentifizierung verwendet wird  
* Webanwendungen mit formularbasiertem Zugriff  
* Web-APIs, die Sie für umfassende Anwendungen auf unterschiedlichen Geräten verfügbar machen möchten  
* Hinter einem Remotedesktopgateway gehostete Anwendungen  

## <a name="how-does-the-service-work-with-connectors"></a>Wie funktioniert der Dienst mit Connectors?
Um den Anwendungsproxy nutzen zu können, müssen Sie einen als Connector bezeichneten schlanken Windows Server-Dienst in Ihrem Netzwerk installieren. Mit dem Connector müssen Sie keine eingehenden Ports öffnen oder Ressourcen in der DMZ anordnen. Bei hohem Datenverkehrsaufkommen für Ihre Apps können Sie weitere Connectors hinzufügen. Der Lastenausgleich wird vom Dienst übernommen. Connectors sind zustandslos und rufen alle Informationen nach Bedarf aus der Cloud ab.

Informationen zu Connectors finden Sie unter [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md). 

Wenn Benutzer per Remotezugriff auf Anwendungen zugreifen, stellen sie eine Verbindung mit dem veröffentlichten Endpunkt her. Benutzer authentifizieren sich in Azure AD und werden dann über den Connector an die lokale Anwendung geleitet.

 ![Azure AD-Anwendungsproxy – Diagramm](./media/active-directory-appssoaccess-whatis/azureappproxxy.png)

1. Der Benutzer greift auf die Anwendung über den Anwendungsproxy zu und wird zur Authentifizierung an die Azure AD-Anmeldeseite geleitet.
2. Nach der erfolgreichen Anmeldung wird ein Token generiert und an den Benutzer gesendet.
3. Der Benutzer sendet das Token an den Anwendungsproxy, der den Benutzerprinzipalnamen (User Principal Name, UPN) und den Sicherheitsprinzipalnamen (Security Principal Name, SPN) aus dem Token abruft und die Anforderung dann an den Connector weiterleitet.
4. Der Connector fordert im Namen des Benutzers ein Kerberos-Ticket an, das für die interne Authentifizierung (Windows) verwendet werden kann. Dies wird als eingeschränkte Kerberos-Delegierung bezeichnet.
5. Active Directory ruft das Kerberos-Ticket ab.
6. Das Ticket wird an den Anwendungsserver gesendet und überprüft.
7. Die Antwort wird über den Anwendungsproxy an den Benutzer gesendet.

### <a name="single-sign-on"></a>Einmaliges Anmelden
Der Azure AD-Anwendungsproxy ermöglicht einmaliges Anmelden (SSO) für Anwendungen, die die integrierte Windows-Authentifizierung (IWA) verwenden oder Ansprüche unterstützen. Wenn Ihre Anwendung die integrierte Windows-Authentifizierung verwendet, nimmt der Anwendungsproxy mithilfe der eingeschränkten Kerberos-Delegierung die Identität des Benutzers an, um das einmalige Anmelden zu ermöglichen. Wenn Sie über eine Anwendung verfügen, die Ansprüche unterstützt und die Azure Active Directory vertraut, wird das einmalige Anmelden möglich, weil der Benutzer bereits über Azure Active Directory authentifiziert wurde.

Weitere Informationen zu Kerberos finden Sie unter [All you want to know about Kerberos Constrained Delegation (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd) (Alles über die eingeschränkte Kerberos-Delegierung, KCD).

## <a name="how-to-get-started"></a>Erste Schritte
Stellen Sie sicher, dass Sie über ein Azure AD Basic- oder Premium-Abonnement und ein Azure AD-Verzeichnis verfügen, für die Sie als globaler Administrator angemeldet sind. Sie benötigen auch Azure AD Basic- oder Premium-Lizenzen für den Verzeichnisadministrator und Benutzer, die auf die Apps zugreifen. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md) .

Das Einrichten des Anwendungsproxys erfolgt in zwei Schritten:

1. [Aktivieren des Anwendungsproxys und Konfigurieren des Connectors](active-directory-application-proxy-enable.md).    
2. [Veröffentlichen von Anwendungen](active-directory-application-proxy-publish.md) – Verwenden Sie den schnell und einfach einzusetzenden Assistenten, um Ihre lokalen Apps zu veröffentlichen und den Remotezugriff darauf zu ermöglichen.

## <a name="whats-next"></a>Wie geht es weiter?
Der Anwendungsproxy bietet Ihnen noch viele weitere Möglichkeiten:

* [Veröffentlichen von Anwendungen mit Ihrem eigenen Domänennamen](active-directory-application-proxy-custom-domains.md)
* [Arbeiten mit vorhandenen lokalen Proxyservern](application-proxy-working-with-proxy-servers.md) 
* [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)
* [Arbeiten mit Anwendungen, die Ansprüche unterstützen](active-directory-application-proxy-claims-aware-apps.md)
* [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md)

Aktuelle Neuigkeiten und Updates finden Sie im [Blog zum Anwendungsproxy](http://blogs.technet.com/b/applicationproxyblog/)




<!--HONumber=Feb17_HO2-->


