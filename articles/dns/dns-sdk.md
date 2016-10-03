<properties 
   pageTitle="Erstellen von DNS-Zonen und Ressourceneintragssätzen für Azure DNS mithilfe des .NET SDK | Microsoft Azure" 
   description="Enthält Informationen zum Erstellen von DNS-Zonen und Ressourceneintragssätzen in Azure DNS mithilfe des .NET SDK." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# Erstellen von DNS-Zonen und -Ressourceneintragssätzen mithilfe des .NET SDK

Sie können die Vorgänge zum Erstellen, Löschen oder Aktualisieren von DNS-Zonen, Ressourceneintragssätzen und Einträgen mithilfe des DNS SDK mit der .NET DNS Management-Bibliothek automatisieren. Ein vollständiges Visual Studio-Projekt steht Ihnen [hier](https://www.microsoft.com/de-DE/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True) zur Verfügung.

## Erstellen eines Dienstprinzipalkontos

Normalerweise wird der programmgesteuerte Zugriff auf Azure-Ressourcen über ein dediziertes Konto und nicht über Ihre eigenen Benutzeranmeldeinformationen gewährt. Diese dedizierten Konten werden als „Dienstprinzipalkonten“ bezeichnet. Zum Verwenden des Azure DNS SDK-Beispielprojekts müssen Sie zuerst ein Dienstprinzipalkonto erstellen und diesem die richtigen Berechtigungen zuweisen.

1. Führen Sie diese [Anleitungsschritte](../resource-group-authenticate-service-principal.md) aus, um ein Dienstprinzipalkonto zu erstellen (für das Azure DNS SDK-Beispielprojekt wird die kennwortbasierte Authentifizierung vorausgesetzt).

2. Erstellen Sie eine Ressourcengruppe ([Anleitung](../azure-portal/resource-group-portal.md)).

3. Verwenden Sie Azure RBAC, um dem Dienstprinzipalkonto Berechtigungen vom Typ „DNS Zone Contributor“ (DNS-Zone – Mitwirkender) für die Ressourcengruppe zu gewähren ([Anleitung](../active-directory/role-based-access-control-configure.md)).

4. Bearbeiten Sie die Datei „program.cs“ wie folgt, wenn Sie das Azure DNS SDK-Beispielprojekt verwenden:
	* Fügen Sie die richtigen Werte für „tenatId“, „clientId“ (auch als Konto-ID bezeichnet), „secret“ (Kennwort des Dienstprinzipalkontos) und „subscriptionId“ gemäß Schritt 1 ein.
	* Geben Sie den Ressourcengruppennamen ein, den Sie in Schritt 2 gewählt haben.
	* Geben Sie einen DNS-Zonennamen Ihrer Wahl ein.

## NuGet-Pakete und Namespacedeklarationen

Zum Verwenden des Azure DNS .NET SDK müssen Sie das NuGet-Paket **Azure DNS Management Library** und andere erforderliche Azure-Pakete installieren.
 
1. Öffnen Sie in **Visual Studio** ein vorhandenes oder neues Projekt.

2. Navigieren Sie zu **Tools** **>** **NuGet-Paket-Manager** **>** **NuGet-Pakete für Projektmappe verwalten...**.

3. Klicken Sie auf **Durchsuchen**, aktivieren Sie das Kontrollkästchen **Vorabversion einbeziehen**, und geben Sie im Suchfeld den Text **Microsoft.Azure.Management.Dns** ein.

4. Wählen Sie das Paket aus, und klicken Sie auf **Installieren**, um es dem Visual Studio-Projekt hinzuzufügen.
 
5. Wiederholen Sie den obigen Vorgang, um auch die folgenden Pakete zu installieren: **Microsoft.Rest.ClientRuntime.Azure.Authentication** und **Microsoft.Azure.Management.ResourceManager**.

## Hinzufügen von Namespacedeklarationen

Fügen Sie die folgenden Namespacedeklarationen hinzu:

	using Microsoft.Rest.Azure.Authentication;
	using Microsoft.Azure.Management.Dns;
	using Microsoft.Azure.Management.Dns.Models;

## Initialisieren des DNS-Verwaltungsclients

*DnsManagementClient* enthält die Methoden und Eigenschaften, die für die Verwaltung von DNS-Zonen und Ressourceneintragssätzen erforderlich sind. Mit dem folgenden Code wird die Anmeldung am Dienstprinzipalkonto durchgeführt und ein DnsManagementClient-Objekt erstellt.

	// Build the service credentials and DNS management client
	var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
	var dnsClient = new DnsManagementClient(serviceCreds);
	dnsClient.SubscriptionId = subscriptionId;

## Erstellen oder Aktualisieren einer DNS-Zone

Für die Erstellung einer DNS-Zone wird zuerst ein Zone-Objekt erstellt, das die DNS-Zonenparameter enthält. Da DNS-Zonen nicht mit einer bestimmten Region verknüpft sind, wird der Standort auf „global“ festgelegt. In diesem Beispiel wird der Zone auch ein [Azure Resource Manager-„Tag“](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) hinzugefügt.

Für die eigentliche Erstellung oder Aktualisierung der Zone in Azure DNS wird das Zonenobjekt, das die Zonenparameter enthält, an die *DnsManagementClient.Zones.CreateOrUpdateAsyc*-Methode übergeben.

>[AZURE.NOTE] DnsManagementClient unterstützt drei Betriebsmodi: synchron („CreateOrUpdate“), asynchron („CreateOrUpdateAsync“) oder asynchron mit Zugriff auf die HTTP-Antwort („CreateOrUpdateWithHttpMessagesAsync“). Sie können je nach Ihren Anwendungsanforderungen einen beliebigen Modus auswählen.

Azure DNS unterstützt die optimistische Parallelität, die [Etags](dns-getstarted-create-dnszone.md) genannt wird. In diesem Beispiel wird Azure DNS durch die Angabe von „*“ für den „If-None-Match“-Header angewiesen, eine DNS-Zone zu erstellen, falls diese noch nicht vorhanden ist. Der Aufruf schlägt fehl, wenn in der jeweiligen Ressourcengruppe bereits eine Zone mit dem angegebenen Namen vorhanden ist.

	// Create zone parameters
	var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
	
	// Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
	dnsZoneParams.Tags = new Dictionary<string, string>();
	dnsZoneParams.Tags.Add("dept", "finance");
	
	// Create the actual zone.
	// Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
	// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
	// Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
	var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## Erstellen von DNS-Ressourceneintragssätzen und -Einträgen

DNS-Einträge werden als Ressourceneintragssatz verwaltet. Ein Ressourceneintragssatz ist eine Gruppe von Einträgen mit demselben Namen und Eintragstyp in einer Zone. Der Name des Ressourceneintragssatzes ist relativ zum Zonennamen, nicht zum vollqualifizierten DNS-Namen.

Zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes wird ein „RecordSet“-Parameterobjekt erstellt und an *DnsManagementClient.RecordSets.CreateOrUpdateAsync* übergeben. Wie bei DNS-Zonen auch, werden drei Betriebsmodi unterstützt: synchron („CreateOrUpdate“), asynchron („CreateOrUpdateAsync“) oder asynchron mit Zugriff auf die HTTP-Antwort („CreateOrUpdateWithHttpMessagesAsync“).

Ebenfalls wie bei DNS-Zonen verfügen Vorgänge für Ressourceneintragssätze über Unterstützung der optimistischen Nebenläufigkeit. Da in diesem Beispiel weder „If-Match“ noch „If-None-Match“ angegeben ist, wird der Ressourceneintragssatz immer erstellt. Mit diesem Aufruf werden alle vorhandenen Ressourceneintragssätze mit demselben Namen und Eintragstyp in dieser DNS-Zone überschrieben.

	// Create record set parameters
	var recordSetParams = new RecordSet();
	recordSetParams.TTL = 3600;

	// Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
	recordSetParams.ARecords = new List<ARecord>();
	recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

	// Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
	recordSetParams.Metadata = new Dictionary<string, string>();
	recordSetParams.Metadata.Add("user", "Mary");

	// Create the actual record set in Azure DNS
	// Note: no ETAG checks specified, will overwrite existing record set if one exists
	var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## Abrufen von Zonen und Ressourceneintragssätzen

Mit den Methoden *DnsManagementClient.Zones.Get* und *DnsManagementClient.RecordSets.Get* werden einzelne Zonen bzw. Ressourceneintragssätze abgerufen. Ressourceneintragssätze werden anhand des Typs, Namens und der Zone (und Ressourcengruppe) bestimmt, in der sie sich befinden. Zonen werden anhand des Namens und der Ressourcengruppe bestimmt, in der sie sich befinden.

	var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
	
## Aktualisieren eines vorhandenen Ressourceneintragssatzes

Rufen Sie zum Aktualisieren eines vorhandenen DNS-Ressourceneintragssatzes zuerst den Ressourceneintragssatz auf, und aktualisieren Sie dann seinen Inhalt, bevor Sie die Änderung übermitteln. In diesem Beispiel geben wir das „ETag“ aus dem abgerufenen Ressourceneintragssatz im Parameter „If-Match“ an. Der Aufruf schlägt fehl, wenn ein gleichzeitiger Vorgang den Ressourceneintragssatz in der Zwischenzeit geändert hat.

	var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

	// Add a new record to the local object.  Note that records in a record set must be unique/distinct
	recordSet.ARecords.Add(new ARecord("5.6.7.8"));

	// Update the record set in Azure DNS
	// Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
	recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## Auflisten von Zonen und Ressourceneintragssätzen

Verwenden Sie zum Auflisten von Zonen die *DnsManagementClient.Zones.List...*-Methoden, die entweder das Auflisten aller Zonen einer bestimmten Ressourcengruppe oder aller Zonen eines bestimmten Azure-Abonnements (ressourcenübergreifend) unterstützen. Verwenden Sie zum Auflisten von Ressourceneintragssätzen die *DnsManagementClient.RecordSets.List...*-Methoden, die entweder das Auflisten aller Ressourceneintragssätze einer bestimmten Zone oder nur der Ressourceneintragssätze eines bestimmten Typs unterstützen.

Beachten Sie beim Auflisten von Zonen und Ressourceneintragssätzen, dass die Ergebnisse auf mehrere Seiten aufgeteilt sein können. Im folgenden Beispiel wird veranschaulicht, wie Sie die Seiten mit den Ergebnissen durchlaufen können. (Um die Aufteilung auf Seiten zu erzwingen, wird eine sehr kleine Seitengröße von „2“ verwendet. In der Praxis sollte dieser Parameter weggelassen und die Standardseitengröße verwendet werden.)

	// Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
	// In practice, to improve performance you would use a large page size or just use the system default
	int recordSets = 0;
	var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
	recordSets += page.Count();

	while (page.NextPageLink != null)
	{
		page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
		recordSets += page.Count();
	}

## Nächste Schritte

Laden Sie das [Azure DNS .NET SDK-Beispielprojekt](https://www.microsoft.com/de-DE/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True) herunter, in dem weitere Beispiele für die Verwendung des Azure DNS .NET SDK enthalten sind, z.B. Beispiele für andere DNS-Eintragstypen.

<!---HONumber=AcomDC_0921_2016-->