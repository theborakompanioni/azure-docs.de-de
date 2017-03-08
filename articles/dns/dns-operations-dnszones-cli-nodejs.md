---
title: "Verwalten von DNS-Zonen in Azure DNS – Azure CLI 1.0 | Microsoft-Dokumentation"
description: "Sie können DNS-Zonen mithilfe der Azure CLI 1.0 verwalten. In diesem Artikel erfahren Sie, wie Sie DNS-Zonen in Azure DNS aktualisieren, löschen und erstellen."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 9b545937f3e375dfcef815a66263a57bd5042f69
ms.lasthandoff: 02/27/2017

---

# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-10"></a>Verwalten von DNS-Zonen in Azure DNS mithilfe von Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure-Befehlszeilenschnittstelle](dns-operations-dnszones-cli.md)
> * [PowerShell](dns-operations-dnszones.md)

In diesem Leitfaden erfahren Sie, wie Sie Ihre DNS-Zonen mithilfe der plattformübergreifenden, für Windows, Mac und Linux verfügbaren Azure CLI 1.0 verwalten. Sie können Ihre DNS-Zonen auch mithilfe von [Azure PowerShell](dns-operations-dnszones.md) oder über das Azure-Portal verwalten.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* [Azure CLI 1.0:](dns-operations-dnszones-cli-nodejs.md) Unsere Befehlszeilenschnittstelle für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* [Azure CLI 2.0:](dns-operations-dnszones-cli.md) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="introduction"></a>Einführung

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a>Hilfe

Alle CLI 1.0-Befehle im Zusammenhang mit Azure DNS beginnen mit `azure network dns`. Für jeden Befehl ist eine Hilfe verfügbar, die mit der Option `--help` (Kurzform: `-h`) angezeigt werden kann.  Beispiel:

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `azure network dns zone create` -Befehl erstellt. Entsprechende Hilfeinformationen finden Sie unter `azure network dns zone create -h`.

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt:

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>So erstellen Sie eine DNS-Zone mit Tags

Das folgende Beispiel zeigt, wie Sie unter Verwendung des Parameters `--tags` (Kurzform: `-t`) eine DNS-Zone mit zwei [Azure Resource Manager-Tags](dns-zones-records.md#tags) (*project = demo* und *env = test*) erstellen:

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a>Abrufen einer DNS-Zone

Verwenden Sie zum Abrufen einer DNS-Zone `azure network dns zone show`. Entsprechende Hilfeinformationen finden Sie unter `azure network dns zone show -h`.

Das folgende Beispiel gibt die DNS-Zone *contoso.com* und die zugehörigen Daten aus der Ressourcengruppe *MyResourceGroup* zurück. 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

Das folgende Beispiel ist die Antwort.

```
info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

Beachten Sie, dass von `azure network dns zone show` keine DNS-Einträge zurückgegeben werden. Verwenden Sie zum Auflisten von DNS-Einträgen `azure network dns record-set list`.


## <a name="list-dns-zones"></a>Auflisten von DNS-Zonen

Verwenden Sie zum Aufzählen von DNS-Zonen `azure network dns zone list`. Entsprechende Hilfeinformationen finden Sie unter `azure network dns zone list -h`.

Wenn Sie die Ressourcengruppe angeben, werden nur die Zonen innerhalb der Ressourcengruppe aufgelistet:

```azurecli
azure network dns zone list MyResourceGroup
```

Wenn Sie die Ressourcengruppe weglassen, werden alle Zonen im Abonnement aufgelistet:

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a>Aktualisieren einer DNS-Zone

Änderungen an einer DNS-Zonenressource können mithilfe von `azure network dns zone set`vorgenommen werden. Entsprechende Hilfeinformationen finden Sie unter `azure network dns zone set -h`.

Dieser Befehl aktualisiert keinen der DNS-Ressourceneintragssätze in der Zone (siehe [Verwalten von DNS-Ressourceneintragssätzen](dns-operations-recordsets-cli-nodejs.md)). Es wird nur verwendet, um Eigenschaften der Zonenressource selbst zu aktualisieren. Diese Eigenschaften sind gegenwärtig auf die [Azure Resource Manager-„Tags“](dns-zones-records.md#tags) für die Zonenressource beschränkt.

Das folgende Beispiel zeigt, wie Sie die Tags einer DNS-Zone aktualisieren. Die vorhandenen Tags werden durch den angegebenen Wert ersetzt.

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a>Löschen einer DNS-Zone

DNS-Zonen können mithilfe von `azure network dns zone delete`gelöscht werden. Entsprechende Hilfeinformationen finden Sie unter `azure network dns zone delete -h`.

> [!NOTE]
> Beim Löschen einer DNS-Zone werden auch alle DNS-Einträge in der Zone gelöscht. Dieser Vorgang kann nicht rückgängig gemacht werden. Wenn die DNS-Zone verwendet wird, tritt in Diensten, die die Zone verwenden, ein Fehler auf.
>
>Informationen dazu, wie Sie Zonen vor versehentlichem Löschen schützen, finden Sie unter [How to protect DNS zones and records](dns-protect-zones-recordsets.md) (Schützen von DNS-Zonen und -Einträgen).

Dieser Befehl fordert Sie zur Bestätigung auf. Der optionale Schalter `--quiet` (Kurzform: `-q`) unterdrückt diese Aufforderung.

Das folgende Beispiel zeigt, wie die Zone *contoso.com* aus der Ressourcengruppe *MyResourceGroup* gelöscht wird.

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Ressourceneintragssätze und Einträge in Ihrer DNS-Zone verwalten](dns-getstarted-create-recordset-cli-nodejs.md).

Erfahren Sie, wie Sie [Ihre Domäne an Azure DNS delegieren](dns-domain-delegation.md).


