---
title: "Übersicht über Azure DNS | Microsoft Docs"
description: "Übersicht über Azure DNS-Hostingdienste in Microsoft Azure. Hosten Ihrer Domäne in Microsoft Azure."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 42d47741e414b2de177f1fd75b3e1ac3fde96579
ms.openlocfilehash: 32b6c12a07f67d39d7a1dcf213f0372d3c3cd40c

---

# <a name="azure-dns-overview"></a>Azure DNS – Übersicht

Das Domain Name System (DNS) ist für die Übersetzung (oder Auflösung) eines Website- oder Dienstnamens in die IP-Adresse verantwortlich. Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten.

DNS-Domänen in Azure DNS werden im globalen Azure-Netzwerk von DNS-Servern gehostet. Wir verwenden Anycast-Netzwerke, sodass jede DNS-Abfrage vom nächsten verfügbaren DNS-Server beantwortet wird. Damit wird eine schnelle Leistung und hohe Verfügbarkeit für Ihre Domäne sichergestellt.

Der Azure DNS-Dienst basiert auf Azure Resource Manager (ARM). Daher profitiert er von ARM-Funktionen, wie z. B. die rollenbasierte Zugriffskontrolle, Überwachungsprotokolle und Ressourcensperren. Ihren Domänen und Einträge können über das Azure-Portal, Azure PowerShell-Cmdlets und die plattformübergreifende Azure-Befehlszeilenschnittstelle verwaltet werden. Anwendungen, die eine automatische DNS-Verwaltung erfordern, können über die REST-API und SDKs mit dem Dienst zusammenarbeiten.

Azure DNS unterstützt derzeit nicht den Kauf von Domänennamen. Wenn Sie Domänen erwerben möchten, müssen Sie eine von einem Drittanbieter angebotene Registrierungsstelle für Domänennamen verwenden. Die Registrierungsstelle erhebt in der Regel eine geringe jährliche Gebühr. Die Domänen können dann in Azure DNS für die Verwaltung von DNS-Einträgen gehostet werden. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md) .

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer DNS-Zone](dns-getstarted-create-dnszone-portal.md)




<!--HONumber=Nov16_HO3-->


