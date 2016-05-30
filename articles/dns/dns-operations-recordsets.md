<properties 
   pageTitle="Verwalten von DNS-Datensatzgruppen und Einträgen in Azure DNS | Microsoft Azure" 
   description="Verwalten von DNS-Datensatzgruppen und Einträgen in Azure DNS, wenn Sie Ihre Domäne in Azure DNS hosten. Alle PowerShell-Befehle für Vorgänge für Datensatzgruppen und Einträge." 
   services="dns" 
   documentationCenter="na" 
   authors="cherylmc" 
   manager="carmon" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="05/06/2016"
   ms.author="cherylmc"/>

# Verwalten von DNS-Einträgen und DNS-Ressourceneintragssätzen mit PowerShell



> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure-Befehlszeilenschnittstelle](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)



In diesem Artikel wird gezeigt, wie Sie Ressourceneintragssätze und Einträge für die DNS-Zone mit Azure PowerShell verwalten.

Es ist wichtig, den Unterschied zwischen DNS-Ressourceneintragssätzen und einzelnen DNS-Einträgen zu verstehen. Eine Datensatzgruppe ist die Auflistung von Einträgen in einer Zone mit dem gleichen Namen und dem gleichen Typ. Weitere Informationen finden Sie unter [Erstellen von DNS-Einträgen und -Ressourceneintragssätzen mit dem Azure-Portal](dns-getstarted-create-recordset-portal.md).

Sie müssen die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager installieren, um Ressourceneintragssätze und Einträge zu verwalten. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Weitere Informationen zur Arbeit mit PowerShell finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).



## Erstellen eines neuen Ressourceneintragssatzes und eines Eintrags

Weitere Informationen zum Erstellen eines Ressourceneintragssatzes im Azure-Portal finden Sie unter [Erstellen von DNS-Einträgen mit dem Azure-Portal](dns-getstarted-create-recordset.md).

## Abrufen einer Datensatzgruppe

Verwenden Sie `Get-AzureRmDnsRecordSet` zum Abrufen eines vorhandenen Ressourceneintragssatzes. Geben Sie den relativen Namen des Ressourceneintragssatzes, den Eintragstyp und die Zone an.

	$rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Wie bei `New-AzureRmDnsRecordSet` muss der Name des Eintrags ein relativer Name sein, also ohne Zonenname.

Die Zone kann entweder durch den Zonennamen und den Namen der Ressourcengruppe oder ein Zonenobjekt angegeben werden:

	$zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
	$rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone
	
Mit `Get-AzureRmDnsRecordSet` wird ein lokales Objekt zurückgegeben, das den in Azure DNS erstellten Ressourceneintragssatz darstellt.

## Auflisten von Datensatzgruppen

Durch Auslassen der Parameter *–Name* und/oder *–RecordType* kann `Get-AzureRmDnsRecordSet` auch zum Auflisten von Ressourceneintragssätzen verwendet werden:

### So listen Sie alle Ressourceneintragssätze auf 

Mit diesem Beispiel werden alle Ressourceneintragssätze zurückgegeben, unabhängig vom Namen oder Eintragstyp:

	$list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### So listen Sie Ressourceneintragssätze eines bestimmten Eintragstyps auf

Mit diesem Beispiel werden alle Ressourceneintragssätze zurückgegeben, die dem angegebenen Eintragstyp entsprechen. In diesem Fall sind es A-Einträge.

	$list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup 

Die Zone kann entweder mit den Parametern *–ZoneName* und *–ResourceGroupName* (wie gezeigt) oder mit einem Zonenobjekt angegeben werden.

	$zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
	$list = Get-AzureRmDnsRecordSet -Zone $zone

## Hinzufügen eines Eintrags zu einer Datensatzgruppe

Einträge werden mithilfe des Cmdlets `Add-AzureRmDnsRecordConfig` den Ressourceneintragssätzen hinzugefügt. Dies ist ein Offline-Vorgang; nur das lokale Objekt, das die Datensatzgruppe darstellt, wird geändert.

Die Parameter zum Hinzufügen von Einträgen zu einer Datensatzgruppe variieren je nach Typ der Datensatzgruppe. Wenn Sie beispielsweise einen Ressourceneintragssatz des Typs *A* verwenden, können Sie nur Einträge mit dem Parameter *-IPv4Address* angeben.

Weitere Einträge können jedem Ressourceneintragssatz hinzugefügt werden, indem `Add-AzureRmDnsRecordConfig` erneut aufgerufen wird. Sie können einer Datensatzgruppe bis zu 20 Einträge hinzufügen. Ressourceneintragssätze vom Typ *CNAME* können allerdings höchstens einen Eintrag enthalten, und ein Ressourceneintragssatz darf nicht zwei identische Einträge enthalten. Leere Datensatzgruppen (ohne Einträge) können erstellt werden, sie werden jedoch nicht im Azure DNS-Namenserver angezeigt.

Sobald der Ressourceneintragssatz die gewünschte Sammlung von Einträgen enthält, muss er mit dem Cmdlet `Set-AzureRmDnsRecordSet` übertragen werden. Nachdem ein Ressourceneintragssatz übertragen wurde, wird der vorhandene Ressourceneintragssatz in Azure DNS durch den bereitgestellten Ressourceneintragssatz ersetzt.

### So erstellen Sie einen A-Ressourceneintragssatz mit einem einzelnen Eintrag

	$rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup 
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
	Set-AzureRmDnsRecordSet -RecordSet $rs

Die Reihenfolge der Vorgänge zum Erstellen einer Datensatzgruppe kann auch "weitergeleitet" werden, wobei das Datensatzgruppenobjekt über die Pipe anstatt als Parameter übergeben wird. Zum Beispiel:

	New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### Zusätzliche Beispiele für Datensatztypen

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## Ändern von vorhandenen Ressourceneintragssätzen

Das Ändern von vorhandenen Ressourceneintragssätzen folgt einem ähnlichen Muster wie das Erstellen von Ressourceneintragssätzen. Die Reihenfolge der Vorgänge ist folgendermaßen:

1.	Verwenden Sie `Get-AzureRmDnsRecordSet` zum Abrufen des vorhandenen Ressourceneintragssatzes.

2.	Ändern Sie den Ressourceneintragssatz durch Hinzufügen von Einträgen, Entfernen von Einträgen, Ändern der Eintragsparameter oder durch Ändern der Gültigkeitsdauer des Ressourceneintragssatzes. Diese Änderungen erfolgen offline; nur das lokale Objekt, das die Datensatzgruppe darstellt, wird geändert.

3.	Übertragen Sie die Änderungen mit dem Cmdlet `Set-AzureRmDnsRecordSet`. Dadurch wird der vorhandene Ressourceneintragssatz in Azure DNS durch den bereitgestellten Ressourceneintragssatz ersetzt.


### So aktualisieren Sie einen Eintrag in einem vorhandenen Ressourceneintragssatz

In diesem Beispiel wird die IP-Adresse eines vorhandenen A-Eintrags geändert:

	$rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	$rs.Records[0].Ipv4Address = "134.170.185.46"
	Set-AzureRmDnsRecordSet -RecordSet $rs 

Das Cmdlet `Set-AzureRmDnsRecordSet` verwendet *ETag*-Überprüfungen, um sicherzustellen, dass gleichzeitige Änderungen nicht überschrieben werden. Verwenden Sie das Flag *-Overwrite*, um diese Überprüfungen zu unterdrücken. Weitere Informationen finden Sie unter [About Etags and Tags](dns-getstarted-creatednszone.md#tagetag) (Informationen zu ETags und Tags).

### So ändern Sie einen SOA-Eintrag

Sie können der automatisch erstellten SOA-Datensatzgruppe an der Zonenspitze (Name = „@“) keine Einträge hinzufügen oder daraus entfernen, Sie können jedoch die Parameter im SOA-Eintrag (außer „Host“) und die Gültigkeitsdauer der Datensatzgruppe ändern.

Im folgenden Beispiel wird veranschaulicht, wie Sie die Eigenschaft *Email* des SOA-Eintrags ändern:

	$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	$rs.Records[0].Email = "admin.contoso.com"
	Set-AzureRmDnsRecordSet -RecordSet $rs 

### So ändern Sie NS-Einträge an der Zonenspitze

Sie können dem automatisch erstellten NS-Ressourceneintragssatz an der Zonenspitze (Name = „@“) keine Einträge hinzufügen, Einträge nicht daraus entfernen und keine Einträge dieses Ressourceneintragssatzes ändern. Sie können nur die Gültigkeitsdauer (TTL) der Datensatzgruppe ändern.

Im folgenden Beispiel wird veranschaulicht, wie Sie die Eigenschaft "TTL" der NS-Datensatzgruppe ändern:

	$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	$rs.Ttl = 300
	Set-AzureRmDnsRecordSet -RecordSet $rs 

### So fügen Sie Einträge zu einem vorhandenen Ressourceneintragssatz hinzu

In diesem Beispiel werden einem vorhandenen Ressourceneintragssatz zwei zusätzliche MX-Einträge hinzugefügt:

	$rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
	Set-AzureRmDnsRecordSet -RecordSet $rs 

## Entfernen eines Eintrags aus einer vorhandenen Datensatzgruppe

Einträge können mithilfe von `Remove-AzureRmDnsRecordConfig` aus einem Ressourceneintragssatz entfernt werden. Beachten Sie, dass der entfernte Eintrag für alle Parameter exakt mit einem vorhandenen Eintrag übereinstimmen muss. Änderungen müssen mit `Set-AzureRmDnsRecordSet` übertragen werden.

Durch Entfernen des letzten Eintrags aus einer Datensatzgruppe wird die Datensatzgruppe nicht gelöscht. Weitere Informationen finden Sie nachfolgend unter [Löschen eines Ressourceneintragssatzes](#delete-a-record-set).


	$rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
	Set-AzureRmDnsRecordSet -RecordSet $rs

Die Reihenfolge der Vorgänge zum Entfernen einer Datensatzgruppe kann auch "weitergeleitet" werden, wobei das Datensatzgruppenobjekt über die Pipe anstatt als Parameter übergeben wird. Beispiel:

	Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### Entfernen eines AAAA-Eintrags aus einer Datensatzgruppe

	$rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines CNAME-Eintrags aus einer Datensatzgruppe

Da eine CNAME-Datensatzgruppe höchstens einen Eintrag enthalten kann, bleibt beim Entfernen dieses Eintrags eine leere Datensatzgruppe zurück.

	$rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup	
	Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines MX-Eintrags aus einer Datensatzgruppe

	$rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup	
	Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines NS-Eintrags aus einer Datensatzgruppe
	
	$rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines SRV-Eintrags aus einer Datensatzgruppe

	$rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines TXT-Eintrags aus einer Datensatzgruppe

	$rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
	Set-AzureRmDnsRecordSet -RecordSet $rs

## Löschen einer Datensatzgruppe

Ressourceneintragssätze können mit dem Cmdlet `Remove-AzureRmDnsRecordSet` gelöscht werden. Sie können die SOA- und NS-Ressourceneintragssätze an der Zonenspitze (Name = „@“) nicht löschen, die beim Erstellen der Zone automatisch erstellt werden. Sie werden automatisch gelöscht, wenn die Zone gelöscht wird.

Verwenden Sie eine der folgenden drei Möglichkeiten, um eine Datensatzgruppe zu löschen:

### Angeben aller Parameter mit Namen

Mit dem optionalen Switch *-Force* kann die Bestätigungsaufforderung unterdrückt werden.

	Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### Angeben des Ressourceneintragssatzes mit Namen und Typ und der Zone mittels Objekt

	$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
	Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### Geben Sie den Datensatz mittels Objekt an:

	$rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Die Angabe des Ressourceneintragssatzes mithilfe eines Objekts ermöglicht die ETag-Überprüfung, um sicherzustellen, dass gleichzeitige Änderungen nicht gelöscht werden. Das optionale Flag *-Overwrite* unterdrückt diese Überprüfungen. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag).

Das Datensatzgruppenobjekt kann auch weitergeleitet werden, anstatt als Parameter übergeben zu werden:

	Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md). Weitere Informationen zum Automatisieren von DNS finden Sie unter [Erstellen von DNS-Zonen und -Ressourceneintragssätzen mithilfe des .NET SDK](dns-sdk.md).

Wenn Sie mit Reverse-DNS-Einträgen arbeiten möchten, finden Sie weitere Informationen unter [Verwalten von Reverse-DNS-Einträgen](dns-reverse-dns-record-operations-ps.md).
 

<!---HONumber=AcomDC_0518_2016-->