---
title: "Einführung in die IP-Datenflussüberprüfung in Azure Network Watcher | Microsoft Docs"
description: "Diese Seite enthält eine Übersicht über die Network Watcher-Funktion zur IP-Datenflussüberprüfung."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: bf015f8f646ecce6821379affd4d041329967fc8
ms.contentlocale: de-de
ms.lasthandoff: 03/04/2017

---

# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Einführung in die IP-Datenflussüberprüfung in Azure Network Watcher

Bei der IP-Datenflussüberprüfung wird basierend auf 5-Tupel-Informationen geprüft, ob ein Paket zu oder von einem virtuellen Computer zugelassen oder verweigert wird. Diese Informationen enthalten die Richtung, das Protokoll, die lokale IP-Adresse, die Remote-IP-Adresse, den lokalen Port und den Remoteport. Wenn das Paket von einer Sicherheitsgruppe abgelehnt wird, wird der Name der Regel, die das Paket verweigert hat, zurückgegeben. Es können beliebige Quell- oder Ziel-IP-Adressen ausgewählt werden. Damit hilft dieses Feature Administratoren bei der schnellen Diagnose von Verbindungsproblemen mit dem Internet und in der lokalen Umgebung.

Die IP-Datenflussüberprüfung nutzt als Ziel eine Netzwerkschnittstelle eines virtuellen Computers. Der Datenfluss zu oder von dieser Netzwerkschnittstelle wird dann basierend auf den konfigurierten Einstellungen überprüft. Diese Funktion ist nützlich, um zu bestätigen, ob eine Regel in einer Netzwerksicherheitsgruppe ein- oder ausgehenden Datenverkehr eines virtuellen Computers blockiert.

Es muss in jeder Region, in der Sie die IP-Datenflussüberprüfung ausführen möchten, eine Instanz von Network Watcher erstellt werden. Network Watcher ist ein regionaler Dienst, der nur für Ressourcen in derselben Region ausgeführt werden kann. Dies hat jedoch keine Auswirkungen auf die Ergebnisse der IP-Datenflussüberprüfung, da trotzdem die Route zurückgegeben wird, die der Netzwerkschnittstelle zugeordnet ist.

![1][1]

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Artikel erfahren Sie, wie Sie im Portal ermitteln, ob ein Paket für einen bestimmten virtuellen Computer zugelassen oder verweigert wird. [Überprüfen mit der IP-Datenflussüberprüfung im Portal, ob Datenverkehr auf einem virtuellen Computer zugelassen wird](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png













