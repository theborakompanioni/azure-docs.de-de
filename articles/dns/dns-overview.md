---
title: "Übersicht über Azure DNS | Microsoft Docs"
description: "Übersicht über DNS-Hostingdienste in Microsoft Azure. Hosten Ihrer Domäne in Microsoft Azure."
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
ms.date: 04/19/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: 3705457e4c90f8869496f7f5177531bd128d1057
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---

# <a name="azure-dns-overview"></a>Übersicht über Azure DNS

Das Domain Name System (DNS) ist für die Übersetzung (oder Auflösung) eines Website- oder Dienstnamens in die IP-Adresse verantwortlich. Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten.

![Übersicht über DNS](./media/dns-overview/scenario.png)

## <a name="features"></a>Features

* **Zuverlässigkeit und Leistung**: DNS-Domänen in Azure DNS werden im globalen Azure-Netzwerk von DNS-Namenservern gehostet. Wir verwenden Anycast-Netzwerke, sodass jede DNS-Abfrage vom nächsten verfügbaren DNS-Server beantwortet wird. Damit wird eine schnelle Leistung und hohe Verfügbarkeit für Ihre Domäne sichergestellt.

* **Nahtlose Integration**: Der Azure-DNS-Dienst kann verwendet werden, um DNS-Einträge für Ihre Azure-Dienste zu verwalten und auch um DNS für externe Ressourcen bereitzustellen. Azure DNS ist in das Azure-Portal integriert und verwendet die gleichen Anmeldeinformationen, die gleiche Abrechnung und den gleichen Supportvertrag wie Ihre anderen Azure-Dienste.

* **Sicherheit**: Der Azure DNS-Dienst basiert auf Azure Resource Manager. Daher profitiert er von Resource Manager-Funktionen, wie z.B. die rollenbasierte Zugriffskontrolle, Überwachungsprotokolle und Ressourcensperren. Ihren Domänen und Einträge können über das Azure-Portal, Azure PowerShell-Cmdlets und die plattformübergreifende Azure-Befehlszeilenschnittstelle verwaltet werden. Anwendungen, die eine automatische DNS-Verwaltung erfordern, können über die REST-API und SDKs mit dem Dienst zusammenarbeiten.

Azure DNS unterstützt derzeit nicht den Kauf von Domänennamen. Wenn Sie Domänen erwerben möchten, müssen Sie eine von einem Drittanbieter angebotene Registrierungsstelle für Domänennamen verwenden. Die Registrierungsstelle erhebt in der Regel eine geringe jährliche Gebühr. Die Domänen können dann in Azure DNS für die Verwaltung von DNS-Einträgen gehostet werden. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md) .

## <a name="pricing"></a>Preise

Die Abrechnung von Azure DNS basiert auf der Anzahl von DNS-Zonen, die in Azure gehostet werden, und der Anzahl der DNS-Abfragen. Weitere Informationen zu Preisen finden Sie in den [Preisangaben zu Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

## <a name="faq"></a>Häufig gestellte Fragen

Häufig gestellte Fragen zu Azure DNS finden Sie unter der [Häufig gestellte Fragen zu Azure DNS](dns-faq.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu DNS-Zonen und -Einträgen im folgenden Artikel: [DNS-Zonen und -Einträge: Übersicht](dns-zones-records.md).

Erfahren Sie, wie Sie in Azure DNS [eine DNS-Zone erstellen](./dns-getstarted-create-dnszone-portal.md).

Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.


