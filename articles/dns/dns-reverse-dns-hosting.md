---
title: Hosten von Reverse-DNS-Lookupzonen in Azure DNS | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mit Azure DNS die Reverse-DNS-Lookupzonen für Ihre IP-Adressbereiche hosten."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 3e10b25d2f9b91c96af2958fef6dc6a4fdbff301
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a>Hosten von Reverse-DNS-Lookupzonen in Azure DNS

In diesem Artikel wird erläutert, wie Sie Reverse-DNS-Lookupzonen für Ihre zugewiesenen IP-Adressbereiche in Azure DNS hosten. Die durch die Reverse-Lookupzone angegebenen IP-Adressbereiche müssen Ihrer Organisation zugewiesen werden – in der Regel von Ihrem Internetdienstanbieter.

Wenn Sie Reverse-DNS für IP-Adressen von Azure, die Ihrem Azure-Dienst zugewiesen sind, konfigurieren möchten, finden Sie weitere Informationen unter [Konfigurieren von Reverse-Lookup für die Ihrem Azure-Dienst zugeordneten IP-Adressen](dns-reverse-dns-for-azure-services.md).

Bevor Sie diesen Artikel lesen, sollten Sie sich mit dieser [Übersicht über Reverse-DNS und die Unterstützung in Azure](dns-reverse-dns-overview.md) vertraut machen.

In diesem Artikel werden die Schritte zum Erstellen Ihrer ersten Reverse-Lookup-DNS-Zone und des zugehörigen Eintrag mit dem Azure-Portal, Azure PowerShell, Azure CLI 1.0 oder Azure CLI 2.0 erläutert.

## <a name="create-a-reverse-lookup-dns-zone"></a>Erstellen einer Reverse-DNS-Lookupzone

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Klicken Sie im Hubmenü auf **Neu** > **Netzwerk** und anschließend auf **DNS-Zone**, um das Blatt **DNS-Zone erstellen** zu öffnen.

   ![DNS-Zone](./media/dns-reverse-dns-hosting/figure1.png)

1. Benennen Sie Ihre DNS-Zone auf dem Blatt **DNS-Zone erstellen**. Der Name der Zone wird für IPv4- und IPv6-Präfixe unterschiedlich definiert. Gehen Sie zum Benennen der Zone entsprechend den Anweisungen für [IPv4](#ipv4) oder [IPv6](#ipv6) vor. Klicken Sie nach dem Abschluss auf **Erstellen**, um die Zone zu erstellen.

### <a name="ipv4"></a>IPv4

Der Name einer IPv4-Reverse-Lookupzone basiert auf dem IP-Adressbereich, den sie darstellt. Er sollte das folgende Format aufweisen: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Beispiele finden Sie unter [Übersicht über Reverse-DNS und die Unterstützung in Azure](dns-reverse-dns-overview.md#ipv4).

> [!NOTE]
> Beim Erstellen von klassenlosen Reverse-DNS-Lookupzonen in Azure DNS müssen Sie im Zonennamen einen Bindestrich (`-`) anstelle eines Schrägstrichs („/“) verwenden.
>
> Beispiel: Für den IP-Adressbereich 192.0.2.128/26 müssen Sie `128-26.2.0.192.in-addr.arpa` als Zonennamen verwenden, nicht `128/26.2.0.192.in-addr.arpa`.
>
> Dies liegt daran, dass zwar beide von den DNS-Standards unterstützt werden, Azure DNS jedoch DNS-Zonennamen mit dem Schrägstrichzeichen (`/`) nicht unterstützt.

Das folgende Beispiel zeigt, wie Sie eine Reverse-DNS-Zone der „Klasse C“ mit dem Namen `2.0.192.in-addr.arpa` in Azure DNS über das Azure-Portal erstellen:

 ![Erstellen einer DNS-Zone](./media/dns-reverse-dns-hosting/figure2.png)

Als „Ressourcengruppenstandort“ wird der Standort der Ressourcengruppe definiert, dies hat keine Auswirkung auf die DNS-Zone. Der Standort der DNS-Zone ist immer „global“ und wird nicht angezeigt.

Die folgenden Beispiele zeigen, wie diese Aufgabe mit Azure PowerShell und der Azure-Befehlszeilenschnittstelle durchgeführt wird:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

Der Name einer IPv6-Reverse-Lookupzone sollte folgendes Format aufweisen: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Beispiele finden Sie unter [Übersicht über Reverse-DNS und die Unterstützung in Azure](dns-reverse-dns-overview.md#ipv6).


Das folgende Beispiel zeigt, wie Sie eine IPv6-Reverse-DNS-Lookupzone mit dem Namen `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS über das Azure-Portal erstellen:

 ![Erstellen einer DNS-Zone](./media/dns-reverse-dns-hosting/figure3.png)

Als „Ressourcengruppenstandort“ wird der Standort der Ressourcengruppe definiert, dies hat keine Auswirkung auf die DNS-Zone. Der Standort der DNS-Zone ist immer „global“ und wird nicht angezeigt.

Die folgenden Beispiele zeigen, wie diese Aufgabe mit Azure PowerShell und der Azure-Befehlszeilenschnittstelle durchgeführt wird:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a>Azure CLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a>Azure CLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>Delegieren einer Reverse-DNS-Lookupzone

Nachdem Sie Ihre Reverse-DNS-Lookupzone erstellt haben, müssen Sie sicherstellen, dass die Zone aus der übergeordneten Zone delegiert werden kann. Mithilfe der DNS-Delegierung sucht der DNS-Namensauflösungsprozess die Namenserver, auf denen Ihre Reverse-DNS-Lookupzone gehostet wird. Dadurch können diese Namenserver DNS-Reverse-Abfragen für die IP-Adressen in Ihrem Adressbereich beantworten.

Für Forward-Lookupzonen wird der Vorgang der Delegierung einer DNS-Zone unter [Delegieren einer Domäne an Azure DNS](dns-delegate-domain-azure-dns.md) beschrieben. Die Delegierung für Reverse-Lookupzonen funktioniert auf dieselbe Weise. Der einzige Unterschied besteht darin, dass Sie die Namenserver mit dem Internetdienstanbieter konfigurieren müssen, der den IP-Adressbereich bereitgestellt hat, nicht mit der Registrierungsstelle Ihres Domänennamens.

## <a name="create-a-dns-ptr-record"></a>Erstellen eines DNS-PTR-Eintrags

### <a name="ipv4"></a>IPv4

Im folgenden Beispiel wird die Erstellung eines PTR-Eintrags in einer Reverse-DNS-Zone in Azure DNS ausführlich erläutert. Informationen zu anderen Eintragstypen und zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen und - Ressourceneintragssätzen im Azure-Portal](dns-operations-recordsets-portal.md).

1.    Wählen Sie oben auf dem Blatt **DNS-Zone** die Option **+ Ressourceneintragssatz** aus, um das Blatt **Ressourceneintragssatz hinzufügen** zu öffnen.

 ![DNS-Zone](./media/dns-reverse-dns-hosting/figure4.png)

1. Wählen Sie auf dem Blatt **Datensatzgruppe hinzufügen** 
1. im Menü **Typ** die Option **PTR** für den Datensatz aus.  
1. Der Name des Datensatzes für einen PTR-Eintrag muss der restliche Teil der IPv4-Adresse in umgekehrter Reihenfolge sein. In diesem Beispiel werden die ersten drei Oktette bereits als Teil des Namens der Zone (.2.0.192) eingetragen. Aus diesem Grund wird im Feld „Name“ nur das letzte Oktett angegeben. Beispielsweise könnten Sie Ihrer Datensatzgruppe für eine Ressource mit der IP-Adresse 192.0.2.15 den Namen **15** geben.  
1. Geben Sie im Feld **Domänenname** den vollqualifizierten Domänennamen (FQDN) der Ressource an, die die IP-Adresse verwendet.
1. Wählen Sie unten auf dem Blatt **OK**, um den DNS-Eintrag zu erstellen.

 ![Ressourceneintragssatz hinzufügen](./media/dns-reverse-dns-hosting/figure5.png)

Im Folgenden finden Sie Beispiele zum Durchführen dieser Aufgabe mit PowerShell und der Azure-Befehlszeilenschnittstelle:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a>Azure CLI 1.0

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a>Azure CLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

Im folgenden Beispiel werden die einzelnen Schritte zur Erstellung eines neuen PTR-Eintrags erläutert. Informationen zu anderen Eintragstypen und zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen und - Ressourceneintragssätzen im Azure-Portal](dns-operations-recordsets-portal.md).

1. Wählen Sie oben auf dem Blatt **DNS-Zone** die Option **+ Datensatzgruppe** aus, um das Blatt **Datensatzgruppe hinzufügen** zu öffnen.

  ![Blatt „DNS-Zone“](./media/dns-reverse-dns-hosting/figure6.png)

2. Wählen Sie auf dem Blatt **Datensatzgruppe hinzufügen** 
3. im Menü **Typ** die Option **PTR** für den Datensatz aus.  
4. Der Name des Datensatzes für einen PTR-Eintrag muss der restliche Teil der IPv6-Adresse in umgekehrter Reihenfolge sein. Er darf keine Nullkomprimierung enthalten. In diesem Beispiel wurden die ersten 64 Bits der IPv6-Adresse bereits als Teil des Zonennamens aufgefüllt (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa). Aus diesem Grund werden im Feld „Name“ nur die letzten 64 Bits angegeben. Die letzten 64 Bits der IP-Adresse werden in umgekehrter Reihenfolge eingegeben, wobei ein Punkt als Trennzeichen zwischen den einzelnen hexadezimalen Zahlen verwendet wird. Beispielsweise könnten Sie einer Datensatzgruppe für eine Ressource, deren IP-Adresse 2001:0db8:abdc:0000:f524:10bc:1af9:405e lautet, den Namen **e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f** geben.  
5. Geben Sie im Feld **Domänenname** den vollqualifizierten Domänennamen (FQDN) der Ressource an, die die IP-Adresse verwendet.
6. Wählen Sie unten auf dem Blatt **OK**, um den DNS-Eintrag zu erstellen.

![Blatt „Datensatzgruppe hinzufügen“](./media/dns-reverse-dns-hosting/figure7.png)

Im Folgenden finden Sie Beispiele zum Durchführen dieser Aufgabe mit PowerShell und der Azure-Befehlszeilenschnittstelle:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a>Azure CLI 1.0

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a>Azure CLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Anzeigen von Datensätzen

Um die erstellten Datensätze anzuzeigen, navigieren Sie im Azure-Portal zu Ihrer DNS-Zone. Im unteren Teil des Blatts **DNS-Zone** finden Sie die Einträge für die DNS-Zone. Sie sehen die standardmäßigen NS- und SOA-Einträge, die in jeder Zone erstellt werden, sowie alle neuen, von Ihnen erstellen Einträge.

### <a name="ipv4"></a>IPv4

Blatt „DNS-Zone“ mit IPv4-PTR-Einträgen:

![Blatt „DNS-Zone“](./media/dns-reverse-dns-hosting/figure8.png)

Die folgenden Beispiele zeigen, wie die PTR-Einträge mithilfe von PowerShell oder der Azure-Befehlszeilenschnittstelle angezeigt werden:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

Blatt „DNS-Zone“ mit IPv6-PTR-Einträgen:

![Blatt „DNS-Zone“](./media/dns-reverse-dns-hosting/figure9.png)

Die folgenden Beispiele zeigen, wie die Einträge mithilfe von PowerShell und der Azure-Befehlszeilenschnittstelle angezeigt werden:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Kann ich Reverse-DNS-Lookupzonen für meine vom ISP zugewiesenen IP-Adressblöcke in Azure DNS hosten?

Ja. Das Hosten der Reverse-Lookupzonen (ARPA) für eigene IP-Adressbereiche in Azure DNS wird vollständig unterstützt.

Erstellen Sie die Reverse-Lookupzone in Azure DNS, wie in diesem Artikel erläutert, und [delegieren Sie die Zone](dns-domain-delegation.md) dann gemeinsam mit Ihrem Internetdienstanbieter.  Sie können dann die PTR-Einträge für jedes Reverse-Lookup auf dieselbe Weise wie andere Eintragstypen verwalten.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>Wie viel kostet das Hosten meiner Reverse-DNS-Lookupzone?

Das Hosten der Reverse-DNS-Lookupzone für Ihren vom ISP zugewiesenen IP-Adressblock in Azure DNS wird mit den [Standardsätzen für Azure DNS](https://azure.microsoft.com/pricing/details/dns/) berechnet.

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kann ich Reverse-DNS-Lookupzonen für sowohl IPv4- als auch IPv6-Adressen in Azure DNS hosten?

Ja. In diesem Artikel wird erläutert, wie Reverse-DNS-Lookupzonen für sowohl IPv4 als auch IPv6 in Azure DNS erstellt werden.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Kann ich eine vorhandene Reverse-DNS-Lookupzone importieren?

Ja. Sie können mit der Azure-Befehlszeilenschnittstelle vorhandene DNS-Zonen in Azure DNS importieren. Dies funktioniert für Forward-Lookupzonen und Reverse-Lookupzonen.

Weitere Informationen finden Sie unter [Importieren und Exportieren einer DNS-Zonendatei mit der Azure-Befehlszeilenschnittstelle](dns-import-export.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Reverse-DNS finden Sie unter [Reverse DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup) in der Wikipedia.
<br>
Erfahren Sie, wie Sie [Reverse-DNS-Einträge für Ihre Azure-Dienste verwalten](dns-reverse-dns-for-azure-services.md).

