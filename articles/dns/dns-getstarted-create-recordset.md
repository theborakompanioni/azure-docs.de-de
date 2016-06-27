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



# Erstellen von DNS-Ressourceneintragssätzen und Ressourceneinträgen mit PowerShell


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-recordset-cli.md)

Dieser Artikel beschreibt das Erstellen von Ressourceneinträgen und Ressourceneintragssätzen mit Windows PowerShell. Nach dem Erstellen der DNS-Zone fügen Sie die DNS-Ressourceneinträge für Ihre Domäne hinzu. Zu diesem Zweck müssen Sie zunächst Grundlegendes zu DNS-Einträgen und Datensatzgruppen verstehen.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## Stellen Sie sicher, dass Sie über die neueste Version von PowerShell verfügen.

Stellen Sie sicher, dass Sie die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager installiert haben. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Erstellen eines Ressourceneintragssatzes und eines Ressourceneintrags

In diesem Abschnitt wird beschrieben, wie Sie einen Ressourceneintragssatz sowie einen Ressourceneintrag erstellen.


### 1\. Verbinden mit Ihrem Abonnement

Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

	Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto.

	Get-AzureRmSubscription

Geben Sie das Abonnement an, das Sie verwenden möchten.

	Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Weitere Informationen zur Arbeit mit PowerShell finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).


### 2\. Erstellen einer Datensatzgruppe

Sie erstellen Ressourceneintragssätze mit dem Cmdlet `New-AzureRmDnsRecordSet`. Beim Erstellen eines Ressourceneintragssatzes müssen Sie den Namen des Ressourceneintragssatzes, die Zone, die Gültigkeitsdauer (TTL) und den Ressourceneintragstyp angeben.

Zum Erstellen eines Ressourceneintragssatzes auf oberster Ebene der Zone (in diesem Fall „contoso.com“) verwenden Sie den Ressourceneintragsnamen „@“, einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Konvention.

Im folgenden Beispiel wird ein Ressourceneintragssatz mit dem relativen Namen „www“ in der DNS-Zone „contoso.com“ erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes ist „www.contoso.com“. Der Ressourceneintragstyp ist „A“, und die Gültigkeitsdauer beträgt 60 Sekunden. Wenn Sie diesen Schritt ausgeführt haben, verfügen Sie über einen leeren Ressourceneintragssatz „www“, der der Variablen *$rs* zugewiesen ist.

	$rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### Wenn ein Ressourceneintragssatz bereits vorhanden ist

Wenn ein Ressourceneintragssatz bereits vorhanden ist, schlägt der Befehl fehl, es sei denn, der Switch *-Overwrite* wird verwendet. Die Option *-Overwrite* löst eine Bestätigungsaufforderung aus, die mit dem Switch *-Force* unterdrückt werden kann.


	$rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


In diesem Beispiel geben Sie die Zone mithilfe des Zonennamens und des Ressourcengruppennamens an. Alternativ können Sie ein Zonenobjekt angeben, wie es von `Get-AzureRmDnsZone` oder `New-AzureRmDnsZone` zurückgegeben wird.

	$zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
	$rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

Mit `New-AzureRmDnsRecordSet` wird ein lokales Objekt zurückgegeben, das den in Azure DNS erstellten Ressourceneintragssatz darstellt.

### 3\. Hinzufügen eines Eintrags

Damit Sie den neu erstellten Ressourceneintragssatz „www“ verwenden können, müssen Sie Einträge hinzufügen. Mit dem folgenden Beispiel können Sie dem Ressourceneintragssatz „www“ IPv4-*A*-Einträge hinzufügen. In diesem Beispiel wird die Variable *$rs* verwendet, die Sie im vorherigen Schritt festgelegt haben.

Das Hinzufügen von Einträgen zu einem Ressourceneintragssatz mithilfe von `Add-AzureRmDnsRecordConfig` ist ein Offlinevorgang. Nur die lokale Variable *$rs* wird aktualisiert.


	Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
	Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### 4\. Übergeben der Änderungen

Übergeben Sie die Änderungen an die Datensatzgruppe. Verwenden Sie `Set-AzureRmDnsRecordSet`, um Änderungen am Ressourceneintragssatz in Azure DNS hochzuladen.

	Set-AzureRmDnsRecordSet -RecordSet $rs

### 5\. Abrufen des Ressourceneintragssatzes

Sie können den Ressourceneintragssatz mit `Get-AzureRmDnsRecordSet` aus Azure DNS abrufen, wie das folgende Beispiel veranschaulicht.


	Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


	Name              : www
	ZoneName          : contoso.com
	ResourceGroupName : MyAzureResourceGroup
	Ttl               : 3600
	Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
	RecordType        : A
	Records           : {134.170.185.46, 134.170.188.221}
	Tags              : {}


Sie können auch nslookup oder andere DNS-Tools verwenden, um den neuen Ressourceneintragssatz abzufragen.

Wenn Sie die Domäne noch nicht an die Azure DNS-Namenserver delegiert haben, müssen Sie Name, Server und Adresse für Ihre Zone explizit angeben.


	nslookup www.contoso.com ns1-01.azure-dns.com

	Server: ns1-01.azure-dns.com
	Address:  208.76.47.1

	Name:    www.contoso.com
	Addresses:  134.170.185.46
    	        134.170.188.221

## Erstellen eines Ressourceneintragssatzes jedes Typs mit einem einzelnen Eintrag


Die folgenden Beispiele zeigen, wie Sie einen Eintragssatz jedes Eintragstyps erstellen. Jeder Eintragssatz enthält einen einzelnen Eintrag.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## Nächste Schritte

[Verwalten von DNS-Zonen mithilfe der PowerShell](dns-operations-dnszones.md)

[Verwalten von DNS-Einträgen und DNS-Ressourceneintragssätzen mit PowerShell](dns-operations-recordsets.md)

[Automatisieren von Azure-Vorgängen mit dem .NET SDK](dns-sdk.md)

<!---HONumber=AcomDC_0615_2016-->