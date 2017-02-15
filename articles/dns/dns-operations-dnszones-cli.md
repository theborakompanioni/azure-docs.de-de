---
title: Verwalten von DNS-Zonen mithilfe der Befehlszeilenschnittstelle | Microsoft Docs
description: "Sie können DNS-Zonen mithilfe der Azure-Befehlszeilenschnittstelle (CLI) verwalten. Aktualisieren, Löschen und Erstellen von DNS-Zonen in Azure DNS"
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
ms.date: 12/07/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: 927503bb18a63db1da2c92e034dbccb7707afab4

---

# <a name="how-to-manage-dns-zones-using-cli"></a>Verwalten von DNS-Zonen mithilfe der Befehlszeilenschnittstelle (CLI)

> [!div class="op_single_selector"]
> * [Azure-Befehlszeilenschnittstelle](dns-operations-dnszones-cli.md)
> * [PowerShell](dns-operations-dnszones.md)

In dieser Anleitung wird erläutert, wie Sie Ihre DNS-Zonenressourcen mithilfe der plattformübergreifenden Azure-Befehlszeilenschnittstelle (CLI) verwalten.

Für diese Anweisungen wird die Microsoft Azure-Befehlszeilenschnittstelle (CLI) verwendet. Zur Verwendung dieser Azure DNS-Befehle müssen Sie auf die neueste Azure-CLI aktualisieren. Sie können die Azure-Befehlszeilenschnittstelle für Windows, Linux oder Mac installieren. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).

Azure DNS ist ein nur über Azure-Ressourcen-Manager verfügbarer Dienst. Er verfügt nicht über ein „klassisches“ Bereitstellungsmodell. Sie müssen sicherstellen, dass die Azure-CLI für den Resource Manager-Modus konfiguriert ist. Verwenden Sie dazu den `azure config mode arm`-Befehl.

Falls die Meldung *Fehler: „dns“ ist kein Azure-Befehl* angezeigt wird, verwenden Sie die Azure-CLI wahrscheinlich im Azure Service Management-Modus und nicht im Resource Manager-Modus.

Alle CLI-Befehle im Zusammenhang mit Azure DNS beginnen mit `azure network dns`. Für jeden Befehl ist eine Hilfe verfügbar, die mit der Option `--help` (Kurzform: `-h`) angezeigt werden kann.  Beispiel:

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `azure network dns zone create` -Befehl erstellt. Entsprechende Hilfeinformationen finden Sie unter `azure network dns zone create -h`.

Die folgenden Beispiele zeigen, wie Sie eine DNS-Zone mit oder ohne [Azure Resource Manager-Tags](dns-zones-records.md#tags) erstellen.

### <a name="to-create-a-dns-zone"></a>So erstellen Sie eine DNS-Zone

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>So erstellen Sie eine DNS-Zone mit Tags

Das folgende Beispiel zeigt, wie Sie eine DNS-Zone mit zwei Tags (*project = demo* und *env = test*) mit dem Parameter `--tags` (Kurzform: `-t`) erstellen.

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a>Abrufen einer DNS-Zone

Verwenden Sie zum Abrufen einer DNS-Zone `azure network dns zone show`. Entsprechende Hilfeinformationen finden Sie unter `azure network dns zone show -h`.

Das folgende Beispiel gibt die DNS-Zone *contoso.com* und die zugehörigen Daten aus der Ressourcengruppe *MyResourceGroup* zurück. 

```azurecli
azure network dns zone show MyResourceGroup contoso.com

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

Dieser Befehl aktualisiert keinen der DNS-Ressourceneintragssätze in der Zone (siehe [Verwalten von DNS-Ressourceneintragssätzen](dns-operations-recordsets.md)). Es wird nur verwendet, um Eigenschaften der Zonenressource selbst zu aktualisieren. Diese Eigenschaften sind gegenwärtig auf die [Azure Resource Manager-„Tags“](dns-zones-records.md#tags) für die Zonenressource beschränkt.

Das folgende Beispiel zeigt, wie Sie die Tags einer DNS-Zone aktualisieren. Die vorhandenen Tags werden durch den angegebenen Wert ersetzt.

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a>Löschen einer DNS-Zone

DNS-Zonen können mithilfe von `azure network dns zone delete`gelöscht werden. Entsprechende Hilfeinformationen finden Sie unter `azure network dns zone delete -h`.

> [!NOTE]
> Durch das Löschen einer DNS-Zone werden auch alle DNS-Einträge in der Zone gelöscht. Dieser Vorgang kann nicht rückgängig gemacht werden. Wenn die DNS-Zone verwendet wird, tritt in Diensten, die die Zone verwenden, ein Fehler auf.
>
>Informationen dazu, wie Sie Zonen vor versehentlichem Löschen schützen, finden Sie unter [How to protect DNS zones and records](dns-protect-zones-recordsets.md) (Schützen von DNS-Zonen und -Einträgen).

Dieser Befehl fordert Sie zur Bestätigung auf. Der optionale Schalter `--quiet` (Kurzform: `-q`) unterdrückt diese Aufforderung.

Das folgende Beispiel zeigt, wie die Zone *contoso.com* aus der Ressourcengruppe *MyResourceGroup* gelöscht wird.

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Ressourceneintragssätze und Einträge in Ihrer DNS-Zone verwalten](dns-getstarted-create-recordset-cli.md).
<br>
Erfahren Sie, wie Sie [Ihre Domäne an Azure DNS delegieren](dns-domain-delegation.md).




<!--HONumber=Dec16_HO2-->


