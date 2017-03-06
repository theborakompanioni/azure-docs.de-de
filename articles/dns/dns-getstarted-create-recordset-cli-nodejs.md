---
title: "Erstellen von DNS-Ressourceneinträgen mit Azure CLI 1.0 | Microsoft-Dokumentation"
description: "So erstellen Sie Hosteinträge für Azure-DNS Einrichten von Ressourceneintragssätzen und Ressourceneinträgen mit Azure CLI 1.0"
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
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 45599ea87b536b74cc652ef28f91a6058c7c8e4a
ms.lasthandoff: 02/27/2017

---

# <a name="create-dns-records-using-the-azure-cli-10"></a>Erstellen von DNS-Ressourceneinträgen mit Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI 1.0](dns-getstarted-create-recordset-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-recordset-cli.md)

In diesem Artikel wird das Erstellen von Ressourceneinträgen und Ressourceneintragssätzen mit Azure CLI 1.0 beschrieben.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* [Azure CLI 1.0:](dns-getstarted-create-recordset-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* [Azure CLI 2.0:](dns-getstarted-create-recordset-cli.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="introduction"></a>Einführung

Bevor Sie DNS-Einträge in Azure DNS erstellen, müssen Sie zunächst verstehen, wie DNS-Einträge von Azure DNS in DNS-Ressourceneintragssätzen organisiert werden.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Weitere Informationen zu DNS-Einträgen in Azure DNS finden Sie unter [DNS-Zonen und -Einträge](dns-zones-records.md).

## <a name="create-a-record-set-and-record"></a>Erstellen eines Ressourceneintragssatzes und eines Ressourceneintrags

In diesem Abschnitt erfahren Sie, wie Sie DNS-Einträge in Azure DNS erstellen. Bei den Beispielen wird vorausgesetzt, dass Sie bereits [Azure CLI 1.0 installiert haben, angemeldet sind und eine DNS-Zone erstellt haben](dns-getstarted-create-dnszone-cli-nodejs.md).

In allen Beispielen auf dieser Seite werden DNS-Einträge vom Typ „A“ verwendet. Andere Eintragstypen und weitere Details zum Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen finden Sie unter [Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen mit Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).

## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Verwenden Sie zum Erstellen eines DNS-Eintrags den Befehl `azure network dns record-set add-record`. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set add-record -h`.

Beim Erstellen eines Eintrags müssen Sie den Ressourcengruppennamen, den Zonennamen und den Namen des Eintragssatzes sowie den Eintragstyp und die Details zu dem zu erstellenden Eintrag angeben.

Ist der Eintragssatz noch nicht vorhanden, wird er vom Befehl für Sie erstellt. Ist der Eintragssatz bereits vorhanden, fügt dieser Befehl den von Ihnen angegebenen Eintrag zum vorhandenen Eintragssatz hinzu. 

Bei der Erstellung eines neuen Eintragssatzes wird ein Standardwert von 3600 für die Gültigkeitsdauer (Time-To-Live, TTL) verwendet. Anweisungen zur Verwendung verschiedener TTLs finden Sie unter [Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen mit Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).

Im folgenden Beispiel wird in der Ressourcengruppe *MyResourceGroup* in der Zone *contoso.com* ein A-Eintrag namens *www* erstellt. Die IP-Adresse des A-Eintrags lautet *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Zum Erstellen eines Ressourceneintragssatzes auf oberster Ebene der Zone (in diesem Fall „contoso.com“) verwenden Sie den Namen des Eintrags „@“, einschließlich der Anführungszeichen:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

Die zum Angeben der Eintragsdaten verwendeten Parameter variieren abhängig vom Eintragstyp. Beispiel: Für einen Eintrag vom Typ „A“ geben Sie die IPv4-Adresse mit dem Parameter `-a <IPv4 address>` an. Informationen zu den Parametern für andere Eintragstypen erhalten Sie mithilfe von `azure network dns record-set add-record -h`. Beispiele für die einzelnen Eintragstypen finden Sie unter [Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen mit Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).


## <a name="verify-name-resolution"></a>Überprüfen der Namensauflösung

Mithilfe von DNS-Tools wie nslookup oder dig oder mit dem [PowerShell-Cmdlet Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx) können Sie testen, ob Ihre DNS-Einträge auf den Azure DNS-Namenservern vorhanden sind.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie [die DNS-Abfrage direkt an einen der Namenserver für Ihre Zone richten](dns-getstarted-create-dnszone.md#test-name-servers). Fügen Sie im folgenden Befehl die passenden Werte für Ihre Eintragszone ein.

```
nslookup
> set type=A
> server ns1-01.azure-dns.com
> www.contoso.com

Server:  ns1-01.azure-dns.com
Address:  40.90.4.1

Name:    www.contoso.com
Address:  1.2.3.4
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie [Ihren Domänennamen an die Azure DNS-Namenserver delegieren](dns-domain-delegation.md).

Erfahren Sie, wie Sie [DNS-Zonen mit Azure CLI 1.0 verwalten](dns-operations-dnszones-cli-nodejs.md).

Informieren Sie sich, wie Sie [DNS-Ressourceneinträge und DNS-Ressourceneintragssätze mit Azure CLI 1.0 verwalten](dns-operations-recordsets-cli-nodejs.md).


