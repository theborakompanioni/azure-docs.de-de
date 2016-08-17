<properties
	pageTitle="Azure AD-Verbund – Kompatibilitätsliste"
	description="Auf dieser Seite sind Identitätsanbieter anderer Anbieter aufgeführt, die zur Implementierung des einmaligen Anmeldens verwendet werden können."
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="femila"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/14/2016"
	ms.author="billmath"/>

# Azure AD-Verbund – Kompatibilitätsliste
Azure Active Directory ermöglicht das einmalige Anmelden und bietet eine höhere Sicherheit beim Zugriff auf Anwendungen für Office 365 und andere Microsoft Online Services sowohl für Hybridimplementierungen als auch für reine Cloudimplementierungen, ohne dass zu diesem Zweck eine Drittanbieterlösung erforderlich ist. Wie die meisten Microsoft Online Services ist auch Office 365 in Azure Active Directory integriert, um die Verzeichnisdienste sowie die Authentifizierungs- und Autorisierungsfeatures nutzen zu können. Azure Active Directory ermöglicht zudem das einmalige Anmelden für Tausende SaaS-Anwendungen und lokale Webanwendungen. Die unterstützten SaaS-Anwendungen sind im Azure Active Directory-Anwendungskatalog aufgeführt.

Organisationen, die bereits Investitionen in Verbundlösungen anderer Anbieter getätigt haben, erhalten in diesem Thema Unterstützung bei der Konfiguration des einmaligen Anmeldens für Ihre Windows Server Active Directory-Benutzer mit Microsoft Online Services unter Verwendung von Identitätsanbietern anderer Anbieter, die in der unten stehenden Kompatibilitätsliste für den Azure Active Directory-Verbund aufgeführt sind.


Die ![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Oxford Computer Group](http://oxfordcomputergroup.com/) hat im Auftrag von Microsoft das einmalige Anmelden bei Verwendung von nicht von Microsoft stammenden Identitätsanbietern für verschiedene gängige Azure Active Directory-Anwendungsfälle getestet.

Informationen dazu, wie Sie Ihren nicht von Microsoft stammenden Identitätsanbieter in diese Liste aufnehmen lassen können, wenden Sie sich unter [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com) an die Oxford Computer Group.

>[AZURE.IMPORTANT] Dabei hat die Oxford Computer Group nur die Verbundfunktionalität dieser Szenarien mit einmaligem Anmelden getestet. Synchronisierung, zweistufige Authentifizierung oder andere Elemente dieser Szenarien mit einmaligem Anmelden wurden nicht getestet.

>Auch die Anmeldung mit alternativer ID oder UPN wurde nicht getestet.



- [Azure Active Directory](#azure-active-directory)
- [Optimal IDM Virtual Identity Server Federation Services](#optimal-idm-virtual-identity-server-federation-services)
- [PingFederate 6.11](#pingfederate-611)
- [PingFederate 7.2](#pingfederate-72)
- [PingFederate 8.x](#pingfederate-8.x)
- [Centrify](#centrify)
- [IBM Tivoli Federated Identity Manager 6.2.2](#ibm-tivoli-federated-identity-manager-622)
- [SecureAuth IdP 7.2.0](#secureauth-idp-720)
- [CA SiteMinder 12.52](#ca-siteminder-1252)
- [RadiantOne CFS 3.0](#radiantone-cfs-30)
- [Okta](#okta)
- [OneLogin](#onelogin)
- [NetIQ Access Manager 4.0.1](#netiq-access-manager-401)
- [BIG-IP mit Access Policy Manager BIG-IP Version 11.3x - 11.6x](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x)
- [VMware Workspace Portal Version 2.1](#vmware-workspace-portal-version-21)
- [Sign&go 5.3](#signampgo-53)
- [IceWall Federation Version 3.0](#icewall-federation-version-30)
- [CA Secure Cloud](#ca-secure-cloud)
- [Dell One Identity Cloud Access Manager v7.1](#dell-one-identity-cloud-access-manager-v71)
- [AuthAnvil Single Sign On 4.5](#authavil-single-sign-on-45)

>[AZURE.IMPORTANT] Da es sich um Drittanbieterprodukte handelt, bietet Microsoft keinen Support für die Bereitstellung, Konfiguration und Problembehandlung sowie keine bewährten Methoden oder Unterstützung bei Problemen und Fragen im Zusammenhang mit diesen Identitätsanbietern. Wenn Sie Unterstützung benötigen oder Fragen zu diesen Identitätsanbietern haben, wenden Sie sich direkt an die unterstützten Drittanbieter.

>Diese Identitätsanbieter von Drittanbietern wurden auf Interoperabilität mit Microsoft Cloud Services unter Verwendung der WS-Verbund- und WS-Trust-Protokolle getestet. Das SAML-Protokoll wurde im Rahmen dieser Tests nicht verwendet.

## Azure Active Directory 
Azure Active Directory kann Benutzer durch einen Verbund mit Ihrer lokalen Active Directory-Bereitstellung oder ohne lokalen Verbundserver authentifizieren. Im letzteren Fall erfolgt die Authentifizierung über die Kennwortsynchronisierung.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für diese Art der Anmeldung:


| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|
|Moderne Anwendungen unter Verwendung von ADAL wie Office 2016| Unterstützt|Keine|

Weitere Informationen zur Verwendung von Azure Active Directory mit den Active Directory-Verbunddiensten finden Sie unter [Verbund mit AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

Weitere Informationen zur Verwendung von Azure Active Directory mit Kennwortsynchronisierung finden Sie unter [Azure AD Connect](active-directory-aadconnect.md).


## Optimal IDM Virtual Identity Server Federation Services 
Optimal IDM Virtual Identity Server Federation Services kann Benutzer in lokalen Active Directory-Implementierungen eines Kunden authentifizieren.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:


| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Weitere Informationen zu Clientzugriffsrichtlinien finden Sie unter [Limiting Access to Office 365 Services Based on the Location of the Client](https://technet.microsoft.com/library/hh526961.aspx) (Einschränken des Zugriffs auf Office 365-Dienste auf Grundlage des Clientstandorts).|



## PingFederate 6.11 

PingFederate 6.11 implementiert den weit verbreiteten WS-Verbund-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:


| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine (bei früheren Versionen muss ein Upgrade auf 6.11 durchgeführt werden)|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Die PingFederate-Anweisungen zur Konfiguration dieses Sicherheitstokendiensts zur Implementierung des einmaligen Anmeldens für Active Directory-Benutzer finden Sie in dieser [PDF-Datei](http://go.microsoft.com/fwlink/?LinkID=266321).

## PingFederate 7.2 
PingFederate 7.2 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:


| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Die PingFederate-Anweisungen zur Konfiguration dieses Sicherheitstokendiensts zur Implementierung des einmaligen Anmeldens für Active Directory-Benutzer finden Sie [hier](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2).

## PingFederate 8.x 
PingFederate 8.x implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:


| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Die PingFederate-Anweisungen zur Konfiguration dieses Sicherheitstokendiensts zur Implementierung des einmaligen Anmeldens für Active Directory-Benutzer finden Sie [hier](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction).

## Centrify 
Mit Centrify wird das einmalige Anmelden im Verbund für Office 365 ermöglicht, ohne zu diesem Zweck einen lokalen Verbundserver bereitstellen zu müssen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:


| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Client Access Control wird nicht unterstützt 

Weitere Informationen zu Centrify finden Sie [hier](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp).|

## IBM Tivoli Federated Identity Manager 6.2.2 
IBM Tivoli Federated Identity Manager 6.2.2 mit IBM Security Access Manager for Microsoft Applications 1.4 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu IBM Tivoli Federated Identity Manager finden Sie unter [IBM Security Access Manager for Microsoft Applications](http://www-01.ibm.com/support/docview.wss?uid=swg24029517).

## SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu SecureAuth finden Sie unter [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## CA SiteMinder 12.52 
CA SiteMinder Federation 12.52 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu CA SiteMinder finden Sie unter [CA SiteMinder Federation.](http://www.ca.com/us/products/ca-single-sign-on.html)

## RadiantOne CFS 3.0 
RadiantOne Cloud Federation Service (CFS) 3.0 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu RadiantOne CFS finden Sie unter [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## Okta 
Okta implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:


| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Für die integrierte Windows-Authentifizierung muss ein zusätzlicher Webserver sowie eine zusätzliche Okta-Anwendung eingerichtet werden.|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu Okta finden Sie unter [Okta](https://www.okta.com/).
 
## OneLogin 
Die im Mai 2014 getestete OneLogin-Version implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu OneLogin finden Sie unter [OneLogin](https://www.onelogin.com/).

## NetIQ Access Manager 4.0.1 
NetIQ Access Manager 4.0.1 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |*Kerberos-Verträge werden unterstützt|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

*NetIQ unterstützt die Kerberos-Authentifizierung über die Konfiguration eines Kerberos-Vertrags. Wenn Sie Unterstützung bei dieser Konfiguration benötigen, wenden Sie sich an NetIQ, oder lesen Sie den Einrichtungsleitfaden. Weitere Informationen zu NetIQ Access Manager finden Sie unter [NetIQ Access Manager](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html).

## BIG-IP mit Access Policy Manager BIG-IP Version 11.3x - 11.6x 
BIG-IP mit Access Policy Manager (APM) BIG-IP Version 11.3x - 11.6x implementiert den weit verbreiteten SAML-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Nicht unterstützt |Nicht unterstützt|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu BIG-IP Access Policy Manager finden Sie unter [BIG-IP Access Policy Manager](https://f5.com/products/modules/access-policy-manager).

Die BIG-IP Access Policy Manager-Anweisungen zur Konfiguration dieses Sicherheitstokendiensts zur Implementierung des einmaligen Anmeldens für Active Directory-Benutzer finden Sie in dieser [PDF-Datei](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf).

## VMware Workspace Portal Version 2.1 
VMware Workspace Portal Version 2.1 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu VMware Workspace Portal Version 2.1 finden Sie in dieser [PDF-Datei](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf).

## Sign&go 5.3 
Sign&go 5.3 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Kerberos-Verträge werden unterstützt |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|


Sign&go 5.3 unterstützt die Kerberos-Authentifizierung über die Konfiguration eines Kerberos-Vertrags. Wenn Sie Unterstützung bei dieser Konfiguration benötigen, wenden Sie sich an Ilex, oder lesen Sie [hier](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf) den Einrichtungsleitfaden.


## IceWall Federation Version 3.0 
IceWall Federation Version 3.0 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu IceWall Federation finden Sie [hier](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) und [hier](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html).

## CA Secure Cloud 

CA Secure Cloud implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu CA Secure Cloud finden Sie unter [CA Secure Cloud](http://www.ca.com/us/products/security-as-a-service.aspx).

## Dell One Identity Cloud Access Manager v7.1 
Dell One Identity Cloud Access Manager implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|

Weitere Informationen zu Dell One Identity Cloud Access Manager finden Sie unter [Dell One Identity Cloud Access Manager](http://software.dell.com/products/cloud-access-manager).

 Anweisungen zum Konfigurieren dieses Sicherheitstokendiensts zur Implementierung des einmaligen Anmeldens für Office 365-Benutzer finden Sie unter [Configure Office 365 Users](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) (Konfigurieren von Office 365-Benutzern).

## AuthAnvil Single Sign On 4.5 
AuthAnvil Single Sign On 4.5 implementiert den weit verbreiteten WS-Verbund-/WS-Trust-Identitätsstandard, um das einmalige Anmelden zu ermöglichen und ein Framework für den Austausch von Attributen bereitzustellen.

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- |Support |Ausnahmen|
| --------- | --------- |--------- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| E-Mail-Clients wie Outlook und ActiveSync | Unterstützt |Keine|


Weitere Informationen finden Sie unter [AuthAnvil Single Sign On](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-).

<!-----HONumber=AcomDC_0720_2016-->
