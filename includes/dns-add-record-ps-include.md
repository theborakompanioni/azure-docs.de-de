### Erstellen einer AAAA-Datensatzgruppe mit einem einzelnen Datensatz

	$rs = New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Erstellen einer CNAME-Datensatzgruppe mit einem einzelnen Datensatz

	$rs = New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Erstellen einer MX-Datensatzgruppe mit einem einzelnen Datensatz

In diesem Beispiel verwenden wir den Namen des Datensatzes „@“ zum Erstellen des MX-Eintrags auf oberster Ebene der Zone (in diesem Fall „contoso.com“). Dies ist bei MX-Einträgen üblich.

	$rs = New-AzureRmDnsRecordSet -Name "@" -RecordType MX -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Erstellen einer NS-Datensatzgruppe mit einem einzelnen Datensatz

	$rs = New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Erstellen eines PTR-Eintragssatzes mit einem einzelnen Eintrag
In diesem Fall ist „my-arpa-zone.com“ die ARPA-Zone, die Ihren IP-Adressbereich darstellt. Jeder PTR-Eintragssatz in dieser Zone entspricht einer IP-Adresse innerhalb dieses IP-Adressbereichs.

	$rs = New-AzureRmDnsRecordSet -Name "10" -RecordType PTR -Ttl 3600 -ZoneName my-arpa-zone.com -ResourceGroupName MyAzureResourceGroup
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Ptrdname "myservice.contoso.com"
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Erstellen einer SRV-Datensatzgruppe mit einem einzelnen Datensatz

Wenn Sie einen SRV-Eintrag im Zonenstamm erstellen, geben Sie im Eintragsnamen einfach *\_service* und *\_protocol* an. Es ist nicht erforderlich, „@“ im Datensatznamen anzugeben.

	$rs = New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Add-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
	Set-AzureRmDnsRecordSet -RecordSet $rs

### Erstellen eines TXT-Ressourceneintragssatzes mit einem einzelnen Datensatz

	$rs = New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
	Set-AzureRmDnsRecordSet -RecordSet $rs

<!---HONumber=AcomDC_0928_2016-->