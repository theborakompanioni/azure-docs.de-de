---
title: "Verwalten von Reverse-DNS-Einträgen in Azure DNS mithilfe von PowerShell | Microsoft-Dokumentation"
description: "Azure DNS ermöglicht die Verwaltung von Reverse-DNS- oder PTR-Einträgen für Azure-Dienste mithilfe von PowerShell in Resource Manager."
services: DNS
documentationcenter: na
author: s-malone
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: b95703c5-e94e-4009-ab37-0c3f7908783c
ms.service: DNS
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/28/2016
ms.author: smalone
translationtype: Human Translation
ms.sourcegitcommit: dd020bf625510eb90af2e1ad19c155831abd7e75
ms.openlocfilehash: 78de3b8dd2d8bd0992bfbacb1079825dca486442
ms.lasthandoff: 02/10/2017


---
# <a name="manage-reverse-dns-records-for-your-azure-services-using-powershell"></a>Verwalten von Reverse-DNS-Einträgen für Ihre Azure-Dienste mithilfe von PowerShell

[!INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]


[!INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]


[!INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]

Weitere Informationen zum klassischen Bereitstellungsmodell finden Sie unter [Mit Azure PowerShell Ihre Reverse-DNS-Einträge für Ihre Dienste verwalten](dns-reverse-dns-record-operations-classic-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Überprüfung der Reverse-DNS-Einträge

Um sicherzustellen, dass kein Dritter Reverse-DNS-Einträge erstellen kann, die Ihren Domänen zugeordnet sind, erlaubt Azure die Erstellung von Reverse-DNS-Einträgen nur, wenn Folgendes zutrifft:

* Der ReverseFqdn-Wert entspricht dem vollqualifizierten Domänennamen (FQDN) der öffentlichen IP-Adressressource, für die er angegeben wurde, oder er entspricht dem FQDN einer beliebigen öffentlichen IP-Adresse innerhalb des gleichen Abonnements (lautet also beispielsweise „contosoapp1.northus.cloudapp.azure.com.“).
* Der ReverseFqdn-Wert wird vorwärts zum Namen oder zur IP-Adresse der öffentlichen IP-Adresse aufgelöst, für die er angegeben wurde, oder zu einem FQDN einer beliebigen öffentlichen IP-Adresse bzw. zu einer IP-Adresse innerhalb des gleichen Abonnements. „ReverseFqdn“ lautet also beispielsweise „app1.contoso.com“. Dies ist ein CName-Alias für „contosoapp1.northus.cloudapp.azure.com“.

Überprüfungen werden nur durchgeführt, wenn die Reverse-DNS-Eigenschaft für eine öffentliche IP-Adresse eingerichtet oder geändert wird. Es finden keine regelmäßigen Neuüberprüfungen statt.

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>Hinzufügen von Reverse-DNS zu vorhandenen öffentlichen IP-Adressen

Sie können mit dem Cmdlet `Set-AzureRmPublicIpAddress` einer vorhandenen öffentlichen IP-Adresse Reverse-DNS hinzufügen:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName "NRP-DemoRG-PS"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

Wenn Sie Reverse-DNS einer vorhandenen öffentlichen IP-Adresse hinzufügen möchten, die noch keinen DNS-Namen besitzt, müssen Sie auch einen DNS-Namen angeben. Hierzu können Sie das Cmdlet „Set-AzureRmPublicIpAddress“ verwenden:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName "NRP-DemoRG-PS"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Erstellen einer öffentlichen IP-Adresse mit Reverse-DNS

Sie können eine neue öffentliche IP-Adresse mit der angegebenen Reverse-DNS-Eigenschaft hinzufügen, indem Sie das Cmdlet `New-AzureRmPublicIpAddress` ausführen:

```powershell
New-AzureRmPublicIpAddress -Name "PublicIP2" -ResourceGroupName "NRP-DemoRG-PS" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Anzeigen von Reverse-DNS für vorhandene öffentliche IP-Adressen

Mithilfe des Cmdlets `Get-AzureRmPublicIpAddress` können Sie den konfigurierten Wert für eine vorhandene öffentliche IP-Adresse anzeigen:

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIP2" -ResourceGroupName "NRP-DemoRG-PS"
```

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Entfernen Sie eine Reverse-DNS aus vorhandenen öffentlichen IP-Adressen.

Mithilfe des Cmdlets `Set-AzureRmPublicIpAddress` können Sie eine Reverse-DNS-Eigenschaft aus einer öffentlichen IP-Adresse entfernen. Dies erfolgt, indem der ReverseFqdn-Eigenschaftswert auf „leer“ gesetzt wird:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName "NRP-DemoRG-PS"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

[!INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[!INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]


