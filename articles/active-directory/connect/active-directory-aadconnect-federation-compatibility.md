---
title: "Azure AD-Verbund – Kompatibilitätsliste"
description: "Auf dieser Seite sind Identitätsanbieter anderer Anbieter aufgeführt, die zur Implementierung des einmaligen Anmeldens verwendet werden können."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: 6b0fd60e79308b75c3a2797ff25065b8a500a3b4
ms.contentlocale: de-de
ms.lasthandoff: 06/10/2017

---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD-Verbund – Kompatibilitätsliste
Azure Active Directory ermöglicht das einmalige Anmelden und bietet eine höhere Sicherheit beim Zugriff auf Anwendungen für Office 365 und andere Microsoft Online Services sowohl für Hybridimplementierungen als auch für reine Cloudimplementierungen, ohne dass zu diesem Zweck eine Drittanbieterlösung erforderlich ist. Wie die meisten Microsoft Online Services ist auch Office 365 in Azure Active Directory integriert, um die Verzeichnisdienste sowie die Authentifizierungs- und Autorisierungsfeatures nutzen zu können. Azure Active Directory ermöglicht zudem das einmalige Anmelden für Tausende SaaS-Anwendungen und lokale Webanwendungen. Die unterstützten SaaS-Anwendungen sind im Azure Active Directory-Anwendungskatalog aufgeführt.

Organisationen, die bereits Investitionen in Verbundlösungen anderer Anbieter getätigt haben, erhalten in diesem Thema Unterstützung bei der Konfiguration des einmaligen Anmeldens für Ihre Windows Server Active Directory-Benutzer mit Microsoft Online Services unter Verwendung von Identitätsanbietern anderer Anbieter, die in der unten stehenden Kompatibilitätsliste für den Azure Active Directory-Verbund aufgeführt sind. 

![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
Die [Oxford Computer Group](http://oxfordcomputergroup.com/) hat im Auftrag von Microsoft das einmalige Anmelden bei Verwendung von nicht von Microsoft stammenden Identitätsanbietern für verschiedene gängige Azure Active Directory-Anwendungsfälle getestet.

Informationen dazu, wie Sie Ihren nicht von Microsoft stammenden Identitätsanbieter in diese Liste aufnehmen lassen können, wenden Sie sich unter [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com)an die Oxford Computer Group.

> [!IMPORTANT]
> Dabei hat die Oxford Computer Group nur die Verbundfunktionalität dieser Szenarien mit einmaligem Anmelden getestet. Synchronisierung, zweistufige Authentifizierung oder andere Elemente dieser Szenarien mit einmaligem Anmelden wurden nicht getestet.
> 
> Auch die Anmeldung mit alternativer ID oder UPN wurde nicht getestet.
> 
> 

* [Azure Active Directory](#azure-active-directory)
* [AuthAnvil Single Sign On 4.5](#authanvil-single-sign-on-45)
* [BIG-IP mit Access Policy Manager BIG-IP Version 11.3x - 11.6x](#big-ip-with-access-policy-manager-big-ip-ver-113x--116x) 
* [BitGlass](#bitglass)
* [CA Secure Cloud](#ca-secure-cloud) 
* [CA SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
* [Centrify](#centrify) 
* [Dell One Identity Cloud Access Manager v7.1](#dell-one-identity-cloud-access-manager-v71) 
* [DigitalPersona Composite Authentication (Zusammengesetzte DigitalPersona-Authentifizierung)](#digitalpersona-composite-authentication)
* [IBM Tivoli Federated Identity Manager 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
* [IceWall Federation Version 3.0](#icewall-federation-version-30) 
* [Memority](#memority)
* [NetIQ Access Manager 4.x](#netiq-access-manager-4x) 
* [Okta](#okta) 
* [OneLogin](#onelogin) 
* [Optimal IDM Virtual Identity Server Federation Services](#optimal-idm-virtual-identity-server-federation-services) 
* [PingFederate 6.11, 7.2, 8.x](#pingfederate-611-72-8x)
* [RadiantOne CFS 3.0](#radiantone-cfs-30) 
* [Sailpoint IdentityNow](#sailpoint-identitynow)
* [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
* [Sign&go 5.3](#signgo-53) 
* [SoftBank Technology Online Service Gate](#softbank)
* [VMware Workspace One](#vmware-workspace-one)



> [!IMPORTANT]
> Da es sich um Drittanbieterprodukte handelt, bietet Microsoft keinen Support für die Bereitstellung, Konfiguration und Problembehandlung sowie keine bewährten Methoden oder Unterstützung bei Problemen und Fragen im Zusammenhang mit diesen Identitätsanbietern. Wenn Sie Unterstützung benötigen oder Fragen zu diesen Identitätsanbietern haben, wenden Sie sich direkt an die unterstützten Drittanbieter.
> 
> Diese Identitätsanbieter von Drittanbietern wurden auf Interoperabilität mit Microsoft Cloud Services unter Verwendung der WS-Verbund- und WS-Trust-Protokolle getestet. Das SAML-Protokoll wurde im Rahmen dieser Tests nicht verwendet.
> 


## <a name="azure-active-directory"></a>Azure Active Directory

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für diese Art der Anmeldung: 

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |
| Moderne Anwendungen unter Verwendung von ADAL wie Office 2016 |Unterstützt |Keine |

Weitere Informationen zur Verwendung von Azure Active Directory mit den AD FS finden Sie unter [Active Directory-Verbunddienste (AD FS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

Weitere Informationen zur Verwendung von Azure Active Directory mit Kennwortsynchronisierung finden Sie unter [Azure AD Connect](active-directory-aadconnect.md).

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil Single Sign On 4.5

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen finden Sie unter [AuthAnvil Single Sign On (AuthAnvil-basierte einmalige Anmeldung)](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-).


## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP mit Access Policy Manager BIG-IP Version 11.3x - 11.6x

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden: 

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Nicht unterstützt |Nicht unterstützt |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu BIG-IP Access Policy Manager finden Sie unter [BIG-IP Access Policy Manager](https://f5.com/products/modules/access-policy-manager) 

Die BIG-IP Access Policy Manager-Anweisungen zur Konfiguration dieses Sicherheitstokendiensts zur Implementierung des einmaligen Anmeldens für ihre Active Directory-Benutzer, finden Sie in dieser [BIG-IP](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf).

## <a name="bitglass"></a>BitGlass

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu BitGlass finden Sie unter [BitGlass](http://www.bitglass.com).

## <a name="ca-secure-cloud"></a>CA Secure Cloud

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu CA Secure Cloud finden Sie unter [CA Secure Cloud (CA-Sicherheitscloud)](http://www.ca.com/us/products/security-as-a-service.aspx).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12.52 SP1 Cumulative Release 4

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden: 

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu CA SiteMinder finden Sie unter [CA SiteMinder Federation](http://www.ca.com/us/products/ca-single-sign-on.html). 

## <a name="centrify"></a>Centrify

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Client Access Control wird nicht unterstützt |

Weitere Informationen zu Centrify finden Sie unter [Centrify](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp).

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell One Identity Cloud Access Manager v7.1

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zum Dell One Identity Cloud Access Manager finden Sie unter [Dell One Identity Cloud Access Manager (Dell One Identity Cloud-Zugangsverwaltung)](http://software.dell.com/products/cloud-access-manager).

 Anweisungen zum Konfigurieren dieses Sicherheitstokendiensts zur Implementierung des einmaligen Anmeldens für Office 365-Benutzer finden Sie unter [Configure Office 365 Users (Konfigurieren von Office 365 Benutzern)](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365). 

## <a name="digitalpersona-composite-authentication"></a>Zusammengesetzte DigitalPersona-Authentifizierung  

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt|
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen finden Sie unter [DigitalPersona Composite Authentication (Zusammengesetzte DigitalPersona-Authentifizierung)](http://www.crossmatch.com/uploadedFiles/Support/Reference_Material/DigitalPersona-Office-365-Deployment-Guide.pdf).


## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli Federated Identity Manager 6.2.2

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden: 

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu IBM Tivoli Federated Identity Manager finden Sie unter [IBM Security Access Manager for Microsoft Applications (IBM-Zugangsverwaltung für Microsoft-Applikationen)](http://www-01.ibm.com/support/docview.wss?uid=swg24029517).

## <a name="icewall-federation-version-30"></a>IceWall Federation Version 3.0

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu IceWall Federation finden Sie unter [IceWall Federation Version 3.0](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) und [IceWall Federation mit Office 365](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html).

## <a name="memority"></a>Memority

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für diese Art der Anmeldung: 

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zum Verwenden von Memority finden Sie unter [Memority](http://www.memority.com).


## <a name="netiq-access-manager-4x"></a>NetIQ Access Manager 4.x

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine|
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine|
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen finden Sie unter [NetIQ Access Manager (NetIQ-Zugangsverwaltung)](https://www.netiq.com/documentation/access-manager-43/admin/data/b65ogn0.html#b12iqp0m).

## <a name="okta"></a>Okta

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden: 

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Für die integrierte Windows-Authentifizierung muss ein zusätzlicher Webserver sowie eine zusätzliche Okta-Anwendung eingerichtet werden. |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu Okta finden Sie unter [Okta](https://www.okta.com/).

## <a name="onelogin"></a>OneLogin

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden: 

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Integrierte Windows-Authentifizierung |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu OneLogin finden Sie unter [OneLogin](https://www.onelogin.com/).

## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimal IDM Virtual Identity Server Federation Services

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |

Weitere Informationen zu Client-Zugriffsrichtlinien finden Sie unter [Limiting Access to Office 365 Services Based on the Location of the Client (Beschränkung des Zugriffs auf Office 365 Dienste basierend auf der Lokation des Clients)](https://technet.microsoft.com/library/hh526961.aspx).





## <a name="pingfederate-611-72-8x"></a>PingFederate 6.11, 7.2, 8.x

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Die PingFederate-Anweisungen zur Konfiguration dieses Sicherheitstokendiensts zur Implementierung des einmaligen Anmeldens für Active Directory-Benutzer finden Sie hier: 

- [PingFederate 6.11](http://go.microsoft.com/fwlink/?LinkID=266321)
- [PingFederate 7.2](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)
- [PingFederate 8.x](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden: 

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu RadiantOne CFS finden Sie unter [RadiantOne CFS](http://www.radiantlogic.com/products/radiantone-cfs/).

## <a name="sailpoint-identitynow"></a>Sailpoint IdentityNow

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen finden Sie unter [Sailpoint IdentityNow](https://www.sailpoint.com/idaas-identity-as-a-service-identitynow/).

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden: 

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Keine |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zu SecureAuth finden Sie unter [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).














## <a name="signgo-53"></a>Sign&go 5.3

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Kerberos-Verträge werden unterstützt |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Keine |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Sign&go 5.3 unterstützt die Kerberos-Authentifizierung über die Konfiguration eines Kerberos-Vertrags.  Wenn Sie Unterstützung bei dieser Konfiguration benötigen, wenden Sie sich an Ilex, oder lesen Sie die Setup-Anleitung [Sign&go](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)

## <a name="softbank-technology-online-service-gate"></a>SoftBank Technology Online Service Gate

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen zum SoftBank Technology Online Service Gate finden Sie unter [Softbank](https://www.softbanktech.jp/service/list/osg-pro-ent/)

## <a name="vmware-workspace-one"></a>VMware Workspace One

Nachfolgend finden Sie die Matrix der unterstützten Szenarien für das einmalige Anmelden:

| Client- | Support | Ausnahmen |
| --- | --- | --- |
| Webbasierte Clients wie Exchange Web Access und SharePoint Online |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| Rich Client-Anwendungen wie Lync, Office-Abonnement, CRM |Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt |
| E-Mail-Clients wie Outlook und ActiveSync |Unterstützt |Keine |

Weitere Informationen finden Sie unter [VMware Workspace One](http://www.vmware.com/pdf/vidm-office365-saml.pdf)


