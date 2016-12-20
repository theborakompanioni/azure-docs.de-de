---
title: Verwalten von DNS-Zonen mithilfe von PowerShell | Microsoft Docs
description: "Sie können DNS-Zonen mithilfe der Azure Powershell verwalten. Aktualisieren, Löschen und Erstellen von DNS-Zonen in Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 02d720a04fdc0fa302c2cb29b0af35ee92c14b3b
ms.openlocfilehash: f6403718f106b5021ea5b41e0e91a11bd92d4830

---
# <a name="how-to-manage-dns-zones-using-powershell"></a>Verwalten von DNS-Zonen mithilfe der PowerShell

> [!div class="op_single_selector"]
> * [Azure-Befehlszeilenschnittstelle](dns-operations-dnszones-cli.md)
> * [PowerShell](dns-operations-dnszones.md)

In diesem Artikel wird gezeigt, wie die DNS-Zone mithilfe von PowerShell verwaltet wird. Sie müssen die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager (1.0 oder höher) installieren, um diese Schritte auszuführen. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) .

## <a name="create-a-new-dns-zone"></a>Erstellen einer neuen DNS-Zone

Informationen zum Erstellen einer DNS-Zone finden Sie unter [Erste Schritte mit Azure DNS mithilfe von PowerShell](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Abrufen einer DNS-Zone

Verwenden Sie zum Abrufen einer DNS-Zone das Cmdlet `Get-AzureRmDnsZone`. Dieser Vorgang gibt ein DNS-Zonenobjekt zurück, das einer vorhandenen Zone in Azure DNS entspricht. Das Objekt enthält Daten über die Zone (z.B. die Anzahl der Ressourceneintragssätze), jedoch nicht die Ressourceneintragssätze selbst.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
```

## <a name="list-dns-zones"></a>Auflisten von DNS-Zonen

Durch Auslassen des Zonennamens in `Get-AzureRmDnsZone` können Sie alle Zonen in einer Ressourcengruppe auflisten: Dieser Vorgang gibt ein Array von Zonenobjekten zurück.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

## <a name="update-a-dns-zone"></a>Aktualisieren einer DNS-Zone

Änderungen an einer DNS-Zonenressource können mithilfe von `Set-AzureRmDnsZone`vorgenommen werden. Dadurch wird keine der DNS-Datensatzgruppen in der Zone aktualisiert (siehe [Verwalten von DNS-Einträgen](dns-operations-recordsets.md)). Es wird nur verwendet, um Eigenschaften der Zonenressource selbst zu aktualisieren. Dies ist derzeit auf die Azure-Ressourcen-Manager-"Tags" für die Zonenressource beschränkt. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag) .

Verwenden Sie eine der folgenden zwei Möglichkeiten, um die DNS-Zone zu aktualisieren:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Angeben der Zone mithilfe des Zonennamens und der Ressourcengruppe

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]
```

### <a name="specify-the-zone-using-a-zone-object"></a>Angeben der Zone mithilfe eines $zone-Objekts

Geben Sie die Zone mithilfe eines $zone-Objekts von `Get-AzureRmDnsZone`an. Wenn Sie `Set-AzureRmDnsZone` mit einem $zone-Objekt verwenden, werden ETag-Überprüfungen verwendet, um sicherzustellen, dass gleichzeitige Änderungen nicht überschrieben werden. Sie können den optionalen Switch *-Overwrite* verwenden, um diese Überprüfungen zu unterdrücken. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag) .

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
<..modify $zone.Tags here...>
Set-AzureRmDnsZone -Zone $zone [-Overwrite]
```

## <a name="delete-a-dns-zone"></a>Löschen einer DNS-Zone

DNS-Zonen können mit dem Cmdlet „Remove-AzureRmDnsZone“ gelöscht werden.

Vor dem Löschen einer DNS-Zone in Azure DNS müssen Sie alle Datensatzgruppen löschen, mit Ausnahme der NS- und SOA-Einträge im Zonenstamm, die beim Erstellen der Zone automatisch erstellt wurden.

Verwenden Sie eine der folgenden zwei Möglichkeiten, um die DNS-Zone zu löschen:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Geben Sie die Zone mithilfe des Zonennamens und des Ressourcengruppennamens an.

Dieser Vorgang hat den optionalen Switch `-Force`, der die Eingabeaufforderung zur Bestätigung des Entfernens der DNS-Zone unterdrückt.

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]
```

### <a name="specify-the-zone-using-a-zone-object"></a>Angeben der Zone mithilfe eines $zone-Objekts

Geben Sie die Zone mithilfe eines $zone-Objekts von `Get-AzureRmDnsZone`an. Dieser Vorgang hat den optionalen Switch `-Force`, der die Eingabeaufforderung zur Bestätigung des Entfernens der DNS-Zone unterdrückt. Wie bei `Set-AzureRmDnsZone`ermöglicht die Angabe der Zone mithilfe eines $zone-Objekts die ETag-Überprüfung, um sicherzustellen, dass gleichzeitige Änderungen nicht gelöscht werden.

Das optionale Flag `-Overwrite` unterdrückt diese Überprüfungen. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag) .

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]
```

Das Zonenobjekt kann auch weitergeleitet werden, anstatt als Parameter übergeben zu werden:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen einer DNS-Zone müssen [Ressourceneintragssätze und Einträge](dns-getstarted-create-recordset.md) zum Auflösen von Namen für Ihre Internetdomäne erstellt werden.




<!--HONumber=Nov16_HO3-->


