---
title: Erstellen von SharePoint-Serverfarmen in Azure | Microsoft Docs
description: Erstellen Sie schnell eine neue SharePoint 2013- oder SharePoint 2016-Farm in Azure mithilfe des Marketplace im Azure-Portal.
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 89b124da-019d-4179-86dd-ad418d05a4f2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 32e30b44c2f7cfa9c1069190fdc53dbe6e9f4cd5
ms.openlocfilehash: b668e32bf00ec32c7ffa77594bd1fb07440cf1ac
ms.lasthandoff: 03/01/2017


---
# <a name="create-sharepoint-server-farms-using-the-azure-portal-marketplace"></a>Erstellen von SharePoint-Serverfarmen mithilfe des Marketplace im Azure-Portal

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a>SharePoint 2013-Farmen
Mit dem Marketplace im Microsoft Azure-Portal können Sie schnell eine vorkonfigurierte SharePoint Server 2013-Farm erstellen. Damit können Sie viel Zeit sparen, wenn Sie eine grundlegende oder hochverfügbare SharePoint-Farm für eine Dev/Test-Umgebung benötigen oder wenn Sie SharePoint Server 2013 als Zusammenarbeitslösung für Ihre Organisation bewerten möchten.

> [!NOTE]
> Das Element **SharePoint-Serverfarm** wurde aus dem Azure Marketplace des Azure-Portals entfernt. Es wurde durch die Elemente **SharePoint 2013 non-HA Farm** (Nicht hoch verfügbare SharePoint 2013-Farm) und **SharePoint 2013 HA Farm** (Hoch verfügbare SharePoint 2013-Farm) ersetzt.
>
>

Die grundlegende SharePoint-Farm besteht aus drei virtuellen Computern mit der folgenden Konfiguration:

![SharePoint-Farm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

Sie können diese Farmkonfiguration für eine vereinfachte Einrichtung für die SharePoint-App-Entwicklung oder für eine erste Bewertung von SharePoint 2013 verwenden.

So erstellen Sie die grundlegende SharePoint-Farm (mit drei Servern):

1. Klicken Sie [hier](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klicken Sie auf **Bereitstellen**.
3. Klicken Sie im Bereich **SharePoint 2013 non-HA Farm** (Nicht hoch verfügbare SharePoint 2013-Farm) auf **Erstellen**.
4. Geben Sie die Einstellungen für die Schritte im Bereich **Create SharePoint 2013 non-HA Farm** (Nicht hoch verfügbare SharePoint 2013-Farm erstellen) ein, und klicken Sie dann auf **Erstellen**.

Die hochverfügbare SharePoint-Farm besteht aus neun virtuellen Computern mit der folgenden Konfiguration:

![SharePoint-Farm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

Sie können diese Farmkonfiguration verwenden, um höhere Clientlasten, Hochverfügbarkeit eines externen SharePoint-Standorts sowie SQL Server Always On-Verfügbarkeitsgruppen für eine SharePoint-Farm zu testen. Außerdem können Sie diese Konfiguration für die SharePoint-App-Entwicklung in einer Hochverfügbarkeitsumgebung nutzen.

So erstellen Sie die hoch verfügbare SharePoint-Farm (neun Server):

1. Klicken Sie [hier](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klicken Sie auf **Bereitstellen**.
3. Klicken Sie im Bereich **SharePoint 2013 HA Farm** (Hoch verfügbare SharePoint 2013-Farm) auf **Erstellen**.
4. Geben Sie die Einstellungen für die sieben Schritte im Bereich **Create SharePoint 2013 HA Farm** (Hoch verfügbare SharePoint 2013-Farm erstellen) ein, und klicken Sie dann auf **Erstellen**.

> [!NOTE]
> Mit einer kostenlosen Azure-Testversion kann keine **nicht hoch verfügbare SharePoint 2013-Farm** bzw. **hoch verfügbare SharePoint 2013-Farm** erstellt werden.
>
>

Das Azure-Portal erstellt beide Farmen in einem virtuellen Cloudnetzwerk mit einer über das Internet zugänglichen Webpräsenz. Es gibt keine Standort-zu-Standort-VPN- oder ExpressRoute-Verbindung mit Ihrem Unternehmensnetzwerk.

> [!NOTE]
> Wenn Sie im Azure-Portal SharePoint-Farmen mit normaler oder hoher Verfügbarkeit erstellen, können Sie keine vorhandene Ressourcengruppe angeben. Um diese Einschränkung zu umgehen, erstellen Sie diese Farmen mit Azure PowerShell. Weitere Informationen finden Sie unter [Erstellen von SharePoint 2013-Entwicklungs-/Testumgebungen mit Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).
>
>

## <a name="sharepoint-2016-farms"></a>SharePoint 2016-Farmen
[In diesem Artikel](https://technet.microsoft.com/library/mt723354.aspx) finden Sie Anweisungen zum Erstellen der folgenden SharePoint Server 2016-Einzelserverfarm.

![SharePoint-Farm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>Verwalten der SharePoint-Farmen
Sie können die Server dieser Farmen über Remotedesktopverbindungen verwalten. Weitere Informationen finden Sie unter [Anmelden beim virtuellen Computer](virtual-machines-windows-hero-tutorial.md#connect-to-the-virtual-machine-and-sign-on).

Auf der SharePoint-Website für die Zentraladministration können Sie eigene Websites, SharePoint-Anwendungen und andere Funktionen konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Nächste Schritte
* Lernen Sie weitere Konfigurationen von [SharePoint](https://technet.microsoft.com/library/dn635309.aspx) in Azure-Infrastrukturdiensten kennen.

