<properties
   pageTitle="Erste Schritte mit Azure DNS | Microsoft Azure"
   description="Erfahren Sie, wie Sie DNS-Zonen für Azure DNS erstellen. Dies ist eine schrittweise Anleitung für die Erstellung der ersten DNS-Zone, um mit dem Hosten der DNS-Domäne mithilfe der PowerShell zu beginnen."
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

# Erstellen einer DNS-Zone mithilfe von Powershell

> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-dnszone-cli.md)

In diesem Artikel werden die einzelnen Schritte zum Erstellen einer DNS-Zone mithilfe von PowerShell erläutert. Die DNS-Zone kann auch mithilfe der Befehlszeilenschnittstelle oder über das Azure-Portal erstellt werden.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Informationen zu ETags und Tags

### <a name="etags"></a>ETags

Angenommen, zwei Personen oder zwei Prozesse versuchen, einen DNS-Eintrag zur gleichen Zeit zu ändern. Welcher hat Vorrang? Und weiß derjenige, dass er gerade die Änderungen einer anderen Person überschrieben hat?

Azure DNS verwendet ETags, um gleichzeitige Änderungen an derselben Ressource sicher zu handhaben. Jeder DNS-Ressourcen (Zone oder Datensatzgruppe) ist ein ETag zugeordnet. Sobald eine Ressource abgerufen wird, wird auch sein ETag abgerufen. Beim Aktualisieren einer Ressource haben Sie die Möglichkeit, die ETags wieder zurückzugeben, damit Azure DNS überprüfen kann, ob das ETag auf dem Server übereinstimmt. Da jedes Update einer Ressource dazu führt, dass das ETag erneut generiert wird, weist eine Nichtübereinstimmung des ETags darauf hin, dass eine gleichzeitige Änderung vorgenommen wurde. ETags werden auch beim Erstellen einer neuen Ressource verwendet, um sicherzustellen, dass die Ressource nicht bereits vorhanden ist.

Standardmäßig verwendet Azure DNS PowerShell ETags, um gleichzeitige Änderungen an Zonen und Datensatzgruppen zu blockieren. Der optionale Switch *-Overwrite* kann verwendet werden, um ETag-Überprüfungen zu unterdrücken. In diesem Fall werden gleichzeitig vorgenommene Änderungen überschrieben.

Auf der Ebene der REST-API von Azure DNS werden ETags mithilfe von HTTP-Headern angegeben. Ihr Verhalten ist in der folgenden Tabelle angegeben:

|Header|Verhalten|
|------|--------|
|Keine|PUT ist immer erfolgreich (keine ETag-Prüfung)|
|If-match <ETag>|PUT ist nur erfolgreich, wenn die Ressource vorhanden ist und das ETag übereinstimmt.|
|If-match * | PUT ist nur erfolgreich, wenn eine Ressource vorhanden ist.|
|If-none-match * |	PUT ist nur erfolgreich, wenn die Ressource nicht vorhanden ist.|

### <a name="tags"></a>Tags

Tags unterscheiden sich von ETags. Tags sind eine Liste von Name-Wert-Paaren, die von Azure Resource Manager zum Beschriften von Ressourcen zu Abrechnungs- oder Gruppierungszwecken verwendet werden. Weitere Informationen zu Tags finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../resource-group-using-tags.md).

Azure DNS PowerShell unterstützt Tags für Zonen und Ressourceneintragssätze, die mit den `-Tag`-Optionsparametern angegeben wurden.


## Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:
	
- Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.
	
- Sie müssen die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager (1.0 oder höher) installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Schritt 1: Anmelden

Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

	Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto.

	Get-AzureRmSubscription 

Geben Sie das Abonnement an, das Sie verwenden möchten.

	Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## Schritt 2: Erstellen einer Ressourcengruppe

Der Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global und nicht regional sind, hat die Auswahl des Speicherorts für die Ressourcengruppe jedoch keine Auswirkungen auf Azure DNS.

Dieser Schritt kann übersprungen werden, wenn Sie eine vorhandene Ressourcengruppe verwenden.

	New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## Schritt 3: Registrieren

Der Azure DNS-Dienst wird vom Ressourcenanbieter "Microsoft.Network" verwaltet. Ihr Azure-Abonnement muss für die Verwendung dieses Ressourcenanbieters registriert werden, bevor Sie Azure DNS verwenden können. Dieser Schritt muss für jedes Abonnement einmal ausgeführt werden.

	Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## Schritt 4: Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `New-AzureRmDnsZone`-Cmdlet erstellt. Weiter unten finden Sie Beispiele für das Erstellen einer DNS-Zone mit oder ohne Tags. Weitere Informationen über Tags erhalten Sie im Abschnitt zu [Tags](#tags) in diesem Artikel.

>[AZURE.NOTE] In Azure DNS müssen Zonennamen ohne abschließenden Punkt **.** angegeben werden. Beispiel: **contoso.com** statt **contoso.com.**.

### So erstellen Sie eine DNS-Zone

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

	New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### So erstellen Sie eine DNS-Zone mit Tags

Das folgende Beispiel zeigt, wie Sie eine DNS-Zone mit zwei Tags erstellen: *project = demo* und *env = test*. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

	New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## Anzeigen von Datensätzen

Beim Erstellen einer DNS-Zone werden auch die folgenden DNS-Einträge erstellt:

- Der *Start of Authority*-Eintrag (SOA). Dieser ist im Stamm jeder DNS-Zone vorhanden.

- Die autoritativen Namenserver (NS)-Einträge. Diese zeigen, welche Namenserver die Zone hosten. Azure DNS verwendet einen Pool von Namenservern, sodass verschiedene Namenserver verschiedenen Zonen in Azure DNS zugewiesen werden können. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).

Verwenden Sie zum Anzeigen dieser Einträge `Get-AzureRmDnsRecordSet`:

	Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

	Name              : @
	ZoneName          : contoso.com
	ResourceGroupName : MyResourceGroup
	Ttl               : 3600
	Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
	RecordType        : SOA
	Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
	Tags              : {}

	Name              : @
	ZoneName          : contoso.com
	ResourceGroupName : MyResourceGroup
	Ttl               : 3600
	Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
	RecordType        : NS
	Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
	Tags              : {}


Ressourceneintragssätze am Stamm (oder *Apex*) einer DNS-Zone verwenden **@** als Name des Ressourceneintragssatzes.


## Test

Sie können Ihre DNS-Zone mit DNS-Tools wie nslookup, dig oder mit dem [PowerShell-Cmdlet Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx) testen.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie die DNS-Abfrage direkt auf einen der Namenserver für die Zone leiten. Die Namenserver für die Zone sind in den NS-Einträgen enthalten, die von `Get-AzureRmDnsRecordSet` (oben) aufgelistet werden. Ersetzen Sie im folgenden Befehl die Werte durch die für Ihre Zone ordnungsgemäßen Werte.

	nslookup
	> set type=SOA
	> server ns1-01.azure-dns.com
	> contoso.com

	Server: ns1-01.azure-dns.com
	Address:  208.76.47.1

	contoso.com
        	primary name server = ns1-01.azure-dns.com
        	responsible mail addr = msnhst.microsoft.com
        	serial  = 1
        	refresh = 900 (15 mins)
        	retry   = 300 (5 mins)
        	expire  = 604800 (7 days)
        	default TTL = 300 (5 mins)


## Nächste Schritte

Nach dem Erstellen einer DNS-Zone müssen [Ressourceneintragssätze und Einträge](dns-getstarted-create-recordset.md) zum Auflösen von Namen für Ihre Internetdomäne erstellt werden.

<!---HONumber=AcomDC_0824_2016-->