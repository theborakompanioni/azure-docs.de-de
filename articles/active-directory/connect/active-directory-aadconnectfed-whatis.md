---
title: Azure AD Connect und Verbund | Microsoft Docs
description: "Diese Seite ist der zentrale Speicherort für die gesamte Dokumentation zu AD FS-Vorgängen mit Azure AD Connect"
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
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 39b0b15dba098c0ddc59e39a81bfb6809f8cb2eb
ms.lasthandoff: 03/07/2017


---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect und Verbund
Mit Azure AD Connect können Sie einen Verbund mit dem lokalen AD FS und Azure AD konfigurieren. Mit der Verbundanmeldung können sich Benutzer bei Azure AD-basierten Dienste mit ihren lokalen Kennwörtern anmelden. Während sie in ihrem Unternehmensnetzwerk angemeldet sind, können sie auf Cloudressourcen zugreifen, ohne ihre Kennwörter erneut eingeben zu müssen. Mit der Verbundoption für AD FS können Sie eine neue Windows Server 2012 R2-Farm bereitstellen oder eine Farm angeben.

Dieses Thema ist das zentrale Thema zu verbundbezogenen Funktionen für Azure AD Connect und enthält eine Liste mit Links zu allen anderen verwandten Themen. Links zu Azure AD Connect finden Sie unter "Integrieren Ihrer lokalen Identitäten in Azure Active Directory".

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect – Verbundthemen
| Thema | Inhalt und Relevanz |
|:--- |:--- |
| **Azure AD Connect-Optionen für die Benutzeranmeldung** | |
| [Grundlegendes zu Benutzeranmeldeoptionen](active-directory-aadconnect-user-signin.md) |Beschreibung der verschiedenen Benutzeranmeldeoptionen und ihres Einflusses auf die Benutzerfreundlichkeit der Azure-Anmeldung. |
| **AD FS-Installation mit Azure AD Connect** | |
| [Voraussetzungen](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) |Erforderliche Komponenten für eine erfolgreiche Installation von AD FS über Azure AD Connect |
| [Konfigurieren einer AD FS-Farm](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) |Installieren einer neuen AD FS-Farm mit Azure AD Connect |
| **Ändern der AD FS-Konfiguration** | |
| [Reparieren der Vertrauensstellung](active-directory-aadconnect-federation-management.md#repairthetrust) |Reparieren der aktuellen Vertrauensstellung zwischen lokalem AD FS und Office 365/Azure |
| [Hinzufügen eines neuen AD FS-Servers](active-directory-aadconnect-federation-management.md#addadfsserver) |Erweitern einer AD FS-Farm mit einer zusätzlichen AD FS-Serverinstallation nach der Erstinstallation |
| [Hinzufügen eines neuen AD FS-WAP-Servers](active-directory-aadconnect-federation-management.md#addwapserver) |Erweitern einer AD FS-Farm mit einer zusätzlichen WAP-Serverinstallation nach der Erstinstallation |
| [Hinzufügen einer neuen Verbunddomäne](active-directory-aadconnect-federation-management.md#addfeddomain) |Hinzufügen einer weiteren Domäne für den Verbund mit Azure AD |
| [Aktualisieren des SSL-Zertifikats](active-directory-aadconnectfed-ssl-update.md)| Aktualisieren des SSL-Zertifikats für eine Active Directory-Verbunddienste-Farm (AD FS)|
| **Aufgaben nach der Installation** | |
| [Benutzerdefiniertes Firmenlogo/benutzerdefinierte Abbildung hinzufügen](active-directory-aadconnect-federation-management.md#customlogo) |Ändern der Anmeldeerfahrung durch Angeben des benutzerdefinierten Logos, das auf der AD FS-Anmeldeseite angezeigt wird |
| [Anmeldebeschreibung hinzufügen](active-directory-aadconnect-federation-management.md#addsignindescription) |Ändern der Anmeldebeschreibung auf der AD FS-Anmeldeseite |
| [Ändern von AD FS-Anspruchsregeln](active-directory-aadconnect-federation-management.md#modclaims) |Ändern/Hinzufügen von Anspruchsregeln in AD FS entsprechend der Azure AD Connect-Synchronisierungskonfiguration |

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)
* [AD FS-Bereitstellung in Azure](active-directory-aadconnect-azure-adfs.md)
* [Gebietsübergreifende, hochverfügbare AD FS-Bereitstellung in Azure mit Azure Traffic Manager](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)

