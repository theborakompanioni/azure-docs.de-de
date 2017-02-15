---
title: "Richtlinienreferenz für Azure API Management"
description: "Erfahren Sie mehr über die verfügbaren Richtlinien zum Konfigurieren von API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 9d4ac609-b221-4032-93ae-e27a1254d43d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: 6b77e338e1c7f0f79ea3c25b0b073296f7de0dcf
ms.openlocfilehash: 988e0cfd8329a89ba21ab2ea292785b144e08dab


---
# <a name="azure-api-management-policy-reference"></a>Richtlinienreferenz für Azure API Management
Dieser Abschnitt stellt einen Index der Richtlinien bereit, die in der [API Management-Richtlinienreferenz][API Management policy reference] enthalten sind. Weitere Informationen zum Hinzufügen und Konfigurieren von Richtlinien finden Sie unter [Richtlinien in API Management][Policies in API Management].

Richtlinienausdrücke können als Attributwerte oder Textwerte in einer beliebigen API Management-Richtlinie verwendet werden, sofern in der Richtlinie nicht anders angegeben. Einige Richtlinien, beispielsweise [Ablaufsteuerung][Control flow] und [Variable festlegen][Set variable], basieren auf Richtlinienausdrücken. Weitere Informationen finden Sie unter [Erweiterte Richtlinien][Advanced policies] und [Richtlinienausdrücke][Policy expressions].

## <a name="policy-reference-index"></a>Richtlinienreferenz – Index
* [Richtlinien für die Zugriffsbeschränkung][Access restriction policies]
  * [HTTP-Header überprüfen][Check HTTP header] – Erfordert das Vorhandensein und/oder einen Wert eines HTTP-Headers.
  * [Limit call rate (Aufrufrate begrenzen)][Limit call rate by subscription] – Verhindert API-Lastspitzen, indem die Aufrufrate jeweils pro Abonnement beschränkt wird.
  * [Limit call rate by key (Aufrufrate nach Schlüssel begrenzen)](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) – Verhindert API-Lastspitzen, indem die Aufrufrate jeweils pro Schlüssel beschränkt wird.
  * [Beschränkung für Aufrufer-IP][Restrict caller IPs] – Filtert (erlaubt/blockiert) Aufrufe von bestimmten IP-Adressen und/oder Adressbereichen.
  * [Set usage quota by subscription (Nutzungskontingent nach Abonnement festlegen)][Set usage quota by subscription] – Ermöglicht die Durchsetzung eines erneuerbaren oder für die Lebensdauer gültigen Kontingents für Aufrufe und/oder Bandbreite auf Grundlage des Abonnements.
  * [Set usage quota by key (Nutzungskontingent nach Schlüssel festlegen)](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) – Ermöglicht die Durchsetzung eines erneuerbaren oder für die Lebensdauer gültigen Kontingents für Aufrufe und/oder Bandbreite auf Grundlage des Schlüssels.
  * [JWT überprüfen][Validate JWT] – Erzwingt das Vorhandensein und die Gültigkeit eines JWT, das entweder aus einem angegebenen HTTP-Header oder aus einem angegebenen Abfrageparameter extrahiert wurde.
* [Erweiterte Richtlinien][Advanced policies]
  * [Ablaufsteuerung][Control flow] – Bedingte Anwendung von Richtlinienanweisungen basierend auf den Ergebnissen der Auswertung von booleschen [Ausdrücken][expressions].
  * [Anforderung weiterleiten][Forward request] – Leitet die Anforderung an den Back-End-Dienst.
  * [Protokoll an Event Hub][Log to Event Hub] – Sendet Nachrichten im angegebenen Format an ein von einem [Protokollierungstool](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) definiertes Nachrichtenziel.
  * [Wiederholen](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) – Wiederholt die Ausführung der eingeschlossenen Richtlinienanweisungen, falls und bis die Bedingung erfüllt ist. Die Ausführung wird mit den angegebenen Zeitintervallen und bis zur angegebenen Anzahl der Wiederholungsversuche wiederholt.
  * [Zurückgegebene Antwort](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) – bricht die Pipeline-Ausführung ab und gibt die angegebene Antwort unmittelbar an den Aufrufer zurück.
  * [Unidirektionale Anforderung senden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) – sendet eine Anforderung an die angegebene URL, ohne auf eine Antwort zu warten.
  * [Sendeanforderung](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) – sendet eine Anforderung an die angegebene URL.
  * [Anforderungsmethode festlegen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) – dient der Vornahme von Änderungen der HTTP-Anforderungsmethode.
  * [Statuscode festlegen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) – ändert den HTTP-Statuscode auf den angegebenen Wert.
  * [Variable festlegen][Set variable] – Speichert einen Wert in einer benannten [Kontextvariablen][context], um später darauf zugreifen zu können.
  * [Ablaufverfolgung](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) – Fügt eine Zeichenfolge in der Ausgabe für den [API-Inspektor](api-management-howto-api-inspector.md) hinzu.
  * [Warten](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) : Wartet darauf, dass eingeschlossene Richtlinien für „Send request“ (Sendeanforderung), „Get value from cache“ (Wert aus dem Cache abrufen) oder „Control flow“ (Ablaufsteuerung) abgeschlossen werden, bevor der Vorgang fortgesetzt wird.
* [Authentifizierungsrichtlinien][Authentication policies]
  * [Standardauthentifizierung][Authenticate with Basic] – Authentifizierung mit einem Back-End-Dienst unter Verwendung der Standardauthentifizierung.
  * [Authentifizierung mit Clientzertifikat][Authenticate with client certificate] – Authentifizierung mit einem Back-End-Dienst unter Verwendung von Clientzertifikaten.
* [Cachingrichtlinien][Caching policies] 
  * [Aus Cache abrufen][Get from cache] – Führt eine Cachesuche aus und gibt ggf. eine gültige Antwort aus dem Cache zurück.
  * [In Cache ablegen][Store to cache] – Cacheantwort gemäß der angegebenen Konfiguration für die Cachesteuerung.
  * [Get value from cache (Wert aus dem Cache abrufen)](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) – ruft ein zwischengespeichertes Element nach Schlüssel ab.
  * [Store value in cache (Wert im Cache speichern)](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) – speichert ein Element im Cache auf Basis des Schlüssels.
  * [Wert aus dem Cache entfernen](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) – Entfernt ein Element im Cache nach Schlüssel.
* [Domänenübergreifende Richtlinien][Cross domain policies] 
  * [Domänenübergreifende Aufrufe zulassen][Allow cross-domain calls] – Erlaubt API-Aufrufe von browserbasierten Clients aus, die Adobe Flash und Microsoft Silverlight verwenden.
  * [CORS][CORS] – Fügt Unterstützung für Cross-Origin Resource Sharing (CORS) einer Operation oder einer API hinzu, um domänenübergreifende Aufrufe von browserbasierten Clients aus zu ermöglichen.
  * [JSONP][JSONP] – Fügt Unterstützung für JSON mit Padding (JSONP) einer Operation oder einer API hinzu, um domänenübergreifende Aufrufe von browserbasierten Clients mit JavaScript aus zu ermöglichen.
* [Transformationsrichtlinien][Transformation policies] 
  * [JSON in XML konvertieren][Convert JSON to XML] – Konvertiert den Anforderungs- oder Antworttext von JSON in XML.
  * [XML in JSON konvertieren][Convert XML to JSON] – Konvertiert den Anforderungs- oder Antworttext von XML in JSON.
  * [Zeichenfolge in Text ersetzen][Find and replace string in body] – Sucht nach einer Antwort- oder Anforderung-Teilzeichenfolge und ersetzt diese durch eine andere Teilzeichenfolge.
  * [URLs in Inhalt maskieren][Mask URLs in content] – Ändert (maskiert) Links im Antworttext, sodass diese über das Gateway auf den äquivalenten Link zeigen.
  * [Back-End-Dienst festlegen][Set backend service] – Ändert den Back-End-Dienst für eine eingehende Anforderung.
  * [Text festlegen][Set body] – Legt den Nachrichtentext für eingehende und ausgehende Anforderungen fest.
  * [HTTP-Header setzen][Set HTTP header] – Weist einem vorhandenen Antwort- und/oder Anforderungsheader einen Wert zu oder fügt einen neuen Antwort- und/oder Anforderungsheader hinzu.
  * [Abfrageparameter setzen][Set query string parameter] – Fügt Abfrageparameter hinzu, löscht diese oder ersetzt deren Werte.
  * [URL umschreiben][Rewrite URL] – Konvertiert eine Anforderungs-URL von der öffentlichen Form in die vom Webdienst erwartete Form.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Richtlinienausdrücken finden Sie im folgenden Video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Check HTTP header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Restrict caller IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Validate JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log to Event Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store to cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON to XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML to JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Mask URLs in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set HTTP header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Rewrite URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Policies in API Management]: api-management-howto-policies.md
[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx





<!--HONumber=Dec16_HO2-->


