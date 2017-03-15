---
title: Grundlegendes zur Sicherheit von Azure IoT Hub | Microsoft-Dokumentation
description: "Entwicklerhandbuch: Steuern des Zugriffs auf IoT Hub für Geräte-Apps und Back-End-Apps. Enthält Informationen zu Sicherheitstoken und zur Unterstützung von X.509-Zertifikaten."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 45631e70-865b-4e06-bb1d-aae1175a52ba
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 0644efd8753c33c0404b45f567759c0be666bcef
ms.lasthandoff: 03/07/2017


---
# <a name="control-access-to-iot-hub"></a>Verwalten des Zugriffs auf IoT Hub
## <a name="overview"></a>Übersicht
In diesem Artikel werden die Optionen zum Sichern Ihres IoT Hubs beschrieben. IoT Hub verwendet *Berechtigungen*, um Zugriff auf den Endpunkt jedes IoT Hubs zu gewähren. Berechtigungen beschränkten den Zugriff auf einen IoT Hub basierend auf der Funktionalität.

Dieser Artikel beschreibt Folgendes:

* Die verschiedenen Berechtigungen, die Sie einem Gerät oder einer Back-End-App für den Zugriff auf Ihre IoT Hubs gewähren können.
* Der Authentifizierungsvorgangs und die verwendeten Token zum Überprüfen der Berechtigungen.
* Festlegen des Geltungsbereichs für Anmeldeinformationen zum Begrenzen des Zugriffs auf bestimmte Ressourcen.
* IoT Hub-Unterstützung für X.509-Zertifikate.
* Benutzerdefinierte Authentifizierungsmechanismen, die auf vorhandene Geräteidentitätsregistrierungen Authentifizierungsschemas zurückgreifen.

### <a name="when-to-use"></a>Einsatzgebiete
Sie müssen über Berechtigungen für den Zugriff auf IoT Hub-Endpunkte verfügen. Beispielsweise muss ein Gerät ein Token mit den Anmeldeinformationen zusammen mit jeder an IoT Hub gesendeten Nachricht enthalten.

## <a name="access-control-and-permissions"></a>Access Control und Berechtigungen
Sie können [Berechtigungen](#iot-hub-permissions) auf folgende Weise gewähren:

* **Richtlinien für den gemeinsamen Zugriff auf IoT Hub-Ebene**. SAS-Richtlinien können eine beliebige Kombination von [Berechtigungen](#iot-hub-permissions) gewähren. Sie können Richtlinien im [Azure-Portal][lnk-management-portal] oder programmgesteuert mithilfe von [IoT Hub-Ressourcenanbieter-REST-APIs][lnk-resource-provider-apis] definieren. Ein neu erstellter IoT Hub verfügt über die folgenden Standardrichtlinien:
  
  * **iothubowner**: Richtlinie mit sämtlichen Berechtigungen.
  * **service**: Richtlinie mit **ServiceConnect**-Berechtigung.
  * **device**: Richtlinie mit **DeviceConnect**-Berechtigung.
  * **registryRead**: Richtlinie mit **RegistryRead**-Berechtigung.
  * **registryReadWrite**: Richtlinie mit den Berechtigungen **RegistryRead** und „RegistryWrite“.
  * **Sicherheitsanmeldeinformationen auf Gerätebasis**. Jeder IoT Hub enthält eine [Identitätsregistrierung][lnk-identity-registry]. Sie können für jedes Gerät in dieser Identitätsregistrierung Sicherheitsanmeldeinformationen konfigurieren, die **DeviceConnect**-Berechtigungen erteilen, deren Gültigkeitsbereich auf die entsprechenden Geräteendpunkte festgelegt ist.

Beispielsweise In einer normalen IoT-Lösung:

* Die Geräteverwaltungskomponente verwendet die Richtlinie *registryReadWrite* .
* Die Ereignisprozessorkomponente verwendet die Richtlinie *service* .
* Die Laufzeit-Geschäftslogikkomponente für Geräte verwendet die Richtlinie *service*.
* Einzelne Geräte stellen unter Verwendung von Anmeldeinformationen eine Verbindung her, die in der IoT Hub-Identitätsregistrierung gespeichert sind.

> [!NOTE]
> Weitere Informationen finden Sie unter [Berechtigungen](#iot-hub-permissions).

## <a name="authentication"></a>Authentifizierung
Azure IoT Hub gewährt Zugriff auf Endpunkte, indem ein Token zur Verifizierung mit gemeinsam genutzten Zugriffsrichtlinien und Sicherheitsanmeldeinformationen für die Identitätsregistrierung verglichen wird.

Sicherheitsanmeldeinformationen, beispielsweise symmetrische Schlüssel, werden niemals über eine physische Verbindung gesendet.

> [!NOTE]
> Der Azure IoT Hub-Ressourcenanbieter wird über Ihr Azure-Abonnement geschützt – ebenso wie alle Anbieter im [Azure Resource Manager][lnk-azure-resource-manager].
> 
> 

Weitere Informationen zur Erstellung und Verwendung von Sicherheitstoken finden Sie im Artikel zu den [IoT Hub-Sicherheitstoken][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokolldetails
Jedes unterstützte Protokoll, z.B. MQTT, AMQP und HTTP, transportiert Token auf unterschiedliche Weise.

Bei Verwendung von MQTT weist das CONNECT-Paket die Geräte-ID als Client-ID, {iothubhostname}/{deviceId} im Feld „Benutzername“ und ein SAS-Token im Feld „Kennwort“ auf. {iothubhostname} sollte der vollständige CNAME des IoT Hubs (z.B. „contoso.azure-devices.net“) sein.

Bei Verwendung von [AMQP][lnk-amqp] unterstützt IoT Hub [SASL PLAIN][lnk-sasl-plain] und die [auf AMQP-Ansprüchen basierte Sicherheit][lnk-cbs].

Wenn Sie eine auf AMQP-Ansprüchen beruhende Sicherheit verwenden, gibt der Standard vor, wie die oben aufgeführten Token übertragen werden.

Für SASL PLAIN kann der **Benutzername** Folgendes sein:

* `{policyName}@sas.root.{iothubName}`, wenn Token auf IoT Hub-Ebene verwendet werden.
* `{deviceId}@sas.{iothubname}`, wenn Token für Geräte verwendet werden.

In beiden Fällen enthält das Kennwortfeld das Token gemäß der Beschreibung unter [IoT Hub-Sicherheitstoken][lnk-sas-tokens].

HTTP implementiert die Authentifizierung, indem ein gültiges Token in den Anforderungsheader **Authorization** eingeschlossen wird.

#### <a name="example"></a>Beispiel
Benutzername (bei der Geräte-ID wird Groß-/Kleinschreibung berücksichtigt): `iothubname.azure-devices.net/DeviceId`

Kennwort (SAS-Token mit dem Tool [Device Explorer][lnk-device-explorer] generieren): `SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> Die [Azure IoT SDKs][lnk-sdks] generieren automatisch Token, wenn eine Verbindung mit dem Dienst hergestellt wird. In einigen Fällen unterstützen die Azure IoT SDKs nicht alle Protokolle oder Authentifizierungsmethoden.
> 
> 

### <a name="special-considerations-for-sasl-plain"></a>Besonderheiten für SASL PLAIN
Bei Verwendung von SASL PLAIN mit AMQP kann ein Client, der eine Verbindung mit einem IoT Hub herstellt, ein einziges Token für jede TCP-Verbindung verwenden. Wenn das Token abläuft, wird die TCP-Verbindung mit dem Dienst getrennt, und es wird ein Versuch zur erneuten Verbindungsherstellung ausgelöst. Dieses Verhalten ist für eine Back-End-App unproblematisch, für eine Geräte-App jedoch schädlich. Dies hat folgende Gründe:

* Gateways stellen eine Verbindung üblicherweise für eine Vielzahl von Geräten her. Bei Verwendung von SASL PLAIN muss für jedes Gerät, das eine Verbindung mit einem IoT Hub herstellen möchte, eine eigene TCP-Verbindung erstellt werden. Bei diesem Szenario erhöht sich der Verbrauch von Rechen- und Netzwerkressourcen erheblich, und es kommt zu einer höheren Latenz für jede Geräteverbindung.
* Der erhöhte Ressourcenverbrauch bei der erneuten Verbindungsherstellung nach jedem Tokenablauf wirkt sich negativ auf Geräte mit Ressourceneinschränkungen aus.

## <a name="scope-iot-hub-level-credentials"></a>Gültigkeitsbereich für Anmeldeinformationen auf IoT Hub-Ebene
Sie können den Bereich für Sicherheitsrichtlinien auf IoT Hub-Ebene festlegen, indem Sie Token mit eingeschränktem Ressourcen-URI erstellen. Beispielsweise ist der Endpunkt zum Senden von Gerät-zu-Cloud-Nachrichten von einem Gerät **/devices/{deviceId}/messages/events**. Sie können eine SAS-Richtlinie auf IoT Hub-Ebene mit **DeviceConnect**-Berechtigungen auch verwenden, um ein Token zu signieren, dessen resourceURI-Element **/devices/{deviceId}** ist. Bei diesem Ansatz wird ein Token erstellt, das nur zum Senden von Nachrichten im Namen der des Geräts **deviceId** verwendet werden kann.

Dieser Mechanismus ist mit einer [Event Hubs-Herausgeberrichtlinie][lnk-event-hubs-publisher-policy] vergleichbar und ermöglicht die Implementierung von benutzerdefinierten Authentifizierungsmethoden.

## <a name="security-tokens"></a>Sicherheitstoken
IoT Hub verwendet Sicherheitstoken zum Authentifizieren von Geräten und Diensten, um das Senden von Schlüsseln über das Netzwerk zu vermeiden. Darüber hinaus sind Sicherheitstoken im Hinblick auf Gültigkeitszeitraum und Bereich beschränkt. [Azure IoT SDKs][lnk-sdks] generieren Token automatisch, ohne dass eine spezielle Konfiguration erforderlich ist. In einigen Szenarien müssen Sie Sicherheitstoken jedoch direkt generieren und verwenden. Zu diesen Szenarien gehören die direkte Verwendung von MQTT-, AMQP- oder HTTP-Oberflächen oder die Implementierung des Tokendienstmusters, wie unter [Benutzerdefinierte Geräteauthentifizierung][lnk-custom-auth] erläutert.

IoT Hub ermöglicht Geräten darüber hinaus die Authentifizierung bei IoT Hub mithilfe von [X.509-Zertifikaten][lnk-x509]. 

### <a name="security-token-structure"></a>Struktur von Sicherheitstoken
Sie verwenden Sicherheitstoken, um zeitlich begrenzten Zugriff auf Geräte und Dienste für bestimmte Funktionen in IoT Hub zu gewähren. Um sicherzustellen, dass nur autorisierte Geräte und Dienste eine Verbindung herstellen können, müssen Sicherheitstoken mit einem Schlüssel für den gemeinsamen Zugriff oder einem symmetrischen Schlüssel signiert werden. Diese Schlüssel werden mit einer Geräteidentität in der Identitätsregistrierung gespeichert.

Ein mit einem Schlüssel für den gemeinsamen Zugriff signiertes Token gewährt Zugriff auf alle Funktionen, die den SAS-Richtlinienberechtigungen zugeordnet sind. Dagegen erteilt ein mit einem symmetrischen Schlüssel der Geräteidentität signiertes Token nur die **DeviceConnect** -Berechtigung für die zugeordnete Geräteidentität.

Das Sicherheitstoken weist das folgende Format auf:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Hier sind die erwarteten Werte:

| Wert | Beschreibung |
| --- | --- |
| {signature} |Eine HMAC-SHA256-Signaturzeichenfolge in folgendem Format: `{URL-encoded-resourceURI} + "\n" + expiry`. **Wichtig:**Der Schlüssel wird aus Base64 decodiert und als Schlüssel für die HMAC-SHA256-Berechnung verwendet. |
| {resourceURI} |Das URI-Präfix (nach Segment) der Endpunkte, auf die mit diesem Token zugegriffen werden kann – beginnend mit dem Hostnamen des IoT Hubs (kein Protokoll). Beispiel: `myHub.azure-devices.net/devices/device1` |
| {expiry} |UTF8-Zeichenfolge, dargestellt als die Anzahl von Sekunden seit dem 1. Januar 1970 um 00:00:00 UTC. |
| {URL-encoded-resourceURI} |URL-Codierung des Ressourcen-URI (beides in Kleinbuchstaben) |
| {policyName} |Der Name der gemeinsam genutzten Zugriffsrichtlinie, auf die dieses Token verweist. Nicht vorhanden, wenn das Token auf Geräteregistrierungs-Anmeldeinformationen verweist. |

**Hinweis zum Präfix**: Das URI-Präfix wird nach Segment, nicht nach Zeichen berechnet. Beispielsweise ist `/a/b` ein Präfix für `/a/b/c`, aber nicht für `/a/bc`.

Der folgende „Node.js“-Codeausschnitt zeigt eine Funktion namens **GenerateSasToken**, die das Token aus den Eingaben `resourceUri, signingKey, policyName, expiresInMins` berechnet. In den nächsten Abschnitten wird erläutert, wie die verschiedenen Eingaben für die verschiedenen Anwendungsfälle für Token initialisiert werden.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri);

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Hier ist zum Vergleich der entsprechende Python-Code zum Generieren eines Sicherheitstokens:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [!NOTE]
> Da der Gültigkeitszeitraum des Tokens auf dem IoT Hub-Computer überprüft wird, muss die Abweichung der Uhr des Computers, auf dem das Token generiert wird, minimal sein.
> 
> 

### <a name="use-sas-tokens-in-a-device-app"></a>Verwenden von SAS-Token in einer Geräte-App
Es gibt zwei Methoden zum Abrufen von **DeviceConnect**-Berechtigungen mit IoT Hub über Sicherheitstoken: mit einem [symmetrischen Identitätsschlüssel aus der Identitätsregistrierung](#use-a-symmetric-key-in-the-identity-registry) oder mit einem [Schlüssel für den gemeinsamen Zugriff](#use-a-shared-access-policy).

Denken Sie daran, dass alle Funktionen, auf die auf Geräten zugegriffen werden kann, für Endpunkte mit dem Präfix `/devices/{deviceId}` absichtlich verfügbar gemacht werden.

> [!IMPORTANT]
> Die einzige Möglichkeit, dass IoT Hub ein bestimmtes Gerät authentifiziert, ist durch die Verwendung des symmetrischen Schlüssels der Geräteidentität. Wenn eine SAS-Richtlinie verwendet wird, um auf Gerätefunktionen zuzugreifen, muss die Lösung die Komponente, die das Sicherheitstoken ausgibt, als vertrauenswürdige Unterkomponente ansehen.
> 
> 

Die geräteseitigen Endpunkte sind (unabhängig vom Protokoll):

| Endpunkt | Funktionalität |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |Senden von Gerät-an-Cloud-Nachrichten. |
| `{iot hub host name}/devices/{deviceId}/devicebound` |Empfangen von Cloud-an-Gerät-Nachrichten. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Verwenden eines symmetrischen Schlüssels in der Identitätsregistrierung
Wenn Sie den symmetrischen Schlüssel einer Geräteidentität zum Generieren eines Tokens verwenden, wird das policyName-Element (`skn`) des Tokens nicht angegeben.

Beispielsweise muss ein Token, das für den Zugriff auf alle Gerätefunktionen erstellt wird, die folgenden Parameter aufweisen:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`
* Signaturschlüssel: ein symmetrischer Schlüssel für die `{device id}` -Identität
* Kein Richtlinienname
* Eine Ablaufzeit

Ein Beispiel mit der oben genannten Node.js-Funktion:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Das Ergebnis, das Zugriff auf alle Funktionen für „device1“ gewährt wird, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [!NOTE]
> Es ist möglich, mit dem .NET-[Geräteexplorer][lnk-device-explorer]-Tool oder dem plattformübergreifenden Befehlszeilen-Hilfsprogramm [iothub-explorer][lnk-iothub-explorer] auf Knotenbasis ein SAS-Token zu generieren.
> 
> 

### <a name="use-a-shared-access-policy"></a>Verwenden einer SAS-Richtlinie
Beim Erstellen eines Tokens aus einer SAS-Richtlinie muss das Feld für den Richtliniennamen `skn` auf den Namen der verwendeten Richtlinie festgelegt werden. Darüber hinaus muss die Richtlinie die **DeviceConnect** -Berechtigung erteilen.

Die beiden wichtigsten Szenarien für die Verwendung von SAS-Richtlinien für den Zugriff auf Gerätefunktionen sind:

* [Protokollgateways für Clouds][lnk-endpoints]
* [Tokendienste][lnk-custom-auth], die verwendet werden, um benutzerdefinierte Authentifizierungsschemas zu implementieren

Da die SAS-Richtlinie potenziell Zugriff gewähren kann, um als ein beliebiges Gerät eine Verbindung herzustellen, ist es wichtig, beim Erstellen von Sicherheitstoken den richtigen Ressourcen-URI zu verwenden. Dies ist besonders wichtig für Tokendienste, die das Token mit dem Ressourcen-URI auf ein bestimmtes Gerät festlegen müssen. Dieser Punkt ist weniger relevant für Protokollgateways, da sie Datenverkehr bereits für alle Geräte vermitteln.

Beispielsweise würde ein Tokendienst, der die vorab erstellte SAS-Richtlinie mit dem Namen **device** verwendet, ein Token mit den folgenden Parametern erstellen:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`
* Signaturschlüssel: einer der Schlüssel der `device` -Richtlinie
* Richtlinienname: `device`
* Eine Ablaufzeit

Ein Beispiel mit der oben genannten Node.js-Funktion:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Das Ergebnis, das Zugriff auf alle Funktionen für „device1“ gewährt wird, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Ein Protokollgateway könnte das gleiche Token für alle Geräte verwenden, indem einfach der Ressourcen-URI auf `myhub.azure-devices.net/devices`festgelegt wird.

### <a name="use-security-tokens-from-service-components"></a>Verwenden der Sicherheitstoken von Dienstkomponenten
Dienstkomponenten können nur Sicherheitstoken mithilfe von SAS-Richtlinien generieren, die die entsprechenden Berechtigungen erteilen, wie bereits erläutert.

Hier sind die Dienstfunktionen, die für die Endpunkte verfügbar gemacht werden:

| Endpunkt | Funktionalität |
| --- | --- |
| `{iot hub host name}/devices` |Erstellen, Aktualisieren, Abrufen und Löschen von Geräteidentitäten. |
| `{iot hub host name}/messages/events` |Empfangen von Gerät-an-Cloud-Nachrichten. |
| `{iot hub host name}/servicebound/feedback` |Empfangen von Feedback für Cloud-an-Gerät-Nachrichten. |
| `{iot hub host name}/devicebound` |Senden von Cloud-an-Gerät-Nachrichten. |

Beispielsweise würde ein Dienst, der zum Generieren die vorab erstellte SAS-Richtlinie mit dem Namen **registryRead** verwendet, ein Token mit den folgenden Parametern erstellen:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices`
* Signaturschlüssel: einer der Schlüssel der `registryRead` -Richtlinie
* Richtlinienname: `registryRead`
* Eine Ablaufzeit
  
    var endpoint ="myhub.azure-devices.net/devices";   var policyName = 'device';   var policyKey = '...';
  
    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Das Ergebnis, das Lesezugriff für alle Geräteidentitäten gewähren würde, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Unterstützte X.509-Zertifikate
Sie können ein beliebiges X.509-Zertifikat zum Authentifizieren eines Geräts bei IoT Hub verwenden. Die Zertifikate umfassen:

* **Ein vorhandenes X.509-Zertifikat**. Einem Gerät ist möglicherweise bereits ein X.509-Zertifikat zugeordnet. Das Gerät kann dieses Zertifikat für die Authentifizierung bei IoT Hub verwenden.
* **Ein selbst generiertes und selbstsigniertes X.509-Zertifikat**. Ein Gerätehersteller oder interner Bereitsteller kann diese Zertifikate generieren und den entsprechenden privaten Schlüssel (und das Zertifikat) auf dem Gerät speichern. Sie können dafür Tools wie etwa [OpenSSL][lnk-openssl] und das Windows-Hilfsprogramm [SelfSignedCertificate][lnk-selfsigned] verwenden.
* **Ein von einer Zertifizierungsstelle signiertes X.509-Zertifikat**. Sie können auch ein von einer Zertifizierungsstelle generiertes und signiertes X.509-Zertifikat verwenden, um ein Gerät zu identifizieren und das Gerät bei IoT Hub zu authentifizieren.

Ein Gerät verwendet entweder ein X.509-Zertifikat oder ein Sicherheitstoken für die Authentifizierung, aber nicht beides.

### <a name="register-an-x509-certificate-for-a-device"></a>Registrieren eines X.509-Zertifikats für ein Gerät
Das [Azure IoT-Dienst-SDK für C#][lnk-service-sdk] (mindestens Version 1.0.8) unterstützt die Registrierung von Geräten, die ein X.509-Zertifikat für die Authentifizierung verwenden. Andere APIs wie beispielsweise für den Import/Export von Geräten unterstützen ebenfalls X.509-Zertifikate.

### <a name="c-support"></a>C\#-Unterstützung
Die **RegistryManager** -Klasse stellt eine programmgesteuerte Methode zum Registrieren eines Geräts bereit. Insbesondere die Methoden **AddDeviceAsync** und **UpdateDeviceAsync** ermöglichen Ihnen die Registrierung und Aktualisierung eines Geräts in der IoT Hub-Identitätsregistrierung. Diese beiden Methoden nutzen eine **Device** -Instanz als Eingabe. Die **Device**-Klasse enthält eine **Authentication**-Eigenschaft, die Ihnen die Angabe primärer und sekundärer X.509-Zertifikatfingerabdrücke ermöglicht. Der Fingerabdruck stellt einen SHA-1-Hash des X.509-Zertifikats dar (gespeichert mithilfe binärer DER-Codierung). Sie haben die Möglichkeit, einen primären Fingerabdruck oder einen sekundären Fingerabdruck oder beides anzugeben. Primäre und sekundäre Fingerabdrücke werden unterstützt, um Szenarien mit Zertifikat-Rollover zu behandeln.

> [!NOTE]
> IoT Hub benötigt oder speichert nicht das gesamte X.509-Zertifikat, sondern nur den Fingerabdruck.
> 
> 

Hier sehen Sie einen C\#-Beispielcodeausschnitt zum Registrieren eines Geräts mithilfe eines X.509-Zertifikats:

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

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Verwenden eines X.509-Zertifikats bei Laufzeitvorgängen
Das [Azure IoT-Geräte-SDK für .NET][lnk-client-sdk] (mindestens Version 1.0.11) unterstützt die Verwendung von X.509-Zertifikaten.

### <a name="c-support"></a>C\#-Unterstützung
Die Klasse **DeviceAuthenticationWithX509Certificate** unterstützt die Erstellung von  **DeviceClient**-Instanzen mithilfe eines X.509-Zertifikats.

Hier sehen Sie ein Beispiel für einen Codeausschnitt:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Benutzerdefinierte Geräteauthentifizierung
Mit der [Identitätsregistrierung][lnk-identity-registry] von IoT Hub können Sie Sicherheitsanmeldeinformationen und die Zugriffssteuerung pro Gerät mithilfe von [Token][lnk-sas-tokens] konfigurieren. Auch wenn eine IoT-Lösung bereits erheblichen Anteil an einer benutzerdefinierten Identitätsregistrierung bzw. einem Authentifizierungsschema hat, können Sie diese vorhandene Infrastruktur durch Erstellen eines *Tokendiensts* in IoT Hub integrieren. Auf diese Weise können Sie andere IoT-Features in der Lösung nutzen.

Ein Tokendienst ist ein benutzerdefinierter Clouddienst. Er verwendet eine *SAS-Richtlinie* von IoT Hub mit **DeviceConnect**-Berechtigungen, um Token mit *Gerätebereich* zu erstellen. Mit diesen Token kann ein Gerät eine Verbindung mit Ihrem IoT Hub herstellen.

  ![Schritte des Tokendienstmusters][img-tokenservice]

Hier sind die wichtigsten Schritte des Tokendienstmusters:

1. Erstellen Sie eine IoT Hub-SAS-Richtlinie mit **DeviceConnect**-Berechtigungen für Ihren IoT Hub. Sie können diese Richtlinie im [Azure-Portal][lnk-management-portal] oder programmgesteuert erstellen. Diese Richtlinie wird vom Tokendienst zum Signieren der von ihm erstellten Token verwendet.
2. Wenn ein Gerät auf IoT Hub zugreifen muss, fordert es von Ihrem Tokendienst ein signiertes Token an. Das Gerät kann mit Ihrer benutzerdefinierten Identitätsregistrierung bzw. mit dem Authentifizierungsschema authentifiziert werden, um die Geräteidentität zu ermitteln, die der Tokendienst zum Erstellen des Tokens verwendet.
3. Der Tokendienst gibt ein Token zurück. Das Token wird mit `/devices/{deviceId}` als `resourceURI` erstellt, wobei `deviceId` das authentifizierte Gerät ist. Der Tokendienst verwendet die SAS-Richtlinie, um das Token zu erstellen.
4. Das Gerät nutzt das Token direkt mit IoT Hub.

> [!NOTE]
> Sie können die .NET-Klasse [SharedAccessSignatureBuilder][lnk-dotnet-sas] oder die Java-Klasse [IotHubServiceSasToken][lnk-java-sas] zum Erstellen eines Tokens im Tokendienst verwenden.
> 
> 

Der Tokendienst kann die Gültigkeitsdauer für das Token wie gewünscht festlegen. Wenn das Token abläuft, trennt IoT Hub die Geräteverbindung. Das Gerät muss dann ein neues Token vom Tokendienst anfordern. Eine kurze Ablaufzeit erhöht die Last für das Gerät und den Tokendienst gleichermaßen.

Damit ein Gerät eine Verbindung mit Ihrem Hub herstellen kann, müssen Sie es der IoT Hub-Identitätsregistrierung hinzufügen – auch wenn das Gerät für die Verbindung ein Token und keinen Geräteschlüssel verwendet. Aus diesem Grund können Sie weiterhin die Zugriffssteuerung pro Gerät nutzen, indem Sie Geräteidentitäten in der [IoT Hub-Identitätsregistrierung][lnk-identity-registry] aktivieren oder deaktivieren, wenn sich das Gerät mit einem Token authentifiziert. Dieser Ansatz verringert die Risiken der Verwendung von Token mit langen Ablaufzeiten.

### <a name="comparison-with-a-custom-gateway"></a>Vergleich mit einem benutzerdefinierten Gateway
Das Tokendienstmuster ist der empfohlene Weg zur Implementierung einer benutzerdefinierten Identitätsregistrierung bzw. eines Authentifizierungsschemas mit IoT Hub. Dies wird empfohlen, da der größte Teil des Lösungsdatenverkehrs weiterhin über IoT Hub abgewickelt wird. Es gibt aber auch Fälle, in denen das benutzerdefinierte Authentifizierungsschema so eng mit dem Protokoll verknüpft ist, dass ein Dienst zum Verarbeiten des gesamten Datenverkehrs (*benutzerdefiniertes Gateway*) erforderlich ist. Ein Beispiel eines solchen Szenarios ist die Verwendung von [Transport Layer Security (TLS) und vorinstallierten Schlüsseln (PSKs)][lnk-tls-psk]. Weitere Informationen finden Sie im Thema [Protokollgateway][lnk-protocols].

## <a name="reference-topics"></a>Referenzthemen:
Die folgenden Referenzthemen enthalten weitere Informationen zum Steuern des Zugriffs auf Ihren IoT Hub.

## <a name="iot-hub-permissions"></a>IoT Hub-Berechtigungen
In der folgenden Tabelle werden die Berechtigungen aufgeführt, die Sie zum Steuern des Zugriffs auf den IoT Hub verwenden können.

| Berechtigung | Hinweise |
| --- | --- |
| **RegistryRead** |Diese Berechtigung gewährt Lesezugriff auf die Identitätsregistrierung. Weitere Informationen finden Sie unter [Identitätsregistrierung][lnk-identity-registry]. <br/>Diese Berechtigung wird von Back-End Cloud Services verwendet. |
| **RegistryReadWrite** |Diese Berechtigung gewährt Lese- und Schreibzugriff auf die Identitätsregistrierung. Weitere Informationen finden Sie unter [Identitätsregistrierung][lnk-identity-registry]. <br/>Diese Berechtigung wird von Back-End Cloud Services verwendet. |
| **ServiceConnect** |Diese Berechtigung gewährt Zugriff auf die clouddienstseitigen Endpunkte für Kommunikation und Überwachung. <br/>Clouddiensten wird die Berechtigung erteilt, D2C-Nachrichten zu empfangen, C2D-Nachrichten zu senden und die zugehörigen Übermittlungsbestätigungen zu empfangen. <br/>Erteilt die Berechtigung zum Abrufen der Übermittlungsbestätigungen für die Dateiuploads. <br/>Gewährt die Berechtigung zum Zugriff auf Gerätezwillinge zum Aktualisieren von Tags und der gewünschten Eigenschaften, zum Abrufen gemeldeter Eigenschaften und zum Ausführen von Abfragen. <br/>Diese Berechtigung wird von Back-End Cloud Services verwendet. |
| **DeviceConnect** |Diese Berechtigung gewährt Zugriff auf die geräteseitigen Endpunkte. <br/>Gewährt die Berechtigung zum Senden von D2C-Nachrichten und zum Empfangen von C2D-Nachrichten. <br/>Gewährt die Berechtigung zum Ausführen eines Dateiupload von einem Gerät. <br/>Gewährt die Berechtigung zum Abrufen von gewünschten Eigenschaftsbenachrichtigungen für Gerätezwillinge und zum Aktualisieren von gemeldeten Eigenschaften. <br/>Gewährt die Berechtigung zum Ausführen eines Dateiupload. <br/>Diese Berechtigung wird von Geräten verwendet. |

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im IoT Hub-Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Azure IoT-SDKs für Geräte und Dienste][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit IoT Hub verwenden können.
* Unter [Referenz – Abfragesprache für Zwillinge und Aufträge][lnk-query] wird die IoT Hub-Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen zu Gerätezwillingen und Aufträgen abrufen können.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie gelernt haben, wie Sie den Zugriff auf IoT Hub beschränken, sind möglicherweise die folgenden Themen im IoT Hub-Entwicklerhandbuch für Sie interessant:

* [Grundlegendes zu Gerätezwillingen – Vorschau][lnk-devguide-device-twins]
* [Aufrufen einer direkten Methode auf einem Gerät][lnk-devguide-directmethods]
* [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, sind möglicherweise die folgenden IoT Hub-Tutorials für Sie interessant:

* [Erste Schritte mit Azure IoT Hub][lnk-getstarted-tutorial]
* [Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub][lnk-c2d-tutorial]
* [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth._iot_hub_service_sas_token
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

