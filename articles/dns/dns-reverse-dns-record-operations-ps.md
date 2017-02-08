---
title: "Verwalten von Reverse-DNS-Einträgen für Ihre Azure-Dienste mit PowerShell | Microsoft Docs"
description: "Mit PowerShell im Ressourcen-Manager Reverse-DNS-Einträge oder PTR-Einträge für Azure verwalten"
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
ms.sourcegitcommit: 0f8bc125855bc5a5b67fde5b0b742c73b5da7610
ms.openlocfilehash: 6e6b3e9c727915cb52a112d38f60bf136d48be2e


---
# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-azure-powershell"></a>Gewusst wie: Verwalten von Reverse-DNS-Einträgen für Ihre Azure-Dienste mit Azure PowerShell

[!INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]


[!INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]


[!INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]

Weitere Informationen zum klassischen Bereitstellungsmodell finden Sie unter [Mit PowerShell Ihre Reverse-DNS-Einträge für Ihre Dienste verwalten](dns-reverse-dns-record-operations-classic-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Überprüfung der Reverse-DNS-Einträge
Um sicherzustellen, dass kein Dritter Reverse-DNS-Einträge erstellen kann, die Ihren Domänen zugeordnet sind, erlaubt Azure die Erstellung von Reverse-DNS-Einträgen nur, wenn Folgendes zutrifft:

* Der ReverseFqdn-Wert entspricht dem vollqualifizierte Domänennamen (FQDN) der öffentlichen IP-Adressressource, für die er angegeben wurde, oder er entspricht dem FQDN einer beliebigen öffentlichen IP-Adresse innerhalb des gleichen Abonnements (lautet also beispielsweise „contosoapp1.northus.cloudapp.azure.com.“).
* Der ReverseFqdn-Wert wird vorwärts zum Namen oder zur IP-Adresse der öffentlichen IP-Adresse aufgelöst, für die er angegeben wurde, oder zu einem FQDN einer beliebigen öffentlichen IP-Adresse bzw. zu einer IP-Adresse innerhalb des gleichen Abonnements. „ReverseFqdn“ lautet also beispielsweise „app1.contoso.com“. Dies ist ein CName-Alias für „contosoapp1.northus.cloudapp.azure.com“.

Überprüfungen werden nur durchgeführt, wenn die Reverse-DNS-Eigenschaft für eine öffentliche IP-Adresse eingerichtet oder geändert wird. Es finden keine regelmäßigen Neuüberprüfungen statt.

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>Hinzufügen von Reverse-DNS zu vorhandenen öffentlichen IP-Adressen
Sie können Reverse-DNS mit dem Cmdlet „Set-AzureRmPublicIpAddress“ einer vorhandenen öffentlichen IP-Adresse hinzufügen:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip

Wenn Sie Reverse-DNS einer vorhandenen öffentlichen IP-Adresse hinzufügen möchten, die noch keinen DNS-Namen besitzt, müssen Sie auch einen DNS-Namen angeben. Hierzu können Sie das Cmdlet „Set-AzureRmPublicIpAddress“ verwenden:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings = New-Object -TypeName Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings
    PS C:\> $pip.DnsSettings.DomainNameLabel = "contosoapp1"
    PS C:\> $pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Erstellen einer öffentlichen IP-Adresse mit Reverse-DNS
Sie können eine neue öffentliche IP-Adresse mit der angegebenen Reverse-DNS-Eigenschaft hinzufügen, indem Sie das Cmdlet „New-AzureRmPublicIpAddress“ verwenden:

    PS C:\> New-AzureRmPublicIpAddress -Name PublicIP2 -ResourceGroupName NRP-DemoRG-PS -Location WestUS -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Anzeigen von Reverse-DNS für vorhandene öffentliche IP-Adressen
Mithilfe des Cmdlets „Get-AzureRmPublicIpAddress“ können Sie den konfigurierten Wert für eine vorhandene öffentliche IP-Adresse anzeigen:

    PS C:\> Get-AzureRmPublicIpAddress -Name PublicIP2 -ResourceGroupName NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Entfernen Sie eine Reverse-DNS aus vorhandenen öffentlichen IP-Adressen.
Mithilfe des Cmdlets Set-AzureRmPublicIpAddress können Sie eine Reverse-DNS-Eigenschaft aus einer öffentlichen IP-Adresse entfernen. Dies erfolgt, indem der ReverseFqdn-Eigenschaftswert auf „leer“ gesetzt wird:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings.ReverseFqdn = ""
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip


[!INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[!INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]




<!--HONumber=Nov16_HO3-->


