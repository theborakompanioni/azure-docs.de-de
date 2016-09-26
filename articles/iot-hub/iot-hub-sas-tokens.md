<properties
 pageTitle="Generieren von IoT Hub-Sicherheitstoken | Microsoft Azure"
 description="Beschreibung der verschiedenen Typen von Sicherheitstoken (z.B. SAS und X.509), die von IoT Hub verwendet werden, und ihre Erstellung."
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="06/07/2016"
 ms.author="elioda"/>

# Verwenden von IoT Hub-Sicherheitstoken und X.509-Zertifikaten

IoT Hub verwendet Sicherheitstoken zum Authentifizieren von Geräten und Diensten, um das Senden von Schlüsseln über das Netzwerk zu vermeiden. Darüber hinaus sind Sicherheitstoken im Hinblick auf Gültigkeitszeitraum und Bereich beschränkt. [Azure IoT Hub SDKs][lnk-apis-sdks] generieren Token automatisch, ohne dass eine spezielle Konfiguration erforderlich ist. In einigen Szenarien muss der Benutzer Sicherheitstoken jedoch direkt generieren und verwenden. Dazu gehören die direkte Verwendung von MQTT-, AMQP- oder HTTP-Oberflächen oder die Implementierung des Tokendienstmusters, wie in der [IoT Hub-Anleitung][lnk-guidance-security] erläutert.

IoT Hub ermöglicht Geräten darüber hinaus die Authentifizierung bei IoT Hub mithilfe von X.509-Zertifikaten. IoT Hub unterstützt die X.509-basierte Authentifizierung für Geräte über die Protokolle AMQP, AMQP über WebSockets und HTTP.

Dieser Artikel beschreibt Folgendes:

* Das Format von Sicherheitstoken und deren Erstellung.
* Die wichtigsten Anwendungsfälle für die Verwendung von Sicherheitstoken zum Authentifizieren von Geräten und Back-End-Diensten.
* Unterstützte X.509-Zertifikate für die Geräteauthentifizierung
* Registrierungsprozess für ein an ein bestimmtes Gerät gebundenes X.509-Clientzertifikat
* Runtime-Workflow zwischen Gerät und IoT Hub unter Verwendung eines X.509-Clientzertifikats für die Authentifizierung


## Struktur von Sicherheitstoken
Sie verwenden Sicherheitstoken, um zeitlich begrenzten Zugriff auf Geräte und Dienste für bestimmte Funktionen in IoT Hub zu gewähren. Um sicherzustellen, dass nur autorisierte Geräte und Dienste eine Verbindung herstellen können, müssen Sicherheitstoken mit einem SAS-Richtlinienschlüssel oder einem symmetrischen Schlüssel, der mit einer Geräteidentität in der Identitätsregistrierung gespeichert ist, signiert werden.

Ein mit einem SAS-Richtlinienschlüssel signiertes Token gewährt Zugriff auf alle Funktionen, die den SAS-Richtlinienberechtigungen zugeordnet sind. Informationen dazu finden Sie im [Abschnitt über Sicherheit im Entwicklungsleitfaden für IoT Hub][lnk-devguide-security]. Dagegen erteilt ein mit einem symmetrischen Schlüssel der Geräteidentität signiertes Token nur die **DeviceConnect**-Berechtigung für die zugeordnete Geräteidentität.

Das Sicherheitstoken weist das folgende Format auf:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Es werden die folgenden Werte erwartet:

| Wert | Beschreibung |
| ----- | ----------- |
| {signature} | Eine HMAC-SHA256-Signaturzeichenfolge in folgendem Format: `{URL-encoded-resourceURI} + "\n" + expiry`. **Wichtig:** Der Schlüssel wird aus Base64 decodiert und als Schlüssel für die HMAC-SHA256-Berechnung verwendet. |
| {resourceURI} | Das URI-Präfix (nach Segment) der Endpunkte, auf die mit diesem Token zugegriffen werden kann, beginnend mit dem Hostnamen des IoT Hubs (kein Protokoll). Beispiel: `myHub.azure-devices.net/devices/device1` |
| {expiry} | UTF8-Zeichenfolge, dargestellt als die Anzahl von Sekunden seit dem 1. Januar 1970 um 00:00:00 UTC. |
| {URL-encoded-resourceURI} | URL-Codierung des Ressourcen-URI (beides in Kleinbuchstaben) |
| {policyName} | Der Name der gemeinsam genutzten Zugriffsrichtlinie, auf die dieses Token verweist. Nicht vorhanden, wenn Token auf Anmeldeinformationen der Geräteregistrierung verweisen. |

**Hinweis zum Präfix**: Das URI-Präfix wird nach Segment, nicht nach Zeichen berechnet. Beispielsweise ist `/a/b` ein Präfix für `/a/b/c`, aber nicht für `/a/bc`.

Dies ist eine Node-Funktion, die das Token aus den Eingaben `resourceUri, signingKey, policyName, expiresInMins` berechnet. Im nächsten Abschnitten wird erläutert, wie die verschiedenen Eingaben für die verschiedenen Anwendungsfälle für Token initialisiert werden.

    var crypto = require('crypto');

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // using crypto
        var decodedPassword = new Buffer(signingKey, 'base64').toString('binary');
        const hmac = crypto.createHmac('sha256', decodedPassword);
        hmac.update(toSign);
        var base64signature = hmac.digest('base64');
        var base64UriEncoded = encodeURIComponent(base64signature);

        // construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        // console.log("signature:" + token);
        return token;
    };
 
 Hier ist der entsprechende Python-Code zum Vergleich:
 
    from base64 import b64encode, b64decode
    from hashlib import sha256
    from hmac import HMAC
    from urllib import urlencode
    
    def generate_sas_token(uri, key, policy_name='device', expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % (uri, int(ttl))
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())
     
        return 'SharedAccessSignature ' + urlencode({
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl)),
            'skn': policy_name
        })

> [AZURE.NOTE] Da der Gültigkeitszeitraum des Tokens auf dem IoT Hub-Computer überprüft wird, ist es wichtig, dass die Abweichung der Uhr des Computers, auf dem das Token generiert wird, minimal ist.

## Verwenden von SAS-Token als Gerät

Es gibt zwei Methoden zum Abrufen von **DeviceConnect**-Berechtigungen mit IoT Hub mit Sicherheitstoken: Identitätsschlüssel für das Gerät oder SAS-Richtlinienschlüssel.

Darüber hinaus ist zu beachten, dass alle Funktionen, auf die auf Geräten zugegriffen werden kann, für Endpunkte mit dem Präfix `/devices/{deviceId}` absichtlich verfügbar gemacht werden.

> [AZURE.IMPORTANT] Die einzige Möglichkeit, dass IoT Hub ein bestimmtes Gerät authentifiziert, ist durch die Verwendung des symmetrischen Schlüssels der Geräteidentität. Wenn eine SAS-Richtlinie verwendet wird, um auf Gerätefunktionen zuzugreifen, muss die Lösung die Komponente, die das Sicherheitstoken ausgibt, als vertrauenswürdige Unterkomponente ansehen.

Die geräteseitigen Endpunkte sind (unabhängig vom Protokoll):

| Endpunkt | Funktionalität |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Senden von Gerät-an-Cloud-Nachrichten. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Empfangen von Cloud-an-Gerät-Nachrichten. |

### Verwenden eines symmetrischen Schlüssels in der Identitätsregistrierung

Wenn Sie den symmetrischen Schlüssel einer Geräteidentität zum Generieren eines Tokens verwenden, wird das policyName-Element (`skn`) des Tokens nicht angegeben.

Beispielsweise muss ein Token, das für den Zugriff auf alle Gerätefunktionen erstellt wird, die folgenden Parameter aufweisen:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`
* Signaturschlüssel: ein symmetrischer Schlüssel für die `{device id}`-Identität
* Kein Richtlinienname
* Eine Ablaufzeit

Ein Beispiel mit der oben genannten Node-Funktion wäre:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Das Ergebnis, das Zugriff auf alle Funktionen für „device1“ gewährt wird, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Es ist möglich, ein sicheres Token mit dem .NET-Tool [Device Explorer][lnk-device-explorer] zu generieren.

### Verwenden einer SAS-Richtlinie

Beim Erstellen eines Tokens aus einer SAS-Richtlinie muss das Feld für den Richtliniennamen `skn` auf den Namen der verwendeten Richtlinie festgelegt werden. Darüber hinaus muss die Richtlinie die **DeviceConnect**-Berechtigung erteilen.

Die beiden wichtigsten Szenarien für die Verwendung von SAS-Richtlinien für den Zugriff auf Gerätefunktionen sind:

* [Protokollgateways für Clouds][lnk-azure-protocol-gateway]
* [Tokendienste][lnk-devguide-security], die verwendet werden, um benutzerdefinierte Authentifizierungsschemas zu implementieren

Da die SAS-Richtlinie potenziell Zugriff gewähren kann, um als ein beliebiges Gerät eine Verbindung herzustellen, ist es wichtig, beim Erstellen von Sicherheitstoken den richtigen Ressourcen-URI zu verwenden. Dies ist besonders wichtig für Tokendienste, die das Token mit dem Ressourcen-URI auf ein bestimmtes Gerät festlegen müssen. Dieser Punkt ist weniger relevant für Protokollgateways, da sie Datenverkehr bereits für alle Geräte vermitteln.

Beispielsweise würde ein Tokendienst, der die vorab erstellte SAS-Richtlinie mit dem Namen **device** verwendet, ein Token mit den folgenden Parametern erstellen:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`
* Signaturschlüssel: einer der Schlüssel der `device`-Richtlinie
* Richtlinienname: `device`
* Eine Ablaufzeit

Ein Beispiel mit der oben genannten Node-Funktion wäre:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Das Ergebnis, das Zugriff auf alle Funktionen für „device1“ gewährt wird, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Ein Protokollgateway könnte das gleiche Token für alle Geräte verwenden, indem einfach der Ressourcen-URI auf `myhub.azure-devices.net/devices` festgelegt wird.

## Verwenden der Sicherheitstoken von Dienstkomponenten

Dienstkomponenten können nur Sicherheitstoken mithilfe von SAS-Richtlinien generieren, die die entsprechenden Berechtigungen erteilen, wie im [Abschnitt über Sicherheit im Entwicklungsleitfaden für IoT Hub][lnk-devguide-security] beschrieben.

Dies sind die Dienstfunktionen, die für die Endpunkte verfügbar gemacht werden:

| Endpunkt | Funktionalität |
| ----- | ----------- |
| `{iot hub host name}/devices` | Erstellen, Aktualisieren, Abrufen und Löschen von Geräteidentitäten. |
| `{iot hub host name}/messages/events` | Empfangen von Gerät-an-Cloud-Nachrichten. |
| `{iot hub host name}/servicebound/feedback` | Empfangen von Feedback für Cloud-an-Gerät-Nachrichten. |
| `{iot hub host name}/devicebound` | Senden von Cloud-an-Gerät-Nachrichten. |

Beispielsweise würde ein Dienst, der zum Generieren die vorab erstellte SAS-Richtlinie mit dem Namen **registryRead** verwendet, ein Token mit den folgenden Parametern erstellen:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices`
* Signaturschlüssel: einer der Schlüssel der `registryRead`-Richtlinie
* Richtlinienname: `registryRead`
* Eine Ablaufzeit

    var endpoint ="myhub.azure-devices.net/devices"; var policyName = 'device'; var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Das Ergebnis, das Lesezugriff für alle Geräteidentitäten gewähren würde, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## Unterstützte X.509-Zertifikate

Sie können ein beliebiges X.509-Zertifikat zum Authentifizieren eines Geräts bei IoT Hub verwenden. Dies umfasst:

-   **Ein vorhandenes X.509-Zertifikat**. Einem Gerät ist möglicherweise bereits ein X.509-Zertifikat zugeordnet. Das Gerät kann dieses Zertifikat für die Authentifizierung bei IoT Hub verwenden.

-   **Ein selbst generiertes und selbstsigniertes X.509-Zertifikat**. Ein Gerätehersteller oder interner Bereitsteller kann diese Zertifikate generieren und den entsprechenden privaten Schlüssel (und das Zertifikat) auf dem Gerät speichern. Sie können Tools wie etwa [OpenSSL] und das Windows-Hilfsprogramm [SelfSignedCertificate] dafür verwenden.

-   **Ein von einer Zertifizierungsstelle signiertes X.509-Zertifikat**. Sie können auch ein von einer Zertifizierungsstelle generiertes und signiertes X.509-Zertifikat verwenden, um ein Gerät zu identifizieren und das Gerät bei IoT Hub zu authentifizieren.

Ein Gerät verwendet entweder ein X.509-Zertifikat oder ein Sicherheitstoken für die Authentifizierung, aber nicht beides.

## Registrieren eines X.509-Clientzertifikats für ein Gerät

Das [Azure IoT-Dienst-SDK für C#][lnk-service-sdk] (mindestens Version 1.0.8) unterstützt Geräte, die ein X.509-Clientzertifikat für die Authentifizierung verwenden. Andere APIs wie beispielsweise für den Import/Export von Geräten unterstützen ebenfalls X.509-Clientzertifikate.

### C#-Unterstützung

Die **RegistryManager**-Klasse stellt eine programmgesteuerte Methode zum Registrieren eines Geräts bereit. Insbesondere die Methoden **AddDeviceAsync** und **UpdateDeviceAsync** ermöglichen einem Benutzer die Registrierung und Aktualisierung eines Geräts in der IoT Hub-Geräteidentitätsregistrierung. Diese beiden Methoden nutzen eine **Device**-Instanz als Eingabe. Die **Device**-Klasse enthält eine **Authentication**-Eigenschaft, die dem Benutzer die Angabe primärer und sekundärer X.509-Zertifikatfingerabdrücke ermöglicht. Der Fingerabdruck stellt einen SHA-1-Hash des X.509-Zertifikats dar (gespeichert mithilfe binärer DER-Codierung). Benutzer haben die Möglichkeit, einen primären Fingerabdruck oder einen sekundären Fingerabdruck oder beides anzugeben. Primäre und sekundäre Fingerabdrücke werden unterstützt, um Szenarios mit Zertifikat-Rollover zu behandeln.

> [AZURE.NOTE] IoT Hub benötigt oder speichert nicht das gesamte X.509-Clientzertifikat, sondern nur den Fingerabdruck.

Hier sehen Sie einen C#-Beispielcodeausschnitt zum Registrieren eines Geräts mithilfe eines X.509-Clientzertifikats:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

## Verwenden eines X.509-Clientzertifikats während Laufzeitvorgängen

Das [Azure IoT-Geräte-SDK für .NET][lnk-client-sdk] (mindestens Version 1.0.11) unterstützt die Verwendung von X.509-Clientzertifikaten.

### C#-Unterstützung

Die Klasse **DeviceAuthenticationWithX509Certificate** unterstützt die Erstellung von **DeviceClient**-Instanzen mithilfe eines X.509-Clientzertifikats.

Hier sehen Sie ein Beispiel für einen Codeausschnitt:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

[lnk-apis-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-guidance-security]: iot-hub-guidance.md#customauth
[lnk-devguide-security]: iot-hub-devguide.md#security
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[OpenSSL]: https://www.openssl.org/
[SelfSignedCertificate]: https://technet.microsoft.com/library/hh848633
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device

<!---HONumber=AcomDC_0914_2016-->