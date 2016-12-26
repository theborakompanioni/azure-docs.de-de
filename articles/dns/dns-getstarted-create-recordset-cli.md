---
title: "Erstellen einer Datensatzgruppe und von Einträgen für eine DNS-Zone mithilfe der Befehlszeilenschnittstelle (CLI) | Microsoft Docs"
description: "Erstellen von Hosteinträgen für Azure DNS. Einrichten von Datensatzgruppen und Einträgen mithilfe der Befehlszeilenschnittstelle (CLI)"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/09/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: e377f176fe24a8e7e42d409f86d6b0093ce5e7c4

---

# <a name="create-dns-record-sets-and-records-by-using-cli"></a>Erstellen von DNS-Ressourceneintragssätzen und Einträgen über die CLI

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-recordset-cli.md)

Dieser Artikel beschreibt das Erstellen von Einträgen und Eintragssätzen mithilfe der CLI. Zu diesem Zweck müssen Sie zunächst Grundlegendes zu DNS-Einträgen und Datensatzgruppen verstehen.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

In diesem Abschnitt erfahren Sie, wie Sie DNS-Einträge in Azure DNS erstellen. Bei den Beispielen wird vorausgesetzt, dass Sie bereits [die Azure-Befehlszeilenschnittstelle installiert haben, angemeldet sind und eine DNS-Zone erstellt haben](dns-getstarted-create-dnszone-cli.md).

In allen Beispielen auf dieser Seite werden DNS-Einträge vom Typ „A“ verwendet. Andere Eintragstypen und weitere Details zum Verwalten von DNS-Einträgen und Eintragstypen finden Sie unter [Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen über die Befehlszeilenschnittstelle](dns-operations-recordsets-cli.md).

## <a name="create-a-record-set-and-record"></a>Erstellen eines Ressourceneintragssatzes und eines Ressourceneintrags

Dieser Abschnitt zeigt, wie Sie einen Eintragssatz und Einträge erstellen. In diesem Beispiel erstellen Sie einen Eintragssatz mit dem relativen Namen „www“ in der DNS-Zone „contoso.com“. Der vollqualifizierte Name der Einträge ist „www.contoso.com“. Der Eintragstyp ist „A“, und die Gültigkeitsdauer beträgt 60 Sekunden. Wenn Sie diesen Schritt ausgeführt haben, haben Sie eine leere Datensatzgruppe erstellt.

Zum Erstellen eines Eintragssatzes auf oberster Ebene der Zone (in diesem Fall „contoso.com“) verwenden Sie den Namen des Eintrags „"@",“, einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Konvention.

### <a name="1-create-a-record-set"></a>1. Erstellen einer Datensatzgruppe

Wenn Ihr neuer Eintrag den gleichen Namen und Typ besitzt wie ein bereits vorhandener Eintrag, müssen Sie ihn dem vorhandenen Ressourceneintragssatz hinzufügen. Sie können diesen Schritt überspringen und weiter unten mit [Hinzufügen von Datensätzen](#add-records) fortfahren. Besitzt Ihr neuer Eintrag einen anderen Namen und Typ als alle bereits vorhandenen Datensätze, müssen Sie einen neuen Ressourceneintragssatz erstellen.

Ressourceneintragssätze werden mithilfe des Befehls `azure network dns record-set create` erstellt. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set create -h`.  

Beim Erstellen eines Ressourceneintragssatzes müssen Sie den Namen des Ressourceneintragssatzes, die Zone, die Gültigkeitsdauer (TTL) und den Ressourceneintragstyp angeben. 

```azurecli
azure network dns record-set create myresourcegroup contoso.com www A 60
```

Wenn Sie diesen Schritt ausgeführt haben, verfügen Sie über eine leere „www“-Datensatzgruppe. Damit Sie den neu erstellten Ressourceneintragssatz „www“ verwenden können, müssen Sie zuerst Einträge hinzufügen.

### <a name="2-add-records"></a>2. Hinzufügen von Datensätzen

Einträge werden Eintragssätzen mithilfe von `azure network dns record-set add-record`hinzugefügt. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set add-record -h`.

Die Parameter zum Hinzufügen von Einträgen zu einer Datensatzgruppe variieren je nach Typ der Datensatzgruppe. Wenn Sie beispielsweise einen Ressourceneintragssatz des Typs „A“ verwenden, können Sie nur Einträge mit dem Parameter `-a <IPv4 address>` angeben. Informationen zu den Parametern für andere Eintragstypen erhalten Sie mithilfe von `azure network dns record-set add-record -h`.

Mit dem folgenden Befehl können Sie dem weiter oben erstellten Ressourceneintragssatz „www“ einen A-Eintrag hinzufügen:

```azurecli
azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 1.2.3.4
```

### <a name="verify-name-resolution"></a>Überprüfen der Namensauflösung

Mithilfe von DNS-Tools wie nslookup oder dig oder mit dem [PowerShell-Cmdlet Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx) können Sie testen, ob Ihre DNS-Einträge auf den Azure DNS-Namenservern vorhanden sind.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie [die DNS-Abfrage direkt an einen der Namenserver für Ihre Zone richten](dns-getstarted-create-dnszone.md#test-name-servers). Fügen Sie im folgenden Befehl die passenden Werte für Ihre Eintragszone ein.

    nslookup
    > set type=A
    > server ns1-01.azure-dns.com
    > www.contoso.com

    Server:  ns1-01.azure-dns.com
    Address:  40.90.4.1

    Name:    www.contoso.com
    Address:  1.2.3.4

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie [Ihren Domänennamen an die Azure DNS-Namenserver delegieren](dns-domain-delegation.md).

Erfahren Sie, wie Sie [DNS-Zonen über die Azure-Befehlszeilenschnittstelle verwalten](dns-operations-dnszones-cli.md).

Informieren Sie sich, wie Sie [DNS-Einträge und Ressourceneintragssätze über die Azure-Befehlszeilenschnittstelle verwalten](dns-operations-recordsets-cli.md).




<!--HONumber=Dec16_HO2-->


