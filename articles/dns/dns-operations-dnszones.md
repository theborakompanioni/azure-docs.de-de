<properties 
   pageTitle="Verwalten von DNS-Zonen mithilfe von PowerShell | Microsoft Azure" 
   description="Sie können DNS-Zonen mithilfe der Azure Powershell verwalten. Aktualisieren, Löschen und Erstellen von DNS-Zonen in Azure DNS" 
   services="dns" 
   documentationCenter="na" 
   authors="cherylmc" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/16/2016"
   ms.author="cherylmc"/>

# Verwalten von DNS-Zonen mithilfe der PowerShell

> [AZURE.SELECTOR]
- [Azure-Befehlszeilenschnittstelle](dns-operations-dnszones-cli.md)
- [PowerShell](dns-operations-dnszones.md)



In diesem Artikel wird gezeigt, wie die DNS-Zone mithilfe von PowerShell verwaltet wird. Sie müssen die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager (1.0 oder höher) installieren, um diese Schritte auszuführen. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).


## Erstellen einer neuen DNS-Zone

Informationen zum Erstellen einer DNS-Zone finden Sie unter [Erste Schritte mit Azure DNS mithilfe von PowerShell](dns-getstarted-create-dnszone.md).

## Abrufen einer DNS-Zone

Verwenden Sie zum Abrufen einer DNS-Zone das Cmdlet `Get-AzureRmDnsZone`. Dieser Vorgang gibt ein DNS-Zonenobjekt zurück, das einer vorhandenen Zone in Azure DNS entspricht. Das Objekt enthält Daten über die Zone (z.B. die Anzahl der Ressourceneintragssätze), jedoch nicht die Ressourceneintragssätze selbst.

	$zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## Auflisten von DNS-Zonen

Durch Auslassen des Zonennamens in `Get-AzureRmDnsZone` können Sie alle Zonen in einer Ressourcengruppe auflisten: Dieser Vorgang gibt ein Array von Zonenobjekten zurück.

	$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## Aktualisieren einer DNS-Zone

Änderungen an einer DNS-Zonenressource können mithilfe von `Set-AzureRmDnsZone` vorgenommen werden. Dadurch wird keine der DNS-Datensatzgruppen in der Zone aktualisiert (siehe [Verwalten von DNS-Einträgen](dns-operations-recordsets.md)). Es wird nur verwendet, um Eigenschaften der Zonenressource selbst zu aktualisieren. Dies ist derzeit auf die Azure-Ressourcen-Manager-"Tags" für die Zonenressource beschränkt. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#Etags-and-tags).

Verwenden Sie eine der folgenden zwei Möglichkeiten, um die DNS-Zone zu aktualisieren:

### Angeben der Zone mithilfe des Zonennamens und der Ressourcengruppe

	Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### Angeben der Zone mithilfe eines $zone-Objekts

Geben Sie die Zone mithilfe eines $zone-Objekts von `Get-AzureRmDnsZone` an. Wenn Sie `Set-AzureRmDnsZone` mit einem $zone-Objekt verwenden, werden ETag-Überprüfungen verwendet, um sicherzustellen, dass gleichzeitige Änderungen nicht überschrieben werden. Sie können den optionalen Switch *-Overwrite* verwenden, um diese Überprüfungen zu unterdrücken. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#Etags-and-tags).


	$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
	<..modify $zone.Tags here...>
	Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## Löschen einer DNS-Zone

DNS-Zonen können mit dem Cmdlet „Remove-AzureRmDnsZone“ gelöscht werden.
 
Vor dem Löschen einer DNS-Zone in Azure DNS müssen Sie alle Datensatzgruppen löschen, mit Ausnahme der NS- und SOA-Einträge im Zonenstamm, die beim Erstellen der Zone automatisch erstellt wurden.

Verwenden Sie eine der folgenden zwei Möglichkeiten, um die DNS-Zone zu löschen:

### Geben Sie die Zone mithilfe des Zonennamens und des Ressourcengruppennamens an.

Dieser Vorgang hat einen optionalen Switch *-Force*, der die Eingabeaufforderung zur Bestätigung des Entfernens der DNS-Zone unterdrückt.

	Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force] 

### Angeben der Zone mithilfe eines $zone-Objekts 

Geben Sie die Zone mithilfe eines $zone-Objekts von `Get-AzureRmDnsZone` an. Dieser Vorgang hat einen optionalen Switch *-Force*, der die Eingabeaufforderung zur Bestätigung des Entfernens der DNS-Zone unterdrückt. Wie bei `Set-AzureRmDnsZone` ermöglicht die Angabe der Zone mithilfe eines $zone-Objekts die ETag-Überprüfung, um sicherzustellen, dass gleichzeitige Änderungen nicht gelöscht werden. <BR> Das optionale Flag *-Overwrite* unterdrückt diese Überprüfungen. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#Etags-and-tags).

	$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
	Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



Das Zonenobjekt kann auch weitergeleitet werden, anstatt als Parameter übergeben zu werden:

	Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## Nächste Schritte

Nach dem Erstellen einer DNS-Zone müssen [Ressourceneintragssätze und Einträge](dns-getstarted-create-recordset.md) zum Auflösen von Namen für Ihre Internetdomäne erstellt werden.

<!---HONumber=AcomDC_0817_2016-->