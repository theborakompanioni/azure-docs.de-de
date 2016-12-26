---
title: Erstellen einer DNS-Zone mithilfe der Befehlszeilenschnittstelle| Microsoft Docs
description: "Erfahren Sie, wie Sie DNS-Zonen in Azure DNS erstellen. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihrer ersten DNS-Zone mithilfe der Azure-Befehlszeilenschnittstelle."
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
ms.date: 12/05/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: 5bbd490925e5e25f10044af55af49daa494ee026

---

# <a name="create-an-azure-dns-zone-using-cli"></a>Erstellen einer Azure-DNS-Zone mithilfe der Befehlszeilenschnittstelle

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-dnszone-cli.md)

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der plattformübergreifenden, für Windows, Mac und Linux verfügbaren Azure-Befehlszeilenschnittstelle eine DNS-Zone erstellen. Die DNS-Zone kann auch mithilfe von PowerShell oder über das Azure-Portal erstellt werden.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.
* Sie müssen die neueste Version der für Windows, Mac und Linux verfügbaren Azure-Befehlszeilenschnittstelle installieren. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).

## <a name="step-1---sign-in-and-create-a-resource-group"></a>Schritt 1: Anmelden und Erstellen einer Ressourcengruppe

### <a name="switch-cli-mode"></a>Wechseln des CLI-Modus

Azure DNS verwendet den Azure-Ressourcen-Manager. Achten Sie darauf, zum CLI-Modus zur Verwendung von ARM-Befehlen zu wechseln.

```azurecli
azure config mode arm
```

### <a name="sign-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto

Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert. Denken Sie daran, dass Sie nur OrgID-Konten verwenden können.

```azurecli
azure login
```

### <a name="select-the-subscription"></a>Auswählen des Abonnements

Überprüfen Sie die Abonnements für das Konto.

```azurecli
azure account list
```

Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Der Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global und nicht regional sind, hat die Auswahl des Speicherorts für die Ressourcengruppe jedoch keine Auswirkungen auf Azure DNS.

Dieser Schritt kann übersprungen werden, wenn Sie eine vorhandene Ressourcengruppe verwenden.

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a>Registrieren des Ressourcenanbieters

Der Azure DNS-Dienst wird vom Ressourcenanbieter "Microsoft.Network" verwaltet. Ihr Azure-Abonnement muss für die Verwendung dieses Ressourcenanbieters registriert werden, bevor Sie Azure DNS verwenden können. Dieser Schritt muss einmal für jedes Abonnement ausgeführt werden.

```azurecli
azure provider register --namespace Microsoft.Network
```

## <a name="step-2---create-a-dns-zone"></a>Schritt 2: Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `azure network dns zone create` -Befehl erstellt. Hilfe zu diesem Befehl erhalten Sie mit `azure network dns zone create -h`.

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

## <a name="step-3---verify"></a>Schritt 3: Überprüfen

### <a name="view-records"></a>Anzeigen von Datensätzen

Beim Erstellen einer DNS-Zone werden auch die folgenden DNS-Einträge erstellt:

* Der Start of Authority-Eintrag (SOA). Dieser ist im Stamm jeder DNS-Zone vorhanden.
* Die autoritativen Namenserver (NS)-Einträge. Diese zeigen, welche Namenserver die Zone hosten. Azure DNS verwendet einen Pool von Namenservern, sodass verschiedene Namenserver verschiedenen Zonen in Azure DNS zugewiesen werden können. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md) .

Verwenden Sie zum Anzeigen dieser Einträge `azure network dns-record-set list`:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com

info:    Executing command network dns record-set list
+ Looking up the DNS Record Sets
data:    Name                            : @
data:    Type                            : NS
data:    TTL                             : 172800
data:    Records:
data:      ns1-01.azure-dns.com.
data:      ns2-01.azure-dns.net.
data:      ns3-01.azure-dns.org.
data:      ns4-01.azure-dns.info.
data:
data:    Name                            : @
data:    Type                            : SOA
data:    TTL                             : 3600
data:    Email                           : azuredns-hostmaster.microsoft.com
data:    Host                            : ns1-01.azure-dns.com.
data:    Serial Number                   : 2
data:    Refresh Time                    : 3600
data:    Retry Time                      : 300
data:    Expire Time                     : 2419200
data:    Minimum TTL                     : 300
data:
info:    network dns record-set list command OK
```

> [!NOTE]
> Ressourceneintragssätze am Stamm (oder *Apex*) einer DNS-Zone verwenden **@** als Name des Ressourceneintragssatzes.

### <a name="test-name-servers"></a>Testen der Namenserver

Mithilfe von DNS-Tools wie nslookup oder dig oder mit dem PowerShell-Cmdlet `Resolve-DnsName` können Sie testen, ob Ihre DNS-Zone auf den Azure DNS-Namenservern vorhanden ist.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie die DNS-Abfrage direkt auf einen der Namenserver für die Zone leiten. Die Namenserver für die Zone sind in den NS-Einträgen enthalten, die von „azure network dns-record-set show“ aufgelistet werden. Ersetzen Sie im folgenden Befehl die Werte durch die für Ihre Zone ordnungsgemäßen Werte.

Im folgenden Beispiel wird die Domäne „contoso.com“ mit dig und unter Verwendung der Namenserver abgefragt, die für die DNS-Zone zugewiesen sind. Die Abfrage muss mit dig durchgeführt werden und auf einen Namenserver, für den wir *@\<Namenserver für die Zone\>* verwendet haben, sowie auf den Zonennamen verweisen.

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

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen einer DNS-Zone müssen Sie [Ressourceneintragssätze und Einträge erstellen](dns-getstarted-create-recordset-cli.md), um DNS-Einträge für Ihre Internetdomäne zu erstellen.




<!--HONumber=Dec16_HO2-->


