---
title: "Azure AD- und Active Directory-Anforderungen für Azure RemoteApp | Microsoft Docs"
description: "Erfahren Sie, wie Sie Active Directory für Azure RemoteApp einrichten können."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 66366b25-6012-45fa-a4f6-da0ddfe0b486
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 232901ab919c63ea70e52afb845240b41a517c51
ms.lasthandoff: 03/31/2017


---
# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + Active Directory-Anforderungen für Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Wenn Sie mittels AD Connect einen Verbund für Ihre Azure RemoteApp-Hybrid-Sammlung oder eine Cloudsammlung erstellen möchten, müssen Sie folgende Aktionen ausführen.

### <a name="connect-azure-ad-and-active-directory"></a>Verbinden von Azure AD und Active Directory
Wenn Sie Ihren Azure AD-Mandanten und Ihre lokalen Active Directory-Umgebungen verbinden möchten, verwenden Sie AD Connect. Sie benötigen nur [4 Klicks](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) , um die beiden Verzeichnisse zu verbinden.

Hinweis: Für Hybrid-Sammlungen ist Verzeichnissynchronisierung erforderlich.

### <a name="make-sure-your-domaincom-match"></a>Sicherstellen der Übereinstimmung von „@domain.com“
Bevor Sie beginnen, stellen Sie sicher, dass der UPN für die lokale Gesamtstruktur mit dem Suffix Ihrer Azure AD-Domäne übereinstimmt. 

Nachdem Sie das UPN-Domänensuffix in Azure AD eingerichtet haben, werden alle Benutzer, die sich bei Azure RemoteApp anmelden, als „user@<the suffix you set up>“ angemeldet. Stellen Sie sicher, dass Benutzer sich mit derselben user@suffix auch bei der lokalen Domäne anmelden können. In bestimmten Fällen wurde möglicherweise ein Domänenname in Azure AD eingerichtet und für den lokalen Benutzer ein anderer Domänensuffix festgelegt. In diesem Fall können Ihre Benutzer über Azure RemoteApp keine Verbindung mit Computern oder Ressourcen herstellen, die der Domäne angehören.

Wenn Sie Ihr UPN-Domänensuffix in AAD z.B. als „contoso.com“ einrichten, einige Benutzer aber lokal bzw. in AD so konfiguriert sind, dass die Anmeldung mit @contoso.uk erfolgt, können sich diese Benutzer nicht ordnungsgemäß bei der ARA-Sammlung anmelden. Der UPN der Benutzer muss in AAD und AD identisch sein, damit die Anmeldung möglich ist.

### <a name="create-objects-for-azure-remoteapp"></a>Erstellen von Objekten für Azure RemoteApp
Sie müssen außerdem die folgenden lokalen Active Directory-Objekte erstellen:

* Ein Dienstkonto für den Zugriff auf Domänenressourcen für RemoteApp-Programme durch Verknüpfen von RDSH-Endpunkten mit der lokalen Domäne.
* Eine Organisationseinheit (OE), die RemoteApp-Computerobjekte enthält. Die Verwendung einer Organisationseinheit ist empfehlenswert (jedoch nicht unbedingt erforderlich), um die Konten und Richtlinien, die Sie mit RemoteApp verwenden, zu isolieren.

Sie benötigen diese beiden Objekte beim Erstellen Ihrer RemoteApp-Sammlung, daher führen Sie diese Schritte unbedingt zuerst aus.


