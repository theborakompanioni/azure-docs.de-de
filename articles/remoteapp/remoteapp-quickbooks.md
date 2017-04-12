---
title: Bereitstellen von QuickBooks in Azure RemoteApp | Microsoft Docs
description: Erfahren Sie, wie Sie QuickBooks mit Azure RemoteApp freigeben.
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c5d00753-77c0-4f0d-a5df-9451b46a31d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 17fbef55920c4f06f895739b89a0acc6ff08bd9d
ms.lasthandoff: 03/31/2017


---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Bereitstellen von QuickBooks in Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Verwenden Sie die folgenden Informationen, um QuickBooks als App in Azure RemoteApp freizugeben.

Sie können QuickBooks 2015 Enterprise mit Azure RemoteApp in einer Hybrid- oder Cloud-Sammlung freigeben. Die Unternehmensdatei muss sich auf einem virtuellen Computer befinden, auf dem der QuickBooks-Datenbankserver ausgeführt wird, der von den Azure RemoteApp-Servern getrennt ist. Speichern Sie die Unternehmensdatei niemals auf Ihrem Azure RemoteApp-Image, da es in diesem Fall zu Datenverlusten kommen kann. Nur QuickBooks Enterprise unterstützt das Hosten der QuickBooks-Datei auf einer externen Freigabe, wobei der QuickBooks-Datenbankserver über eine standardmäßige Windows-Netzwerkumgebung zugänglich ist.   

> [!IMPORTANT]
> Der QuickBooks-Datenbankserver, auf dem die Unternehmensdatei gehostet wird, muss sich auf einem separaten virtuellen Computer in demselben VNET wie die Azure RemoteApp-Sammlung befinden.  
> 
> 

## <a name="steps-to-deploy-quickbooks"></a>Schritte zum Bereitstellen von QuickBooks
1. Erstellen Sie einen virtuellen Azure-Computer, und installieren Sie QuickBooks und den QuickBooks-Datenbankserver, und stellen Sie die Unternehmensdatei auf einem virtuellen Azure-Computer bereit.  Achten Sie darauf, die Firewallregeln richtig zu konfigurieren.
2. Installieren Sie QuickBooks auf einem [benutzerdefinierten Image](remoteapp-imageoptions.md), und erstellen Sie eine [Azure RemoteApp-Sammlung](remoteapp-collections.md) (Cloud oder Hybrid) in demselben VNET, in dem sich der virtuelle Computer zum Hosten des QuickBooks-Datenbankservers mit Unternehmensdateien befindet. 
3. [Veröffentlichen](remoteapp-publish.md) der QuickBooks-App für Benutzer
4. Starten Sie den von Azure RemoteApp gehosteten QuickBooks-Client, navigieren Sie mit der standardmäßigen Windows-Netzwerkverbindung zum virtuellen Computer, auf dem der QuickBooks-Datenbankserver gehostet wird, und öffnen Sie die Unternehmensdatei. 

## <a name="documentation-references"></a>Dokumentationsverweise
* Von QuickBooks [unterstützte Konfigurationen](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
* QuickBooks- [Bereitstellungsoptionen](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Sie können sich auch meine Ignite-Präsentation mit dem Titel [Fundamentals of Microsoft Azure RemoteApp Management and Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) (in englischer Sprache) ansehen. Der QuickBooks-Teil beginnt bei 1:02:45.

## <a name="deployment-architecture"></a>Bereitstellungsarchitektur
![QuickBooks- und Azure RemoteApp-Bereitstellung](./media/remoteapp-quickbooks/ra-quickbooks.png)


