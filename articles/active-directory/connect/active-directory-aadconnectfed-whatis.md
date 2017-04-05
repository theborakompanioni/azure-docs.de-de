---
title: Azure AD Connect und Verbund | Microsoft-Dokumentation
description: "Diese Seite ist der zentrale Speicherort für die gesamte Dokumentation zu AD FS-Vorgängen mit Azure AD Connect."
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 28df9bfec4db8e3f57db61eecc631545e389085a
ms.lasthandoff: 03/28/2017


---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect und Verbund
Mit Azure Active Directory (Azure AD) Connect können Sie einen Verbund mit lokalen Active Directory-Verbunddiensten (AD FS) und Azure AD konfigurieren. Mit der Verbundanmeldung können sich Benutzer mit ihren lokalen Kennwörtern bei Azure AD-basierten Diensten anmelden, während sie in ihrem Unternehmensnetzwerk angemeldet sind - und das, ohne ihre Kennwörter erneut eingeben zu müssen. Mithilfe der Verbundoption mit AD FS können Sie eine neue Installation von AD FS bereitstellen oder eine vorhandene Installation in einer Windows Server 2012 R2-Farm angeben.

Dieses Thema ist das zentrale Thema zu verbundbezogenen Funktionen für Azure AD Connect. Es enthält zudem eine Liste mit Links zu allen anderen verwandten Themen. Links zu Azure AD Connect finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).

## <a name="azure-ad-connect-federation-topics"></a>Azure AD Connect: Verbundthemen
| Thema | Inhalt und Relevanz |
|:--- |:--- |
| **Azure AD Connect-Optionen für die Benutzeranmeldung** | |
| [Grundlegendes zu Benutzeranmeldeoptionen](active-directory-aadconnect-user-signin.md) |Beschreibung der verschiedenen Benutzeranmeldeoptionen und ihres Einflusses auf die Benutzerfreundlichkeit der Azure-Anmeldung |
| **Installieren von AD FS mit Azure AD Connect** | |
| [Voraussetzungen](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) |Erforderliche Komponenten für eine erfolgreiche Installation von AD FS über Azure AD Connect |
| [Konfigurieren einer AD FS-Farm](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) |Installieren einer neuen AD FS-Farm mit Azure AD Connect |
| **Ändern der AD FS-Konfiguration** | |
| [Reparieren der Vertrauensstellung](active-directory-aadconnect-federation-management.md#repairthetrust) |Reparieren der aktuellen Vertrauensstellung zwischen lokalem AD FS und Office 365/Azure |
| [Hinzufügen eines neuen AD FS-Servers](active-directory-aadconnect-federation-management.md#addadfsserver) |Erweitern einer AD FS-Farm mit einer zusätzlichen AD FS-Serverinstallation nach der Erstinstallation |
| [Hinzufügen eines neuen AD FS-WAP-Servers](active-directory-aadconnect-federation-management.md#addwapserver) |Erweitern einer AD FS-Farm mit einer zusätzlichen WAP-Serverinstallation (Web Application Proxy) nach der Erstinstallation |
| [Hinzufügen einer neuen Verbunddomäne](active-directory-aadconnect-federation-management.md#addfeddomain) |Hinzufügen einer weiteren Domäne für den Verbund mit Azure AD |
| [Aktualisieren des SSL-Zertifikats](active-directory-aadconnectfed-ssl-update.md)| Aktualisieren des SSL-Zertifikats für eine AD FS-Farm |
| **Aufgaben nach der Installation** | |
| [Hinzufügen eines benutzerdefinierten Firmenlogos bzw. einer benutzerdefinierten Abbildung](active-directory-aadconnect-federation-management.md#customlogo) |Ändern der Anmeldeerfahrung durch Angeben des benutzerdefinierten Logos, das auf der AD FS-Anmeldeseite angezeigt wird |
| [Hinzufügen einer Anmeldebeschreibung](active-directory-aadconnect-federation-management.md#addsignindescription) |Ändern der Anmeldebeschreibung auf der AD FS-Anmeldeseite |
| [Ändern von AD FS-Anspruchsregeln](active-directory-aadconnect-federation-management.md#modclaims) |Ändern oder Hinzufügen der Anspruchsregeln in AD FS entsprechend der Azure AD Connect-Synchronisierungskonfiguration |

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)
* [AD FS-Bereitstellung in Azure](active-directory-aadconnect-azure-adfs.md)
* [Gebietsübergreifende, hochverfügbare AD FS-Bereitstellung in Azure mit Azure Traffic Manager](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)

