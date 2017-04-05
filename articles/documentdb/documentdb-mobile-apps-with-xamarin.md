---
title: Erstellen von mobilen Anwendungen mit Xamarin und Azure DocumentDB | Microsoft-Dokumentation
description: "Ein Tutorial, in dem eine Xamarin iOS-, Android- oder Forms-Anwendung mit Azure DocumentDB erstellt wird. DocumentDB ist eine schnelle, globale Clouddatenbank für mobile Apps."
services: documentdb
documentationcenter: .net
author: arramac
manager: monicar
editor: 
ms.assetid: ff97881a-b41a-499d-b7ab-4f394df0e153
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 3e058505f7c17d19d2cebca053badb3505a00f13
ms.lasthandoff: 03/28/2017


---
# <a name="build-mobile-applications-with-xamarin-and-azure-documentdb"></a>Erstellen von mobilen Anwendungen mit Xamarin und Azure DocumentDB
Die meisten mobilen Apps müssen Daten in der Cloud speichern, und Azure DocumentDB ist eine Clouddatenbank für mobile Apps. Sie besitzt alle Komponenten, die Entwickler von mobilen Produkten benötigen. Es handelt sich um eine vollständig verwaltete NoSQL-Database-as-a-Service-Lösung, die bei Bedarf skaliert werden kann. Diese Lösung kann Benutzern Ihrer Anwendung Daten unabhängig vom Standort weltweit transparent bereitstellen. Mit dem [Azure DocumentDB .NET Core SDK](documentdb-sdk-dotnet-core.md) können Sie erreichen, dass mobile Xamarin-Apps ohne mittlere Ebene direkt mit DocumentDB interagieren.

Dieser Artikel enthält ein Tutorial zur Erstellung mobiler Apps mit Xamarin und DocumentDB. Sie finden den vollständigen Quellcode für das Tutorial und Informationen zum Verwalten von Benutzern und Berechtigungen unter [Xamarin und DocumentDB auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).

## <a name="documentdb-capabilities-for-mobile-apps"></a>DocumentDB-Funktionen für mobile Apps
DocumentDB enthält die folgenden Hauptfunktionen für Entwickler von mobilen Apps:

![DocumentDB-Funktionen für mobile Apps](media/documentdb-mobile-apps-with-xamarin/documentdb-for-mobile.png)

* Umfassende Abfragen für schemalose Daten. DocumentDB speichert Daten als schemalose JSON-Dokumente in heterogenen Sammlungen. Die Lösung bietet [umfassende und schnelle Abfragen](documentdb-sql-query.md), ohne sich um Schemas oder Indizes kümmern zu müssen.
* Schneller Durchsatz. Mit DocumentDB dauert das Lesen und Schreiben von Dokumenten nur wenige Millisekunden. Entwickler können den benötigten Durchsatz angeben, der in DocumentDB mit SLAs von 99,99 % sichergestellt wird.
* Unbegrenzte Skalierung. Ihre DocumentDB-Sammlungen [wachsen mit Ihrer App mit](documentdb-partition-data.md). Sie können mit einer kleinen Datenmenge und einem Durchsatz von Hunderten von Anforderungen pro Sekunde beginnen. Ihre Sammlungen können auf eine Größe im Petabyte-Bereich anwachsen, und der Durchsatz kann auf Hunderte Millionen Anforderungen pro Sekunde vergrößert werden.
* Globale Verteilung. Benutzer mobiler Apps sind (häufig weltweit) unterwegs. DocumentDB ist eine [global verteilte Datenbank](documentdb-distribute-data-globally.md). Klicken Sie auf die Karte, um Ihre Daten den Benutzern zugänglich machen.
* Integrierte Autorisierung. Mit DocumentDB können Sie gängige Muster wie [benutzerspezifische Daten](https://aka.ms/documentdb-xamarin-todouser) oder von mehreren Benutzern gemeinsam genutzte Daten ohne benutzerdefinierten komplexen Autorisierungscode leicht implementieren.
* Geoabfragen. Viele mobile Apps verfügen heutzutage über Geokontextfunktionen. Dank der erstklassigen Unterstützung von [Geotypen](documentdb-geospatial.md) ist das Erstellen dieser Funktionen mit DocumentDB einfach zu erreichen.
* Binäre Anlagen. Ihre App-Daten enthalten häufig binäre Blobs. Die native Unterstützung von Anlagen vereinfacht die Nutzung von DocumentDB als zentrale Anwendung für Ihre App-Daten.

## <a name="documentdb-and-xamarin-tutorial"></a>Tutorial zu DocumentDB und Xamarin
Im folgenden Tutorial wird veranschaulicht, wie Sie eine mobile Anwendung mit Xamarin und DocumentDB erstellen. Den vollständigen Quellcode für das Tutorial finden Sie unter [Xamarin und DocumentDB auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).

### <a name="get-started"></a>Erste Schritte
Die ersten Schritte mit DocumentDB sind einfach. Erstellen Sie im Azure-Portal ein neues DocumentDB-Konto. Klicken Sie auf die Registerkarte **Schnellstart**. Laden Sie ein Xamarin Forms-Beispiel für eine Aufgabenliste herunter, für das bereits eine Verbindung mit Ihrem DocumentDB-Konto besteht. 

![DocumentDB-Schnellstart für mobile Apps](media/documentdb-mobile-apps-with-xamarin/documentdb-quickstart.png)

Falls Sie bereits über eine Xamarin-App verfügen, können Sie einfach das [DocumentDB NuGet-Paket](documentdb-sdk-dotnet-core.md) hinzufügen. DocumentDB unterstützt freigegebene Xamarin.IOS-, Xamarin.Android- und Xamarin Forms-Bibliotheken.

### <a name="work-with-data"></a>Arbeiten mit Daten
Ihre Datensätze werden in DocumentDB als schemalose JSON-Dokumente in heterogenen Sammlungen gespeichert. Sie können Dokumente mit verschiedenen Strukturen in derselben Sammlung speichern:

```cs
    var result = await client.CreateDocumentAsync(collectionLink, todoItem);
```

In Ihren Xamarin-Projekten können Sie in die Sprache integrierte Abfragen für schemalose Daten verwenden:

```cs
    var query = await client.CreateDocumentQuery<ToDoItem>(collectionLink)
                    .Where(todoItem => todoItem.Complete == false)
                    .AsDocumentQuery();

    Items = new List<TodoItem>();
    while (query.HasMoreResults) {
        Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
    }
```
### <a name="add-users"></a>Hinzufügen von Benutzern
Wie bei vielen anderen Beispielen für die ersten Schritte auch, wird für das DocumentDB-Beispiel, das Sie heruntergeladen haben, eine Authentifizierung für den Dienst mit dem Masterschlüssel durchgeführt, der im Code der App hartcodiert ist. Diese Standardvorgehensweise ist für eine App, die Sie nicht nur in Ihrem lokalen Emulator ausführen möchten, nicht zu empfehlen. Wenn ein nicht autorisierter Benutzer in den Besitz des Masterschlüssels gelangt, kann die Sicherheit aller Daten in Ihrem DocumentDB-Konto gefährdet sein. Stattdessen soll Ihre App nur auf die Datensätze des angemeldeten Benutzers zugreifen. Mit DocumentDB können Entwickler für Anwendungen Lese- oder Lese-/Schreibberechtigung für eine Sammlung, eine anhand eines Partitionsschlüssels gruppierte Dokumentgruppe oder ein bestimmtes Dokument erteilen. 

Führen Sie diese Schritte aus, um die Aufgabenlisten-App in eine Aufgabenlisten-App für mehrere Benutzer zu ändern: 

  1. Fügen Sie die Anmeldung bei Ihrer App mithilfe von Facebook, Active Directory oder eines anderen Anbieters hinzu.

  2. Erstellen Sie eine DocumentDB-UserItems-Sammlung mit **/userId** als Partitionsschlüssel. Indem Sie den Partitionsschlüssel für Ihre Sammlung angeben, kann DocumentDB beliebig skaliert werden, wenn die Zahl der App-Benutzer zunimmt, während weiterhin schnelle Abfragen möglich sind.

  3. Fügen Sie den DocumentDB-Ressourcentokenbroker hinzu. Mit dieser einfachen Web-API werden Benutzer authentifiziert und Token mit kurzer Gültigkeitsdauer für angemeldete Benutzer ausgestellt, wobei nur Zugriff auf die Dokumente in ihrer Partition besteht. In diesem Beispiel wird der Ressourcentokenbroker in App Service gehostet.

  4. Ändern Sie die App so, dass der Ressourcentokenbroker mit Facebook authentifiziert wird, und fordern Sie die Ressourcentoken für den angemeldeten Facebook-Benutzer an. Greifen Sie anschließend auf die Benutzerdaten in der UserItems-Sammlung zu.  

Sie finden ein vollständiges Codebeispiel für dieses Muster unter [Resource Token Broker auf GitHub](http://aka.ms/documentdb-xamarin-todouser). In diesem Diagramm ist die Lösung dargestellt:

![DocumentDB-Benutzer und Berechtigungsbroker](media/documentdb-mobile-apps-with-xamarin/documentdb-resource-token-broker.png)

Wenn zwei Benutzer Zugriff auf dieselbe Aufgabenliste haben sollen, können Sie dem Zugriffstoken im Ressourcentokenbroker weitere Berechtigungen hinzufügen.

### <a name="scale-on-demand"></a>Bedarfsabhängige Skalierung
DocumentDB ist eine verwaltete Datenbank in Form eines Diensts (Database as a Service). Wenn die Zahl Ihrer Benutzer zunimmt, fällt für Sie kein Aufwand in Bezug auf die Bereitstellung von VMs oder die Erhöhung der Anzahl von Kernen an. Sie müssen DocumentDB lediglich mitteilen, wie viele Vorgänge pro Sekunde (Durchsatz) Ihre App benötigt. Sie können den Durchsatz auf der Registerkarte **Skalieren** angeben, indem Sie für den Durchsatz die Kennzahl „Anforderungseinheiten pro Sekunde“ (Request Units per Second, RUs) verwenden. Für den Lesevorgang eines Dokuments mit 1 KB wird z.B. 1 RU benötigt. Sie können der Metrik **Durchsatz** auch Warnungen hinzufügen, um die Zunahme des Datenverkehrs zu überwachen und den Durchsatz bei der Auslösung von Warnungen programmgesteuert zu ändern.

![Bedarfsabhängiger DocumentDB-Skalierungsdurchsatz](media/documentdb-mobile-apps-with-xamarin/documentdb-scale.png)

### <a name="go-planet-scale"></a>Weltweite Verfügbarkeit
Wenn die Beliebtheit Ihrer App zunimmt, gewinnen Sie ggf. Benutzer auf der ganzen Welt hinzu. Oder vielleicht möchten Sie auf unvorhergesehene Ereignisse vorbereitet sein. Öffnen Sie im Azure-Portal Ihr DocumentDB-Konto. Klicken Sie auf die Karte, um Ihre Daten laufend in eine beliebige Anzahl von Regionen weltweit zu replizieren. Auf diese Weise stehen Ihre Daten dort zur Verfügung, wo Ihre Benutzer sind. Sie können auch Failoverrichtlinien hinzufügen, um auf Eventualitäten vorbereitet zu sein.

![DocumentDB-Skalierung über geografische Regionen hinweg](media/documentdb-mobile-apps-with-xamarin/documentdb-replicate-globally.png)

Herzlichen Glückwunsch. Sie haben die Lösung erstellt und verfügen nun über eine mobile App mit Xamarin und DocumentDB. Führen Sie ähnliche Schritte zum Erstellen von Cordova-Apps aus, indem Sie das DocumentDB JavaScript SDK verwenden. Erstellen Sie native iOS-/Android-Apps, indem Sie DocumentDB REST-APIs einsetzen.

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich den Quellcode für [Xamarin und DocumentDB auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin) an.
* Laden Sie das [DocumentDB .NET Core SDK](documentdb-sdk-dotnet-core.md) herunter.
* Sehen Sie sich weitere Codebeispiele für [.NET-Anwendungen](documentdb-dotnet-samples.md) an.
* Informieren Sie sich über die [umfassenden Abfragefunktionen von DocumentDB](documentdb-sql-query.md).
* Erfahren Sie mehr über die [Unterstützung von Geodaten in DocumentDB](documentdb-geospatial.md).




