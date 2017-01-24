---
title: "Entwickeln mit regionsübergreifenden DocumentDB-Konten | Microsoft Docs"
description: "Erfahren Sie, wie Sie über Azure DocumentDB, einen vollständig verwalteten NoSQL-Datenbankdienst, auf Ihre auf mehrere Regionen verteilten Daten zugreifen."
services: documentdb
documentationcenter: 
author: kiratp
manager: jhubbard
editor: 
ms.assetid: d4579378-0b3a-44a5-9f5b-630f1fa4c66d
ms.service: documentdb
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: kipandya
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: a0b1418168d493ce0e669a9eb0594d37e741df7d


---
# <a name="developing-with-multi-region-documentdb-accounts"></a>Entwickeln mit regionsübergreifenden DocumentDB-Konten
> [!NOTE]
> Die globale Verteilung von DocumentDB-Datenbanken ist allgemein verfügbar und für alle neu erstellten DocumentDB-Konten automatisch aktiviert. Wir arbeiten daran, die globale Verteilung für alle vorhandenen Konten zu aktivieren. Wenn Sie jedoch in der Zwischenzeit die globale Verteilung für Ihr Konto aktivieren möchten, [kontaktieren Sie den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), der diesen Schritt für Sie ausführt.
>
>

Um von der [globalen Verteilung](documentdb-distribute-data-globally.md)zu profitieren, können Clientanwendungen in einer Liste die Reihenfolge angeben, in der Regionen bei Dokumentvorgängen bevorzugt verwendet werden sollen. Dies lässt sich durch Einrichten einer Verbindungsrichtlinie erreichen. Basierend auf der Azure DocumentDB-Kontokonfiguration, der aktuellen regionalen Verfügbarkeit und der angegebenen Reihenfolgeliste der bevorzugten Regionen wählt das SDK den optimalen Endpunkt für Schreib- und Lesevorgänge aus.

Diese Liste wird beim Initialisieren einer Verbindung mithilfe der DocumentDB-Client-SDKs angegeben. Die SDKs akzeptieren einen optionalen Parameter „PreferredLocations“, bei dem es sich um eine sortierte Liste von Azure-Regionen handelt.

Das SDK sendet automatisch alle Schreibvorgänge an die aktuell für solche Vorgänge ausgewählte Region.

Alle Lesevorgänge werden an die erste verfügbare Region in der Liste der bevorzugten Regionen gesendet. Wenn bei der Anforderung ein Fehler auftritt, führt der Client ein Failover zur nächsten Region auf der Liste durch, usw.

Die Client-SDKs empfangen Leseanforderungen nur aus den in der Liste „PreferredLocations“ angegebenen Regionen. Wenn ein Datenbankkonto in drei Regionen verfügbar ist, der Client aber nur zwei Leseregionen für PreferredLocations angibt, werden auch bei einem Failover keine Lesevorgänge außerhalb der Schreibregion verarbeitet.

Die Anwendung kann die vom SDK ausgewählten aktuellen Schreib- und Leseendpunkte anhand von zwei Eigenschaften überprüfen: WriteEndpoint und ReadEndpoint. Diese stehen im SDK der Version 1.8 und höher zur Verfügung.

Wenn die PreferredLocations-Eigenschaft nicht festgelegt ist, werden alle Anforderungen von der aktuellen Schreibregion verarbeitet.

## <a name="net-sdk"></a>.NET SDK
Das SDK kann ohne Codeänderungen verwendet werden. In diesem Fall sendet das SDK automatisch sowohl Lese- als auch Schreibvorgänge an die aktuelle Schreibregion.

Im .NET SDK, Version 1.8 oder höher, besitzt der ConnectionPolicy-Parameter für den DocumentClient-Konstruktor folgende Eigenschaft: Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Diese Eigenschaft weist den Typ „Collection `<string>` “ auf und sollte eine Liste mit Regionsnamen enthalten. Die Zeichenfolgenwerte sind gemäß der Spalte mit den Regionsnamen auf der Seite [Azure-Regionen][regions] formatiert und enthalten keine Leerzeichen vor und nach dem letzten Zeichen.

Die aktuellen Schreib- und Leseendpunkte sind in DocumentClient.WriteEndpoint bzw. DocumentClient.ReadEndpoint verfügbar.

> [!NOTE]
> Die URLs für die Endpunkte sollten nicht als langfristige Konstanten betrachtet werden. Sie können jederzeit vom Dienst aktualisiert werden. Das SDK verarbeitet diese Änderung automatisch.
>
>

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
    
    ConnectionPolicy connectionPolicy = new ConnectionPolicy();

    //Setting read region selection preference
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS-, JavaScript- und Python-SDKs
Das SDK kann ohne Codeänderungen verwendet werden. In diesem Fall sendet das SDK sowohl Lese- als auch Schreibvorgänge an die aktuelle Schreibregion.

In Version 1.8 oder höher jedes SDKs weist der ConnectionPolicy-Parameter für den DocumentClient-Konstruktor eine neue Eigenschaft auf: DocumentClient.ConnectionPolicy.PreferredLocations. Bei diesem Parameter handelt es sich um ein Zeichenfolgenarray, das eine Liste mit Regionsnamen akzeptiert. Die Namen sind gemäß der Spalte mit den Regionsnamen auf der Seite [Azure-Regionen][regions] formatiert. Sie können auch die vordefinierten Konstanten im bereitgestellten AzureDocuments.Regions-Objekt verwenden.

Die aktuellen Schreib- und Leseendpunkte sind in DocumentClient.getWriteEndpoint bzw. DocumentClient.getReadEndpoint verfügbar.

> [!NOTE]
> Die URLs für die Endpunkte sollten nicht als langfristige Konstanten betrachtet werden. Sie können jederzeit vom Dienst aktualisiert werden. Das SDK verarbeitet diese Änderung automatisch.
>
>

Im Folgenden finden Sie ein Codebeispiel für NodeJS/Javascript. Python und Java folgen demselben Muster.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();

    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>REST
Sobald ein Datenbankkonto in mehreren Regionen zur Verfügung gestellt wurde, können Clients mithilfe einer GET-Anforderung über den folgenden URI die Verfügbarkeit des Kontos abfragen.

    https://{databaseaccount}.documents.azure.com/

Der Dienst gibt eine Liste der Regionen und der zugehörigen DocumentDB-Endpunkt-URIs für die Replikate zurück. Die aktuelle Schreibregion wird in der Antwort angegeben. Der Client kann dann wie folgt den geeigneten Endpunkt für alle weiteren REST-API-Anforderungen auswählen.

Beispielantwort

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* Alle PUT-, POST- und DELETE-Anforderungen müssen an den angegebenen Schreib-URI gesendet werden.
* Alle GET-Anforderungen sowie weitere Anforderungen ohne Schreibzugriff (z.B. Abfragen) können an einen beliebigen vom Client ausgewählten Endpunkt gesendet werden.

Bei Schreibanforderungen an schreibgeschützte Regionen tritt der HTTP-Fehler 403 („Verboten“) auf.

Wenn sich nach der anfänglichen Ermittlungsphase des Clients die Schreibregion ändert, tritt bei nachfolgenden Schreibanforderungen an die vorherige Schreibregion der HTTP-Fehler 403 („Verboten“) auf. Der Client sollte dann erneut eine GET-Anforderung senden, um die Liste der Regionen erneut abzurufen und die aktualisierte Schreibregion zu empfangen.

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie mehr über die globale Verteilung von Daten mit DocumentDB:

* [Globale Verteilung von Daten mit DocumentDB](documentdb-distribute-data-globally.md)
* [Konsistenzebenen](documentdb-consistency-levels.md)
* [So funktioniert der Durchsatz mit mehreren Regionen](documentdb-manage.md)
* [Hinzufügen von Regionen über das Azure-Portal](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/



<!--HONumber=Dec16_HO2-->


