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
ms.date: 12/21/2016
ms.author: zarizvi
translationtype: Human Translation
ms.sourcegitcommit: 078833a21cd309087b842e21e3fc730695c254dd
ms.openlocfilehash: 89fdeca4ffc1a36e8aff829b00483f208fa03e0f


---
# <a name="supported-move-configurations"></a>Unterstützte Konfigurationen für das Verschieben
Sie können Azure-Web-App-Ressourcen mit der [Resource Manager-API zum Verschieben von Ressourcen](../azure-resource-manager/resource-group-move-resources.md) verschieben.

Azure-Web-Apps unterstützen derzeit die folgenden Szenarien beim Verschieben:

* Verschieben des gesamten Inhalts einer Ressourcengruppe (Web-Apps, App Service-Pläne und Zertifikate) in eine andere Ressourcengruppe 
   > [!Note]
   > In diesem Szenario darf die Zielressourcengruppe keine Microsoft.Web-Ressourcen enthalten.

* Verschieben einzelner Web-Apps in eine andere Ressourcengruppe, während sie weiterhin im aktuellen App Service-Plan gehostet werden (Der App Service-Plan bleibt in der alten Ressourcengruppe.)





<!--HONumber=Dec16_HO3-->


