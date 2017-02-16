---
title: Azure API Management-Transformationsrichtlinien | Microsoft-Dokumentation
description: "Erfahren Sie mehr über die Transformationsrichtlinien, die für die Verwendung in Azure API Management verfügbar sind."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7406a8ce-5f9c-4fae-9b0f-e574befb2ee9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: 77fd7b5b339a8ede8a297bec96f91f0a243cc18d
ms.openlocfilehash: 6fc751d956eee68d2bbab50b0f25b928759c9d32

---
# <a name="api-management-transformation-policies"></a>Azure API Management-Transformationsrichtlinien
Dieses Thema enthält eine Referenz für die folgenden API Management-Richtlinien. Weitere Informationen zum Hinzufügen und Konfigurieren von Richtlinien finden Sie unter [Richtlinien in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="a-nametransformationpoliciesa-transformation-policies"></a><a name="TransformationPolicies"></a> Transformationsrichtlinien  
  
-   [JSON in XML konvertieren](api-management-transformation-policies.md#ConvertJSONtoXML) – Konvertiert den Anforderungs- oder Antworttext von JSON in XML.  
  
-   [XML in JSON konvertieren](api-management-transformation-policies.md#ConvertXMLtoJSON) – Konvertiert den Anforderungs- oder Antworttext von XML in JSON.  
  
-   [Zeichenfolge in Text ersetzen](api-management-transformation-policies.md#Findandreplacestringinbody) – Sucht nach einer Zeichenfolge in Antwort oder Anforderung und ersetzt diese durch eine andere Teilzeichenfolge.  
  
-   [URLs in Inhalt maskieren](api-management-transformation-policies.md#MaskURLSContent) – Ändert (maskiert) Links im Antworttext, sodass diese über das Gateway auf den äquivalenten Link zeigen.  
  
-   [Back-End-Dienst festlegen](api-management-transformation-policies.md#SetBackendService) – Ändert den Back-End-Dienst für eine eingehende Anforderung.  
  
-   [Text festlegen](api-management-transformation-policies.md#SetBody) – Legt den Nachrichtentext für eingehende und ausgehende Anforderungen fest.  
  
-   [HTTP-Header setzen](api-management-transformation-policies.md#SetHTTPheader) – Weist einem vorhandenen Antwort- und/oder Anforderungsheader einen Wert zu oder fügt einen neuen Antwort- und/oder Anforderungsheader hinzu.  
  
-   [Abfrageparameter setzen](api-management-transformation-policies.md#SetQueryStringParameter) – Fügt Abfrageparameter hinzu, löscht diese oder ersetzt deren Werte.  
  
-   [URL umschreiben](api-management-transformation-policies.md#RewriteURL) – Konvertiert eine Anforderung-URL von der öffentlichen Form in die vom Webdienst erwartete Form.  
  
-   [XML mithilfe von XSLT transformieren](api-management-transformation-policies.md#XSLTransform) – Wendet eine XSL-Transformation auf XML im Anforderungs- oder Antworttext an.  
  
##  <a name="a-nameconvertjsontoxmla-convert-json-to-xml"></a><a name="ConvertJSONtoXML"></a> JSON in XML konvertieren  
 Die Richtlinie `json-to-xml` konvertiert einen Anforderungs- oder Antworttext von JSON in XML.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <json-to-xml apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|json-to-xml|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|apply|Das Attribut muss auf einen der folgenden Werte festgelegt werden.<br /><br /> – always – Die Konvertierung immer anwenden.<br />– content-type-json – Nur konvertieren, wenn der Content-Type-Header der Antwort das Vorhandensein von JSON angibt.|Ja|N/V|  
|consider-accept-header|Das Attribut muss auf einen der folgenden Werte festgelegt werden.<br /><br /> – true – Die Konvertierung anwenden, wenn JSON im Accept-Header der Anforderung angefordert wird.<br />– false – Die Konvertierung immer anwenden.|Nein|true|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, on-error  
  
-   **Richtlinienbereiche:** global, Produkt, API, Vorgang  
  
##  <a name="a-nameconvertxmltojsona-convert-xml-to-json"></a><a name="ConvertXMLtoJSON"></a> XML in JSON konvertieren  
 Die Richtlinie `xml-to-json` konvertiert einen Anforderungs- oder Antworttext von XML in JSON. Diese Richtlinie kann verwendet werden, um APIs basierend auf reinen XML-Back-End-Webdiensten zu aktualisieren.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <xml-to-json kind="direct" apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|xml-to-json|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|kind|Das Attribut muss auf einen der folgenden Werte festgelegt werden.<br /><br /> – javascript-friendly – Die konvertierte JSON hat ein für JavaScript-Entwickler verständliches Format.<br />– direct – Die konvertierte JSON spiegelt die Struktur des ursprünglichen XML-Dokuments wider.|Ja|N/V|  
|apply|Das Attribut muss auf einen der folgenden Werte festgelegt werden.<br /><br /> – always – Immer konvertieren.<br />– content-type-xml – Nur konvertieren, wenn der Content-Type-Header der Antwort das Vorhandensein von XML angibt.|Ja|N/V|  
|consider-accept-header|Das Attribut muss auf einen der folgenden Werte festgelegt werden.<br /><br /> – true – Die Konvertierung anwenden, wenn XML im Accept-Header der Anforderung angefordert wird.<br />– false – Die Konvertierung immer anwenden.|Nein|true|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, on-error  
  
-   **Richtlinienbereiche:** global, Produkt, API, Vorgang  
  
##  <a name="a-namefindandreplacestringinbodya-find-and-replace-string-in-body"></a><a name="Findandreplacestringinbody"></a> Zeichenfolge in Text ersetzen  
 Die Richtlinie `find-and-replace` sucht nach einer Teilzeichenfolge in der Antwort oder Anforderung und ersetzt diese durch eine andere Teilzeichenfolge.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<find-and-replace from="what to replace" to="replacement" />  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|find-and-replace|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|Aus|Die zu suchende Zeichenfolge.|Ja|N/V|  
|To|Die Ersatzzeichenfolge. Geben Sie eine leere Ersatzzeichenfolge an, um die Suchzeichenfolge zu entfernen.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** global, Produkt, API, Vorgang  
  
##  <a name="a-namemaskurlscontenta-mask-urls-in-content"></a><a name="MaskURLSContent"></a> URLs in Inhalt maskieren  
 Die Richtlinie `redirect-content-urls` ändert (maskiert) Links im Antworttext, sodass diese über das Gateway auf den äquivalenten Link zeigen. Verwenden Sie sie im Abschnitt „outbound“, um Links im Antworttext so zu ändern, dass sie auf das Gateway zeigen. Verwenden Sie sie im Abschnitt „inbound“ für den entgegengesetzten Effekt.  
  
> [!NOTE]
>  Diese Richtlinie ändert keine Headerwerte wie z.B. `Location`-Header. Um Headerwerte zu ändern, verwenden Sie die Richtlinie [set-header](api-management-transformation-policies.md#SetHTTPheader).  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|redirect-content-urls|Stammelement|Ja|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound  
  
-   **Richtlinienbereiche:** global, Produkt, API, Vorgang  
  
##  <a name="a-namesetbackendservicea-set-backend-service"></a><a name="SetBackendService"></a> Back-End-Dienst festlegen  
 Verwenden Sie die Richtlinie `set-backend-service`, um eine eingehende Anforderung an einen anderen Back-End umzuleiten, als in den API-Einstellungen für diesen Vorgang angegeben ist. Diese Richtlinie ändert die Basis-URL des Back-End-Diensts der eingehenden Anforderung in die in der Richtlinie angegebene.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<set-backend-service base-url="base URL of the backend service" />  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<policies>  
    <inbound>  
        <choose>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2013-05")">  
                <set-backend-service base-url="http://contoso.com/api/8.2/" />  
            </when>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2014-03")">  
                <set-backend-service base-url="http://contoso.com/api/9.1/" />  
            </when>  
        </choose>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
 In diesem Beispiel leitet die Richtlinie zum Festlegen des Back-End-Diensts Anforderungen auf Grundlage des in der Abfragezeichenfolge übergebenen Versionswerts an einen anderen Back-End-Dienst um, als in der API angegeben ist.  
  
 Zunächst wird die Basis-URL für den Back-End-Dienst aus den API-Einstellungen abgeleitet. Daher wird die Anforderungs-URL `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` zu `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef`. Dabei ist `http://contoso.com/api/10.4/` die Back-End-Dienst-URL, die in den API-Einstellungen angegeben ist.  
  
 Wenn die Richtlinienanweisung [<choose\>](api-management-advanced-policies.md#choose) angewendet wird, kann sich die Basis-URL für den Back-End-Dienst möglicherweise erneut in `http://contoso.com/api/8.2` oder `http://contoso.com/api/9.1` ändern, abhängig vom Wert des Abfrageparameters für die Version in der Anforderung. Wenn der Wert beispielsweise `"2013-15"` ist, ist die endgültige Anforderungs-URL `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.  
  
 Wenn weitere Transformationen der Anforderung gewünscht sind, können andere [Transformationsrichtlinien](api-management-transformation-policies.md#TransformationPolicies) verwendet werden. Um beispielsweise den Abfrageparameter für die Version zu entfernen, da die Anforderung jetzt an einen versionsspezifischen Back-End weitergeleitet wird, kann die Richtlinie [Abfrageparameter setzen](api-management-transformation-policies.md#SetQueryStringParameter) verwendet werden, um das jetzt redundante Versionsattribut zu entfernen.  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|set-backend-service|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|base-url|Neue Basis-URL für den Back-End-Dienst.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, backend  
  
-   **Richtlinienbereiche:** global, Produkt, API, Vorgang  
  
##  <a name="a-namesetbodya-set-body"></a><a name="SetBody"></a> Text festlegen  
 Verwenden Sie die Richtlinie `set-body`, um den Nachrichtentext für eingehende und ausgehende Anforderungen festzulegen. Um auf den Nachrichtentext zuzugreifen, können Sie die Eigenschaft `context.Request.Body` oder `context.Response.Body` verwenden, je nachdem, ob sich die Richtlinie im Abschnitt „inbound“ oder „outbound“ befindet.  
  
> [!IMPORTANT]
>  Beachten Sie, dass beim Zugriff auf den Nachrichtentext mit `context.Request.Body` oder `context.Response.Body` der ursprüngliche Nachrichtentext standardmäßig verloren geht und festgelegt werden muss, indem der Text wieder im Ausdruck zurückgegeben wird. Um den Nachrichtentext zu erhalten, legen Sie den Parameter `preserveContent` auf `true` fest, wenn Sie auf die Nachricht zugreifen. Wenn `preserveContent` auf `true` festgelegt ist und ein anderer Text vom Ausdruck zurückgegeben wird, wird der zurückgegebene Text verwendet.  
>   
>  Beachten Sie Folgendes, wenn Sie die Richtlinie `set-body` verwenden.  
>   
>  -   Wenn Sie die Richtlinie `set-body` verwenden, um einen neuen oder aktualisierten Text zurückzugeben, müssen Sie `preserveContent` nicht auf `true` festlegen, da Sie den Inhalt des neuen Texts explizit angeben.  
> -   Das Beibehalten des Inhalts einer Antwort in der eingehenden Pipeline ergibt keinen Sinn, da noch keine Antwort vorliegt.  
> -   Das Beibehalten des Inhalts einer Anforderung in der ausgehenden Pipeline ergibt keinen Sinn, da die Anforderung zu diesem Zeitpunkt bereits an den Back-End gesendet wurde.  
> -   Falls diese Richtlinie verwendet wird, wenn kein Nachrichtentext vorhanden ist, z.B. in einem eingehenden GET-Vorgang, wird eine Ausnahme ausgelöst.  
  
 Weitere Informationen finden Sie in den Abschnitten zu `context.Request.Body`, `context.Response.Body` und `IMessage` in der Tabelle [Kontextvariable](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a>Beispiele  
  
#### <a name="literal-text-example"></a>Beispiel für konstanten Text  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-the-body-as-a-string"></a>Beispiel für den Zugriff auf den Text als Zeichenfolge  
  
```xml  
<set-body>  
@{   
    string inBody = context.Request.Body.As<string>(preserveContent: true);   
    if (inBody[0] =='c') {   
        inBody[0] = 'm';   
    }   
    return inBody;   
}  
</set-body>  
```  
  
#### <a name="example-accessing-the-body-as-a-jobject"></a>Beispiel für den Zugriff auf den Text als JObject  
  
```xml  
<set-body>   
@{   
    JObject inBody = context.Request.Body.As<JObject>();   
    if (inBody.attribute == <tag>) {   
        inBody[0] = 'm';   
    }   
    return inBody.ToString();   
}   
</set-body>  
  
```  
  
#### <a name="filter-response-based-on-product"></a>Filtern der Antwort basierend auf dem Produkt  
 In diesem Beispiel wird gezeigt, wie Inhalte gefiltert werden, indem Datenelemente aus der über den Back-End-Dienst empfangenen Antwort entfernt werden, wenn das Produkt `Starter` verwendet wird. Eine Demonstration der Konfiguration und Verwendung dieser Richtlinie finden Sie unter [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) (Cloud Cover-Episode 177 zu weiteren API Management-Funktionen mit Vlad Vinogradsky). Führen Sie einen schnellen Vorlauf bis 34:30 durch. Beginnen Sie bei 31:50, um eine Übersicht über die [API „The Dark Sky Forecast“](https://developer.forecast.io/) zu erhalten, die für diese Demo verwendet wird.  
  
```xml  
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->  
<choose>  
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">  
    <set-body>@{  
        var response = context.Response.Body.As<JObject>();  
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {  
          response.Property (key).Remove ();  
        }  
        return response.ToString();  
      }  
    </set-body>  
  </when>  
</choose>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|set-body|Stammelement Enthält den Text oder einen Ausdruck, der einen Text zurückgibt.|Ja|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend  
  
-   **Richtlinienbereiche:** global, Produkt, API, Vorgang  
  
##  <a name="a-namesethttpheadera-set-http-header"></a><a name="SetHTTPheader"></a> HTTP-Header setzen  
 Die Richtlinie `set-header` weist einem vorhandenen Antwort- und/oder Anforderungsheader einen Wert zu oder fügt einen neuen Antwort- und/oder Anforderungsheader hinzu.  
  
 Fügt eine Liste von HTTP-Headern in eine HTTP-Nachricht ein. In eingehenden Pipelines setzt diese Richtlinie die HTTP-Header für die Anforderung, die an den Zieldienst übergeben wird. In ausgehenden Pipelines legt diese Richtlinie die HTTP-Header für die Antwort fest, die an den Client des Gateways gesendet wird.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with the same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a>Beispiele  
  
#### <a name="example"></a>Beispiel  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a>Weiterleiten von Kontextinformationen an den Back-End-Dienst  
 In diesem Beispiel wird gezeigt, wie die Richtlinie auf API-Ebene angewendet wird, um dem Back-End-Dienst Kontextinformationen bereitzustellen. Eine Demonstration der Konfiguration und Verwendung dieser Richtlinie finden Sie unter [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) (Cloud Cover-Episode 177 zu weiteren API Management-Funktionen mit Vlad Vinogradsky). Führen Sie einen schnellen Vorlauf bis 10:30 durch. Bei 12:10 wird das Aufrufen eines Vorgangs im Entwicklerportal gezeigt, sodass Sie die Richtlinie bei der Arbeit sehen können.  
  
```xml  
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 Weitere Informationen finden Sie unter [Richtlinienausdrücke](api-management-policy-expressions.md) und [Kontextvariable](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|set-header|Stammelement|Ja|  
|value|Der Wert für den zu setzenden Header. Fügen Sie bei mehreren Headern mit dem gleichen Namen weitere `value`-Elemente hinzu.|Ja|  
  
### <a name="properties"></a>Eigenschaften  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|exists-action|Gibt die auszuführende Aktion an, wenn der Header bereits vorhanden ist. Dieses Attribut muss einen der folgenden Werte aufweisen.<br /><br /> – override – Ersetzt den Wert des vorhandenen Headers.<br />– skip – Ersetzt den vorhandenen Headerwert nicht.<br />– append – Fügt den Wert an den vorhandenen Headerwert an.<br />– delete – Entfernt den Header aus der Anforderung.<br /><br /> Bei `override` führt die Auflistung mehrerer Einträge mit demselben Namen dazu, dass der Header gemäß aller Einträge festgelegt wird (die mehrfach aufgeführt sind); nur die aufgelisteten Werte werden im Ergebnis festgelegt.|Nein|override|  
|Name|Der Name des zu setzenden Headers.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** global, Produkt, API, Vorgang  
  
##  <a name="a-namesetquerystringparametera-set-query-string-parameter"></a><a name="SetQueryStringParameter"></a> Abfrageparameter setzen  
 Die Richtlinie `set-query-parameter` fügt Abfrageparameter von Anforderungen hinzu, löscht diese oder ersetzt deren Werte. Mit dieser Richtlinie können Sie erwartete Abfrageparameter an den Back-End-Dienst übergeben, die optional sind oder in der Anforderung nie vorkommen.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with the same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a>Beispiele  
  
#### <a name="example"></a>Beispiel  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with the same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a>Weiterleiten von Kontextinformationen an den Back-End-Dienst  
 In diesem Beispiel wird gezeigt, wie die Richtlinie auf API-Ebene angewendet wird, um dem Back-End-Dienst Kontextinformationen bereitzustellen. Eine Demonstration der Konfiguration und Verwendung dieser Richtlinie finden Sie unter [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) (Cloud Cover-Episode 177 zu weiteren API Management-Funktionen mit Vlad Vinogradsky). Führen Sie einen schnellen Vorlauf bis 10:30 durch. Bei 12:10 wird das Aufrufen eines Vorgangs im Entwicklerportal gezeigt, sodass Sie die Richtlinie bei der Arbeit sehen können.  
  
```xml  
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 Weitere Informationen finden Sie unter [Richtlinienausdrücke](api-management-policy-expressions.md) und [Kontextvariable](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|set-query-parameter|Stammelement|Ja|  
|value|Gibt den Wert des festzulegenden Abfrageparameters an. Fügen Sie bei mehreren Abfrageparametern mit dem gleichen Namen weitere `value`-Elemente hinzu.|Ja|  
  
### <a name="properties"></a>Eigenschaften  
  
|Name|Beschreibung|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|exists-action|Gibt die auszuführende Aktion an, wenn ein Abfrageparameter bereits vorhanden ist. Dieses Attribut muss einen der folgenden Werte aufweisen.<br /><br /> – override – Ersetzt den Wert des vorhandenen Parameters.<br />– skip – Ersetzt den vorhandenen Abfrageparameterwert nicht.<br />– append – Fügt den Wert an den vorhandenen Abfrageparameterwert an.<br />– delete – Entfernt den Abfrageparameter aus der Anforderung.<br /><br /> Bei `override` führt die Auflistung mehrerer Einträge mit demselben Namen dazu, dass der Abfrageparameter gemäß aller Einträge festgelegt wird (die mehrfach aufgeführt sind); nur die aufgelisteten Werte werden im Ergebnis festgelegt.|Nein|override|  
|Name|Gibt den Namen des festzulegenden Abfrageparameters an.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, backend  
  
-   **Richtlinienbereiche:** global, Produkt, API, Vorgang  
  
##  <a name="a-namerewriteurla-rewrite-url"></a><a name="RewriteURL"></a> URL umschreiben  
 Die Richtlinie `rewrite-uri` konvertiert eine Anforderungs-URL von der öffentlichen Form in die vom Webdienst erwartete Form, wie im folgenden Beispiel gezeigt.  
  
-   Öffentliche URL: `http://api.example.com/storenumber/ordernumber`  
  
-   Anforderungs-URL: `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`  
  
 Diese Richtlinie kann verwendet werden, um benutzer- oder browserfreundliche URLs in das vom Webdienst erwartete Format umzuwandeln. Diese Richtlinie muss nur angewendet werden, wenn ein alternatives URL-Format genutzt wird, beispielsweise bereinigte URLs, RESTful-URLs, benutzerfreundliche URLs oder SEO-freundliche URLs, die rein strukturelle URLs sind, die keine Abfragezeichenfolge enthalten, sondern nur den Pfad der Ressource (nach Schema und Berechtigung). Dies geschieht oft aus Gründen der Ästhetik, Benutzerfreundlichkeit oder zur Suchmaschinenoptimierung (SEO).  
  
> [!NOTE]
>  Sie können mit der Richtlinie nur Abfrageparameter hinzufügen. Sie können keine zusätzlichen Vorlagenpfadparameter in die umgeschriebene URL einfügen.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<rewrite-uri template="uri template" />  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/v2/US/hardware/{storenumber}&{ordernumber}?City=city&State=state" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|rewrite-uri|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|-------------|  
|Vorlage|Die eigentliche Webdienst-URL mit allen Abfrageparametern.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound  
  
-   **Richtlinienbereiche:** Produkt, API, Vorgang  
  
##  <a name="a-namexsltransforma-transform-xml-using-an-xslt"></a><a name="XSLTransform"></a> XML mithilfe von XSLT transformieren  
 Die Richtlinie `Transform XML using an XSLT` wendet eine XSL-Transformation auf XML im Anforderungs- oder Antworttext an.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<xsl-transform>  
    <parameter name="User-Agent">@(context.Request.Headers.GetValueOrDefault("User-Agent","non-specified"))</parameter>  
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
        <xsl:output method="xml" indent="yes" />  
        <xsl:param name="User-Agent" />  
        <xsl:template match="* | @* | node()">  
            <xsl:copy>  
                <xsl:if test="self::* and not(parent::*)">  
                    <xsl:attribute name="User-Agent">  
                        <xsl:value-of select="$User-Agent" />  
                    </xsl:attribute>  
                </xsl:if>  
                <xsl:apply-templates select="* | @* | node()" />  
            </xsl:copy>  
        </xsl:template>  
    </xsl:stylesheet>  
  </xsl-transform>  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<policies>  
  <inbound>  
      <base />  
  </inbound>  
  <outbound>  
      <base />  
      <xsl-transform>  
        <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
            <xsl:output omit-xml-declaration="yes" method="xml" indent="yes" />  
            <!-- Copy all nodes directly-->  
            <xsl:template match="node()| @*|*">  
                <xsl:copy>  
                    <xsl:apply-templates select="@* | node()|*" />  
                </xsl:copy>  
            </xsl:template>  
        </xsl:stylesheet>  
    </xsl-transform>  
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemente  
  
|Name|Beschreibung|Erforderlich|  
|----------|-----------------|--------------|  
|xsl-transform|Stammelement|Ja|  
|Parameter|Wird verwendet, um in der Transformation verwendete Variablen zu definieren.|Nein|  
|xsl:stylesheet|Stylesheet-Stammelement. Für alle darunter definierten Elemente und Attribute gilt die standardmäßige [XSLT-Spezifikation](http://www.w3.org/TR/xslt)|Ja|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound  
  
-   **Richtlinienbereiche:** global, Produkt, API, Vorgang  
  
## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Arbeiten mit Richtlinien finden Sie unter [Richtlinien in Azure API Management](api-management-howto-policies.md).  


<!--HONumber=Jan17_HO2-->


