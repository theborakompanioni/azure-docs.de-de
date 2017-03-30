---
title: Erstellen von mobilen Anwendungen mit Xamarin und Azure DocumentDB | Microsoft-Dokumentation
description: "Ein Tutorial, in dem eine Xamarin iOS-, Android- oder Forms-Anwendung mit Azure DocumentDB erstellt wird. DocumentDB ist eine extrem schnelle, weltumspannende Clouddatenbank für mobile Apps."
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
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 4a783397b038b7545bbbfc45813db3d9973c5e19
ms.lasthandoff: 03/22/2017


---
# <a name="build-mobile-applications-with-xamarin-and-azure-documentdb"></a>Erstellen von mobilen Anwendungen mit Xamarin und Azure DocumentDB
Für die meisten mobilen Apps ist es erforderlich, Daten in der Cloud zu speichern, und Azure DocumentDB ist eine Clouddatenbank für mobile Apps. Sie besitzt alle Komponenten, die Entwickler von mobilen Produkten benötigen. Es handelt sich um eine vollständig verwaltete NoSQL-Datenbank als Dienst (Database as a Service), die bedarfsabhängig skaliert werden und Ihre Daten an allen Orten, an denen sich Benutzer weltweit aufhalten, transparent für Ihre Anwendung bereitstellen kann. Mit dem [Azure DocumentDB .NET Core SDK](documentdb-sdk-dotnet-core.md) können Sie erreichen, dass mobile Xamarin-Apps ohne mittlere Ebene direkt mit DocumentDB interagieren.

Dieser Artikel enthält ein Tutorial zur Erstellung von mobilen Apps mit Xamarin und DocumentDB. Sie finden den vollständigen Quellcode für das Tutorial unter [Xamarin und DocumentDB auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin), z.B. auch Informationen zum Verwalten von Benutzern und Berechtigungen.

## <a name="documentdb-capabilities-for-mobile-apps"></a>DocumentDB-Funktionen für mobile Apps
DocumentDB enthält die folgenden Hauptfunktionen für Entwickler von mobilen Apps:

![DocumentDB-Funktionen für mobile Apps](media/documentdb-mobile-apps-with-xamarin/documentdb-for-mobile.png)

* Umfassende Abfragen für schemalose Daten. In DocumentDB werden Daten als schemalose JSON-Dokumente in heterogenen Sammlungen gespeichert, und Sie können [umfassende und schnelle Abfragen](documentdb-sql-query.md) verwenden, ohne sich um Schemas oder Indizes kümmern zu müssen.
* Schnell. Garantiert: Mit DocumentDB dauert das Lesen und Schreiben von Dokumenten nur wenige Millisekunden. Entwickler können den benötigten Durchsatz angeben, der in DocumentDB mit SLAs von 99,99% sichergestellt wird.
* Unbegrenzte Skalierung: Ihre DocumentDB-Sammlungen [wachsen mit Ihrer App](documentdb-partition-data.md). Sie können mit geringen Datengrößen und Hunderten von Anforderungen pro Sekunde beginnen und den Durchsatz dann beliebig auf Hunderte Millionen Anforderungen pro Sekunde und Daten im Petabyte-Bereich erhöhen.
* Globale Verteilung: Die Benutzer Ihrer mobilen App sind häufig auf der ganzen Welt unterwegs. DocumentDB ist eine [global verteilte Datenbank](documentdb-distribute-data-globally.md), und mit nur einem Klick auf einer Karte werden die Daten an den Orten bereitgestellt, an denen sich die Benutzer befinden.
* Integrierte Autorisierung: Mit DocumentDB können Sie gängige Muster wie [benutzerspezifische Daten](https://aka.ms/documentdb-xamarin-todouser) oder von mehreren Benutzern gemeinsam genutzte Daten ohne benutzerdefinierten komplexen Autorisierungscode leicht implementieren.
* Geoabfragen: Viele mobile Apps verfügen heutzutage über Geokontextfunktionen. Dank der erstklassigen Unterstützung von [Geotypen](documentdb-geospatial.md) ist dies mit DocumentDB einfach umzusetzen.
* Binäre Anlagen: Ihre App-Daten enthalten häufig binäre Blobs. Die native Unterstützung von Anlagen vereinfacht die Nutzung von DocumentDB als zentrale Anwendung für Ihre App-Daten.

## <a name="documentdb-and-xamarin-tutorial"></a>Tutorial zu DocumentDB und Xamarin
Im folgenden Tutorial wird veranschaulicht, wie Sie eine mobile Anwendung in fünf einfachen Schritten mit Xamarin und DocumentDB erstellen. Den vollständigen Quellcode für das Tutorial finden Sie unter [Xamarin und DocumentDB auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).

### <a name="get-started"></a>Erste Schritte
Die ersten Schritte mit DocumentDB sind einfach. Navigieren Sie einfach zum Azure-Portal, und erstellen Sie ein neues DocumentDB-Konto. Wechseln Sie anschließend zur Registerkarte „Schnellstart“, und laden Sie ein Xamarin Forms-Beispiel für eine „Aufgabenliste“ herunter, für das bereits eine Verbindung mit Ihrem DocumentDB-Konto besteht. 

![DocumentDB-Schnellstart für mobile Apps](media/documentdb-mobile-apps-with-xamarin/documentdb-quickstart.png)

Falls Sie über eine vorhandene Xamarin-App verfügen, können Sie einfach dieses [DocumentDB-NuGet-Paket](documentdb-sdk-dotnet-core.md) hinzufügen. DocumentDB unterstützt freigegebene Xamarin.IOS-, Xamarin.Android- und Xamarin Forms-Bibliotheken.

### <a name="work-with-data"></a>Arbeiten mit Daten
Ihre Datensätze werden in DocumentDB als schemalose JSON-Dokumente in heterogenen Sammlungen gespeichert. Sie können Dokumente mit verschiedenen Strukturen in derselben Sammlung speichern.

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
Wie bei vielen anderen Beispielen für die ersten Schritte auch, wird für das DocumentDB-Beispiel, das Sie heruntergeladen haben, eine Authentifizierung für den Dienst mit dem Masterschlüssel durchgeführt, der im Code der App hartcodiert ist. Dies ist für eine App, die Sie nicht nur in Ihrem lokalen Emulator ausführen möchten, nicht zu empfehlen. Wenn ein Angreifer in den Besitz des Masterschlüssels gelangt, sind alle Daten Ihres DocumentDB-Kontos kompromittiert. Stattdessen soll für die App nur Zugriff auf die Datensätze für den angemeldeten Benutzer bestehen. Mit DocumentDB können Entwickler für Anwendungen Lese- oder Lese-/Schreibzugriff auf eine Sammlung, einen per Partitionsschlüssel gruppierten Dokumentsatz oder ein bestimmtes Dokument gewähren. 

Hier ist beispielsweise angegeben, wie Sie die Aufgabenlisten-App in eine App für mehrere Benutzer ändern: 

* Fügen Sie die Anmeldung für Ihre App hinzu, indem Sie Facebook, Active Directory oder einen anderen Anbieter verwenden.
* Erstellen Sie eine DocumentDB-UserItems-Sammlung mit „/userId“ als Partitionsschlüssel. Indem Sie den Partitionsschlüssel für Ihre Sammlung angeben, kann DocumentDB beliebig skaliert werden, wenn die Zahl der App-Benutzer zunimmt, während weiterhin schnelle Abfragen möglich sind.
* Fügen Sie den DocumentDB Resource Token Broker hinzu. Dies ist eine einfache Web-API, mit der Benutzer authentifiziert und Token mit kurzer Gültigkeitsdauer für angemeldete Benutzer ausgestellt werden, wobei nur Zugriff auf die Dokumente in der Partition des Benutzers besteht. In diesem Beispiel hosten wir den Resource Token Broker in App Service.
* Ändern Sie die App so, dass die Resource Token Broker-Authentifizierung mit Facebook durchgeführt wird, und fordern Sie die Ressourcentoken für den angemeldeten Facebook-Benutzer an. Greifen Sie anschließend auf die Benutzerdaten in der UserItems-Sammlung zu.  

Sie finden ein vollständiges Codebeispiel für dieses Muster unter [Resource Token Broker auf GitHub](http://aka.ms/documentdb-xamarin-todouser). In diesem Diagramm ist die Lösung dargestellt:

![DocumentDB-Benutzer und Berechtigungsbroker](media/documentdb-mobile-apps-with-xamarin/documentdb-resource-token-broker.png)

Wenn zwei Benutzer Zugriff auf dieselbe Aufgabenliste haben sollen, fügen wir dem Zugriffstoken im Resource Token Broker weitere Berechtigungen hinzu.

### <a name="scale-on-demand"></a>Bedarfsabhängige Skalierung
DocumentDB ist eine verwaltete Datenbank in Form eines Diensts (Database as a Service). Wenn die Zahl Ihrer Benutzer zunimmt, fällt für Sie kein Aufwand in Bezug auf die Bereitstellung von VMs oder die Erhöhung der Anzahl von Kernen an. Sie müssen DocumentDB lediglich mitteilen, wie viele Vorgänge pro Sekunde (Durchsatz) Ihre App benötigt. Sie können den Durchsatz über die Registerkarte „Skalieren“ des Portals angeben, indem Sie für den Durchsatz die Kennzahl „Anforderungseinheiten pro Sekunde“ (Request Units per Second, RUs) verwenden. Für einen Lesevorgang bei einem Dokument mit 1 KB wird z.B. 1 RU benötigt. Sie können auch Warnungen für die Metrik zum „Durchsatz“ hinzufügen, um die Zunahme des Datenverkehrs zu überwachen und den Durchsatz bei der Auslösung von Warnungen programmgesteuert zu ändern.

![Bedarfsabhängiger DocumentDB-Skalierungsdurchsatz](media/documentdb-mobile-apps-with-xamarin/documentdb-scale.png)

### <a name="go-planet-scale"></a>Weltweite Verfügbarkeit
Wenn die Beliebtheit Ihrer App zunimmt, erreichen Sie ggf. Benutzer auf der ganzen Welt. Vielleicht möchten Sie aber auch nur auf der sicheren Seite sein, wenn ein Meteorit die Azure-Rechenzentren trifft, in denen Sie Ihre DocumentDB-Sammlung erstellt haben. Navigieren Sie zum Azure-Portal und zu Ihrem DocumentDB-Konto, und legen Sie durch einen Klick auf eine Karte fest, dass Ihre Daten fortlaufend in den verfügbaren Regionen weltweit repliziert werden sollen. So wird sichergestellt, dass Ihre Daten überall dort verfügbar sind, wo sich Ihre Benutzer befinden, und Sie können Failoverrichtlinien hinzufügen, um auf Notfälle vorbereitet zu sein.

![DocumentDB-Skalierung über geografische Regionen hinweg](media/documentdb-mobile-apps-with-xamarin/documentdb-replicate-globally.png)

Glückwunsch! Sie haben die Lösung erstellt und verfügen nun über eine mobile App mit Xamarin und DocumentDB. Sie können ein ähnliches Muster in Cordova-Apps nutzen, indem Sie das DocumentDB-JavaScript-SDK verwenden, und in nativen iOS-/Android-Apps, indem Sie DocumentDB-REST-APIs einsetzen.

## <a name="next-steps"></a>Nächste Schritte
* Zeigen Sie den Quellcode für [Xamarin und DocumentDB auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin) an.
* Laden Sie das [DocumentDB .NET Core SDK](documentdb-sdk-dotnet-core.md) herunter.
* Sehen Sie sich weitere Codebeispiele für [.NET-Anwendungen](documentdb-dotnet-samples.md) an.
* Informieren Sie sich über die [umfassenden Abfragefunktionen von DocumentDB](documentdb-sql-query.md).
* Erfahren Sie mehr über die [Unterstützung von Geodaten in DocumentDB](documentdb-geospatial.md).




