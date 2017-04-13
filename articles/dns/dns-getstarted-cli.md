---
title: Erste Schritte mit Azure DNS mithilfe der Azure CLI 2.0 | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine DNS-Zone und einen DNS-Eintrag in Azure DNS erstellen. Dies ist eine Schrittanleitung zum Erstellen und Verwalten Ihrer ersten DNS-Zone und Ihres ersten DNS-Eintrags mithilfe der Azure CLI 2.0.
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 981275da75adb11e8fff16f77e31d4ff86affe1f
ms.lasthandoff: 03/11/2017

---

# <a name="get-started-with-azure-dns-using-azure-cli-20"></a>Erste Schritte mit Azure DNS mithilfe der Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der plattformübergreifenden, für Windows, Mac und Linux verfügbaren Azure CLI 2.0 Ihre erste DNS-Zone und Ihren ersten DNS-Eintrag erstellen. Sie können diese Schritte auch im Azure-Portal oder mithilfe von Azure PowerShell ausführen.

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Und schließlich müssen Sie die Namenserver für die Domäne konfigurieren, um Ihre DNS-Zone im Internet zu veröffentlichen. Jeder dieser Schritte wird im Folgenden beschrieben.

Bei diesen Anweisungen wird davon ausgegangen, dass Sie die Azure CLI 2.0 bereits installiert und sich dort angemeldet haben. Hilfe erhalten Sie unter [Verwalten von DNS-Zonen mit der Azure CLI 2.0](dns-operations-dnszones-cli.md).


## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `az network dns zone create` -Befehl erstellt. Hilfe zu diesem Befehl erhalten Sie mit `az network dns zone create -h`.

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```


## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Verwenden Sie zum Erstellen eines DNS-Eintrags den Befehl `az network dns record-set [record type] add-record`. Hilfe beispielsweise zu A-Einträgen finden Sie unter `azure network dns record-set A add-record -h`.

Im folgenden Beispiel wird ein Eintrag mit dem relativen Namen „www“ in der DNS-Zone „contoso.com“ in der Ressourcengruppe „MyResourceGroup“ erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes ist „www.contoso.com“. Der Eintragstyp lautet „A“, die IP-Adresse lautet 1.2.3.4, und es wird ein TTL-Standardwert von 3600 Sekunden (1 Stunde) verwendet.

```azurecli
az network dns record-set A add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

Informationen zu anderen Eintragstypen, zu Eintragssätzen mit mehr als einem Eintrag, zu alternativen TTL-Werten und zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen in Azure DNS mit der Azure CLI 2.0](dns-operations-recordsets-cli.md).


## <a name="view-records"></a>Anzeigen von Datensätzen

Listen Sie die DNS-Einträge in Ihrer Zone mit diesem Befehl auf:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```


## <a name="update-name-servers"></a>Aktualisieren der Namenserver

Wenn Sie sicher sind, dass Ihre DNS-Zone und die Einträge ordnungsgemäß eingerichtet wurden, müssen Sie den Namen Ihrer Domäne für die Verwendung der Azure-DNS-Namenserver konfigurieren. Auf diese Weise können andere Benutzer im Internet Ihre DNS-Einträge finden.

Die Namenserver für Ihre Zone werden durch den Befehl `az network dns zone show` angegeben. Um die Namen der Namenserver anzuzeigen, verwenden Sie eine JSON-Ausgabe, wie im folgenden Beispiel gezeigt.

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Diese Namenserver müssen in der Domänennamen-Registrierungsstelle konfiguriert werden (das ist die Stelle, bei der Sie den Domänennamen erworben haben). Die Registrierungsstelle bietet eine Option an, um die Namenserver für die Domäne einzurichten. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).

Weitere Informationen zum Verwalten von DNS-Zonen in Azure DNS finden Sie unter [Verwalten von DNS-Zonen in Azure DNS mit der Azure CLI 2.0](dns-operations-dnszones-cli.md).

Weitere Informationen zum Verwalten von DNS-Einträgen in Azure DNS finden Sie unter [Verwalten von DNS-Einträgen in Azure DNS mit der Azure CLI 2.0](dns-operations-recordsets-cli.md).

