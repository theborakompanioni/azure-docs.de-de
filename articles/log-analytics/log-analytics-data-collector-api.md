<properties
    pageTitle="HTTP-Datensammler-API von Log Analytics | Microsoft Azure"
    description="Mit der HTTP-Datensammler-API von Log Analytics können Sie dem Log Analytics-Repository über jeden Client, der die REST-API aufrufen kann, POST JSON-Daten hinzufügen. In diesem Artikel wird beschrieben, wie Sie die API verwenden. Außerdem finden Sie Beispiele zum Veröffentlichen von Daten mit verschiedenen Programmiersprachen."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="bwren"/>



# <a name="log-analytics-http-data-collector-api"></a>HTTP-Datensammler-API von Log Analytics

Mit der HTTP-Datensammler-API von Log Analytics können Sie dem Log Analytics-Repository über jeden Client, der die REST-API aufrufen kann, POST JSON-Daten (JavaScript Object Notation) hinzufügen. Auf diese Weise können Sie Daten aus Anwendungen von Drittanbietern oder Skripts senden, beispielsweise aus einem Runbook in Azure Automation.  

## <a name="create-a-request"></a>Erstellen einer Anforderung

In den folgenden beiden Tabellen werden die Attribute aufgeführt, die bei jeder Anforderung an die HTTP-Datensammler-API von Log Analytics erforderlich sind. Jedes Attribut wird weiter unten in diesem Artikel ausführlicher beschrieben.

### <a name="request-uri"></a>Anforderungs-URI

| Attribut | Eigenschaft |
|:--|:--|
| Methode | POST |
| URI | https://<WorkspaceID>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Content-Typ | Anwendung/json |

### <a name="request-uri-parameters"></a>URI-Parameter der Anforderung
| Parameter | Beschreibung |
|:--|:--|
| CustomerID  | Der eindeutige Bezeichner für den Microsoft Operations Management Suite-Arbeitsbereich |
| Ressource    | Der Name der API-Ressource: /api/logs |
| API-Version | Die Version der bei dieser Anforderung verwendeten API. Die aktuelle Version lautet 2016-04-01. |

### <a name="request-headers"></a>Anforderungsheader
| Header | Beschreibung |
|:--|:--|
| Autorisierung | Die Signatur der Autorisierung. Weiter unten in diesem Artikel erhalten Sie Informationen zum Erstellen eines HMAC-SHA256-Headers. |
| Log-Type | Geben Sie den Datensatztyp der übermittelten Daten an. Zurzeit unterstützt der Protokolltyp nur Buchstaben. Ziffern oder Sonderzeichen werden nicht unterstützt. |
| x-ms-date | Das Datum, zu dem die Anforderung verarbeitet wurde, im RFC 1123-Format |
| time-generated-field | Der Name eines Felds in den Daten, das den Zeitstempel des Datenelements enthält. Wenn Sie ein Feld angeben, wird dessen Inhalt für **TimeGenerated** verwendet. Wenn dieses Feld nicht angegeben wurde, ist der Standardwert für **TimeGenerated** die Zeit, zu der die Nachricht erfasst wurde. Der Inhalt des Nachrichtenfelds sollte das ISO 8601-Format (jjjj-mm-ttThh:mm:ssZ) einhalten. |


## <a name="authorization"></a>Autorisierung

Jede Anforderung an die HTTP-Datensammler-API von Log Analytics muss einen „Authorization“-Header enthalten. Um eine Anforderung zu authentifizieren, müssen Sie die Anforderung mit dem primären oder sekundären Schlüssel für den Arbeitsbereich, der die Anforderung gesendet hat, signieren. Übergeben Sie anschließend diese Signatur als Teil der Anforderung.   

Dies ist das Format für den „Authorization“-Header:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

Die *WorkspaceID* ist der eindeutige Bezeichner für den Operations Management Suite-Arbeitsbereich. Die *Signatur* ist ein [HMAC (Hash-based Message Authentication Code)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx), der aus der Anforderung erstellt und dann mit dem [SHA256-Algorithmus](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx) berechnet wird. Anschließend codieren Sie ihn mit der Base64-Codierung.

Verwenden Sie dieses Format zum Codieren der **SharedKey**-Signaturzeichenfolge:

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Dies ist ein Beispiel für eine Signaturzeichenfolge:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Codieren Sie anschließend die Signaturzeichenfolge, indem Sie den HMAC-SHA256-Algorithmus auf die UTF-8-codierte Zeichenfolge anwenden und dann das Ergebnis als Base64 codieren. Verwenden Sie dieses Format:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Die Beispiele in den nächsten Abschnitten enthalten Beispielcode, den Sie zum Erstellen eines „Authorization“-Headers verwenden können.

## <a name="request-body"></a>Anforderungstext

Der Text der Nachricht muss das JSON-Format aufweisen. Er muss einen oder mehrere Datensätze mit Paaren aus Eigenschaftenname und -wert in diesem Format enthalten:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Sie können mehrere Datensätze in einem Anforderungsbatch zusammenfassen. Verwenden Sie dazu das folgende Format. Alle Datensätze müssen denselben Datensatztyp aufweisen.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Datensatztyp und Eigenschaften

Sie definieren einen benutzerdefinierten Datensatztyp beim Senden von Daten über die HTTP-Datensammler-API von Log Analytics. Derzeit können Sie keine Daten in vorhandene Datensatztypen schreiben, die von anderen Datentypen und Lösungen erstellt wurden. Log Analytics liest die eingehenden Daten und erstellt dann die Eigenschaften, die den Datentypen der eingegebenen Werte entsprechen.

Jede Anforderung an die Log Analytics-API muss einen **Log-Type**-Header mit dem Namen für den Datensatztyp enthalten. Das Suffix **_CL** wird automatisch an den eingegebenen Namen angefügt, um es von anderen Protokolltypen wie z.B. benutzerdefinierten Protokollen zu unterscheiden. Wenn Sie beispielsweise den Namen **MyNewRecordType** eingeben, erstellt Log Analytics einen Datensatz vom Typ **MyNewRecordType_CL**. Dadurch wird sichergestellt, dass keine Konflikte zwischen benutzerdefinierten Typnamen und den im Lieferumfang von aktuellen oder zukünftiger Microsoft-Lösungen enthaltenen Typen auftreten.

Um den Datentyp einer Eigenschaft festzulegen, fügt Log Analytics ein Suffix an den Eigenschaftennamen an. Wenn eine Eigenschaft einen NULL-Wert enthält, ist die Eigenschaft nicht in diesem Datensatz enthalten. Diese Tabelle enthält den Datentyp der Eigenschaft und das entsprechende Suffix:

| Datentyp der Eigenschaft | Suffix |
|:--|:--|
| String    | _s |
| Boolean   | _b |
| Doppelt    | _d |
| Datum/Uhrzeit | _t |
| GUID      | _g |


Der Datentyp, den Log Analytics für die einzelnen Eigenschaften verwendet, hängt davon ab, ob der Datensatztyp für den neuen Datensatz bereits vorhanden ist.

- Wenn der Datensatztyp noch nicht vorhanden ist, erstellt Log Analytics einen neuen. Log Analytics verwendet den JSON-Typrückschluss, um den Datentyp für die einzelnen Eigenschaften des neuen Datensatzes zu ermitteln.
- Wenn der Datensatztyp vorhanden ist, erstellt Log Analytics einen neuen Datensatz basierend auf vorhandenen Eigenschaften. Wenn der Datentyp für eine Eigenschaft im neuen Datensatz nicht übereinstimmt und nicht in den vorhandenen Typ konvertiert werden kann oder wenn der Datensatz eine nicht vorhandene Eigenschaft enthält, erstellt Log Analytics eine neue Eigenschaft mit dem relevanten Suffix.

Bei diesem Übermittlungseintrag würde beispielsweise ein Datensatz mit den drei Eigenschaften **number_d**, **boolean_b** und **string_s** erstellt werden:

![Beispieldatensatz 1](media/log-analytics-data-collector-api/record-01.png)

Wenn Sie dann den folgenden Eintrag übermitteln, bei dem alle Werte als Zeichenfolgen formatiert sind, werden die Eigenschaften nicht geändert. Diese Werte können in vorhandene Datentypen konvertiert werden:

![Beispieldatensatz 2](media/log-analytics-data-collector-api/record-02.png)

Wenn Sie dann aber diese weitere Übermittlung durchführen, erstellt Log Analytics die neuen Eigenschaften **boolean_d** und **string_d**. Diese Werte können nicht konvertiert werden:

![Beispieldatensatz 3](media/log-analytics-data-collector-api/record-03.png)

Wenn Sie dann den folgenden Eintrag übermitteln, bevor der Datensatztyp erstellt wurde, würde Log Analytics einen Datensatz mit den drei Eigenschaften **number_s**, **boolean_s** und **string_s** erstellen. In diesem Eintrag sind alle Anfangswerte als Zeichenfolge formatiert:

![Beispieldatensatz 4](media/log-analytics-data-collector-api/record-04.png)

## <a name="return-codes"></a>Rückgabecodes

Der HTTP-Statuscode 202 bedeutet, dass die Anforderung für die Verarbeitung angenommen wurde, aber die Verarbeitung noch nicht abgeschlossen wurde. Dies gibt an, dass der Vorgang erfolgreich abgeschlossen wurde.

Diese Tabelle enthält den vollständigen Satz von Statuscodes, die vom Dienst zurückgegeben werden können:

| Code | Status | Fehlercode | Beschreibung |
|:--|:--|:--|:--|
| 202 | Zulässig |  | Die Anforderung wurde erfolgreich angenommen. |
| 400 | Ungültige Anforderung | InactiveCustomer | Der Arbeitsbereich wurde geschlossen. |
| 400 | Ungültige Anforderung | InvalidApiVersion | Die angegebene API-Version wurde vom Dienst nicht erkannt. |
| 400 | Ungültige Anforderung | InvalidCustomerId | Die angegebene Arbeitsbereichs-ID ist ungültig. |
| 400 | Ungültige Anforderung | InvalidDataFormat | Es wurde ein ungültiges JSON-Format übermittelt. Der Antworttext enthält eventuell weitere Informationen zum Beheben des Fehlers. |
| 400 | Ungültige Anforderung | InvalidLogType | Der angegebene Protokolltyp enthält Sonderzeichen oder Ziffern. |
| 400 | Ungültige Anforderung | MissingApiVersion | Die API-Version wurde nicht angegeben. |
| 400 | Ungültige Anforderung | MissingContentType | Der Inhaltstyp wurde nicht angegeben. |
| 400 | Ungültige Anforderung | MissingLogType | Der erforderliche Protokolltyp für den Wert wurde nicht angegeben. |
| 400 | Ungültige Anforderung | UnsupportedContentType | Der Inhaltstyp wurde nicht auf **application/json** festgelegt. |
| 403 | Verboten (403) | InvalidAuthorization | Der Dienst konnte die Anforderung nicht authentifizieren. Vergewissern Sie sich, dass die Arbeitsbereichs-ID und der Verbindungsschlüssel gültig sind. |
| 500 | Interner Serverfehler | UnspecifiedError | Auf dem Server wurde ein interner Fehler festgestellt.  Versuchen Sie die Anforderung erneut. |
| 503 | Dienst nicht verfügbar | ServiceUnavailable | Der Dienst kann derzeit keine Anforderungen empfangen. Bitte wiederholen Sie die Anforderung. |

## <a name="query-data"></a>Abfragen von Daten

Zum Abfragen von Daten, die über die HTTP-Datensammler-API von Log Analytics übermittelt wurden, suchen Sie nach Datensätzen mit einem **Typ**, der dem von Ihnen angegebenen **LogType**-Wert entspricht, und dem Suffix **_CL**. Wenn Sie z.B. **MyCustomLog** verwendet haben, werden alle Datensätze mit **Type=MyCustomLog_CL** zurückgegeben.


## <a name="sample-requests"></a>Beispielanforderungen

In den nächsten Abschnitten finden Sie Beispiele für das Senden von Daten an die HTTP-Datensammler-API von Log Analytics in verschiedenen Programmiersprachen.

Führen Sie für alle Beispiele folgende Schritte aus, um die Variablen für den „Authorization“-Header festzulegen:

1. Wählen Sie im Operations Management Suite-Portal die Kachel **Einstellungen** und dann die Registerkarte **Verbundene Quellen** aus.
2. Wählen Sie rechts neben **Arbeitsbereichs-ID** das Symbol „Kopieren“ aus, und fügen Sie die ID als Wert der Variablen für die **Kunden-ID** ein.
3. Wählen Sie rechts neben **Primärschlüssel** das Symbol „Kopieren“ aus, und fügen Sie die ID als Wert der Variablen für den **gemeinsam verwendeten Schlüssel** ein.

Sie können auch die Variablen für den Protokolltyp und die JSON-Daten ändern.

### <a name="powershell-sample"></a>PowerShell-Beispiel

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c#-sample"></a>C#-Beispiel

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Python-Beispiel

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, string_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Nächste Schritte

- Verwenden des [Ansicht-Designers](log-analytics-view-designer.md) für das Erstellen benutzerdefinierter Ansichten der von Ihnen übermittelten Daten.



<!--HONumber=Oct16_HO2-->


