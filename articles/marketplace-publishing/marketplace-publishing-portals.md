---
title: "Übersicht über die verschiedenen Portale, die zum Erstellen eines Angebots für den Marketplace benötigt werden | Microsoft Docs"
description: "Übersicht über die verschiedenen Portale, die zum Erstellen eines Angebots für den Marketplace benötigt werden"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 89ce82b3-c28a-4b0d-b37a-db3112160a4e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8087f27a692e1e2d0f2d9434b065ddb902de0d57


---
# <a name="portals-you-will-need"></a>Portale, die Sie benötigen
Bevor Sie mit der Veröffentlichung eines Angebots beginnen, sollten Sie sich mit den verschiedenen Portalen vertraut machen, die Sie dazu benötigen. Im Anschluss folgt eine kurze Zusammenfassung der Portale (Developer Center, Azure-Veröffentlichungsportal und Azure-Portal) in der Reihenfolge, in der Sie mit ihnen interagieren.                                                                            

## <a name="developer-center"></a>Developer Center
[http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure)

### <a name="description"></a>Beschreibung
Das Erstellen Ihres Microsoft Developer Center-Kontos ist eine einmalige Aufgabe. Vergewissern Sie sich vor der Kontoerstellung, dass Ihr Unternehmen nicht bereits über ein Developer Center-Konto verfügt. Während des Prozesses erfassen wir Bankdaten, Steuerinformationen und Adressdaten Ihres Unternehmens.

> [!NOTE]
> Wenn Sie nur kostenlose Angebote (oder Angebote mit Verwendung Ihrer eigenen Lizenz) veröffentlichen, benötigen wir keine steuerlichen Informationen oder Bankdaten.
> 
> 

### <a name="identityaccount-used"></a>Verwendete Identität/Verwendetes Konto
Im Idealfall ist dies eine Verteilerliste oder Sicherheitsgruppe (z.B. azurepublishing@*partnercompany*.com). Diese Verteilerliste oder Sicherheitsgruppe **muss** als Microsoft-Konto registriert sein.

> [!TIP]
> Wir empfehlen die Verwendung einer Verteilerliste oder Sicherheitsgruppe, um die Abhängigkeit von einer Einzelperson zu vermeiden. Es kann jedoch auch ein einzelnes Konto verwendet werden.
> 
> 

## <a name="publishing-portal"></a>Veröffentlichungsportal
[https://publish.windowsazure.com](https://publish.windowsazure.com)

### <a name="description"></a>Beschreibung
Dieses Portal dient zum Bearbeiten und Veröffentlichen des Angebots. Hierzu zählen unter anderem Marketing, Preisgestaltung, Veröffentlichung und ggf. Zertifizierung.

### <a name="identityaccount-used"></a>Verwendete Identität/Verwendetes Konto
Die weiter oben erwähnte Verteilerliste oder Sicherheitsgruppe muss bei der erstmaligen Anmeldung beim Veröffentlichungsportal verwendet werden. Später können dann weitere Benutzer als Co-Admins hinzugefügt werden. So erfolgt die Zuordnung zu den Registrierungsdaten im Developer Center.

## <a name="azure-portal"></a>Azure-Portal
[https://portal.azure.com](https://portal.azure.com)

### <a name="description"></a>Beschreibung
Dies ist das Portal, in dem Sie Ihre bereitgestellten und veröffentlichten Angebote im Azure Marketplace anzeigen können (gilt für virtuelle Computer, Lösungsvorlagen und auf dem Azure-Ressourcen-Manager basierende Entwicklerdienste).

### <a name="identityaccount-used"></a>Verwendete Identität/Verwendetes Konto
Beim Bereitstellen des Angebots über das Veröffentlichungsportal muss eine Abonnement-ID auf die Whitelist gesetzt werden. Das gleiche Abonnement (mit zugeordnetem Benutzernamen und Kennwort) muss bei der Portalanmeldung verwendet werden, um das bereitgestellte Angebot zu testen.

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte: Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)




<!--HONumber=Nov16_HO3-->


