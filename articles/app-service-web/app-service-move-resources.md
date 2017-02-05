---
title: Verschieben von Web-App-Ressourcen in eine andere Ressourcengruppe
description: "Beschreibt die Szenarien, in denen Sie Web-Apps und App Services aus einer Ressourcengruppe in eine andere verschieben können."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: wpickett
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2016
ms.author: zarizvi
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 08b8953b91a87a131f70309e41be3a9ce0123299


---
# <a name="supported-move-configurations"></a>Unterstützte Konfigurationen für das Verschieben
Sie können Azure-Web-App-Ressourcen mit der [ARM-API zum Verschieben von Ressourcen](../resource-group-move-resources.md)verschieben.

Azure-Web-Apps unterstützen derzeit die folgenden Szenarien beim Verschieben:

* Verschieben des gesamten Inhalts einer Ressourcengruppe (Web-Apps, App Service-Pläne und Zertifikate) in eine andere Ressourcengruppe 
  * Hinweis: In diesem Szenario darf die Zielressourcengruppe keine Microsoft.Web-Ressourcen enthalten.
* Verschieben einzelner Web-Apps in eine andere Ressourcengruppe, während sie weiterhin im aktuellen App Service-Plan gehostet werden (Der App Service-Plan bleibt in der alten Ressourcengruppe.)




<!--HONumber=Nov16_HO3-->


