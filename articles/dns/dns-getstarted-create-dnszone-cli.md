<properties
   pageTitle="Erstellen einer DNS-Zone mithilfe der Befehlszeilenschnittstelle| Microsoft Azure"
   description="Erfahren Sie, wie Sie DNS-Zonen für Azure DNS Schritt für Schritt erstellen, um mit dem Hosten der DNS-Domäne mithilfe der Befehlszeilenschnittstelle (CLI) zu beginnen."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# Erstellen einer Azure-DNS-Zone mithilfe der Befehlszeilenschnittstelle


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-dnszone-cli.md)


In diesem Artikel werden die einzelnen Schritte zum Erstellen einer DNS-Zone mithilfe der Befehlszeilenschnittstelle erläutert. Die DNS-Zone kann auch mithilfe von PowerShell oder über das Azure-Portal erstellt werden.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## Voraussetzungen

Für diese Anweisungen wird die Microsoft Azure-Befehlszeilenschnittstelle (CLI) verwendet. Aktualisieren Sie vor dem Verwenden von Azure DNS-Befehlen auf die neueste Version der Azure-Befehlszeilenschnittstelle (mindestens 0.9.8). Geben Sie `azure -v` ein, um zu überprüfen, welche Version der Azure-Befehlszeilenschnittstelle derzeit auf Ihrem Computer installiert ist.

## Schritt 1: Einrichten der Azure-Befehlszeilenschnittstelle

### 1\. Installieren der Azure-Befehlszeilenschnittstelle

Sie können die Azure-Befehlszeilenschnittstelle für Windows, Linux oder Mac installieren. Die folgenden Schritte müssen ausgeführt werden, bevor Sie Azure DNS über die Azure-Befehlszeilenschnittstelle verwalten können. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md). Für DNS-Befehle wird mindestens Version 0.9.8 der Azure-Befehlszeilenschnittstelle benötigt.

Alle Netzwerkanbieterbefehle für die Befehlszeilenschnittstelle können über den folgenden Befehl ermittelt werden:

	azure network

### 2\. Wechseln des CLI-Modus

Azure DNS verwendet den Azure-Ressourcen-Manager. Achten Sie darauf, zum CLI-Modus zur Verwendung von ARM-Befehlen zu wechseln.

	azure config mode arm

### 3\. Anmelden bei Ihrem Azure-Konto

Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert. Denken Sie daran, dass Sie nur ORGID-Konten verwenden können.

    azure login -u "username"

### 4\. Auswählen des Abonnements

Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

    azure account set "subscription name"

### 5\. Erstellen einer Ressourcengruppe

Der Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global und nicht regional sind, hat die Auswahl des Speicherorts für die Ressourcengruppe jedoch keine Auswirkungen auf Azure DNS.

Dieser Schritt kann übersprungen werden, wenn Sie eine vorhandene Ressourcengruppe verwenden.

    azure group create -n myresourcegroup --location "West US"


### 6\. Registrieren

Der Azure DNS-Dienst wird vom Ressourcenanbieter "Microsoft.Network" verwaltet. Ihr Azure-Abonnement muss für die Verwendung dieses Ressourcenanbieters registriert werden, bevor Sie Azure DNS verwenden können. Dieser Schritt muss für jedes Abonnement einmal ausgeführt werden.

	azure provider register --namespace Microsoft.Network


## Schritt 2: Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `azure network dns zone create`-Befehl erstellt. Optional können Sie eine DNS-Zone mit Tags erstellen. Tags sind eine Liste von Name-Wert-Paaren, die von Azure Resource Manager zum Beschriften von Ressourcen zu Abrechnungs- oder Gruppierungszwecken verwendet werden. Weitere Informationen zu Tags finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../resource-group-using-tags.md).

In Azure DNS müssen Zonennamen ohne abschließenden Punkt angegeben werden. Beispiel: **contoso.com** statt **contoso.com.**.


### So erstellen Sie eine DNS-Zone

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt.

Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

    azure network dns zone create myresourcegroup contoso.com

### So erstellen Sie eine DNS-Zone und Tags

Die Azure DNS-CLI unterstützt Tags von DNS-Zonen, die mithilfe des optionalen Parameters *-Tag* angegeben wurden. Das folgende Beispiel zeigt, wie Sie eine DNS-Zone mit zwei Tags erstellen: „project = demo“ und „env = test“.

Verwenden Sie das Beispiel, um eine DNS-Zone und Tags zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

	azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## Anzeigen von Datensätzen

Beim Erstellen einer DNS-Zone werden auch die folgenden DNS-Einträge erstellt:

- Der "Start of Authority" (SOA)-Eintrag. Dieser ist im Stamm jeder DNS-Zone vorhanden.

- Die autoritativen Namenserver (NS)-Einträge. Diese zeigen, welche Namenserver die Zone hosten. Azure DNS verwendet einen Pool von Namenservern, sodass verschiedene Namenserver verschiedenen Zonen in Azure DNS zugewiesen werden können. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).

Verwenden Sie zum Anzeigen dieser Datensätze `azure network dns-record-set show`.<BR> *Syntax: network dns record-set show <Ressourcengruppe> <Name der DNS-Zone> <Name> <Typ>*


Im folgenden Beispiel gibt der Befehl mit der Ressourcengruppe *myresourcegroup*, dem Namen des Ressourceneintragssatzes *"@"* (für einen Stammdatensatz) und dem Typ *SOA* Folgendes aus:


	azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
	info:    Executing command network dns-record-set show
	+ Looking up the DNS record set "@"
	data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
	data:    Name                            : @
	data:    Type                            : Microsoft.Network/dnszones/SOA
	data:    Location                        : global
	data:    TTL                             : 3600
	data:    SOA record:
	data:      Email                         : msnhst.microsoft.com
	data:      Expire time                   : 604800
	data:      Host                          : edge1.azuredns-cloud.net
	data:      Minimum TTL                   : 300
	data:      Refresh time                  : 900
	data:      Retry time                    : 300
	data:                                    :
<BR> Verwenden Sie zum Anzeigen der mit der Zone erstellten NS-Einträge den folgenden Befehl:

	azure network dns record-set show myresourcegroup "contoso.com" "@" NS
	info:    Executing command network dns-record-set show
	+ Looking up the DNS record set "@"
	data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
	data:    Name                            : @
	data:    Type                            : Microsoft.Network/dnszones/NS
	data:    Location                        : global
	data:    TTL                             : 3600
	data:    NS records
	data:        Name server domain name     : ns1-05.azure-dns.com
	data:        Name server domain name     : ns2-05.azure-dns.net
	data:        Name server domain name     : ns3-05.azure-dns.org
	data:        Name server domain name     : ns4-05.azure-dns.info
	data:
	info:    network dns-record-set show command OK

>[AZURE.NOTE] Ressourceneintragssätze am Stamm (oder *Apex*) einer DNS-Zone verwenden **@** als Name des Ressourceneintragssatzes.

## Test

Sie können Ihre DNS-Zone mit DNS-Tools wie nslookup, DIG oder mit dem `Resolve-DnsName`-PowerShell-Cmdlet testen.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie die DNS-Abfrage direkt auf einen der Namenserver für die Zone leiten. Die Namenserver für die Zone sind in den NS-Einträgen enthalten, die von „azure network dns-record-set show“ aufgelistet werden. Ersetzen Sie im folgenden Befehl die Werte durch die für Ihre Zone ordnungsgemäßen Werte.

Im folgenden Beispiel wird DIG zum Abfragen der Domäne "contoso.com" mithilfe der Namenserver verwendet, die der DNS-Zone zugewiesen sind. Die Abfrage muss mit DIG durchgeführt werden und auf einen Namenserver, für den wir *@<Namenserver für die Zone>* verwendet haben, sowie auf den Zonennamen verweisen.

	 <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
	(1 server found)
	global options: +cmd
 	Got answer:
	->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
 	flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
 	WARNING: recursion requested but not available

 	OPT PSEUDOSECTION:
 	EDNS: version: 0, flags:; udp: 4000
  	QUESTION SECTION:
	contoso.com.                        IN      A

 	AUTHORITY SECTION:
	contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
	msnhst.microsoft.com. 6 900 300 604800 300

	Query time: 93 msec
	SERVER: 208.76.47.5#53(208.76.47.5)
	WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
	MSG SIZE  rcvd: 120

## Nächste Schritte

Nach dem Erstellen einer DNS-Zone müssen [Ressourceneintragssätze und Einträge](dns-getstarted-create-recordset-cli.md) zum Auflösen von Namen für Ihre Internetdomäne erstellt werden.

<!---HONumber=AcomDC_1005_2016-->