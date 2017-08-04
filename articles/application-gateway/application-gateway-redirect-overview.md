---
title: "Übersicht über die Umleitung in Azure Application Gateway | Microsoft-Dokumentation"
description: Hier finden Sie Informationen zur Umleitungsfunktion in Azure Application Gateway.
services: application-gateway
documentationcenter: na
author: amsriva
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2017
ms.author: amsriva
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: ea9ae8373ff67bf9557b06bbc8a4b0d82a03e2d0
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---

# <a name="application-gateway-redirect-overview"></a>Übersicht über die Umleitung in Application Gateway

Ein typisches Szenario vieler Webanwendungen ist die Unterstützung der automatischen Umleitung von HTTP zu HTTPS, um sicherzustellen, dass die gesamte Kommunikation zwischen einer Anwendung und ihren Benutzern über einen verschlüsselten Pfad stattfindet. In der Vergangenheit haben Kunden unter anderem dedizierte Back-End-Pools erstellt, die einzig dazu dienten, eingehende HTTP-Anforderungen zu HTTPS umzuleiten.  Application Gateway unterstützt nun die Umleitung von Application Gateway-Datenverkehr. Dies vereinfacht die Anwendungskonfiguration, optimiert die Ressourcennutzung und ermöglicht neue Umleitungsszenarien (einschließlich globale und pfadbasierte Umleitung). Die Application Gateway-Umleitung ist nicht auf HTTP zu HTTPS beschränkt. Vielmehr handelt es sich hierbei um einen allgemeinen Umleitungsmechanismus, mit dem Datenverkehr, der an einem Listener empfangen wird, in Application Gateway an einen anderen Listener umgeleitet werden kann. Auch die Umleitung an eine externe Website wird unterstützt. Die Application Gateway-Umleitung bietet Folgendes:

1. Globale Umleitung von einem Listener zu einem anderen Listener des Gateways. Dies ermöglicht HTTP-zu-HTTPS-Umleitungen auf einer Website.
2. Pfadbasierte Umleitung. Bei dieser Art von Umleitung kann HTTP nur für einen bestimmten Websitebereich zu HTTPS umgeleitet werden – etwa in einem durch /cart/* gekennzeichneten Einkaufswagenbereich.
3. Umleitung an eine externe Website.

![Umleitung](./media/application-gateway-redirect-overview/redirect.png)

Aufgrund dieser Änderung müssen Kunden ein neues Umleitungskonfigurationsobjekt erstellen, das den Ziellistener oder die externe Website angibt, an den bzw. an die die Umleitung erfolgen soll. Das Konfigurationselement unterstützt auch Optionen zum Anfügen von URI-Pfad und Abfragezeichenfolge an die umgeleitete URL. Kunden können bei Bedarf auch auswählen, ob es sich um eine vorübergehende Umleitung (HTTP-Statuscode 302) oder um eine dauerhafte Umleitung (HTTP-Statuscode 301) handelt. Die erstellte Umleitungskonfiguration wird mittels einer neuen Regel an den Quelllistener angefügt. Bei Verwendung einer einfachen Regel wird die Umleitungskonfiguration einem Quelllistener zugeordnet und fungiert als globale Umleitung. Bei Verwendung einer pfadbasierten Regel wird die Umleitungskonfiguration für die URL-Pfadzuordnung definiert und gilt daher nur für den spezifischen Pfadbereich einer Website.

### <a name="next-steps"></a>Nächste Schritte

[Konfigurieren der Umleitung für Application Gateway mit PowerShell](application-gateway-configure-redirect-powershell.md)

