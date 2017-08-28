---
title: "Übersicht über die Media Services Operations-REST-API | Microsoft-Dokumentation"
description: "Übersicht über die Media Services-REST-API"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: a874bc48cc94fff32382ccdb832f0fbd3d08e03f
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="media-services-operations-rest-api-overview"></a>Übersicht über die Media Services Operations-REST-API
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Die **Media Services Operations-REST-API** wird zum Erstellen von Aufträgen, Medienobjekten, Zugriffsrichtlinien und andere Vorgänge für Objekte in einem Media Services-Konto verwendet. Weitere Informationen finden Sie in der [Referenz zur Media Services Operations-REST-API](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Microsoft Azure Media Services ist ein Dienst, der OData-basierte HTTP-Anforderungen akzeptiert und mit ausführlichem JSON- oder atom+pub-Code darauf antworten kann. Da Media Services mit den Azure-Entwicklungsrichtlinien konform ist, muss jeder Client bei der Verbindung mit Media Services eine Reihe obligatorischer HTTP-Header verwenden. Darüber hinaus stehen verschiedene optionale Header zur Verfügung. In den folgenden Abschnitten werden die Header und HTTP-Verben beschrieben, die Sie verwenden können, um Anforderungen zu erstellen und Antworten von Media Services zu empfangen.

Dieses Thema bietet einen Überblick über die Verwendung von REST v2 mit Media Services.

## <a name="considerations"></a>Überlegungen

Berücksichtigen Sie Folgendes, wenn Sie REST verwenden:

* Beim Abfragen von Entitäten gibt es ein Limit von 1.000 Entitäten, die gleichzeitig zurückgegeben werden können, da die öffentliche REST-Version 2 Abfrageergebnisse auf 1.000 Ergebnisse begrenzt. Sie müssen **Skip** und **Take** (.NET) bzw. **top** (REST) wie in [diesem .NET-Beispiel](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) und diesem [REST-API-Beispiel](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities) beschrieben verwenden. 
* Wenn Sie JSON verwenden und angeben, dass das Schlüsselwort **__metadata** in der Anforderung verwendet werden soll (z.B. für Verweise auf ein verknüpftes Objekt), MÜSSEN Sie den **Accept**-Header auf das [ausführliche JSON-Format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) festlegen (siehe folgendes Beispiel). OData versteht die **__metadata**-Eigenschaft in der Anforderung nur, wenn Sie das ausführliche Format verwenden.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Von Media Services unterstützte standardmäßige HTTP-Anforderungsheader
Für jeden Media Services-Aufruf müssen Sie eine Reihe obligatorischer Header in Ihre Anforderung einschließen. Darüber hinaus stehen verschiedene optionale Header zur Auswahl. In der folgenden Tabelle sind die erforderlichen Header aufgeführt:

| Header | Typ | Wert |
| --- | --- | --- |
| Autorisierung |Bearer |Das Bearer-Token ist der einzig zulässige Autorisierungsmechanismus. Der Wert muss außerdem das von ACS bereitgestellte Zugriffstoken enthalten. |
| x-ms-version |Decimal |2.11 |
| DataServiceVersion |Decimal |3.0 |
| MaxDataServiceVersion |Decimal |3.0 |

> [!NOTE]
> Da Media Services OData verwendet, um das zugrunde liegenden Repository für Medienobjekt-Metadaten über REST-APIs verfügbar zu machen, sollten der DataServiceVersion- Header und der MaxDataServiceVersion-Header in jede Anforderung eingeschlossen werden. Falls dies nicht geschieht, geht Media Services aktuell davon aus, dass der DataServiceVersion-Wert 3.0 verwendet wird.
> 
> 

Im Folgenden finden Sie eine Reihe optionaler Header:

| Header | Typ | Wert |
| --- | --- | --- |
| Date |RFC 1123-Datum |Zeitstempel der Anforderung |
| Accept |Content-Typ |Der angeforderte Inhaltstyp für die Antwort, z. B.:<p> -application/json;odata=verbose<p> - application/atom+xml<p> Antworten können unterschiedliche Inhaltstypen aufweisen, z. B. einen BLOB-Abruf, bei dem eine erfolgreiche Antwort den BLOB-Stream als Nutzlast enthält. |
| Accept-Encoding |Gzip, deflate |GZIP- und DEFLATE-Codierung, falls zutreffend. Hinweis: Bei großen Ressourcen kann Media Services diesen Header ignorieren und unkomprimierte Daten zurückgeben. |
| Accept-Language |„en“, „es“ usw. |Gibt die bevorzugte Sprache für die Antwort an. |
| Accept-Charset |Charset-Typ, z. B. „UTF-8“ |Der Standardwert ist UTF-8. |
| X-HTTP-Method |HTTP-Methode |Ermöglicht Clients oder Firewalls, die keine HTTP-Methoden wie PUT oder DELETE unterstützen, die Verwendung dieser Methoden über einen getunnelten GET-Aufruf. |
| Content-Typ |Content-Typ |Der Inhaltstyp des Anforderungstexts in PUT- oder POST-Anforderungen. |
| client-request-id |String |Ein vom Aufrufer definierter Wert, der die angegebene Anforderung identifiziert. Falls angegeben, wird dieser Wert in die Antwortnachricht eingeschlossen, um die Anforderung zuzuordnen. <p><p>**Wichtig**<p>Werte müssen auf 2.096 Bytes (2 KB) begrenzt sein. |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Von Media Services unterstützte standardmäßige HTTP-Antwortheader
Im Folgenden lernen Sie einen Satz von Headern kennen, die je nach der angeforderten Ressource und beabsichtigten Aktion zurückgegeben werden können.

| Header | Typ | Wert |
| --- | --- | --- |
| request-id |String |Ein eindeutiger, vom Dienst generierter Bezeichner für den aktuellen Vorgang. |
| client-request-id |String |Ein Bezeichner, der vom Aufrufer in der ursprünglichen Anforderung angegeben wird, sofern vorhanden. |
| Date |RFC 1123-Datum |Das Datum, zu dem die Anforderung verarbeitet wurde. |
| Content-Typ |Variabel |Der Inhaltstyp des Antworttexts. |
| Content-Encoding |Variabel |GZIP oder DEFLATE, je nachdem, welche Codierung geeignet ist. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Von Media Services unterstützte standardmäßige HTTP-Verben
Im Folgenden finden eine vollständige Liste der HTTP-Verben, die für HTTP-Anforderungen verwendet werden können:

| Verb | Beschreibung |
| --- | --- |
| GET |Gibt den aktuellen Wert eines Objekts zurück. |
| POST |Erstellt ein Objekt auf Grundlage der bereitgestellten Daten oder sendet einen Befehl. |
| PUT |Ersetzt ein Objekt oder erstellt ein benanntes Objekt (falls zutreffend). |
| DELETE |Löscht ein Objekt. |
| MERGE |Aktualisiert ein vorhandenes Objekt anhand von Änderungen der benannten Eigenschaft. |
| HEAD |Gibt die Metadaten eines Objekts für eine GET-Antwort zurück. |

## <a name="discover-media-services-model"></a>Ermitteln des Media Services-Modells
Um Media Services-Entitäten schnell aufzufinden, können Sie den $metadata-Vorgang verwenden. Dadurch können Sie alle gültigen Entitätstypen, Entitätseigenschaften, Zuordnungen, Funktionen, Aktionen usw. abrufen. Im folgenden Beispiel wird gezeigt, wie Sie den URI „https://media.windows.net/API/$metadata“ erstellen.

Sie sollten "?api-version=2.x" an das Ende des URIs anhängen, wenn Sie die Metadaten in einem Browser anzeigen möchten. Andernfalls sollte der x-ms-version-Header nicht in die Anforderung eingeschlossen werden.

## <a name="connect-to-media-services"></a>Verbinden mit Mediendiensten

Informationen zum Herstellen einer Verbindung mit der AMS-API finden Sie unter [Zugreifen auf die Azure Media Services-API per Azure AD-Authentifizierung](media-services-use-aad-auth-to-access-ams-api.md). Nach der erfolgreichen Verbindung mit „https://media.windows.net“ erhalten Sie eine 301 Redirect-Antwort, in der ein anderer Media Services-URI angegeben ist. Nachfolgende Aufrufe müssen an den neuen URI gesendet werden.

## <a name="next-steps"></a>Nächste Schritte

Wie Sie mit REST auf AMS APIs zugreifen, erfahren Sie unter [Use Azure AD authentication to access the Azure Media Services API with .NET (Zugreifen auf die Azure Media Services-API mit .NET mithilfe der Azure AD-Authentifizierung)](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


