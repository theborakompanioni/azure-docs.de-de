---
title: Erstellen einer DNS-Zone mit der Azure CLI 2.0 | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie DNS-Zonen in Azure DNS erstellen. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihrer ersten DNS-Zone mithilfe der Azure CLI 2.0."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 1514426a-133c-491a-aa27-ee0962cea9dc
ms.service: dns
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: 00a4dfbe070129ce6dc84f006d793007d55dba3f
ms.lasthandoff: 03/01/2017

---

# <a name="create-an-azure-dns-zone-using-azure-cli-20"></a>Erstellen einer Azure DNS-Zone mit der Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure CLI 1.0](dns-getstarted-create-dnszone-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-dnszone-cli.md)

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der plattformübergreifenden, für Windows, Mac und Linux verfügbaren Azure-Befehlszeilenschnittstelle eine DNS-Zone erstellen. Die DNS-Zone kann auch mithilfe von [Azure PowerShell](dns-getstarted-create-dnszone.md) oder über das [Azure-Portal](dns-getstarted-create-dnszone-portal.md) erstellt werden.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* [Azure CLI 1.0:](dns-getstarted-create-dnszone-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* [Azure CLI 2.0:](dns-getstarted-create-dnszone-cli.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="introduction"></a>Einführung

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Einrichten der Azure CLI 2.0 für Azure DNS

### <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

* Installieren Sie die neueste Version der für Windows, Mac und Linux verfügbaren Azure-Befehlszeilenschnittstelle. Weitere Informationen finden Sie unter [Install the Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2) (Installieren der Azure CLI 2.0).

### <a name="sign-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto

Öffnen Sie ein Konsolenfenster, und authentifizieren Sie sich mit Ihren Anmeldeinformationen. Weitere Informationen finden Sie unter „Anmelden bei Azure über die Azure-Befehlszeilenschnittstelle (CLI)“.

```azurecli
az login
```

### <a name="select-the-subscription"></a>Auswählen des Abonnements

Überprüfen Sie die Abonnements für das Konto.

```azurecli
az account list
```

### <a name="choose-which-of-your-azure-subscriptions-to-use"></a>Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Der Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global und nicht regional sind, hat die Auswahl des Speicherorts für die Ressourcengruppe jedoch keine Auswirkungen auf Azure DNS.

Dieser Schritt kann übersprungen werden, wenn Sie eine vorhandene Ressourcengruppe verwenden.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `az network dns zone create` -Befehl erstellt. Hilfe zu diesem Befehl erhalten Sie mit `az network dns zone create --help`.

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

```azurecli
az network dns zone create --resource-group myresourcegroup --name contoso.com
```

## <a name="verify-your-dns-zone"></a>Überprüfen der DNS-Zone

### <a name="view-records"></a>Anzeigen von Datensätzen

Beim Erstellen einer DNS-Zone werden auch die folgenden DNS-Einträge erstellt:

* Der *Start of Authority*-Eintrag (SOA). Dieser Eintrag ist im Stamm jeder DNS-Zone vorhanden.
* Die autoritativen Namenserver (NS)-Einträge. Diese Einträge zeigen, welche Namenserver die Zone hosten. Azure DNS verwendet einen Pool von Namenservern, sodass verschiedene Namenserver verschiedenen Zonen in Azure DNS zugewiesen werden können. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).

Verwenden Sie zum Anzeigen dieser Einträge `az network dns record-set list`:

```azurecli
az network dns record-set list --resource-group MyResourceGroup --zone-name contoso.com
```

Die Antwort für `az network dns record-set list` sehen Sie hier:

```json
[
  {
    "etag": "510f4711-8b7f-414e-b8d5-c0383ea3d62e",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@",
    "metadata": null,
    "name": "@",
    "nsRecords": [
      {
        "nsdname": "ns1-04.azure-dns.com."
      },
      {
        "nsdname": "ns2-04.azure-dns.net."
      },
      {
        "nsdname": "ns3-04.azure-dns.org."
      },
      {
        "nsdname": "ns4-04.azure-dns.info."
      }
    ],
    "resourceGroup": "myresourcegroup",
    "ttl": 172800,
    "type": "Microsoft.Network/dnszones/NS"
  },
  {
    "etag": "0e48e82f-f194-4d3e-a864-7e109a95c73a",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@",
    "metadata": null,
    "name": "@",
    "resourceGroup": "myresourcegroup",
    "soaRecord": {
      "email": "azuredns-hostmaster.microsoft.com",
      "expireTime": 2419200,
      "host": "ns1-04.azure-dns.com.",
      "minimumTtl": 300,
      "refreshTime": 3600,
      "retryTime": 300,
      "serialNumber": 1
    },
    "ttl": 3600,
    "type": "Microsoft.Network/dnszones/SOA"
  }
]
```

> [!NOTE]
> Ressourceneintragssätze am Stamm (oder *Apex*) einer DNS-Zone verwenden **@** als Name des Ressourceneintragssatzes.

### <a name="test-name-servers"></a>Testen der Namenserver

Mithilfe von DNS-Tools wie nslookup oder dig oder mit dem PowerShell-Cmdlet `Resolve-DnsName` können Sie testen, ob Ihre DNS-Zone auf den Azure DNS-Namenservern vorhanden ist.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie die DNS-Abfrage direkt auf einen der Namenserver für die Zone leiten. Die Namenserver für die Zone sind gemäß Angabe durch `az network dns record-set list` in den NS-Einträgen enthalten.

Im folgenden Beispiel wird die Domäne „contoso.com“ mit dig und unter Verwendung der Namenserver abgefragt, die der DNS-Zone zugewiesen sind. Ersetzen Sie die Werte durch die für Ihre Zone ordnungsgemäßen Werte.

```
  > dig @ns1-01.azure-dns.com contoso.com
  
  <<>> DiG 9.10.2-P2 <<>> @ns1-01.azure-dns.com contoso.com
(1 server found)
global options: +cmd
  Got answer:
->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
  flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
  WARNING: recursion requested but not available

  OPT PSEUDOSECTION:
  EDNS: version: 0, flags:; udp: 4000
  QUESTION SECTION:
contoso.com.                        IN      A

  AUTHORITY SECTION:
contoso.com.         3600     IN      SOA     ns1-01.azure-dns.com. azuredns-hostmaster.microsoft.com. 1 3600 300 2419200 300

Query time: 93 msec
SERVER: 208.76.47.5#53(208.76.47.5)
WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
MSG SIZE  rcvd: 120
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen einer DNS-Zone können Sie in Ihrer Zone [DNS-Ressourceneintragssätze und -Einträge erstellen](dns-getstarted-create-recordset-cli.md).


