---
title: "Liste mit Ports und URLs, die für Azure RemoteApp im Virtual Network von Kunden auf die Positivliste gesetzt werden | Microsoft Docs"
description: "Erfahren Sie, welche Ports und URLs Sie für die Kommunikation über Azure RemoteApp konfigurieren müssen."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: 5a001ff7-14c9-47fa-9b39-78fd5a5f0250
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 79f6398b861597e3303231bc17f69be1bde947ae
ms.openlocfilehash: ef052d074b593e4bbecfdbfb3af76d1ba1249e33


---
# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Liste mit Ports und URLs zum Gewähren des Zugriffs für Azure RemoteApp im Virtual Network des Kunden
> [!IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Lesen Sie die folgenden Informationen zu Ports, wenn Sie eine Azure RemoteApp-Cloud- oder Hybridsammlung in einem virtuellen Netzwerk (VNET) bereitstellen. Weitere Informationen über virtuelle Netzwerke erhalten Sie unter [Übersicht über virtuelle Netzwerke](../virtual-network/virtual-networks-overview.md). Angenommen, Sie haben eine Netzwerksicherheitsgruppe (NSG) erstellt, bei der der Datenverkehr auf die Ressourcen des virtuellen Netzwerks in Ihrer Sammlung beschränkt wird. Stellen Sie in diesem Fall sicher, dass die folgenden Ports erreichbar sind und von den Sicherheitsrichtlinien im virtuellen Netzwerk zugelassen werden. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)

## <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Für das Azure RemoteApp-Subnetz ist der Zugriff auf diese Endpunkte und URLs erforderlich:
* *.servicebus.windows.net
* *.servicebus.net
* https://*.remoteapp.windowsazure.com  
* https://www.remoteapp.windowsazure.com 
* https://*remoteapp.windowsazure.com  
* https://*.core.windows.net  
* Ausgehend: TCP: 443, TCP: 10101 - 10175 
* Optional – UDP: 10201-10275  

## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp-Clients benötigen Zugriff auf diese Endpunkte und URLs:
Mit Clients sind die Desktops, Geräte usw. gemeint, mit denen Benutzer eine Verbindung mit den Apps herstellen, die in der Azure RemoteApp-Sammlung bereitgestellt werden.

* https://telemetry.remoteapp.windowsazure.com  
* https://*.remoteapp.windowsazure.com (die optionalen UDP-Ports gelten für diese Adresse) 
* https://login.windows.net  
* https://login.microsoftonline.com  
* https://www.remoteapp.windowsazure.com 
* https://*.core.windows.net  
* Ausgehend: TCP: 443  
* Optional – UDP: 3391 




<!--HONumber=Dec16_HO3-->


