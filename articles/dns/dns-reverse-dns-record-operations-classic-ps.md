---
title: "Verwalten von Reverse-DNS-Einträgen für Ihre (klassischen) Azure-Dienste mit PowerShell | Microsoft Docs"
description: "Mit PowerShell im klassischen Bereitstellungsmodell Reverse-DNS-Einträge oder PTR-Einträge für Azure-Dienste verwalten  "
services: DNS
documentationcenter: na
author: s-malone
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9c24d176-6bce-4277-a14c-80fe44a20a87
ms.service: DNS
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/28/2016
ms.author: smalone
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: fe5c9cfe0edd769635301fdcb40fc1f71d430165


---
# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Gewusst wie: Verwalten von Reverse-DNS-Einträgen für Ihre (klassischen) Azure-Dienste mit Azure PowerShell

[!INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]


[!INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]


> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Modell ausführen](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Überprüfung der Reverse-DNS-Einträge
Um sicherzustellen, dass kein Dritter Reverse-DNS-Einträge erstellen kann, die Ihren Domänen zugeordnet sind, erlaubt Azure die Erstellung von Reverse-DNS-Einträgen nur, wenn Folgendes zutrifft:

* Der Reverse-DNS-FQDN ist der Name des Clouddiensts, für den er angegeben wurde, oder ein beliebiger Clouddienstname innerhalb des gleichen Abonnements (beispielsweise „contosoapp1.cloudapp.net.“).
* Der Reverse-DNS-FQDN wird vorwärts zum Namen oder zur IP-Adresse des Clouddiensts aufgelöst, für den er angegeben wurde, oder zu einem beliebigen Clouddienstnamen bzw. zu einer IP-Adresse innerhalb des gleichen Abonnements. Der Reverse-DNS-Wert lautet also beispielsweise „app1.contoso.com“. Dies ist ein CName-Alias für „contosoapp1.cloudapp.net“.

Überprüfungen werden nur durchgeführt, wenn die Reverse-DNS-Eigenschaft für einen Clouddienst eingerichtet oder geändert wird. Es finden keine regelmäßigen Neuüberprüfungen statt.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Hinzufügen von Reverse-DNS zu vorhandenen Clouddiensten
Sie können einem vorhandenen Clouddienst einen Reverse-DNS-Eintrag mit dem Cmdlet „Set-AzureService“ hinzufügen:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Erstellen eines Clouddiensts mit Reverse-DNS
Sie können einen neuen Clouddienst mit der angegebenen Reverse-DNS-Eigenschaft hinzufügen, indem Sie das Cmdlet Set-AzureService verwenden:

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Anzeigen von Reverse-DNS für vorhandene Clouddienste
Sie können den konfigurierten Wert für einen vorhandenen Clouddienst mithilfe des Cmdlets „Get-AzureService“ anzeigen:

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Entfernen von Reverse-DNS aus vorhandenen Clouddiensten
Sie können eine Reverse-DNS-Eingenschaft mit dem Cmdlet Set-AzureService aus einem vorhandenen Clouddienst entfernen. Dies erfolgt, indem der Wert für die Reverse-DNS-Eigenschaft auf „leer“ gesetzt wird:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[!INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[!INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]




<!--HONumber=Dec16_HO2-->


