<properties
   pageTitle="Erstellen eines Ressourceneintragssatzes und von Einträgen für eine DNS-Zone mithilfe von PowerShell | Microsoft Azure"
   description="Erstellen von Hosteinträgen für Azure DNS. Einrichten von Datensatzgruppen und Einträgen mithilfe von PowerShell"
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
   ms.date="05/06/2016"
   ms.author="cherylmc"/>



# Erstellen von DNS-Ressourceneintragssätzen und Einträgen über PowerShell


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-recordset-cli.md)

Dieser Artikel beschreibt das Erstellen von Einträgen und Ressourceneintragssätzen mithilfe von PowerShell. Nach dem Erstellen der DNS-Zone müssen Sie die DNS-Einträge für Ihre Domäne hinzufügen. Zu diesem Zweck benötigen Sie zunächst grundlegende Informationen zu DNS-Datensätzen und Datensatzgruppen.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## Voraussetzungen

Stellen Sie sicher, dass Sie die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager installiert haben. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Erstellen einer Datensatzgruppe und eines Datensatzes

Dieser Abschnitt zeigt, wie Sie eine Datensatzgruppe sowie Datensätze erstellen.


### 1\. Verbinden mit Ihrem Abonnement 

Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

	Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto.

	Get-AzureRmSubscription 

Geben Sie das Abonnement an, das Sie verwenden möchten.

	Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Weitere Informationen zur Arbeit mit PowerShell finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).


### 2\. Erstellen einer Datensatzgruppe

Ressourceneintragssätze werden mit dem Cmdlet `New-AzureRmDnsRecordSet` erstellt. Beim Erstellen eines Ressourceneintragssatzes müssen Sie den Namen des Ressourceneintragssatzes, die Zone, die Gültigkeitsdauer (TTL) und den Eintragstyp angeben.

Zum Erstellen einer Datensatzgruppe auf oberster Ebene der Zone (in diesem Fall "contoso.com") verwenden Sie den Namen des Datensatzes "@", einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Konvention.

Im folgenden Beispiel wird ein Ressourceneintragssatz mit dem relativen Namen *www* in der DNS-Zone *contoso.com* erstellt. Der vollqualifizierte Name der Einträge ist *www.contoso.com*, der Eintragstyp ist *A*, und die Gültigkeitsdauer beträgt 60 Sekunden. Wenn Sie diesen Schritt ausgeführt haben, verfügen Sie über einen leeren *www*-Ressourceneintragssatz, der der Variablen *$rs* zugewiesen ist.

	$rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### Wenn ein Ressourceneintragssatz bereits vorhanden ist

Wenn ein Ressourceneintragssatz bereits vorhanden ist, schlägt der Befehl fehl, es sei denn, der Switch *-Overwrite* wird verwendet. Die Option *-Overwrite* löst eine Bestätigungsaufforderung aus, die mit dem Switch *-Force* unterdrückt werden kann.


	$rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


Im obigen Beispiel wird die Zone durch den Zonennamen und den Namen der Ressourcengruppe angegeben. Alternativ können Sie ein Zonenobjekt angeben, wie es von `Get-AzureRmDnsZone` oder `New-AzureRmDnsZone` zurückgegeben wird.

	$zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
	$rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

Mit `New-AzureRmDnsRecordSet` wird ein lokales Objekt zurückgegeben, das den in Azure DNS erstellten Ressourceneintragssatz darstellt.

### 3\. Hinzufügen eines Eintrags

Damit Sie den neu erstellten Ressourceneintragssatz *www* verwenden können, müssen Sie Einträge hinzufügen. Mit dem folgenden Beispiel können Sie dem Ressourceneintragssatz *www* IPv4-*A*-Einträge hinzufügen. In diesem Beispiel wird die Variable $rs verwendet, die Sie im vorherigen Schritt festgelegt haben.

Das Hinzufügen von Einträgen zu einem Ressourceneintragssatz mithilfe von `Add-AzureRmDnsRecordConfig` ist ein Offlinevorgang. Nur die lokale Variable *$rs* wird aktualisiert.


	Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### 4\. Übergeben der Änderungen

Übergeben Sie die Änderungen an die Datensatzgruppe. Verwenden Sie `Set-AzureRmDnsRecordSet`, um Änderungen am Ressourceneintragssatz in Azure DNS hochzuladen.

	Set-AzureRmDnsRecordSet -RecordSet $rs

### 5\. Abrufen des Ressourceneintragssatzes

Sie können den Ressourceneintragssatz mit `Get-AzureRmDnsRecordSet` aus Azure DNS abrufen, wie im folgenden Beispiel veranschaulicht.


	Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


	Name              : www
	ZoneName          : contoso.com
	ResourceGroupName : MyAzureResourceGroup
	Ttl               : 3600
	Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
	RecordType        : A
	Records           : {134.170.185.46, 134.170.188.221}
	Tags              : {}


Sie können auch nslookup oder andere DNS-Tools verwenden, um die neue Datensatzgruppe abzufragen.

Wenn Sie die Domäne noch nicht an die Azure DNS-Namenserver delegiert haben, müssen Sie die Namenserveradresse für die Zone explizit angeben.


	nslookup www.contoso.com ns1-01.azure-dns.com

	Server: ns1-01.azure-dns.com
	Address:  208.76.47.1

	Name:    www.contoso.com
	Addresses:  134.170.185.46
    	        134.170.188.221

## Zusätzliche Beispiele für Datensatztypen


Die folgenden Beispiele zeigen, wie Sie eine Datensatzgruppe jedes Datensatztyps erstellen, die jeweils eine einzelnen Datensatz enthält.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## Nächste Schritte

[Verwalten von DNS-Zonen](dns-operations-dnszones.md)

[Verwalten von DNS-Einträgen](dns-operations-recordsets.md)

[Automatisieren von Azure-Vorgängen mit dem .NET SDK](dns-sdk.md)
 

<!---HONumber=AcomDC_0518_2016-->