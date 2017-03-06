---
title: "Erstellen von DNS-Ressourceneinträgen mit Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Hosteinträge für Azure DNS erstellen und Ressourceneintragssätze und Ressourceneinträge per Azure CLI 2.0 einrichten."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: ca713647a9f795d7803c32eac7fba9b5b6ac95cf
ms.lasthandoff: 03/01/2017

---

# <a name="how-to-create-dns-records-using-the-azure-cli-20"></a>Erstellen von DNS-Ressourceneinträgen per Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI 1.0](dns-getstarted-create-recordset-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-recordset-cli.md)

Dieser Artikel beschreibt das Erstellen von Einträgen und Eintragssätzen mit der Azure-CLI.

## <a name="introduction"></a>Einführung

Bevor Sie DNS-Einträge in Azure DNS erstellen, müssen Sie zunächst verstehen, wie DNS-Einträge von Azure DNS in DNS-Ressourceneintragssätzen organisiert werden.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Weitere Informationen zu DNS-Einträgen in Azure DNS finden Sie unter [DNS-Zonen und -Einträge](dns-zones-records.md).

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* [Azure CLI 1.0:](dns-getstarted-create-recordset-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* [Azure CLI 2.0:](dns-getstarted-create-recordset-cli.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="create-a-record-set-and-record"></a>Erstellen eines Ressourceneintragssatzes und eines Ressourceneintrags

In diesem Abschnitt erfahren Sie, wie Sie DNS-Einträge in Azure DNS erstellen. Bei den Beispielen wird vorausgesetzt, dass Sie bereits [Azure CLI 2.0 installiert haben, angemeldet sind und eine DNS-Zone erstellt haben](dns-getstarted-create-dnszone-cli.md).

In allen Beispielen auf dieser Seite werden DNS-Einträge vom Typ „A“ verwendet. Andere Eintragstypen und weitere Details zum Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen finden Sie unter [Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen mit Azure CLI 2.0](dns-operations-recordsets-cli.md).

## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Verwenden Sie zum Erstellen eines DNS-Eintrags den Befehl `az network dns record-set ? add`. (Hierbei ist „?“ der Eintragstyp). Entsprechende Hilfeinformationen finden Sie unter `az network dns record-set --help`.

Beim Erstellen eines Eintrags müssen Sie den Ressourcengruppennamen, den Zonennamen und den Namen des Eintragssatzes sowie den Eintragstyp und die Details zu dem zu erstellenden Eintrag angeben.

Ist der Eintragssatz noch nicht vorhanden, wird er vom Befehl für Sie erstellt. Ist der Eintragssatz bereits vorhanden, fügt dieser Befehl den von Ihnen angegebenen Eintrag zum vorhandenen Eintragssatz hinzu. 

Bei der Erstellung eines neuen Eintragssatzes wird ein Standardwert von 3600 für die Gültigkeitsdauer (Time-To-Live, TTL) verwendet. Anweisungen zur Verwendung verschiedener TTLs finden Sie unter [Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen mit Azure CLI 2.0](dns-operations-recordsets-cli.md).

Im folgenden Beispiel wird in der Ressourcengruppe *MyResourceGroup* in der Zone *contoso.com* ein A-Eintrag namens *www* erstellt. Die IP-Adresse des A-Eintrags lautet *1.2.3.4*.

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

Zum Erstellen eines Ressourceneintragssatzes auf oberster Ebene der Zone (in diesem Fall „contoso.com“) verwenden Sie den Namen des Eintrags „@“, einschließlich der Anführungszeichen:

```azurecli
az network dns record-set a --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

Die zum Angeben der Eintragsdaten verwendeten Parameter variieren abhängig vom Eintragstyp. Beispiel: Für einen Eintrag vom Typ „A“ geben Sie die IPv4-Adresse mit dem Parameter `--ipv4-address <IPv4 address>` an. Informationen zu den Parametern für andere Eintragstypen erhalten Sie mithilfe von `az network dns record --help`. Beispiele für die einzelnen Eintragstypen finden Sie unter [Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen mit Azure CLI 2.0](dns-operations-recordsets-cli.md).


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

Erfahren Sie, wie Sie [DNS-Zonen per Azure CLI 2.0 verwalten](dns-operations-dnszones-cli.md).

Informieren Sie sich, wie Sie [DNS-Ressourceneinträge und DNS-Ressourceneintragssätze mit Azure CLI 2.0 verwalten](dns-operations-recordsets-cli.md).


