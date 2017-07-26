---
title: "Reverse-DNS für Azure-Dienste | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Reverse-DNS-Lookups für in Azure gehostete Dienste konfigurieren."
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
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 63701e1ce0c1c6dcf2ce02ebce272b8280395e7f
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Konfigurieren von Reverse-DNS für in Azure gehostete Dienste

In diesem Artikel wird erläutert, wie Reverse-DNS-Lookups für in Azure gehostete Dienste konfiguriert werden.

Dienste in Azure verwenden IP-Adressen, die von Azure zugewiesen werden und sich im Besitz von Microsoft befinden. Diese Reverse-DNS-Einträge (PTR-Einträge) müssen in den entsprechenden Reverse-DNS-Lookupzonen im Besitz von Microsoft erstellt werden. In diesem Artikel wird die Vorgehensweise dazu erläutert.

Dieses Szenario sollte nicht mit der Möglichkeit verwechselt werden, [Reverse-DNS-Lookupzonen für Ihre zugewiesenen IP-Adressbereiche in Azure DNS zu hosten](dns-reverse-dns-hosting.md). In diesem Fall müssen die durch die Reverse-Lookupzone angegebenen IP-Adressbereiche Ihrer Organisation zugewiesen werden, in der Regel von Ihrem Internetdienstanbieter.

Bevor Sie diesen Artikel lesen, sollten Sie sich mit dieser [Übersicht über Reverse-DNS und die Unterstützung in Azure](dns-reverse-dns-overview.md) vertraut machen.

Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).
* Im Resource Manager-Bereitstellungsmodell werden Computeressourcen (z.B. virtuelle Computer, VM-Skalierungsgruppen oder Service Fabric-Cluster) über eine öffentliche IP-Adressressource verfügbar gemacht. Reverse-DNS-Lookups werden mithilfe der ReverseFqdn-Eigenschaft der öffentlichen IP-Adresse konfiguriert.
* Im klassischen Bereitstellungsmodell werden Computeressourcen mithilfe von Clouddiensten verfügbar gemacht. Reverse-DNS-Lookups werden mithilfe der ReverseDnsFqdn-Eigenschaft des Clouddiensts konfiguriert.

Reverse-DNS wird für Azure App Service derzeit nicht unterstützt.

## <a name="validation-of-reverse-dns-records"></a>Überprüfung der Reverse-DNS-Einträge

Ein Drittanbieter sollte nicht in der Lage sein, Reverse-DNS-Einträge für seine Azure-Dienstzuordnung zu Ihren DNS-Domänen zu erstellen. Um dies zu verhindern, lässt Azure nur die Erstellung von Reverse-DNS-Einträgen zu, bei denen der im Reverse-DNS-Eintrag angegebene Domänenname mit dem DNS-Namen oder einer öffentlichen IP-Adresse oder einem Clouddienst im selben Azure-Abonnement identisch ist oder in diesen bzw. diese aufgelöst wird.

Diese Überprüfung wird nur beim Festlegen oder Ändern des Reverse-DNS-Eintrags durchgeführt. Es finden keine regelmäßigen Neuüberprüfungen statt.

Beispiel: Angenommen, die öffentliche IP-Adressressource weist den DNS-Namen contosoapp1.northus.cloudapp.azure.com und die IP-Adresse 23.96.52.53 auf. Der ReverseFqdn für die Adresse kann folgendermaßen angegeben werden:
* Der DNS-Name für die öffentliche IP-Adresse contosoapp1.northus.cloudapp.azure.com
* Der DNS-Name für eine andere öffentliche IP-Adresse im selben Abonnement, z.B. contosoapp2.westus.cloudapp.azure.com
* Ein Vanity-DNS-Name, z.B. app1.contoso.com, sofern dieser Name *zuerst* als CNAME für contosoapp1.northus.cloudapp.azure.com oder eine andere öffentliche IP-Adresse im selben Abonnement konfiguriert wird.
* Ein Vanity-DNS-Name, z.B. app1.contoso.com, sofern dieser Name *zuerst* als A-Datensatz für die IP-Adresse 23.96.52.53 oder die IP-Adresse einer anderen öffentlichen IP-Adresse im selben Abonnement konfiguriert wird.

Die gleichen Einschränkungen gelten für Reverse-DNS für Clouddienste.


## <a name="reverse-dns-for-publicipaddress-resources"></a>Reverse-DNS für öffentliche IP-Adressressourcen

In diesem Abschnitt finden Sie ausführliche Anweisungen zum Konfigurieren von Reverse-DNS für öffentliche IP-Adressressourcen im Resource Manager-Bereitstellungsmodell mit Azure PowerShell, Azure CLI 1.0 oder Azure CLI 2.0. Das Konfigurieren von Reverse-DNS für öffentliche IP-Adressressourcen über das Azure-Portal wird derzeit nicht unterstützt.

Azure unterstützt derzeit Reverse-DNS nur für öffentliche IPv4-IP-Adressressourcen. Für IPv6 wird dies nicht unterstützt.

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a>Hinzufügen von Reverse-DNS zu vorhandenen öffentlichen IP-Adressen

#### <a name="powershell"></a>PowerShell

So fügen Sie vorhandenen öffentlichen IP-Adressen Reverse-DNS hinzu

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

Um einer vorhandenen öffentlichen IP-Adresse, die noch keinen DNS-Namen aufweist, Reverse-DNS hinzuzufügen, müssen Sie auch einen DNS-Namen angeben.

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

So fügen Sie vorhandenen öffentlichen IP-Adressen Reverse-DNS hinzu

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

Um einer vorhandenen öffentlichen IP-Adresse, die noch keinen DNS-Namen aufweist, Reverse-DNS hinzuzufügen, müssen Sie auch einen DNS-Namen angeben.

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

So fügen Sie vorhandenen öffentlichen IP-Adressen Reverse-DNS hinzu

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

Um einer vorhandenen öffentlichen IP-Adresse, die noch keinen DNS-Namen aufweist, Reverse-DNS hinzuzufügen, müssen Sie auch einen DNS-Namen angeben.

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Erstellen einer öffentlichen IP-Adresse mit Reverse-DNS

So erstellen Sie eine öffentliche neue Adresse mit bereits angegebener Reverse-DNS-Eigenschaft

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Anzeigen von Reverse-DNS für eine vorhandene öffentliche IP-Adresse

So zeigen Sie den konfigurierten Wert für eine vorhandene öffentliche IP-Adresse an

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Entfernen Sie eine Reverse-DNS aus vorhandenen öffentlichen IP-Adressen.

So entfernen Sie eine Reverse-DNS-Eigenschaft von einer öffentlichen IP-Adresse

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Konfigurieren von Reverse-DNS für Clouddienste

Dieser Abschnitt enthält ausführliche Anweisungen zum Konfigurieren von Reverse-DNS für Clouddienste im klassischen Bereitstellungsmodell mit Azure PowerShell. Das Konfigurieren von Reverse-DNS für Clouddienste über das Azure-Portal, Azure CLI 1.0 oder Azure CLI 2.0 wird nicht unterstützt.

### <a name="add-reverse-dns-to-existing-cloud-services"></a>Hinzufügen von Reverse-DNS zu vorhandenen Clouddiensten

So fügen Sie einem vorhandenen Clouddienst einen Reverse-DNS-Eintrag hinzu

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Erstellen eines Clouddiensts mit Reverse-DNS

So erstellen Sie einen neuen Clouddienst mit bereits angegebener Reverse-DNS-Eigenschaft

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Anzeigen von Reverse-DNS für vorhandene Clouddienste

So zeigen Sie die Reverse-DNS-Eigenschaft für einen vorhandenen Clouddienst an

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Entfernen von Reverse-DNS aus vorhandenen Clouddiensten

So entfernen Sie die Reverse-DNS-Eigenschaft von einem vorhandenen Clouddienst

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="how-much-do-reverse-dns-records-cost"></a>Was kosten Reverse-DNS-Einträge?

Sie sind kostenlos!  Es fallen keine zusätzlichen Kosten für Reverse-DNS-Einträge oder -Abfragen an.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Werden meine Reverse-DNS-Einträge vom Internet aufgelöst?

Ja. Sobald Sie die Reverse-DNS-Eigenschaft für Ihren Azure-Dienst festlegen, verwaltet Azure alle DNS-Delegierungen und DNS-Zonen, die erforderlich sind, um sicherzustellen, dass der Reverse-DNS-Eintrag für alle Internetbenutzer aufgelöst wird.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>Werden für meine Azure-Dienste Standard-Reverse-DNS-Einträge erstellt?

Nein. Reverse-DNS ist eine optionale Funktion. Es werden keine Standard-DNS-Einträge erstellt, wenn Sie keine konfigurieren.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Wie sieht das Format für den vollqualifizierten Domänennamen (FQDN) aus?

FQDNs werden aufsteigend angegeben und müssen einen Punkt am Ende haben (z.B. „app1.contoso.com.“).

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a>Was geschieht bei Fehlern bei der Überprüfung des angegebenen Reverse-DNS?

Wenn bei der Reverse-DNS-Überprüfung ein Fehler auftritt, tritt beim Konfigurieren des Reverse-DNS-Eintrags ein Fehler auf. Korrigieren Sie den Reverse-DNS-Wert entsprechend den Anforderungen, und versuchen Sie es erneut.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Kann ich Reverse-DNS für Azure App Service konfigurieren?

Nein. Reverse-DNS wird für Azure App Service nicht unterstützt.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Kann ich für meinen Azure-Dienst mehrere Reverse-DNS-Einträge konfigurieren?

Nein. Azure unterstützt für jeden Azure-Clouddienst und jede öffentliche IP-Adresse nur einen Reverse-DNS-Eintrag.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>Kann ich Reverse-DNS für öffentliche IPv6-IP-Adressressourcen konfigurieren?

Nein. Azure unterstützt derzeit Reverse-DNS nur für öffentliche IPv4-IP-Adressressourcen und -Clouddienste.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kann ich aus Azure Compute Services heraus E-Mails an externe Domänen senden?

Nein. [Azure Compute Services unterstützt das Senden von E-Mails an externe Domänen nicht.](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Reverse-DNS finden Sie unter [Reverse DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup) in der Wikipedia.
<br>
Erfahren Sie, wie [die Reverse-Lookupzone für Ihren vom ISP zugewiesenen IP-Adressbereich in Azure DNS gehostet wird](dns-reverse-dns-for-azure-services.md).


