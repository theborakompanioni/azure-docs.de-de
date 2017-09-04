---
title: "Azure API Management-Richtlinien für die Zugriffsbeschränkung | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Richtlinien für die Zugriffsbeschränkung, die für die Verwendung in Azure API Management verfügbar sind."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.translationtype: HT
ms.sourcegitcommit: 07e5e15f4f4c4281a93c8c3267c0225b1d79af45
ms.openlocfilehash: 70cccd5ac7fbea36f02150e3da319a043c22c677
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="api-management-access-restriction-policies"></a>API Management-Richtlinien für die Zugriffsbeschränkung
Dieses Thema bietet eine Referenz für die folgenden API Management-Richtlinien. Weitere Informationen zum Hinzufügen und Konfigurieren von Richtlinien finden Sie unter [Richtlinien in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AccessRestrictionPolicies"></a> Richtlinien für die Zugriffsbeschränkung  
  
-   [HTTP-Header überprüfen](api-management-access-restriction-policies.md#CheckHTTPHeader) – erfordert das Vorhandensein und/oder einen Wert eines HTTP-Headers.  
  
-   [Limit call rate (Aufrufrate begrenzen)](api-management-access-restriction-policies.md#LimitCallRate) – verhindert API-Lastspitzen, indem die Aufrufrate jeweils pro Abonnement beschränkt wird.  
  
-   [Limit call rate (Aufrufrate nach Schlüssel begrenzen)](#LimitCallRateByKey) – verhindert API-Lastspitzen, indem die Aufrufrate jeweils pro Schlüssel beschränkt wird.  
  
-   [Beschränkung für Aufrufer-IP](api-management-access-restriction-policies.md#RestrictCallerIPs) – Filtert (erlaubt/blockiert) Aufrufe von bestimmten IP-Adressen und/oder Adressbereichen.  
  
-   [Set usage quota by subscription (Nutzungskontingent nach Abonnement festlegen)](api-management-access-restriction-policies.md#SetUsageQuota) – ermöglicht die Durchsetzung eines erneuerbaren oder für die Lebensdauer gültigen Kontingents für Aufrufe und/oder Bandbreite auf Grundlage des Abonnements.  
  
-   [Set usage quota by key (Nutzungskontingent nach Schlüssel festlegen)](#SetUsageQuotaByKey) – ermöglicht die Durchsetzung eines erneuerbaren oder für die Lebensdauer gültigen Kontingents für Aufrufe und/oder Bandbreite auf Grundlage des Schlüssels.  
  
-   [JWT überprüfen](api-management-access-restriction-policies.md#ValidateJWT) – erzwingt das Vorhandensein und die Gültigkeit eines JWT, das entweder aus einem angegebenen HTTP-Header oder aus einem angegebenen Abfrageparameter extrahiert wurde.  
  
##  <a name="CheckHTTPHeader"></a> HTTP-Header überprüfen  
 Verwenden Sie die `check-header`-Richtlinie, um zu erzwingen, dass eine Anforderung einen angegebenen HTTP-Header enthält. Sie können optional überprüfen, ob der Header einen bestimmten Wert aufweist, oder einen Bereich zulässiger Werte überprüfen. Wenn die Überprüfung nicht erfolgreich abgeschlossen wird, beendet die Richtlinie die Verarbeitung und gibt den von der Richtlinie festgelegten HTTP-Statuscode und die zugehörige Fehlermeldung zurück.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|check-header|Stammelement.|Ja|  
|value|Zulässiger HTTP-Headerwert. Wenn mehrere Wertelemente angegeben sind, wird die Überprüfung als erfolgreich gewertet, wenn für einen beliebigen dieser Werte eine Übereinstimmung vorhanden ist.|Nein|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|failed-check-error-message|Die Fehlermeldung, die im HTTP-Antworttext zurückgegeben wird, wenn der Header nicht vorhanden ist oder einen ungültigen Wert aufweist. In dieser Meldung müssen alle Sonderzeichen ordnungsgemäß mit Escapezeichen versehen sein.|Ja|N/V|  
|failed-check-httpcode|Der HTTP-Statuscode, der zurückgeben werden wird, wenn der Header nicht vorhanden ist oder einen ungültigen Wert aufweist.|Ja|N/V|  
|header-name|Der Name des zu überprüfenden HTTP-Headers.|Ja|N/V|  
|ignore-case|Kann auf „true“ oder „false“ festgelegt werden. Wenn dieses Attribut auf „true“ festgelegt ist, wird beim Vergleichen des Headerwerts mit dem Satz zulässiger Werte die Groß- und Kleinschreibung ignoriert.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound, outbound  
  
-   **Richtlinienbereiche**: global, Produkt, API, Vorgang  
  
##  <a name="LimitCallRate"></a> Aufrufrate nach Abonnement begrenzen  
 Die `rate-limit`-Richtlinie verhindert API-Nutzungsspitzen auf Abonnementbasis, indem sie die Aufrufrate auf eine angegebene Anzahl pro angegebenem Zeitraum begrenzt. Wenn diese Richtlinie ausgelöst wird, empfängt der Aufrufer einen `429 Too Many Requests`-Antwortstatuscode.  
  
> [!IMPORTANT]
>  Diese Richtlinie kann pro Richtliniendokument nur einmal verwendet werden.  
>   
>  [Richtlinienausdrücke](api-management-policy-expressions.md) können in keinem der Richtlinienattribute für diese Richtlinie werden.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit calls="20" renewal-period="90" />  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|set-limit|Stammelement.|Ja|  
|api|Fügen Sie mindestens eins dieser Elemente hinzu, um eine Aufrufratenbegrenzung für APIs innerhalb des Produkts zu erzwingen. Produkt- und API-Aufrufratenbegrenzungen werden unabhängig voneinander angewendet.|Nein|  
|operation|Fügen Sie mindestens eins dieser Elemente hinzu, um eine Aufrufratenbegrenzung auf Vorgänge innerhalb einer API zu erzwingen. Aufrufratenbegrenzungen für Produkte, APIs und Vorgänge werden unabhängig voneinander angewendet.|Nein|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|Name|Der Name der API, auf die die Ratenbegrenzung angewendet werden soll.|Ja|N/V|  
|calls|Die maximale Gesamtanzahl von Aufrufen, die während des in der `renewal-period` angegebenen Zeitraums zulässig sind.|Ja|N/V|  
|renewal-period|Der Zeitraum in Sekunden, nach dem das Kontingent zurückgesetzt wird.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound  
  
-   **Richtlinienbereiche**: Produkt  
  
##  <a name="LimitCallRateByKey"></a> Aufrufrate nach Schlüssel begrenzen  
 Die `rate-limit-by-key`-Richtlinie verhindert API-Nutzungsspitzen auf Schlüsselbasis, indem sie die Aufrufrate auf eine angegebene Anzahl pro angegebenem Zeitraum beschränkt. Der Schlüssel kann einen beliebigen Zeichenfolgenwert aufweisen und wird in der Regel über einen Richtlinienausdruck angegeben. Optional kann eine inkrementelle Bedingung hinzugefügt werden, um anzugeben, welche Anforderungen für den Grenzwert gezählt werden sollen. Wenn diese Richtlinie ausgelöst wird, empfängt der Aufrufer einen `429 Too Many Requests`-Antwortstatuscode.  
  
 Weitere Informationen und Beispiele zu dieser Richtlinie finden Sie unter [Erweiterte Anforderungsbegrenzung mit Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Diese Richtlinie kann pro Richtliniendokument nur einmal verwendet werden.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird die Ratenbegrenzung anhand der IP-Adresse des Aufrufers bestimmt.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit-by-key  calls="10"  
              renewal-period="60"  
              increment-condition="@(context.Response.StatusCode == 200)"  
              counter-key="@(context.Request.IpAddress)"/>  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|set-limit|Stammelement.|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|calls|Die maximale Gesamtanzahl von Aufrufen, die während des in der `renewal-period` angegebenen Zeitraums zulässig sind.|Ja|N/V|  
|counter-key|Der Schlüssel, der für die Ratenbegrenzungsrichtlinie verwendet werden soll.|Ja|N/V|  
|increment-condition|Der boolesche Ausdruck, der angibt, ob die Anforderung für das Kontingent gezählt werden soll (`true`).|Nein|N/V|  
|renewal-period|Der Zeitraum in Sekunden, nach dem das Kontingent zurückgesetzt wird.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound  
  
-   **Richtlinienbereiche**: global, Produkt, API, Vorgang  
  
##  <a name="RestrictCallerIPs"></a> Aufrufer-IPs beschränken  
 Die `ip-filter`-Richtlinie filtert (erlaubt/blockiert) Aufrufe von bestimmten IP-Adressen und/oder Adressbereichen.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|ip-filter|Stammelement.|Ja|  
|address|Gibt eine einzelne IP-Adresse an, nach der gefiltert werden soll.|Mindestens ein `address`- oder `address-range`-Element ist erforderlich.|  
|address-range from="Adresse" to="Adresse"|Gibt einen Bereich von IP-Adressen an, nach dem gefiltert werden soll.|Mindestens ein `address`- oder `address-range`-Element ist erforderlich.|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|address-range from="Adresse" to="Adresse"|Ein IP-Adressbereich, für den diese Richtlinie gelten soll.|Erforderlich, wenn das `address-range`-Element verwendet wird.|N/V|  
|ip-filter action="allow &#124; forbid"|Gibt an, ob Aufrufe für die angegebenen IP-Adressen oder -Adressbereiche erlaubt oder blockiert werden sollen.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound  
  
-   **Richtlinienbereiche**: global, Produkt, API, Vorgang  
  
##  <a name="SetUsageQuota"></a> Nutzungskontingent nach Abonnement festlegen  
 Die `quota`-Richtlinie erzwingt ein erneuerbares oder für die Lebensdauer gültiges Aufruf- und/oder Bandbreitenkontingent pro Abonnement.  
  
> [!IMPORTANT]
>  Diese Richtlinie kann pro Richtliniendokument nur einmal verwendet werden.  
>   
>  [Richtlinienausdrücke](api-management-policy-expressions.md) können in keinem der Richtlinienattribute für diese Richtlinie werden.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|quota|Stammelement.|Ja|  
|api|Fügen Sie mindestens eins dieser Elemente hinzu, um ein Kontingent für APIs innerhalb des Produkts zu erzwingen. Produkt- und API-Kontingente werden unabhängig voneinander angewendet.|Nein|  
|operation|Fügen Sie mindestens eins dieser Elemente hinzu, um ein Kontingent auf Vorgänge innerhalb einer API zu erzwingen. Kontingente für Produkte, APIs und Vorgänge werden unabhängig voneinander angewendet.|Nein|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|Name|Der Name der API oder des Vorgangs, für die bzw. den das Kontingent gilt.|Ja|N/V|  
|bandwidth|Die maximale Gesamtanzahl von Kilobytes, die während des in der `renewal-period` angegebenen Zeitraums zulässig sind.|Es müssen entweder `calls` oder `bandwidth` oder beide Attribute zusammen angegeben werden.|N/V|  
|calls|Die maximale Gesamtanzahl von Aufrufen, die während des in der `renewal-period` angegebenen Zeitraums zulässig sind.|Es müssen entweder `calls` oder `bandwidth` oder beide Attribute zusammen angegeben werden.|N/V|  
|renewal-period|Der Zeitraum in Sekunden, nach dem das Kontingent zurückgesetzt wird.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound  
  
-   **Richtlinienbereiche**: Produkt  
  
##  <a name="SetUsageQuotaByKey"></a> Nutzungskontingent nach Schlüssel festlegen  
 Die `quota-by-key`-Richtlinie erzwingt ein erneuerbares oder für die Lebensdauer gültiges Aufruf- und/oder Bandbreitenkontingent pro Schlüssel. Der Schlüssel kann einen beliebigen Zeichenfolgenwert aufweisen und wird in der Regel über einen Richtlinienausdruck angegeben. Optional kann eine inkrementelle Bedingung hinzugefügt werden, um anzugeben, welche Anforderungen für das Kontingent gezählt werden sollen.  
  
 Weitere Informationen und Beispiele zu dieser Richtlinie finden Sie unter [Erweiterte Anforderungsbegrenzung mit Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Diese Richtlinie kann pro Richtliniendokument nur einmal verwendet werden.  
>   
>  [Richtlinienausdrücke](api-management-policy-expressions.md) können in keinem der Richtlinienattribute für diese Richtlinie werden.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird das Kontingent anhand der IP-Adresse des Aufrufers bestimmt.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"  
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"  
                      counter-key="@(context.Request.IpAddress)" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|quota|Stammelement.|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|bandwidth|Die maximale Gesamtanzahl von Kilobytes, die während des in der `renewal-period` angegebenen Zeitraums zulässig sind.|Es müssen entweder `calls` oder `bandwidth` oder beide Attribute zusammen angegeben werden.|N/V|  
|calls|Die maximale Gesamtanzahl von Aufrufen, die während des in der `renewal-period` angegebenen Zeitraums zulässig sind.|Es müssen entweder `calls` oder `bandwidth` oder beide Attribute zusammen angegeben werden.|N/V|  
|counter-key|Der Schlüssel, der für die Kontingentrichtlinie verwendet werden soll.|Ja|N/V|  
|increment-condition|Der boolesche Ausdruck, der angibt, ob die Anforderung für das Kontingent gezählt werden soll (`true`).|Nein|N/V|  
|renewal-period|Der Zeitraum in Sekunden, nach dem das Kontingent zurückgesetzt wird.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound  
  
-   **Richtlinienbereiche**: global, Produkt, API, Vorgang  
  
##  <a name="ValidateJWT"></a> JWT überprüfen  
 Die `validate-jwt`-Richtlinie erzwingt das Vorhandensein und die Gültigkeit eines JWT (Java Web Token), das entweder aus einem angegebenen HTTP-Header oder aus einem angegebenen Abfrageparameter extrahiert wurde.  
  
> [!IMPORTANT]
>  Die `validate-jwt`-Richtlinie erfordert, dass der über `exp` registrierte Anspruch in das JWT einbezogen wird, sofern nicht das `require-expiration-time`-Attribut angegeben und auf `false` festgelegt ist.  
> Die `validate-jwt`-Richtlinie unterstützt die Signaturalgorithmen HS256 und RS256. Für HS256 muss der Schlüssel inline innerhalb der Richtlinie in Base64-codiertem Format angegeben werden. Für RS256 muss der Schlüssel über einen Open ID-Konfigurationsendpunkt bereitgestellt werden.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<validate-jwt   
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"   
    failed-validation-httpcode="http status code to return on failure"   
    failed-validation-error-message="error message to return on failure"   
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"   
    clock-skew="allowed clock skew in seconds">  
  <issuer-signing-keys>  
    <key>base64 encoded signing key</key>  
    <!-- if there are multiple keys, then add additional key elements -->  
  </issuer-signing-keys>  
  <audiences>  
    <audience>audience string</audience>  
    <!-- if there are multiple possible audiences, then add additional audience elements -->  
  </audiences>  
  <issuers>  
    <issuer>issuer string</issuer>  
    <!-- if there are multiple possible issuers, then add additional issuer elements -->  
  </issuers>  
  <required-claims>  
    <claim name="name of the claim as it appears in the token" match="all|any">  
      <value>claim value as it is expected to appear in the token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of the configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a>Beispiele  
  
#### <a name="azure-mobile-services-token-validation"></a>Azure Mobile Services-Tokenüberprüfung  
  
```xml  
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">  
    <issuers>  
        <issuer>urn:microsoft:windows-azure:zumo</issuer>  
    </issuers>  
    <audiences>  
        <audience>Facebook</audience>  
    </audiences>  
    <issuer-signing-keys>  
        <zumo-master-key id="0">insert key here</zumo-master-key>  
    </issuer-signing-keys>  
</validate-jwt>  
```  
  
#### <a name="azure-active-directory-token-validation"></a>Azure Active Directory-Tokenüberprüfung  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />  
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  

  
#### <a name="azure-active-directory-b2c-token-validation"></a>Azure Active Directory B2C-Tokenüberprüfung  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  
  
#### <a name="authorize-access-to-operations-based-on-token-claims"></a>Autorisieren des Zugriffs auf Vorgänge basierend auf Tokenansprüchen  
 Dieses Beispiel zeigt die Verwendung der Richtlinie [JWT überprüfen](api-management-access-restriction-policies.md#ValidateJWT), um den Zugriff auf Vorgänge basierend auf Tokenansprüchen vorab zu autorisieren. Eine Demonstration der Konfiguration und Verwendung dieser Richtlinie finden Sie unter [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) (Cloud Cover-Folge 177 zu weiteren API Management-Funktionen mit Vlad Vinogradsky). Führen Sie einen schnellen Vorlauf bis 13:50 durch. Ab 18:50 finden Sie eine Demonstration des Aufrufs eines Vorgangs über das Entwicklerportal, sowohl mit dem als auch ohne das erforderliche Autorisierungstoken.  
  
```xml  
<!-- Copy the following snippet into the inbound section at the api (or higher) level to pre-authorize access to operations based on token claims -->  
<set-variable name="signingKey" value="insert signing key here" />  
<choose>  
  <when condition="@(context.Request.Method.Equals("patch",StringComparison.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="edit">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <when condition="@(new [] {"post", "put"}.Contains(context.Request.Method,StringComparer.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="create">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <otherwise>  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
    </validate-jwt>  
  </otherwise>  
</choose>  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|validate-jwt|Stammelement.|Ja|  
|audiences|Enthält eine Liste der zulässigen audience-Ansprüche, die im Token vorhanden sein können. Wenn mehrere audience-Werte vorhanden sind, wird jeder Wert ausprobiert, bis entweder alle verbraucht sind (in diesem Fall gibt es einen Überprüfungsfehler) oder ein Wert erfolgreich ist. Mindestens ein audience-Wert muss angegeben werden.|Nein|  
|issuer-signing-keys|Eine Liste von Base64-codierten Sicherheitsschlüsseln, die zum Überprüfen von signierten Token verwendet werden. Wenn mehrere Sicherheitsschlüssel vorhanden sind, wird jeder Schlüssel ausprobiert, bis entweder alle verbraucht sind (in diesem Fall gibt es einen Überprüfungsfehler) oder ein Schlüssel erfolgreich ist (hilfreich für ein Tokenrollover). Schlüsselelemente verfügen über ein optionales `id`-Attribut, das für einen Abgleich mit dem `kid`-Anspruch verwendet wird.|Nein|  
|issuers|Eine Liste der zulässigen Prinzipale, die das Token ausgestellt haben. Wenn mehrere issuer-Werte vorhanden sind, wird jeder Wert ausprobiert, bis entweder alle verbraucht sind (in diesem Fall gibt es einen Überprüfungsfehler) oder ein Wert erfolgreich ist.|Nein|  
|openid-config|Das Element, das zum Angeben eines kompatiblen Open ID-Konfigurationsendpunkts verwendet wird, von dem Signaturschlüssel und Aussteller abgerufen werden können.|Nein|  
|required-claims|Enthält eine Liste von Ansprüchen, deren Vorhandensein im Token erwartet wird, damit dieses als gültig eingestuft wird. Wenn das `match`-Attribut auf `all` festgelegt ist, muss jeder Anspruchswert in der Richtlinie im Token vorhanden sein, damit die Überprüfung erfolgreich ist. Wenn das `match`-Attribut auf `any` festgelegt ist, muss mindestens ein Anspruch im Token vorhanden sein, damit die Überprüfung erfolgreich ist.|Nein|  
|zumo-master-key|Der Hauptschlüssel für Token, die von Azure Mobile Services ausgestellt wurden.|Nein|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|clock-skew|Zeitspanne. Bietet einen kleinen Spielraum für den Fall, dass der Ablaufanspruch des Tokens im Token vorhanden ist und nach dem aktuellen Datum und der aktuellen Uhrzeit liegt.|Nein|0 Sekunden|  
|failed-validation-error-message|Die Fehlermeldung, die im HTTP-Antworttext zurückgegeben werden soll, wenn das JWT die Überprüfung nicht besteht. In dieser Meldung müssen alle Sonderzeichen ordnungsgemäß mit Escapezeichen versehen sein.|Nein|Die Standardfehlermeldung hängt vom Überprüfungsproblem ab, z.B. „JWT nicht vorhanden“.|  
|failed-validation-httpcode|Der HTTP-Statuscode, der zurückgegeben werden soll, wenn das JWT die Überprüfung nicht besteht.|Nein|401|  
|header-name|Der Name des HTTP-Headers, der das Token enthält.|Es muss entweder `header-name` oder `query-paremeter-name` angegeben werden, nicht jedoch beide Attribute.|N/V|  
|id|Das `id`-Attribut im `key`-Element ermöglicht Ihnen die Angabe der Zeichenfolge, die mit dem `kid`-Anspruch im Token (sofern vorhanden) verglichen wird, um den geeigneten Schlüssel für die Signaturüberprüfung zu ermitteln.|Nein|N/V|  
|match|Das `match`-Attribut im `claim`-Element gibt an, ob jeder Anspruchswert in der Richtlinie im Token vorhanden sein muss, damit die Überprüfung erfolgreich ist. Mögliche Werte:<br /><br /> -                          `all` – jeder Anspruchswert in der Richtlinie muss im Token vorhanden sein, damit die Überprüfung erfolgreich ist.<br /><br /> -                          `any` – mindestens ein Anspruchswert in der Richtlinie muss im Token vorhanden sein, damit die Überprüfung erfolgreich ist.|Nein|alle|  
|query-parameter-name|Der Name des Abfrageparameters, der das Token enthält.|Es muss entweder `header-name` oder `query-paremeter-name` angegeben werden, nicht jedoch beide Attribute.|N/V|  
|require-expiration-time|Boolescher Wert. Gibt an, ob ein Ablaufanspruch im Token erforderlich ist.|Nein|true|
|require-scheme|Der Name des Tokenschemas, z.B. „Bearer“. Wenn dieses Attribut festgelegt ist, stellt die Richtlinie sicher, das das angegebene Schema im Wert für den Autorisierungsheader vorhanden ist.|Nein|–|
|require-signed-tokens|Boolescher Wert. Gibt an, ob ein Token signiert sein muss.|Nein|true|  
|url|URL des Open ID-Konfigurationsendpunkts, von dem die Open ID-Konfigurationsmetadaten abgerufen werden können. Verwenden Sie für Azure Active Directory diese URL: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration`. Verwenden Sie dabei den Namen Ihres Verzeichnismandanten, z.B. `contoso.onmicrosoft.com`.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound  
  
-   **Richtlinienbereiche**: global, Produkt, API, Vorgang  
  
## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Arbeiten mit Richtlinien finden Sie unter [Richtlinien in Azure API Management](api-management-howto-policies.md).  

