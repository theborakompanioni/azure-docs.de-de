---
title: "Verweisen einer Unternehmensinternetdomäne auf einen Traffic Manager-Domänennamen | Microsoft Docs"
description: "In diesem Artikel erfahren Sie, wie Sie mit Ihrem Unternehmensdomänennamen auf einen Traffic Manager-Domänennamen verweisen."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 29822946-2d45-4434-ba47-fc180a445cc3
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 0322b3510cfd4f94031d8c1db8f1cc032b997fa8

---

# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>Verweisen einer Unternehmens-Internetdomäne auf einen Azure Traffic Manager-Domänennamen

Wenn Sie ein Traffic Manager-Profil erstellen, weist Azure dem Profil automatisch einen DNS-Name zu. Um einen Namen aus Ihrer DNS-Zone zu verwenden, erstellen Sie einen CNAME-DNS-Eintrag, der dem Domänennamen des Traffic Manager-Profils zugeordnet ist. Sie finden den Traffic Manager-Domänennamen auf der Konfigurationsseite des Traffic Manager-Profils im Abschnitt **Allgemein** .

Um beispielsweise mit dem Namen www.contoso.com auf den Traffic Manager-DNS-Namen contoso.trafficmanager.net zu verweisen, ändern Sie den DNS-Ressourceneintrag wie folgt:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Alle Datenverkehrsanforderungen an *www.contoso.com* werden jetzt an *contoso.trafficmanager.net* umgeleitet.

> [!IMPORTANT]
> Sie können nicht mit Domänen der zweiten Ebene wie *contoso.com*auf Traffic Manager-Domänen verweisen. Die DNS-Protokollstandards erlauben keine CNAME-Einträge für Domänennamen der zweiten Ebene.

## <a name="next-steps"></a>Nächste Schritte

* [Traffic Manager-Routingmethoden](traffic-manager-routing-methods.md)
* [Deaktivieren, Aktivieren oder Löschen eines Traffic Manager-Profils](disable-enable-or-delete-a-profile.md)
* [Deaktivieren oder Aktivieren eines Traffic Manager-Endpunkts](disable-or-enable-an-endpoint.md)



<!--HONumber=Nov16_HO5-->


