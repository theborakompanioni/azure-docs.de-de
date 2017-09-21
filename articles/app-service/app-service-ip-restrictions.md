---
title: "Azure App Service-IP-Einschränkungen | Microsoft-Dokumentation"
description: "Informationen zum Verwenden von IP-Einschränkungen mit Azure App Service"
author: btardif
manager: stefsch
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/12/2017
ms.author: byvinyal
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 83cdc42d412f646ddf1ecd1b65bf9aa46983b26b
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="azure-app-service-static-ip-restrictions"></a>Statische Azure App Service-IP-Einschränkungen #

Durch IP-Einschränkungen können Sie eine Liste von IP-Adressen definieren, für die der Zugriff auf Ihre App blockiert wird. Die Liste der blockierten IP-Adressen kann einzelne IP-Adressen oder einen Bereich von IP-Adressen enthalten, die von einer Subnetzmaske definiert werden.

Wenn von einem Client eine Anforderung an die App generiert wird, wird die IP-Adresse mit der Liste der blockierten IP-Adressen abgeglichen. Liegt eine Übereinstimmung vor, antwortet die App mit dem Statuscode [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403).

IP-Einschränkungen werden auf denselben App Service-Planinstanzen ausgewertet, die Ihrer App zugewiesen sind.

Um eine IP-Einschränkungsregel zu Ihrer App hinzuzufügen, wählen Sie im Menü **Netzwerk**>**IP-Einschränkungen**, und klicken Sie auf **IP-Einschränkungen konfigurieren**.

![IP-Einschränkungen] (media/app-service-ip-restrictions/ip-restrictions.png)

In dieser Ansicht können Sie die Liste der IP-Einschränkungsregeln überprüfen, die für Ihre App definiert sind.

![Liste der IP-Einschränkungen](media/app-service-ip-restrictions/browse-ip-restrictions.png)

Klicken Sie auf **[+] Hinzufügen**, um eine neue IP-Einschränkungsregel hinzuzufügen.

![Hinzufügen von IP-Einschränkungen](media/app-service-ip-restrictions/add-ip-restrictions.png)

