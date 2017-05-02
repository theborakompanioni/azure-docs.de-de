---
title: Erste Schritte mit Azure DNS mithilfe der Azure CLI 1.0 | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine DNS-Zone und einen DNS-Eintrag in Azure DNS erstellen. Dies ist eine Schrittanleitung zum Erstellen und Verwalten Ihrer ersten DNS-Zone und Ihres ersten DNS-Eintrags mithilfe der Azure CLI 1.0.
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: f7943b71bbd16c36df09436973d92539eb62b210
ms.lasthandoff: 04/20/2017

---

# <a name="get-started-with-azure-dns-using-azure-cli-10"></a>Erste Schritte mit Azure DNS mithilfe der Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der plattformübergreifenden, für Windows, Mac und Linux verfügbaren Azure CLI 1.0 Ihre erste DNS-Zone und Ihren ersten DNS-Eintrag erstellen. Sie können diese Schritte auch im Azure-Portal oder mithilfe von Azure PowerShell ausführen.

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Und schließlich müssen Sie die Namenserver für die Domäne konfigurieren, um Ihre DNS-Zone im Internet zu veröffentlichen. Jeder dieser Schritte wird im Folgenden beschrieben.

Bei diesen Anweisungen wird davon ausgegangen, dass Sie die Azure CLI 1.0 bereits installiert und sich dort angemeldet haben. Hilfe erhalten Sie unter [Verwalten von DNS-Zonen mit der Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Vor dem Erstellen der DNS-Zone wird eine Ressourcengruppe für die DNS-Zone erstellt. Hier sehen Sie den Befehl:

```azurecli
azure group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `azure network dns zone create` -Befehl erstellt. Hilfe zu diesem Befehl erhalten Sie mit `azure network dns zone create -h`.

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Verwenden Sie zum Erstellen eines DNS-Eintrags den Befehl `azure network dns record-set add-record`. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set add-record -h`.

Im folgenden Beispiel wird ein Eintrag mit dem relativen Namen „www“ in der DNS-Zone „contoso.com“ in der Ressourcengruppe „MyResourceGroup“ erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes ist „www.contoso.com“. Der Eintragstyp lautet „A“, die IP-Adresse lautet 1.2.3.4, und es wird ein TTL-Standardwert von 3600 Sekunden (1 Stunde) verwendet.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Informationen zu anderen Eintragstypen, zu Eintragssätzen mit mehr als einem Eintrag, zu alternativen TTL-Werten und zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen in Azure DNS mit der Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).


## <a name="view-records"></a>Anzeigen von Datensätzen

Listen Sie die DNS-Einträge in Ihrer Zone mit diesem Befehl auf:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a>Aktualisieren der Namenserver

Wenn Sie sicher sind, dass Ihre DNS-Zone und die Einträge ordnungsgemäß eingerichtet wurden, müssen Sie den Namen Ihrer Domäne für die Verwendung der Azure-DNS-Namenserver konfigurieren. Auf diese Weise können andere Benutzer im Internet Ihre DNS-Einträge finden.

Die Namenserver für Ihre Zone werden durch den Befehl `azure network dns zone show` angegeben:

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 3
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            :
info:    network dns zone show command OK
```

Diese Namenserver müssen in der Domänennamen-Registrierungsstelle konfiguriert werden (das ist die Stelle, bei der Sie den Domänennamen erworben haben). Die Registrierungsstelle bietet eine Option an, um die Namenserver für die Domäne einzurichten. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Löschen aller Ressourcen
 
Führen Sie die folgenden Schritte aus, um alle in diesem Artikel erstellten Ressourcen zu löschen:

```azurecli
azure group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).

Weitere Informationen zum Verwalten von DNS-Zonen in Azure DNS finden Sie unter [Verwalten von DNS-Zonen in Azure DNS mit der Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).

Weitere Informationen zum Verwalten von DNS-Einträgen in Azure DNS finden Sie unter [Verwalten von DNS-Einträgen in Azure DNS mit der Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).


