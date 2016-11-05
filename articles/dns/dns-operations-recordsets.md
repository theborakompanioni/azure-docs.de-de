---
title: Verwalten von DNS-Ressourceneintragssätzen und Einträgen mit dem Azure-Portal | Microsoft Docs
description: Verwalten von DNS-Datensatzgruppen und Einträgen in Azure DNS, wenn Sie Ihre Domäne in Azure DNS hosten. Alle PowerShell-Befehle für Vorgänge für Datensatzgruppen und Einträge.
services: dns
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''

ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: cherylmc

---
# Verwalten von DNS-Einträgen und DNS-Ressourceneintragssätzen mit PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](dns-operations-recordsets-portal.md)
> * [Azure-Befehlszeilenschnittstelle](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)
> 
> 

In diesem Artikel wird gezeigt, wie Sie Ressourceneintragssätze und Einträge für die DNS-Zone mit Windows PowerShell verwalten.

Es ist wichtig, den Unterschied zwischen DNS-Ressourceneintragssätzen und einzelnen DNS-Ressourceneinträgen zu verstehen. Bei einem Ressourceneintragssatz handelt es sich um die Auflistung von Einträgen in einer Zone, die den gleichen Namen und den gleichen Typ aufweisen. Weitere Informationen finden Sie unter [Erstellen von DNS-Ressourceneintragssätzen und -Einträgen mit dem Azure-Portal](dns-getstarted-create-recordset-portal.md).

Sie benötigen die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager, um Ressourceneintragssätze und Einträge zu verwalten. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Weitere Informationen zur Arbeit mit PowerShell finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).

## Erstellen eines neuen Ressourceneintragssatzes und eines Eintrags
Informationen zum Erstellen eines Ressourceneintragssatzes mithilfe von PowerShell finden Sie unter [Erstellen von DNS-Ressourceneintragssätzen und Einträgen über PowerShell](dns-getstarted-create-recordset.md).

## Abrufen einer Datensatzgruppe
Verwenden Sie `Get-AzureRmDnsRecordSet` zum Abrufen eines vorhandenen Ressourceneintragssatzes. Geben Sie den relativen Namen des Ressourceneintragssatzes, den Eintragstyp und die Zone an.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Wie bei `New-AzureRmDnsRecordSet` muss der Name des Eintrags ein relativer Name sein, also ein Name ohne Zonenname.

Sie können die Zone entweder durch den Zonennamen und den Namen der Ressourcengruppe oder durch ein Zonenobjekt angeben:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

Mit `Get-AzureRmDnsRecordSet` wird ein lokales Objekt zurückgegeben, das den in Azure DNS erstellten Ressourceneintragssatz darstellt.

## Auflisten von Datensatzgruppen
Wenn Sie die Parameter *–Name* und/oder *–RecordType* auslassen, können Sie auch `Get-AzureRmDnsRecordSet` zum Auflisten von Ressourceneintragssätzen verwenden:

### So listen Sie alle Ressourceneintragssätze auf
Dieses Beispiel gibt alle Ressourceneintragssätze unabhängig vom Namen oder Eintragstyp zurück:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### So listen Sie Ressourceneintragssätze eines bestimmten Eintragstyps auf
Dadurch werden alle Ressourceneintragssätze zurückgegeben, die dem angegebenen Eintragstyp entsprechen. In diesem Fall handelt es sich beim zurückgegebenen Ressourceneintragssatz um A-Einträge:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Die Zone kann entweder mit den Parametern *–ZoneName* und *–ResourceGroupName* (wie gezeigt) oder mit einem Zonenobjekt angegeben werden:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## Hinzufügen eines Eintrags zu einer Datensatzgruppe
Einträge werden Ressourceneintragssätzen mithilfe des Cmdlets `Add-AzureRmDnsRecordConfig` hinzugefügt. Dies ist ein Offlinevorgang. Nur das lokale Objekt, das den Ressourceneintragssatz darstellt, wird geändert.

Die Parameter zum Hinzufügen von Einträgen zu einer Datensatzgruppe variieren je nach Typ der Datensatzgruppe. Wenn Sie beispielsweise einen Ressourceneintragssatz des Typs „A“ verwenden, können Sie nur Einträge mit dem Parameter *-IPv4Address* angeben.

Weitere Einträge können jedem Ressourceneintragssatz hinzugefügt werden, indem `Add-AzureRmDnsRecordConfig` erneut aufgerufen wird. Sie können einer Datensatzgruppe bis zu 20 Einträge hinzufügen. Ressourceneintragssätze vom Typ „CNAME“ können allerdings höchstens einen Eintrag enthalten, und ein Ressourceneintragssatz darf nicht zwei identische Einträge enthalten. Leere Ressourceneintragssätze (ohne Einträge) können erstellt werden, doch werden sie nicht in den Azure DNS-Namenservern angezeigt.

Wenn der Ressourceneintragssatz die gewünschte Sammlung von Einträgen enthält, muss er mit dem Cmdlet `Set-AzureRmDnsRecordSet` übertragen werden. Nach der Übertragung eines Ressourceneintragssatzes ersetzt er den vorhandenen Ressourceneintragssatz in Azure DNS.

### So erstellen Sie einen A-Ressourceneintragssatz mit einem einzelnen Eintrag
    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Die Reihenfolge der Vorgänge zum Erstellen eines Eintrags kann auch *weitergeleitet* werden. Das bedeutet, Sie übergeben das Ressourceneintragssatz-Objekt über die Pipe anstatt als Parameter. Beispiel:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### Zusätzliche Beispiele für Datensatztypen
[!INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## Ändern von vorhandenen Ressourceneintragssätzen
Die Schritte zum Ändern eines vorhandenen Ressourceneintragssatzes ähneln den Schritten zum Erstellen von Einträgen. Die Reihenfolge der Vorgänge ist folgendermaßen:

1. Verwenden Sie `Get-AzureRmDnsRecordSet` zum Abrufen des vorhandenen Ressourceneintragssatzes.
2. Ändern Sie den Ressourceneintragssatz durch Hinzufügen von Einträgen, Entfernen von Einträgen, Ändern der Eintragsparameter oder durch Ändern der Gültigkeitsdauer des Ressourceneintragssatzes. Dies ist ein Offlinevorgang. Nur das lokale Objekt, das den Ressourceneintragssatz darstellt, wird geändert.
3. Übertragen Sie die Änderungen mit dem Cmdlet `Set-AzureRmDnsRecordSet`. Dadurch wird der vorhandene Ressourceneintragssatz in Azure DNS ersetzt.

### So aktualisieren Sie einen Eintrag in einem vorhandenen Ressourceneintragssatz
In diesem Beispiel wird die IP-Adresse eines vorhandenen A-Eintrags geändert:

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Das Cmdlet `Set-AzureRmDnsRecordSet` verwendet ETag-Überprüfungen, um sicherzustellen, dass gleichzeitige Änderungen nicht überschrieben werden. Verwenden Sie das Flag *-Overwrite*, um diese Überprüfungen zu unterdrücken. Weitere Informationen finden Sie unter [Informationen zu ETags und Tags](dns-getstarted-create-dnszone.md#tagetag).

### So ändern Sie einen SOA-Eintrag
Sie können dem automatisch erstellten SOA-Ressourceneintragssatz an der Zonenspitze (Name = „@“) weder Einträge hinzufügen noch Einträge daraus entfernen. Allerdings können Sie jeden Parameter im SOA-Eintrag (mit Ausnahme von „Host“) und die Gültigkeitsdauer des Ressourceneintragssatzes ändern.

Im folgenden Beispiel wird veranschaulicht, wie Sie die Eigenschaft *Email* des SOA-Eintrags ändern:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### So ändern Sie NS-Einträge an der Zonenspitze
Sie können dem automatisch erstellten NS-Ressourceneintragssatz an der Zonenspitze (Name = "@") keine Einträge hinzufügen, keine Einträge daraus entfernen und keine Einträge ändern. Sie können nur die Gültigkeitsdauer (TTL) des Ressourceneintragssatzes ändern.

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
Einträge können mithilfe von `Remove-AzureRmDnsRecordConfig` aus einem Ressourceneintragssatz entfernt werden. Der entfernte Eintrag muss für alle Parameter exakt mit einem vorhandenen Eintrag übereinstimmen. Änderungen müssen mit `Set-AzureRmDnsRecordSet` übertragen werden.

Durch Entfernen des letzten Eintrags aus einem Ressourceneintragssatz wird der Ressourceneintragssatz nicht gelöscht. Weitere Informationen finden Sie nachfolgend unter [Löschen eines Ressourceneintragssatzes](#delete-a-record-set).

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Die Reihenfolge der Vorgänge zum Entfernen eines Ressourceneintragssatzes kann auch weitergeleitet werden. Das bedeutet, Sie übergeben das Ressourceneintragssatz-Objekt über die Pipe anstatt als Parameter. Beispiel:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### Entfernen eines AAAA-Eintrags aus einem Ressourceneintragssatz
    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines CNAME-Eintrags aus einem Ressourceneintragssatz
Da ein CNAME-Ressourceneintragssatz höchstens einen Eintrag enthalten kann, bleibt beim Entfernen dieses Eintrags ein leerer Ressourceneintragssatz zurück.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines MX-Eintrags aus einem Ressourceneintragssatz
    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines NS-Eintrags aus einem Ressourceneintragssatz
    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines SRV-Eintrags aus einem Ressourceneintragssatz
    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### Entfernen eines TXT-Eintrags aus einem Ressourceneintragssatz
    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## Löschen einer Datensatzgruppe
Ressourceneintragssätze können mit dem Cmdlet `Remove-AzureRmDnsRecordSet` gelöscht werden. Sie können die SOA- und NS-Ressourceneintragssätze an der Zonenspitze (Name = „@“) nicht löschen, die beim Erstellen der Zone automatisch erstellt wurden. Sie werden automatisch gelöscht, wenn die Zone gelöscht wird.

Verwenden Sie eine der folgenden drei Methoden zum Entfernen eines Ressourceneintragssatzes:

### Angeben aller Parameter mit Namen
Mit dem optionalen Switch *-Force* kann die Bestätigungsaufforderung unterdrückt werden.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### Angeben des Ressourceneintragssatzes mit Namen und Typ und der Zone mittels Objekt
    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### Angeben des Ressourceneintragssatzes mittels Objekt
    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Die Angabe des Ressourceneintragssatzes mithilfe eines Objekts ermöglicht die ETag-Überprüfung, um sicherzustellen, dass gleichzeitige Änderungen nicht gelöscht werden. Das optionale Flag *-Overwrite* unterdrückt diese Überprüfungen. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag).

Das Datensatzgruppenobjekt kann auch weitergeleitet werden, anstatt als Parameter übergeben zu werden:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## Nächste Schritte
Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md). Weitere Informationen zum Automatisieren von DNS finden Sie unter [Erstellen von DNS-Zonen und -Ressourceneintragssätzen mithilfe des .NET SDK](dns-sdk.md).

Weitere Informationen zu Reverse-DNS-Einträgen finden Sie unter [Mit PowerShell Ihre Reverse-DNS-Einträge für Ihre Dienste verwalten](dns-reverse-dns-record-operations-ps.md).

<!---HONumber=AcomDC_0824_2016-->