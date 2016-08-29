<properties 
   pageTitle="Erstellen von DNS-Zonen und Ressourceneintragssätzen für Azure DNS mithilfe des .NET SDK | Microsoft Azure" 
   description="Informationen zum Erstellen von DNS-Zonen und Ressourceneintragssätzen für Azure DNS mithilfe des .NET SDK." 
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


# Erstellen von DNS-Zonen und -Ressourceneintragssätzen mithilfe des .NET SDK

Sie können die Vorgänge zum Erstellen, Löschen oder Aktualisieren von DNS-Zonen, Ressourceneintragssätzen und Einträgen mithilfe des DNS SDK mit der .NET DNS Management-Bibliothek automatisieren. Ein vollständiges Visual Studio-Projekt steht Ihnen [hier](http://download.microsoft.com/download/2/A/C/2AC64449-1747-49E9-B875-C71827890126/AzureDnsSDKExample_2015_05_05.zip) zur Verfügung.

## NuGet-Pakete und Namespacedeklarationen

Um den DNS-Client zu verwenden, müssen Sie das NuGet-Paket **Azure DNS Management Library** installieren und die DNS-Management-Namespaces Ihrem Projekt hinzufügen.
 
1. Öffnen Sie in **Visual Studio** ein vorhandenes oder neues Projekt.

2. Wechseln Sie zu **Tools** **>** **NuGet-Paket-Manager** **>** **Paket-Manager-Konsole**.

3. Laden Sie die Azure DNS Management Library herunter.

		using Microsoft.Azure;
		using Microsoft.Azure.Management.Dns;
		using Microsoft.Azure.Management.Dns.Models;

## Initialisieren des DNS-Verwaltungsclients

*DnsManagementClient* enthält die Methoden und Eigenschaften, die für die Verwaltung von DNS-Zonen und Ressourceneintragssätzen erforderlich sind. Damit der Client auf Ihr Abonnement zugreifen kann, müssen Sie geeignete Berechtigungen einrichten und ein AWT-Token generieren. Weitere Informationen finden Sie unter [Authentifizieren von Anforderungen von Azure Resource Manager](https://msdn.microsoft.com/library/azure/dn790557.aspx).

	// get a token for the AAD application (see the article link above for code)
	string jwt = GetAToken();

	// make the TokenCloudCredentials using subscription ID and token
	TokenCloudCredentials tcCreds = new TokenCloudCredentials(subID, jwt);

	// make the DNS management client
	DnsManagementClient dnsClient = new DnsManagementClient(tcCreds);

## Erstellen oder Aktualisieren einer DNS-Zone

Zum Erstellen einer DNS-Zone wird ein Objekt des Typs „Zone“ erstellt und an *dnsClient.Zones.CreateOrUpdate* übergeben. Da DNS-Zonen nicht mit einer bestimmten Region verknüpft sind, wird der Standort auf „global“ festgelegt.<BR> Azure DNS unterstützt die optimistische Parallelität, die [Etags](dns-getstarted-create-dnszone.md) genannt wird. „Etag“ ist eine Eigenschaft der Zone. „IfNoneMatch“ ist eine Eigenschaft in „ZoneCreateOrUpdateParameters“.

	// To create a DNS zone
	Zone z = new Zone("global");
	z.Properties = new ZoneProperties();
	z.Tags.Add("dept", "shopping");
	z.Tags.Add("env", "production");
	ZoneCreateOrUpdateParameters zoneParams = new ZoneCreateOrUpdateParameters(z);
	ZoneCreateOrUpdateResponse responseCreateZone = 
	dnsClient.Zones.CreateOrUpdate("myresgroup", "myzone.com", zoneParams);



## Erstellen oder Aktualisieren von DNS-Einträgen und -DNS-Ressourceneintragssätzen

DNS-Einträge werden als Datensatzgruppe verwaltet. Ein Ressourceneintragssatz ist eine Gruppe von Einträgen mit demselben Namen und Eintragstyp in einer Zone. Zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes wird ein „RecordSet“-Objekt erstellt und an *dnsClient.RecordSets.CreateOrUpdate* übergeben. Beachten Sie, dass der Name des Ressourceneintragssatzes relativ zum Zonennamen und nicht der vollqualifizierte DNS-Namen ist. Der Standort wird auf „global“ festgelegt.<BR> Azure DNS unterstützt die optimistische Parallelität [Etags](dns-getstarted-create-dnszone.md). „Etag“ ist eine Eigenschaft des „RecordSet“-Objekts. „IfNoneMatch“ ist eine Eigenschaft in „RecordSetCreateOrUpdateParameters“.
    


	// To create record sets
	RecordSet rsWwwA = new RecordSet("global");
	rsWwwA.Properties = new RecordProperties(3600);
	rsWwwA.Properties.ARecords = new List<ARecord>();
	rsWwwA.Properties.ARecords.Add(new ARecord("1.2.3.4"));
	rsWwwA.Properties.ARecords.Add(new ARecord("1.2.3.5"));
	RecordCreateOrUpdateParameters recordParams = 
								new RecordCreateOrUpdateParameters(rsWwwA);
	RecordCreateOrUpdateResponse responseCreateA = 
								dnsClient.RecordSets.CreateOrUpdate("myresgroup", 
	"myzone.com", "www", RecordType.A, recordParams);
	
    
## Abrufen von Zonen und Ressourceneintragssätzen

Die Auflistungen *Zones* und *RecordSets* ermöglichen das Abrufen von Zonen bzw. Ressourceneintragssätzen. Ressourceneintragssätze werden anhand des Typs, Namens und der Zone (und Ressourcengruppe) bestimmt, in der sie sich befinden. Zonen werden anhand des Namens und der Ressourcengruppe bestimmt, in der sie sich befinden.

	ZoneGetResponse getZoneResponse = 
	dnsClient.Zones.Get("myresgroup", "myzone.com");
	RecordGetResponse getRSResp = 
	dnsClient.RecordSets.Get("myresgroup", 
	"myzone.com", "www", RecordType.A);

## Auflisten von Zonen und Ressourceneintragssätzen

Verwenden Sie zum Auflisten von Zonen die *List*-Methode in der „Zones“-Auflistung. Verwenden Sie zum Auflisten von Ressourceneintragssätzen die Methode *List* oder *ListAll* in der „RecordSets“-Auflistung. Die *List*-Methode unterscheidet sich von der *ListAll*-Methode darin, dass nur die Ressourceneintragssätze des angegebenen Typs zurückgegeben werden.

Das folgende Beispiel zeigt, wie Sie eine Liste der DNS-Zonen und Ressourceneintragssätze abrufen können:


	ZoneListResponse zoneListResponse = dnsClient.Zones.List("myresgroup", new ZoneListParameters());
	foreach (Zone zone in zoneListResponse.Zones)
	{
    	RecordListResponse recordSets = 
                 			dnsClient.RecordSets.ListAll("myresgroup", "myzone.com", new RecordSetListParameters());

    // do something like write out each record set
	}


## Nächste Schritte

[Visual Studio SDK-Beispielprojekt](http://download.microsoft.com/download/2/A/C/2AC64449-1747-49E9-B875-C71827890126/AzureDnsSDKExample_2015_05_05.zip)

<!---HONumber=AcomDC_0817_2016-->