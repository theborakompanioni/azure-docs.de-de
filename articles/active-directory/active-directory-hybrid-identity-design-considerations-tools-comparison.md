---
title: "Hybrididentität: Vergleich von Tools für die Verzeichnisintegration | Microsoft Docs"
description: "Diese Seite enthält eine umfassende Tabelle mit einem Vergleich der verschiedenen Tools für die Verzeichnisintegration."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/08/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 2fb248f3e71b248ce0b5ae02ebeff291bbd28dce


---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Vergleich von Tools für die Verzeichnisintegration für Hybrid-Identitäten
Im Laufe der Jahre sind die Tools für die Verzeichnisintegration umfangreicher geworden und wurden weiterentwickelt.  Das vorliegende Dokument bietet eine Übersicht über diese Tools und einen Vergleich der Features, die jedes der Tools bietet.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

> [!NOTE]
> Azure AD Connect umfasst die Komponenten und Funktionen, die zuvor als DirSync und AAD Sync veröffentlicht wurden. Diese Tools werden nicht mehr einzeln veröffentlicht, und alle künftigen Verbesserungen werden in den Updates von Azure AD Connect enthalten sein, sodass Sie immer wissen, wo Sie die aktuellsten Funktionen bekommen.
> 
> DirSync and Azure AD Sync sind veraltet. Weitere Informationen finden Sie [hier](active-directory-aadconnect-dirsync-deprecated.md).
> 
> 

Verwenden Sie den folgenden Schlüssel für jede der Tabellen.

● = Jetzt verfügbar  
FR = Künftige Version  
PP = Öffentliche Vorschau  

## <a name="on-premises-to-cloud-synchronization"></a>Synchronisierung vom lokalen Standort zur Cloud
| Feature | Azure Active Directory Connect | Azure Active Directory-Synchronisierungsdienste (AAD Sync) | Azure Active Directory-Synchronisierungstool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Herstellen einer Verbindung mit einer einzelnen lokalen AD-Gesamtstruktur |● |● |● |● |● |
| Herstellen einer Verbindung mit mehreren lokalen AD-Gesamtstrukturen |● |● | |● |● |
| Herstellen einer Verbindung mit mehreren lokalen Exchange-Organisationen |● | | | | |
| Herstellen einer Verbindung mit einem einzelnen lokalen LDAP-Verzeichnis |BV | | |● |● |
| Herstellen einer Verbindung mit mehreren lokalen LDAP-Verzeichnissen |BV | | |● |● |
| Herstellen einer Verbindung mit lokalen Active Directory- und LDAP-Verzeichnissen |BV | | |● |● |
| Herstellen einer Verbindung mit benutzerdefinierten Systeme (z. B. SQL, Oracle, MySQL usw.) |BV | | |● |● |
| Synchronisieren von benutzerdefinierten Attributen (Verzeichniserweiterungen) |● | | | | |
| Verbinden mit lokaler HR-Instanz (d. h. SAP, Oracle eBusiness,PeopleSoft) |BV | | |● |● |
| Unterstützt FIM-Synchronisierungsregeln und -Connectors für die Bereitstellung lokaler Systeme. | | | |● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Synchronisierung von der Cloud zum lokalen Standort
| Feature | Azure Active Directory Connect | Azure Active Directory-Synchronisierungsdienste | Azure Active Directory-Synchronisierungstool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Rückschreiben von Geräten |● | |● | | |
| Rückschreiben von Attributen (für Exchange-Hybridbereitstellung) |● |● |● |● |● |
| Rückschreiben von Benutzer- und Gruppenobjekten |● | | | | |
| Rückschreiben von Kennwörtern (Self-Service-Kennwortzurücksetzung und Kennwortänderung) |● |● | | | |

## <a name="authentication-feature-support"></a>Unterstützung von Authentifizierungsfunktionen
| Feature | Azure Active Directory Connect | Azure Active Directory-Synchronisierungsdienste | Azure Active Directory-Synchronisierungstool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Kennwortsynchronisierung für eine einzelne lokale Active Directory-Gesamtstruktur |● |● |● | | |
| Kennwortsynchronisierung für mehrere lokale Active Directory-Gesamtstrukturen |● |● | | | |
| Einmaliges Anmelden mit Verbund |● |● |● |● |● |
| Rückschreiben von Kennwörtern (Self-Service-Kennwortzurücksetzung und Kennwortänderung) |● |● | | | |

## <a name="set-up-and-installation"></a>Einrichtung und Installation
| Feature | Azure Active Directory Connect | Azure Active Directory-Synchronisierungsdienste | Azure Active Directory-Synchronisierungstool (DirSync) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|
| Unterstützt die Installation auf einem Domänencontroller |● |● |● | |
| Unterstützt die Installation mit SQL Express |● |● |● | |
| Einfaches Upgrade von DirSync |● | | | |
| Lokalisierung der Administrator-UX in die Windows Server-Sprachen |● |● |● | |
| Lokalisierung der Endbenutzer-UX in die Windows Server-Sprachen | | | |● |
| Unterstützung für Windows Server 2008 und Windows Server 2008 R2 |● für Synchronisierung, nicht für den Verbund |● |● |● |
| Unterstützung für Windows Server 2012 und Windows Server 2012 R2 |● |● |● |● |

## <a name="filtering-and-configuration"></a>Filterung und Konfiguration
| Feature | Azure Active Directory Connect | Azure Active Directory-Synchronisierungsdienste | Azure Active Directory-Synchronisierungstool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Filterung nach für Domänen und Organisationseinheiten |● |● |● |● |● |
| Filterung nach den Attributwerten eines Objekts |● |● |● |● |● |
| Zulassen eines minimalen Attributsatzes für die Synchronisierung (MinSync) |● |● | | | |
| Zulassen der Anwendung verschiedener Dienstvorlagen auf Attributflüsse |● |● | | | |
| Zulassen der Entfernung von Attributen aus dem Attributfluss von AD nach Azure AD |● |● | | | |
| Zulassen einer erweiterten Anpassung des Attributflusses |● |● | |● |● |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).




<!--HONumber=Dec16_HO2-->


