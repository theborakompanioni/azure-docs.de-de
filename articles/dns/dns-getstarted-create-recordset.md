---
title: "Erstellen eines Ressourceneintragssatzes und von Einträgen für eine DNS-Zone mithilfe von PowerShell | Microsoft Docs"
description: "Erstellen von Hosteinträgen für Azure DNS. Einrichten von Datensatzgruppen und Einträgen mithilfe von PowerShell"
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
ms.date: 08/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 02d720a04fdc0fa302c2cb29b0af35ee92c14b3b
ms.openlocfilehash: a6b72b96c241e3d459abad0d3986f2d757cf1752

---

# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Erstellen von DNS-Ressourceneintragssätzen und Ressourceneinträgen mit PowerShell

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-recordset-cli.md)

Dieser Artikel beschreibt das Erstellen von Ressourceneinträgen und Ressourceneintragssätzen mit Windows PowerShell. Nach dem Erstellen der DNS-Zone fügen Sie die DNS-Ressourceneinträge für Ihre Domäne hinzu. Zu diesem Zweck müssen Sie zunächst Grundlegendes zu DNS-Einträgen und Datensatzgruppen verstehen.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Stellen Sie sicher, dass Sie über die neueste Version von PowerShell verfügen.

Stellen Sie sicher, dass Sie die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager installiert haben. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) .

## <a name="create-a-record-set-and-record"></a>Erstellen eines Ressourceneintragssatzes und eines Ressourceneintrags

In diesem Abschnitt wird beschrieben, wie Sie einen Ressourceneintragssatz sowie einen Ressourceneintrag erstellen.

### <a name="1-connect-to-your-subscription"></a>1. Verbinden mit Ihrem Abonnement

Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

```powershell
Login-AzureRmAccount
```

Überprüfen Sie die Abonnements für das Konto.

```powershell
Get-AzureRmSubscription
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

Weitere Informationen zur Arbeit mit PowerShell finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="2-create-a-record-set"></a>2. Erstellen einer Datensatzgruppe

Sie erstellen Ressourceneintragssätze mit dem Cmdlet `New-AzureRmDnsRecordSet`. Beim Erstellen eines Ressourceneintragssatzes müssen Sie den Namen des Ressourceneintragssatzes, die Zone, die Gültigkeitsdauer (TTL) und den Ressourceneintragstyp angeben.

Zum Erstellen eines Eintragssatzes auf oberster Ebene der Zone (in diesem Fall „contoso.com“) verwenden Sie den Namen des Eintrags „"@",“, einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Konvention.

Im folgenden Beispiel wird ein Ressourceneintragssatz mit dem relativen Namen „www“ in der DNS-Zone „contoso.com“ erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes ist „www.contoso.com“. Der Ressourceneintragstyp ist „A“, und die Gültigkeitsdauer beträgt 60 Sekunden. Wenn Sie diesen Schritt ausgeführt haben, verfügen Sie über einen leeren Ressourceneintragssatz „www“, der der Variablen *$rs*zugewiesen ist.

```powershell
$rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60
```

#### <a name="if-a-record-set-already-exists"></a>Wenn ein Ressourceneintragssatz bereits vorhanden ist

Wenn ein Ressourceneintragssatz bereits vorhanden ist, schlägt der Befehl fehl, es sei denn, der Switch *-Overwrite* wird verwendet. Die Option *-Overwrite* löst eine Bestätigungsaufforderung aus, die mit dem Switch *-Force* unterdrückt werden kann.

```powershell
$rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]
```

In diesem Beispiel geben Sie die Zone mithilfe des Zonennamens und des Ressourcengruppennamens an. Alternativ können Sie ein Zonenobjekt angeben, wie es von `Get-AzureRmDnsZone` oder `New-AzureRmDnsZone` zurückgegeben wird.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
$rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -Zone $zone [-Tag $tags] [-Overwrite] [-Force]
```

Mit `New-AzureRmDnsRecordSet` wird ein lokales Objekt zurückgegeben, das den in Azure DNS erstellten Ressourceneintragssatz darstellt.

### <a name="3-add-a-record"></a>3. Hinzufügen eines Eintrags

Damit Sie den neu erstellten Eintragssatz „www“ verwenden können, müssen Sie Einträge hinzufügen. Mit dem folgenden Beispiel können Sie dem Ressourceneintragssatz „www“ IPv4- *A* -Einträge hinzufügen. In diesem Beispiel wird die Variable *$rs* verwendet, die Sie im vorherigen Schritt festgelegt haben.

Das Hinzufügen von Einträgen zu einem Ressourceneintragssatz mithilfe von `Add-AzureRmDnsRecordConfig` ist ein Offlinevorgang. Nur die lokale Variable *$rs* wird aktualisiert.

```powershell
Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221
```

### <a name="4-commit-the-changes"></a>4. Übergeben der Änderungen

Übergeben Sie die Änderungen an den Ressourceneintragssatz. Verwenden Sie `Set-AzureRmDnsRecordSet`, um Änderungen am Ressourceneintragssatz in Azure DNS hochzuladen.

```powershell
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="5-retrieve-the-record-set"></a>5. Abrufen des Ressourceneintragssatzes

Sie können den Ressourceneintragssatz mit `Get-AzureRmDnsRecordSet` aus Azure DNS abrufen, wie das folgende Beispiel veranschaulicht.

    Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Sie können auch nslookup oder andere DNS-Tools verwenden, um den neuen Ressourceneintragssatz abzufragen.

Wenn Sie die Domäne noch nicht an die Azure DNS-Namenserver delegiert haben, müssen Sie Name, Server und Adresse für Ihre Zone explizit angeben.

    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Erstellen eines Ressourceneintragssatzes jedes Typs mit einem einzelnen Eintrag

Die folgenden Beispiele zeigen, wie Sie einen Eintragssatz jedes Eintragstyps erstellen. Jeder Eintragssatz enthält einen einzelnen Eintrag.

[!INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="next-steps"></a>Nächste Schritte

[Verwalten von DNS-Zonen mithilfe der PowerShell](dns-operations-dnszones.md)

[Verwalten von DNS-Einträgen und DNS-Ressourceneintragssätzen mit PowerShell](dns-operations-recordsets.md)

[Automatisieren von Azure-Vorgängen mit dem .NET SDK](dns-sdk.md)



<!--HONumber=Nov16_HO3-->


