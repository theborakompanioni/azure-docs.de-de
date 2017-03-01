---
title: "Erstellen eines Ressourceneintragssatzes und von Einträgen für eine DNS-Zone mithilfe von PowerShell | Microsoft Docs"
description: "So erstellen Sie Hosteinträge für Azure-DNS Einrichten von DNS-Ressourceneintragssätzen und Einträgen über PowerShell"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: a8068c5a-f248-4e97-be97-8bd0d79eeffd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 67b780d66eac4199b0a2367f575477191542cfa7
ms.lasthandoff: 02/27/2017

---

# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Erstellen von DNS-Ressourceneintragssätzen und Ressourceneinträgen mit PowerShell

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI 1.0](dns-getstarted-create-recordset-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-recordset-cli.md)

Dieser Artikel beschreibt das Erstellen von Ressourceneinträgen und Ressourceneintragssätzen mit Azure PowerShell.

## <a name="introduction"></a>Einführung

Bevor Sie DNS-Einträge in Azure DNS erstellen, müssen Sie zunächst verstehen, wie DNS-Einträge von Azure DNS in DNS-Ressourceneintragssätzen organisiert werden.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Weitere Informationen zu DNS-Einträgen in Azure DNS finden Sie unter [DNS-Zonen und -Einträge](dns-zones-records.md).

## <a name="create-a-record-set-and-record"></a>Erstellen eines Ressourceneintragssatzes und eines Ressourceneintrags

In diesem Abschnitt erfahren Sie, wie Sie DNS-Einträge in Azure DNS erstellen. Bei den Beispielen wird vorausgesetzt, dass Sie bereits [Azure PowerShell installiert haben, angemeldet sind und eine DNS-Zone erstellt haben](dns-getstarted-create-dnszone.md).

In allen Beispielen auf dieser Seite werden DNS-Einträge vom Typ „A“ verwendet. Andere Eintragstypen und weitere Details zum Verwalten von DNS-Einträgen und Eintragstypen finden Sie unter [Verwalten von DNS-Einträgen und DNS-Ressourceneintragssätzen mit PowerShell](dns-operations-recordsets.md).

Wenn Ihr neuer Eintrag den gleichen Namen und Typ besitzt wie ein bereits vorhandener Eintrag, müssen Sie ihn [dem vorhandenen Ressourceneintragssatz hinzufügen](#add-a-record-to-an-existing-record-set). Besitzt Ihr neuer Eintrag einen anderen Namen und Typ als alle bereits vorhandenen Einträge, müssen Sie [einen neuen Ressourceneintragssatz erstellen](#create-records-in-a-new-record-set). 

### <a name="create-records-in-a-new-record-set"></a>Erstellen von Einträgen in einem neuen Ressourceneintragssatz

Sie erstellen Ressourceneintragssätze mit dem Cmdlet `New-AzureRmDnsRecordSet`. Beim Erstellen eines Ressourceneintragssatzes müssen Sie den Namen des Ressourceneintragssatzes, die Zone, die Gültigkeitsdauer (TTL), den Eintragstyp und die zu erstellenden Einträge angeben.

Zum Erstellen eines Eintragssatzes auf oberster Ebene der Zone (in diesem Fall „contoso.com“) verwenden Sie den Namen des Eintrags „@“, einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Konvention.

Im folgenden Beispiel wird in der DNS-Zone „contoso.com“ ein neuer Ressourceneintragssatz mit dem relativen Namen „www“ erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes ist „www.contoso.com“. Der Eintragstyp ist „A“, und die Gültigkeitsdauer beträgt 3600 Sekunden. Der Ressourceneintragssatz enthält einen einzelnen Eintrag (mit der IP-Adresse&1;.2.3.4).

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

Wenn Sie einen neuen Ressourceneintragssatz mit mehreren Einträgen erstellen möchten, müssen Sie zunächst ein lokales Array mit dem hinzuzufügenden Einträgen erstellen.  Dieses wird wie folgt an `New-AzureRmDnsRecordSet` übergeben:

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

### <a name="add-a-record-to-an-existing-record-set"></a>Hinzufügen eines Eintrags zu einem vorhandenen Ressourceneintragssatz

Führen Sie die folgenden drei Schritte aus, um einem vorhandenen Ressourceneintragssatz einen Eintrag hinzuzufügen:

1. Abrufen des vorhandenen Ressourceneintragssatzes

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Hinzufügen des neuen Eintrags zum lokalen Ressourceneintragssatz. Hierbei handelt es sich um einen Offlinevorgang.

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Committen der Änderung an den Azure DNS-Dienst 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

### <a name="verify-name-resolution"></a>Überprüfen der Namensauflösung

Mithilfe von DNS-Tools wie nslookup oder dig oder mit dem [PowerShell-Cmdlet Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx) können Sie testen, ob Ihre DNS-Einträge auf den Azure DNS-Namenservern vorhanden sind.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie [die DNS-Abfrage direkt an einen der Namenserver für Ihre Zone richten](dns-getstarted-create-dnszone.md#test-name-servers). Fügen Sie im folgenden Beispiel die passenden Werte für Ihre Eintragszone ein.

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

Erfahren Sie, wie Sie [DNS-Zonen über Azure PowerShell verwalten](dns-operations-dnszones.md).

Erfahren Sie, wie Sie [DNS-Einträge und Ressourceneintragssätze über Azure PowerShell verwalten](dns-operations-recordsets.md).



