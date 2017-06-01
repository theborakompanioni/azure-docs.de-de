---
title: Steuern des Azure-Web-App-Verkehrs mit Azure Traffic Manager
description: Dieser Artikel bietet zusammenfassende Informationen zu Azure Traffic Manager im Hinblick auf Azure-Web-Apps.
services: app-service\web
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: 25502e4124442ed1853e3c3d9226107328c29316
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Steuern des Azure-Web-App-Verkehrs mit Azure Traffic Manager
> [!NOTE]
> Dieser Artikel bietet zusammenfassende Informationen zu Microsoft Azure Traffic Manager im Hinblick auf Azure Web Service-Web-Apps. Weitere Informationen über Azure Traffic Manager selbst finden Sie unter den Links am Ende dieses Artikels.
> 
> 

## <a name="introduction"></a>Einführung
Mit Azure Traffic Manager können Sie steuern, wie Anforderungen von Webclients auf Web-Apps in Azure Web Service verteilt werden. Wenn einem Azure Traffic Manager-Profil Azure-Web-App-Endpunkte hinzugefügt werden, verfolgt Azure Traffic Manager den Status Ihrer Web-Apps (aktiv, angehalten oder gelöscht), sodass der gewünschte Endpunkt als Empfänger des Verkehrs gewählt werden kann.

## <a name="load-balancing-methods"></a>Lastenausgleichsmethoden
Azure Traffic Manager verwendet drei verschiedene Lastenausgleichsmethoden. Diese werden in der folgenden Liste beschrieben, soweit sie Azure-Web-Apps betreffen.

* **Failover:** Wenn Sie in verschiedenen Regionen über Web-App-Klone verfügen, können Sie mit dieser Methode eine Web-App konfigurieren, um den gesamten Webclient-Datenverkehr zu bedienen. Dann können Sie eine andere Web-App in einer anderen Region konfigurieren, um den Datenverkehr zu verarbeiten, falls die erste Web-App nicht verfügbar sein sollte.
* **RoundRobin**Wenn Sie über Web-App-Klone in verschiedenen Regionen verfügen, können Sie mit dieser Methode den Verkehr gleichmäßig auf die Web-Apps in unterschiedlichen Regionen verteilen.
* **Leistung**:Mithilfe der Leistungsmethode wird der Verkehr anhand der kürzesten Roundtripzeit an Clients verteilt. Die Leistungsmethode kann für Web-Apps innerhalb derselben Region oder in verschiedenen Regionen verwendet werden.

## <a name="web-apps-and-traffic-manager-profiles"></a>Web-Apps und Traffic Manager-Profile
Für die Konfiguration zur Steuerung des Web-App-Verkehrs erstellen Sie ein Profil in Azure Traffic Manager, das eine der drei zuvor beschriebenen Lastenausgleichsmethoden verwendet. Fügen Sie dann dem Profil die Endpunkte (in diesem Fall Web-Apps) hinzu, für die Sie den Verkehr steuern möchten. Der Web-App-Status (aktiv, angehalten oder gelöscht) wird regelmäßig an das Profil übermittelt, sodass Azure Traffic Manager den Verkehr entsprechend leiten kann.

Beachten Sie die folgenden Aspekte, wenn Sie Azure Traffic Manager mit Azure verwenden:

* Bei reinen Web-App-Bereitstellungen innerhalb derselben Region bieten Web-Apps bereits eine Failover- und RoundRobin-Funktion, unabhängig vom Web-App-Modus.
* Bei Bereitstellungen in derselben Region, die Azure-Web-Apps zusammen mit anderen Azure-Cloud-Diensten verwenden, können Sie beide Endpunkttypen kombinieren, um Hybridszenarios zu ermöglichen.
* Sie können in einem Profil nur einen Web-App-Endpunkt pro Region angeben. Wenn Sie eine Web-App als Endpunkt für eine Region auswählen, stehen die verbleibenden Websites in dieser Region nicht mehr für dieses Profil zur Auswahl zur Verfügung.
* Die Web-App-Endpunkte, die Sie in einem Azure Traffic Manager-Profil festlegen, werden im Abschnitt **Domänennamen** auf der Konfigurationsseite für die Web-App im Profil angezeigt, dort jedoch nicht konfiguriert.
* Nachdem Sie einem Profil eine Web-App hinzugefügt haben, wird in der **Website-URL** im Dashboard der Portalseite der Web-App die benutzerdefinierte Domänen-URL der Web-App angezeigt, wenn Sie diese eingerichtet haben. Anderenfalls wird die URL des Traffic Manager-Profils angezeigt (z. B. `contoso.trafficmgr.com`). Sowohl der direkte Domänenname der Web-App als auch die Traffic Manager-URL werden auf der Konfigurationsseite der Web-App im Abschnitt **Domänennamen** angezeigt.
* Ihre benutzerdefinierten Domänennamen funktionieren wie erwartet. Sie fügen sie Ihren Web-Apps hinzu, müssen jedoch auch die DNS-Zuordnung konfigurieren, um auf die Traffic Manager-URL zu verweisen. Informationen zum Einrichten einer benutzerdefinierten Domäne für eine Azure-Web-App finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine Azure-Website](app-service-web-tutorial-custom-domain.md).
* Sie können einem Azure Traffic Manager-Profil nur Web-Apps hinzufügen, die sich im Standardmodus befinden.

## <a name="next-steps"></a>Nächste Schritte
Einen Überblick über die Konzepte und technischen Aspekte von Azure Traffic Manager finden Sie unter [Traffic Manager-Übersicht](../traffic-manager/traffic-manager-overview.md).

Weitere Informationen zur Verwendung von Traffic Manager mit Web-Apps finden Sie in den Blogbeiträgen [Using Azure Traffic Manager with Azure Web Sites](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) (Verwenden von Azure Traffic Manager mit Azure-Websites) und [Azure Traffic Manager can now integrate with Azure Web Sites](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/) (Azure Traffic Manager kann jetzt in Azure-Websites integriert werden).


