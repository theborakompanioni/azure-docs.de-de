---
title: Erste Schritte mit Azure DNS | Microsoft Docs
description: "Erfahren Sie, wie Sie DNS-Zonen in Azure DNS erstellen. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihrer ersten DNS-Zone über Azure PowerShell."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: d78583b7-e669-435c-819b-7605cf791b0e
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 798717eca5180e445b14351bbfc694ed206ee9dc
ms.lasthandoff: 02/27/2017

---

# <a name="create-a-dns-zone-using-powershell"></a>Erstellen einer DNS-Zone mithilfe von PowerShell

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure CLI 1.0](dns-getstarted-create-dnszone-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-dnszone-cli.md)

Dieser Artikel führt Sie Schritt für Schritt durch die Erstellung einer DNS-Zone über Azure PowerShell. Sie können eine DNS-Zone auch mithilfe der plattformübergreifenden [Azure CLI](dns-getstarted-create-dnszone-cli.md) oder über das [Azure-Portal](dns-getstarted-create-dnszone-portal.md) erstellen.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `New-AzureRmDnsZone` -Cmdlet erstellt. Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

## <a name="verify-your-dns-zone"></a>Überprüfen der DNS-Zone

### <a name="view-records"></a>Anzeigen von Datensätzen

Beim Erstellen einer DNS-Zone werden auch die folgenden DNS-Einträge erstellt:

* Der *Start of Authority*-Eintrag (SOA). Dieser Eintrag ist im Stamm jeder DNS-Zone vorhanden.
* Die autoritativen Namenserver (NS)-Einträge. Diese Einträge zeigen, welche Namenserver die Zone hosten. Azure DNS verwendet einen Pool von Namenservern, sodass verschiedene Namenserver verschiedenen Zonen in Azure DNS zugewiesen werden können. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).

Verwenden Sie zum Anzeigen dieser Einträge `Get-AzureRmDnsRecordSet`:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Name              : @
ZoneName          : contoso.com
ResourceGroupName : MyAzureResourceGroup
Ttl               : 172800
Etag              : f573237b-088c-424a-b53c-08567d87d049
RecordType        : NS
Records           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
Metadata          : 

Name              : @
ZoneName          : contoso.com
ResourceGroupName : MyAzureResourceGroup
Ttl               : 3600
Etag              : bf88a27d-0eec-4847-ad42-f0c83b9a2c32
RecordType        : SOA
Records           : {[ns1-01.azure-dns.com.,azuredns-hostmaster.microsoft.com,3600,300,2419200,300]}
Metadata          : 
```

> [!NOTE]
> Ressourceneintragssätze am Stamm (oder *Apex*) einer DNS-Zone verwenden **@** als Name des Ressourceneintragssatzes.

### <a name="test-name-servers"></a>Testen der Namenserver

Mithilfe von DNS-Tools wie nslookup oder dig oder mit dem [PowerShell-Cmdlet Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx) können Sie testen, ob Ihre DNS-Zone auf den Azure DNS-Namenservern vorhanden ist.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie die DNS-Abfrage direkt auf einen der Namenserver für die Zone leiten. Die Namenserver für die Zone sind in den NS-Einträgen enthalten, die von `Get-AzureRmDnsRecordSet` (oben) aufgelistet werden. Fügen Sie im folgenden Beispiel die passenden Werte für Ihre Zone ein:

```
nslookup
> set type=SOA
> server ns1-01.azure-dns.com
> contoso.com

Server: ns1-01.azure-dns.com
Address:  40.90.4.1

contoso.com
        primary name server = ns1-01.azure-dns.com
        responsible mail addr = azuredns-hostmaster.microsoft.com
        serial  = 1
        refresh = 3600 (1 hour)
        retry   = 300 (5 mins)
        expire  = 2419200 (28 days)
        default TTL = 300 (5 mins)
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen einer DNS-Zone müssen Sie [Ressourceneintragssätze und Einträge erstellen](dns-getstarted-create-recordset.md), um DNS-Einträge für Ihre Internetdomäne zu erstellen.

