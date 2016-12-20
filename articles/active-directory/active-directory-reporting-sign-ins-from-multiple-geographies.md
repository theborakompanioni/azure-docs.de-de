---
title: Anmeldungen aus mehreren geografischen Regionen
description: "Ein Bericht, der auf Benutzer hinweist, bei denen zwei Anmeldungen aus verschiedenen Regionen zu stammen scheinen und die Zeit zwischen den Anmeldungen es dem jeweiligen Benutzer unmöglich macht, die Entfernung zwischen diesen Regionen reisend zurückgelegt zu haben."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: 79259c8a-2388-4747-b41e-c07434ea9a02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1de57f64692ade442f9ef8d1e3b587ffee35d7cf


---
# <a name="sign-ins-from-multiple-geographies"></a>Anmeldungen aus mehreren geografischen Regionen
Dieser Bericht enthält erfolgreiche Anmeldungen eines Benutzers, wobei zwei Anmeldungen aus verschiedenen Regionen zu stammen scheinen, und die Zeit zwischen den Anmeldungen so kurz ist, dass der Benutzer die Entfernung zwischen diesen Regionen nicht zurückgelegt haben kann. Mögliche Ursachen:

* Der Benutzer hat sein Kennwort für andere Benutzer freigegeben.
* Der Benutzer verwendet einen Remotedesktop, um einen Webbrowser für die Anmeldung zu starten.
* Ein Hacker hat sich von einem anderen Land aus in dem Konto eines Benutzers angemeldet.
* Der Benutzer verwendet eine VPN-Verbindung oder einen Proxy.
* Der Benutzer ist über mehrere Geräte gleichzeitig angemeldet, z. B. über einen Desktop und ein Mobiltelefon, und die IP-Adresse des Mobiltelefons ist ungewöhnlich.

Die Ergebnisse aus diesem Bericht zeigen Ihnen die erfolgreichen Anmeldeereignisse, die Dauer zwischen den Anmeldungen, die Regionen, aus denen die Anmeldungen scheinbar erfolgten, und die geschätzte Reisezeit zwischen diesen Regionen, an. Die angezeigte Reisezeit ist lediglich eine Schätzung und unterscheidet sich möglicherweise von der tatsächlichen Reisezeit zwischen den Orten.

![Anmeldungen aus mehreren geografischen Regionen](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)




<!--HONumber=Nov16_HO3-->


