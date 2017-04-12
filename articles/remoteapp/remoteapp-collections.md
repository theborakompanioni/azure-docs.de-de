---
title: "Welche Art von Sammlung benötigen Sie für Azure RemoteApp? | Microsoft Docs"
description: "Lernen Sie die Typen von Sammlungen kennen, die in Azure RemoteApp verfügbar sind."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: c13ec78d-07e9-4646-8194-cf3efafc1760
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 7891ce3b04cd5978440dd8af73dc95d1d123ed92
ms.lasthandoff: 03/31/2017


---
# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Welche Art von Sammlung benötigen Sie für Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Mit Azure RemoteApp können Sie Apps und Ressourcen auf jedem Gerät für Benutzer freigeben. Zu diesem Zweck werden Sammlungen für die Apps und Ressourcen erstellt, und Sie geben dann diese Sammlungen für Benutzer frei. Es gibt zwei unterschiedliche Sammlungsoptionen mit unterschiedlichen Netzwerk- und Authentifizierungsoptionen – welche ist die richtige für Sie?

Betrachten wir die Überlegungen und Optionen, die Sie berücksichtigen bzw. wählen müssen, um Ihre Azure RemoteApp-Sammlung optimal zu nutzen. 

## <a name="quick-differences-between-the-collection-types"></a>Kurzübersicht über die Unterschiede zwischen den Sammlungstypen
|  | Cloud | Hybrid |
| --- | --- | --- |
| Vorhandenes VNET verwenden |Ja |Ja |
| Erfordert mit AD verbundene Konten (DirSync) |Nein |Ja |
| Erfordert Domänenbeitritt |Nein |Ja |
| Erfordert Zugriff von VNET auf Domänencontroller |Nein |Ja |

## <a name="cloud-collections"></a>Cloudsammlungen
* Schnelle Erstellung – Die Sammlung wird schnell bereitgestellt, d. h., die Benutzer erhalten die App in kürzerer Zeit.
* Sie können eigene Apps bereitstellen oder unsere Apps gemeinsam nutzen. Sie können ein benutzerdefiniertes Image (anhand einer Azure-VM erstellt) oder eines der in Ihrem Abonnement enthaltenen Images verwenden.
* Sie müssen keine Verbindung zwischen der Sammlung und der lokalen Domäne konfigurieren.
* Optional können Sie jedoch ein eigenes Azure VNET verwenden, um Zugriff auf Ihre lokale Umgebung für die Datenfreigabe bereitzustellen oder um Nicht-Windows-Authentifizierung bei Ressourcen, z. B. SQL Server (mit Datenbankauthentifizierung), zu verwenden.

Wie erstelle ich eine Cloudsammlung?

* Nur Cloud? Sie erstellen sie mit der Option **Schnellerfassung** im Portal.
* Cloud und VNET? Sie erstellen sie mit der Option **Mit VNET erstellen**. Treten Sie jedoch NICHT einer Domäne bei.

## <a name="hybrid-collections"></a>Hybridsammlungen
* Bieten Vollzugriff auf das lokale Netzwerk und Azure VNET.
* Umfasst den Domänenbeitrittszugriff für Apps und Daten. Remoteanwendungen können beim lokalen Active Directory authentifiziert werden und haben dann Zugriff auf Ressourcen in Ihrer Domäne.
* Ermöglichen mit vorhandenen System Center-Lösungen und Windows-Gruppenrichtlinien erweiterte Überwachung und Verwaltung (über ein benutzerdefiniertes Image auf Grundlage von Windows Server 2012 R2).
* Unterstützen von [ExpressRoute](https://azure.microsoft.com/services/expressroute/) für die Verbindung von Azure VNET mit dem lokalen VNET.

Sie erstellen sie mit der Option **Mit VNET erstellen** und TRETEN einer Domäne BEI.

## <a name="authentication-options"></a>Authentifizierungsoptionen
Azure RemoteApp unterstützt Microsoft-Konten und Azure Active Directory-Konten, jedoch werden nicht alle Methoden von allen Sammlungen unterstützt. 

| Kontotyp |  | Cloud | Cloud und VNET | Hybrid |
| --- | --- | --- | --- | --- |
| Microsoft Account | |Ja |Ja |Nein |
| Azure Active Directory (Azure AD) | | | | |
| Nur Azure AD |Ja |Ja |Nein | |
| AD Connect mit Kennwortsynchronisierung |Ja |Ja |Ja | |
| AD Connect ohne Kennwortsynchronisierung |Ja |Ja |Nein | |
| AD Connect mit AD FS |Ja |Ja |Ja | |
| Von Azure unterstützte Drittidentitätsanbieter (z. B. Ping) |Ja |Ja |Ja | |
| Multi-Factor Authentication | |Ja |Ja |Ja |

### <a name="cloud-and-cloud--vnet"></a>Cloud sowie Cloud und VNET
Mit Cloudsammlungen können Sie Microsoft-Konten, Azure AD-Konten oder eine Kombination von beiden verwenden. Verwenden Sie die Konten, die sich für die Benutzer am besten eignen.

Es gibt keine speziellen Anforderungen für die Verwendung von Microsoft-Konten. 

Wenn Sie Azure AD-Konten verwenden möchten, müssen Sie sicherstellen, dass der Azure AD-Mandant mit dem Mandanten übereinstimmt, der Ihrem Abonnement zugeordnet ist. Bei der Erstellung Ihres Azure RemoteApp-Abonnements wurde der von Ihnen verwendete Azure AD-Mandant automatisch Ihrem Abonnement zugeordnet. Bei jedem Azure AD-Benutzer, dem Sie eine Berechtigung erteilen, muss es sich um diesen Mandanten handeln. Sie können ggf. [den Azure AD-Mandanten ändern](remoteapp-changetenant.md), der Ihrem Abonnement zugeordnet ist.

### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybrid (oder Cloud und Azure AD und AD)
Hybrid-Sammlungen erfordern die Verwendung von Azure AD und lokalem Active Directory. Sie müssen die beiden Verzeichnisse mithilfe von AD Connect integrieren. Sie können jedoch AD Connect auf verschiedene Weise konfigurieren. 

Es gibt zwei AD Connect-Szenarien – Verwendung von Kennwortsynchronisierung oder Verwendung von AD-Verbunddiensten. Ziehen Sie die [Informationen zu AD Connect](../active-directory/active-directory-aadconnect.md) zurate, um das Szenario zu ermitteln, das sich am besten für Ihre Anforderungen eignet.

Sie können auch Azure AD und AD mit einer Cloudsammlung verwenden. Stellen Sie sicher, dass Sie die gleichen Konfigurationsschritte ausführen.

In [Anforderungen von Azure AD und Active Directory für Azure RemoteApp](remoteapp-ad.md) finden Sie Informationen über die erforderlichen Schritte zum Konfigurieren von Azure AD und Active Directory.

## <a name="go-create-your-collection"></a>Jetzt können Sie eine Sammlung erstellen!
Ich denke, ich habe jetzt alles erklärt, was Sie wissen müssen. Jetzt bleibt nur noch die praktische Anwendung – das Erstellen Ihrer ersten Azure RemoteApp-Sammlung.

[Erstellen Sie eine Cloudsammlung](remoteapp-create-cloud-deployment.md), oder [erstellen Sie eine Hybridsammlung](remoteapp-create-hybrid-deployment.md) – fangen Sie einfach an.


