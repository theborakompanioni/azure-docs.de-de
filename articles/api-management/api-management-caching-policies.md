---
title: Cacherichtlinien in Azure API Management | Microsoft-Dokumentation
description: "Erfahren Sie mehr über die Cacherichtlinien, die für die Verwendung in Azure API Management verfügbar sind."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: 77fd7b5b339a8ede8a297bec96f91f0a243cc18d
ms.openlocfilehash: 606670068acd407a8f35e991f0cde12b84cdb1bf

---
# <a name="api-management-caching-policies"></a>Cacherichtlinien für API Management
Dieses Thema enthält eine Referenz für die folgenden API Management-Richtlinien. Weitere Informationen zum Hinzufügen und Konfigurieren von Richtlinien finden Sie unter [Richtlinien in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="a-namecachingpoliciesa-caching-policies"></a><a name="CachingPolicies"></a> Cacherichtlinien  
  
-   Cacherichtlinien für Antworten  
  
    -   [Aus Cache abrufen](api-management-caching-policies.md#GetFromCache): Führt eine Cachesuche aus und gibt ggf. gültige Antworten aus dem Cache zurück.  
  
    -   [In Cache ablegen](api-management-caching-policies.md#StoreToCache): Speichert Antworten gemäß der angegebenen Konfiguration für die Cachesteuerung zwischen.  
  
-   Cacherichtlinien für Werte  
  
    -   [Get value from cache (Wert aus dem Cache abrufen)](#GetFromCacheByKey) – ruft ein zwischengespeichertes Element nach Schlüssel ab.  
  
    -   [Store value in cache (Wert im Cache speichern)](#StoreToCacheByKey) – speichert ein Element im Cache auf Basis des Schlüssels.  
  
    -   [Wert aus dem Cache entfernen](#RemoveCacheByKey) – Entfernt ein Element im Cache nach Schlüssel.  
  
##  <a name="a-namegetfromcachea-get-from-cache"></a><a name="GetFromCache"></a> Aus Cache abrufen  
 Verwenden Sie die `cache-lookup`-Richtlinie zum Durchführen einer Cachesuche und ggf. zum Zurückgeben einer gültigen Antwort aus dem Cache. Diese Richtlinie kann in Fällen angewendet werden, in denen sich der Inhalt von Antworten über längere Zeit nicht ändert. Das Zwischenspeichern von Antworten senkt die Bandbreitennutzung und die Prozessoranforderungen auf dem Back-End-Webserver und verringert die Latenz für API-Consumer.  
  
> [!NOTE]
>  Diese Richtlinie setzt das Vorhandensein einer entsprechenden Richtlinie [In Cache ablegen](api-management-caching-policies.md#StoreToCache) voraus.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">  
  <vary-by-header>Accept</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Accept-Charset</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Authorization</vary-by-header>  
  <!-- should be present when allow-authorized-response-caching is "true"-->  
  <vary-by-header>header name</vary-by-header>  
  <!-- optional, can repeated several times -->  
  <vary-by-query-parameter>parameter name</vary-by-query-parameter>  
  <!-- optional, can repeated several times -->  
</cache-lookup>  
```  
  
### <a name="examples"></a>Beispiele  
  
#### <a name="example"></a>Beispiel  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true">  
            <vary-by-query-parameter>version</vary-by-query-parameter>  
        </cache-lookup>           
    </inbound>  
    <outbound>  
        <cache-store duration="seconds" />  
        <base />          
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a>Beispiel für die Verwendung von Richtlinienausdrücken  
 Dieses Beispiel zeigt die Konfiguration der Dauer des API Management-Antwortcachings. Diese entspricht dem Zwischenspeichern von Antworten des Back-End-Diensts wie durch die `Cache-Control`-Anweisung des Back-End-Diensts angegeben. Eine Demonstration der Konfiguration und Verwendung dieser Richtlinie finden Sie unter [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) (Cloud Cover-Folge 177 zu weiteren API Management-Funktionen mit Vlad Vinogradsky). Führen Sie einen schnellen Vorlauf bis 25:25 durch.  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 Weitere Informationen finden Sie unter [Richtlinienausdrücke](api-management-policy-expressions.md) und [Kontextvariable](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|cache-lookup|Stammelement.|Ja|  
|vary-by-header|Startet das Zwischenspeichern je nach Wert des angegebenen Headers, z.B. Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match.|Nein|  
|vary-by-query-parameter|Antworten werden je nach Wert der angegebenen Abfrageparameter zwischengespeichert. Geben Sie einen oder mehrere Parameter an. Verwenden Sie Semikolons als Trennzeichen. Wenn kein Parameter angegeben ist, werden alle Abfrageparameter verwendet.|Nein|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|allow-private-response-caching|Bei Festlegung auf `true` wird das Zwischenspeichern von Anforderungen erlaubt, die einen Autorisierungsheader enthalten.|Nein|false|  
|downstream-caching-type|Dieses Attribut muss auf einen der folgenden Werte festgelegt werden.<br /><br /> –   none: Downstreamzwischenspeicherung ist unzulässig.<br />–   private: Private Downstreamzwischenspeicherung ist zulässig.<br />–   public: Private und gemeinsam genutzte Downstreamzwischenspeicherung ist zulässig.|Nein|(Keine)|  
|must-revalidate|Wenn die Downstreamzwischenspeicherung aktiviert ist, aktiviert oder deaktiviert dieses Attribut die `must-revalidate`-Cachesteuerungsanweisung in Gatewayantworten.|Nein|true|  
|vary-by-developer|Legen Sie diese Option auf `true` fest, um Antworten pro Entwicklerschlüssel zwischenzuspeichern.|Nein|false|  
|vary-by-developer-groups|Legen Sie diese Option auf `true` fest, um Antworten pro Benutzerrolle zwischenzuspeichern.|Nein|false|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound  
  
-   **Richtlinienbereiche:** API, operation, product  
  
##  <a name="a-namestoretocachea-store-to-cache"></a><a name="StoreToCache"></a> In Cache ablegen  
 Die `cache-store`-Richtlinie speichert Antworten gemäß den angegebenen Cacheeinstellungen zwischen. Diese Richtlinie kann in Fällen angewendet werden, in denen sich der Inhalt von Antworten über längere Zeit nicht ändert. Das Zwischenspeichern von Antworten senkt die Bandbreitennutzung und die Prozessoranforderungen auf dem Back-End-Webserver und verringert die Latenz für API-Consumer.  
  
> [!NOTE]
>  Diese Richtlinie setzt das Vorhandensein einer entsprechenden Richtlinie [Aus Cache abrufen](api-management-caching-policies.md#GetFromCache) voraus.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a>Beispiele  
  
#### <a name="example"></a>Beispiel  
  
```xml  
<policies>  
    <inbound>  
        <base />  
          <cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false">  
                <vary-by-query-parameter>parameter name</vary-by-query-parameter> <!-- optional, can repeated several times -->  
        </cache-lookup>  
    </inbound>  
    <outbound>  
        <base />  
            <cache-store duration="3600" />  
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a>Beispiel für die Verwendung von Richtlinienausdrücken  
 Dieses Beispiel zeigt die Konfiguration der Dauer des API Management-Antwortcachings. Diese entspricht dem Zwischenspeichern von Antworten des Back-End-Diensts wie durch die `Cache-Control`-Anweisung des Back-End-Diensts angegeben. Eine Demonstration der Konfiguration und Verwendung dieser Richtlinie finden Sie unter [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) (Cloud Cover-Folge 177 zu weiteren API Management-Funktionen mit Vlad Vinogradsky). Führen Sie einen schnellen Vorlauf bis 25:25 durch.  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 Weitere Informationen finden Sie unter [Richtlinienausdrücke](api-management-policy-expressions.md) und [Kontextvariable](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|cache-store|Stammelement.|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|duration|Lebensdauer der zwischengespeicherten Einträge, angegeben in Sekunden.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: outbound  
  
-   **Richtlinienbereiche:** API, operation, product  
  
##  <a name="a-namegetfromcachebykeya-get-value-from-cache"></a><a name="GetFromCacheByKey"></a> Wert aus Cache abrufen  
 Verwenden Sie die `cache-lookup-value`-Richtlinie zum Durchführen einer Cachesuche nach Schlüssel und zum Zurückgeben eines zwischengespeicherten Werts. Der Schlüssel kann einen beliebigen Zeichenfolgenwert aufweisen und wird in der Regel über einen Richtlinienausdruck angegeben.  
  
> [!NOTE]
>  Diese Richtlinie setzt das Vorhandensein einer entsprechenden Richtlinie [Wert in Cache ablegen](#StoreToCacheByKey) voraus.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value to use if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a>Beispiel  
 Weitere Informationen und Beispiele zu dieser Richtlinie finden Sie unter [Benutzerdefiniertes Caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|cache-lookup-value|Stammelement.|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|default-value|Ein Wert, der der Variablen zugewiesen wird, wenn die Cacheschlüsselsuche zu keinem Ergebnis führt. Wenn dieses Attribut nicht angegeben wird, wird `null` zugewiesen.|Nein|`null`|  
|key|In der Suche zu verwendender Cacheschlüsselwert.|Ja|N/V|  
|variable-name|Der Name der [Kontextvariablen](api-management-policy-expressions.md#ContextVariables), der der gesuchte Wert zugewiesen wird, wenn die Suche erfolgreich ist. Wenn die Suche zu keinem Ergebnis führt, wird der Variablen der Wert des `default-value`-Attribut oder `null` zugewiesen, wenn das `default-value`-Attribut ausgelassen wird.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** global, API, operation, product  
  
##  <a name="a-namestoretocachebykeya-store-value-in-cache"></a><a name="StoreToCacheByKey"></a> Wert in Cache ablegen  
 `cache-store-value` führt die Cachespeicherung nach Schlüssel durch. Der Schlüssel kann einen beliebigen Zeichenfolgenwert aufweisen und wird in der Regel über einen Richtlinienausdruck angegeben.  
  
> [!NOTE]
>  Diese Richtlinie setzt das Vorhandensein einer entsprechenden Richtlinie [Wert aus Cache abrufen](#GetFromCacheByKey) voraus.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<cache-store-value key="cache key value" value="value to cache" duration="seconds" />  
```  
  
### <a name="example"></a>Beispiel  
 Weitere Informationen und Beispiele zu dieser Richtlinie finden Sie unter [Benutzerdefiniertes Caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|cache-store-value|Stammelement.|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|duration|Der Wert wird für die in Sekunden angegebene Dauer zwischengespeichert.|Ja|N/V|  
|key|Der Cacheschlüssel, unter dem der Wert gespeichert wird.|Ja|N/V|  
|value|Der Wert, der zwischengespeichert werden soll.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** global, API, operation, product  
  
###  <a name="a-nameremovecachebykeya-remove-value-from-cache"></a><a name="RemoveCacheByKey"></a> Wert aus Cache entfernen  
 `cache-remove-value` löscht ein zwischengespeichertes Element, das über seinen Schlüssel identifiziert wird. Der Schlüssel kann einen beliebigen Zeichenfolgenwert aufweisen und wird in der Regel über einen Richtlinienausdruck angegeben.  
  
#### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a>Beispiel  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|cache-remove-value|Stammelement.|Ja|  
  
#### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|key|Der Schlüssel des zuvor zwischengespeicherten Werts, der aus dem Cache entfernt werden soll.|Ja|N/V|  
  
#### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** global, API, operation, product  
  

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Arbeiten mit Richtlinien finden Sie unter [Richtlinien in Azure API Management](api-management-howto-policies.md).  


<!--HONumber=Jan17_HO2-->


