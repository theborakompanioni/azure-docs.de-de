---
title: "Verwalten von DNS-Datensatzgruppen und Einträgen in Azure DNS mithilfe der Azure-Befehlszeilenschnittstelle (CLI) | Microsoft Docs"
description: "Verwalten von DNS-Datensatzgruppen und Einträgen in Azure DNS, wenn Sie Ihre Domäne in Azure DNS hosten. Alle Befehle der Befehlszeilenschnittstelle (CLI) für Vorgänge für Datensatzgruppen und Einträge."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2016
ms.author: jonatul
translationtype: Human Translation
ms.sourcegitcommit: 02d720a04fdc0fa302c2cb29b0af35ee92c14b3b
ms.openlocfilehash: 2bc18d618cf8838209bea9f8a2d323e3b1042709

---

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen über die Befehlszeilenschnittstelle

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-operations-recordsets-portal.md)
> * [Azure-Befehlszeilenschnittstelle](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

In diesem Artikel wird gezeigt, wie Sie Ressourceneintragssätze und Einträge für die DNS-Zone mit der plattformübergreifenden Azure-Befehlszeilenschnittstelle (Command Line Interface, CLI) verwalten.

Es ist wichtig, den Unterschied zwischen DNS-Ressourceneintragssätzen und einzelnen DNS-Ressourceneinträgen zu verstehen. Bei einem Ressourceneintragssatz handelt es sich um die Sammlung von Einträgen in einer Zone, die den gleichen Namen und den gleichen Typ aufweisen. Weitere Informationen finden Sie unter [Grundlegende Informationen zu Ressourceneinträgen und Ressourceneintragssätzen](dns-getstarted-create-recordset-cli.md).

## <a name="configure-the-cross-platform-azure-cli"></a>Konfigurieren der plattformübergreifenden Azure-CLI

Azure DNS ist ein nur über Azure-Ressourcen-Manager verfügbarer Dienst. Er verfügt nicht über eine Azure-Service-Verwaltungs-API. Stellen Sie mit dem Befehl `azure config mode arm` sicher, dass die Azure-CLI für den Resource Manager-Modus konfiguriert ist.

Falls die Meldung **Fehler: „dns“ ist kein Azure-Befehl**angezeigt wird, verwenden Sie die Azure-CLI wahrscheinlich im Azure-Service-Verwaltungs-Modus und nicht im Resource Manager-Modus.

## <a name="create-a-new-record-set-and-record"></a>Erstellen eines neuen Ressourceneintragssatzes und eines Eintrags

Weitere Informationen zum Erstellen eines neuen Ressourceneintragssatzes im Azure-Portal finden Sie unter [Erstellen von DNS-Einträgen und -Ressourceneintragssätzen mit dem Azure-Portal](dns-getstarted-create-recordset-cli.md).

## <a name="retrieve-a-record-set"></a>Abrufen eines Ressourceneintragssatzes

Verwenden Sie `azure network dns record-set show`zum Abrufen eines vorhandenen Ressourceneintragssatzes. Geben Sie die Ressourcengruppe, den Zonennamen, den relativen Namen des Ressourceneintragssatzes und den Eintragstyp an. Ersetzen Sie anhand des folgenden Beispiels die Werte durch Ihre eigenen.

```azurecli
azure network dns record-set show myresourcegroup contoso.com www A
```

## <a name="list-record-sets"></a>Auflisten von Datensatzgruppen

Sie können alle Eintragssätze in einer DNS-Zone mit dem Befehl `azure network dns record-set list` auflisten. Sie müssen den Namen der Ressourcengruppe und der Zone angeben.

### <a name="to-list-all-record-sets"></a>So listen Sie alle Ressourceneintragssätze auf

Dieses Beispiel gibt alle Ressourceneintragssätze unabhängig vom Namen oder Eintragstyp zurück:

```azurecli
azure network dns record-set list myresourcegroup contoso.com
```

### <a name="to-list-record-sets-of-a-given-type"></a>So listen Sie Ressourceneintragssätze eines bestimmten Typs auf

Dadurch werden alle Ressourceneintragssätze zurückgegeben, die dem angegebenen Eintragstyp (in diesem Fall „A“-Einträge) entsprechen:

```azurecli
azure network dns record-set list myresourcegroup contoso.com A
```

## <a name="add-a-record-to-a-record-set"></a>Hinzufügen eines Eintrags zu einer Datensatzgruppe

Einträge werden Eintragssätzen mithilfe des Befehls `azure network dns record-set add-record`hinzugefügt. Die Parameter zum Hinzufügen von Einträgen zu einem Ressourceneintragssatz variieren je nach festgelegtem Typ des Ressourceneintragssatzes. Wenn Sie beispielsweise einen Ressourceneintragssatz vom Typ „A“ verwenden, können Sie nur Einträge mit dem Parameter `-a <IPv4 address>`angeben.

Verwenden Sie den Befehl `azure network dns record-set create`zum Erstellen eines Ressourceneintragssatzes. Geben Sie die Ressourcengruppe, den Zonennamen, den relativen Namen der Datensatzgruppe, den Datensatztyp und die Gültigkeitsdauer (TTL) an. Wenn der Parameter `--ttl` nicht definiert ist, liegt der Standardwert bei vier (Sekunden).

```azurecli
azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300
```

Fügen Sie nach Erstellen des „A“-Ressourceneintragssatzes die IPv4-Adresse mithilfe des Befehls `azure network dns record-set add-record`hinzu.

```azurecli
azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1
```

Die folgenden Beispiele zeigen, wie Sie einen Eintragssatz jedes Eintragstyps erstellen. Jeder Eintragssatz enthält einen einzelnen Eintrag.

[!INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]

## <a name="update-a-record-in-a-record-set"></a>Aktualisieren eines Eintrags in einem vorhandenen Ressourceneintragssatz

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>So fügen Sie eine weitere IP-Adresse (1.2.3.4) einem vorhandenen „A“-Ressourceneintragssatz („www“) hinzu:

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Um einen vorhandenen Wert aus einem Ressourceneintragssatz zu entfernen,

verwenden Sie `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK

## <a name="remove-a-record-from-a-record-set"></a>Entfernen eines Eintrags aus einem Ressourceneintragssatz

Einträge können mithilfe von `azure network dns record-set delete-record`aus einem Ressourceneintragssatz entfernt werden. Der entfernte Eintrag muss bei allen Parametern exakt mit einem vorhandenen Eintrag übereinstimmen.

Durch Entfernen des letzten Eintrags aus einem Ressourceneintragssatz wird der Ressourceneintragssatz nicht gelöscht. Weitere Informationen finden Sie in diesem Artikel im Abschnitt [Löschen eines Ressourceneintragssatzes](#delete).

```azurecli
azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

azure network dns record-set delete myresourcegroup contoso.com www A
```

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Entfernen eines AAAA-Eintrags aus einem Ressourceneintragssatz

```azurecli
azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"
```

### <a name="remove-a-cname-record-from-a-record-set"></a>Entfernen eines CNAME-Eintrags aus einem Ressourceneintragssatz

```azurecli
azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com
```

### <a name="remove-an-mx-record-from-a-record-set"></a>Entfernen eines MX-Eintrags aus einem Ressourceneintragssatz

```azurecli
azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5
```

### <a name="remove-an-ns-record-from-record-set"></a>Entfernen eines NS-Eintrags aus einem Ressourceneintragssatz

```azurecli
azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"
```

### <a name="remove-a-ptr-record-from-a-record-set"></a>Entfernen eines PTR-Eintrags aus einem Ressourceneintragssatz

In diesem Fall ist „my-arpa-zone.com“ die ARPA-Zone, die Ihren IP-Adressbereich darstellt.  Jeder in dieser Zone festgelegte PTR-Eintrag entspricht einer IP-Adresse innerhalb dieses IP-Adressbereichs.

```azurecli
azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"
```

### <a name="remove-an-srv-record-from-a-record-set"></a>Entfernen eines SRV-Eintrags aus einem Ressourceneintragssatz

```azurecli
azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"
```

### <a name="remove-a-txt-record-from-a-record-set"></a>Entfernen eines TXT-Eintrags aus einem Ressourceneintragssatz

```azurecli
azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"
```

## <a name="a-namedeleteadelete-a-record-set"></a><a name="delete"></a>Löschen eines Ressourceneintragssatzes

Ressourceneintragssätze können mit dem Cmdlet `Remove-AzureRmDnsRecordSet` gelöscht werden. Sie können die SOA- und NS-Ressourceneintragssätze an der Zonenspitze (Name = "@") nicht löschen, die beim Erstellen der Zone automatisch erstellt wurden. Sie werden automatisch gelöscht, wenn die Zone gelöscht wird.

Im folgenden Beispiel wird der „A“-Ressourceneintragssatz mit Namen „test-a“ aus der DNS-Zone von „contoso.com“ entfernt:

```azurecli
azure network dns record-set delete myresourcegroup contoso.com  "test-a" A
```

Mit dem optionalen Switch `-q` kann diese Bestätigungsaufforderung unterdrückt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md). Weitere Informationen zum Automatisieren von DNS finden Sie unter [Erstellen von DNS-Zonen und -Datensatzgruppen mithilfe des .NET SDK](dns-sdk.md).

Wenn Sie mit Reverse-DNS-Einträgen arbeiten möchten, finden Sie weitere Informationen unter [Mit der Azure-Befehlszeilenschnittstelle Ihre Reverse-DNS-Einträge für Ihre Dienste verwalten](dns-reverse-dns-record-operations-cli.md).



<!--HONumber=Nov16_HO3-->


