---
title: "Azure Service Fabric-Reverseproxy – sichere Kommunikation | Microsoft-Dokumentation"
description: "Konfigurieren eines Reverseproxys für eine sichere End-to-End-Kommunikation."
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/10/2017
ms.author: kavyako
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 89102e8b7bc01768742ed3e5e2bd8a9fd6c62ee8
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017

---
# <a name="connect-to-a-secure-service-with-the-reverse-proxy"></a>Herstellen einer Verbindung mit einem sicheren Dienst mit dem Reverseproxy

In diesem Artikel wird erläutert, wie eine sichere Verbindung zwischen dem Reverseproxy und Diensten hergestellt und so ein sicherer End-to-End-Kanal ermöglicht wird.

Die Herstellung einer Verbindung mit sicheren Diensten wird nur unterstützt, wenn der Reverseproxy für das Lauschen von HTTPS konfiguriert ist. Dies wird in diesem Dokument vorausgesetzt.
Informationen zum Konfigurieren des Reverseproxys in Service Fabric finden Sie unter [Reverseproxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).

## <a name="secure-connection-establishment-between-the-reverse-proxy-and-services"></a>Herstellung einer sicheren Verbindung zwischen dem Reverseproxy und Diensten 

### <a name="reverse-proxy-authenticating-to-services"></a>Reverseproxy authentifiziert sich bei Diensten:
Der Reverseproxy identifiziert sich bei Diensten mithilfe des zugehörigen Zertifikats durch Angabe der Eigenschaft ***reverseProxyCertificate*** im **Cluster**-[Ressourcentypenabschnitt](../azure-resource-manager/resource-group-authoring-templates.md). Die Dienste können die Logik implementieren, um das vom Reverseproxy bereitgestellte Zertifikat zu überprüfen. Die Dienste können die Details des akzeptierten Clientzertifikats als Konfigurationseinstellungen im Konfigurationspaket angeben. Dieses kann zur Laufzeit gelesen und zum Überprüfen des vom Reverseproxy bereitgestellten Zertifikats verwendet werden. Informationen zum Hinzufügen der Konfigurationseinstellungen finden Sie unter [Verwalten von Anwendungsparametern](service-fabric-manage-multiple-environment-app-configuration.md). 

### <a name="reverse-proxy-verifying-the-services-identity-via-the-certificate-presented-by-the-service"></a>Reverseproxy überprüft die Identität des Diensts über das vom Dienst bereitgestellte Zertifikat:
Für die Serverzertifikatüberprüfung der von den Diensten bereitgestellten Zertifikate unterstützt der Reverseproxy eine der folgenden Optionen: None, ServiceCommonNameAndIssuer und ServiceCertificateThumbprints.
Um eine dieser drei Optionen auszuwählen, geben Sie **ApplicationCertificateValidationPolicy** im Parameterabschnitt des ApplicationGateway/Http-Elements unter [fabricSettings](service-fabric-cluster-fabric-settings.md) ein.

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "None"
              }
            ]
          }
        ],
        ...
}
```

Ausführliche Informationen zur weiteren Konfiguration für jede dieser Optionen finden Sie im nächsten Abschnitt.

### <a name="service-certificate-validation-options"></a>Optionen für die Dienstzertifikatüberprüfung 

- **None:** Der Reverseproxy überspringt die Überprüfung des an den Proxy übermittelten Dienstzertifikats und stellt die sichere Verbindung her. Dies ist das Standardverhalten.
Geben Sie **ApplicationCertificateValidationPolicy** mit dem Wert **None** im Parameterabschnitt des ApplicationGateway/Http-Elements ein.

- **ServiceCommonNameAndIssuer:** Der Reverseproxy überprüft das vom Dienst bereitgestellte Zertifikat basierend auf dem allgemeinen Namen des Zertifikats und dem Fingerabdruck des unmittelbaren Ausstellers. Geben Sie **ApplicationCertificateValidationPolicy** mit dem Wert **ServiceCommonNameAndIssuer** im Parameterabschnitt des ApplicationGateway/Http-Elements ein.

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCommonNameAndIssuer"
              }
            ]
          }
        ],
        ...
}
```

Um die Liste der allgemeinen Namen des Diensts und der Fingerabdrücke des Ausstellers anzugeben, fügen Sie wie weiter unten dargestellt ein **ApplicationGateway/Http/ServiceCommonNameAndIssuer**-Element unter fabricSettings ein. Im Parameterarrayelement können mehrere Paare von allgemeinen Namen des Zertifikats und Fingerabdrücken des Ausstellers hinzugefügt werden. 

Wenn der Endpunkt, mit dem der Reverseproxy eine Verbindung herstellt, ein Zertifikat bereitstellt, dessen allgemeiner Name und Fingerabdruck des Ausstellers mit einem der hier angegebenen Werte übereinstimmt, wird der SSL-Kanal eingerichtet. Bei Nichtübereinstimmung der Zertifikatdetails gibt der Reverseproxy für die Anforderung des Clients eine Fehlermeldung mit dem Statuscode 502 (Ungültiges Gateway) aus. Zudem enthält die HTTP-Statuszeile den Ausdruck „Invalid SSL Certificate“. 

```json
{
"fabricSettings": [
          ...
          {
             "name": "ApplicationGateway/Http/ServiceCommonNameAndIssuer",
            "parameters": [
              {
                "name": "WinFabric-Test-Certificate-CN1",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 b4 22 11"
              },
              {
                "name": "WinFabric-Test-Certificate-CN2",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 11 33 44"
              }
            ]
          }
        ],
        ...
}
```


- **ServiceCertificateThumbprints:** Der Reverseproxy überprüft das an den Proxy übermittelte Dienstzertifikat basierend auf dem zugehörigen Fingerabdruck. Diese Option können Sie auswählen, wenn die Dienste mit selbstsignierten Zertifikaten konfiguriert sind. Geben Sie **ApplicationCertificateValidationPolicy** mit dem Wert **ServiceCertificateThumbprints** im Parameterabschnitt des ApplicationGateway/Http-Elements ein.

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCertificateThumbprints"
              }
            ]
          }
        ],
        ...
}
```

Geben Sie zudem die Fingerabdrücke mit einem **ServiceCertificateThumbprints**-Eintrag im Parameterabschnitt des ApplicationGateway/Http-Elements an. Mehrere Fingerabdrücke können, wie im Folgenden dargestellt, als eine durch Trennzeichen getrennte Liste im Wertfeld angegeben werden:

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "ServiceCertificateThumbprints",
                "value": "78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf,78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 b9"
              }
            ]
          }
        ],
        ...
}
```

Wenn der Fingerabdruck des Serverzertifikats in diesem Konfigurationseintrag aufgeführt ist, stellt der Reverseproxy die SSL-Verbindung her. Andernfalls beendet er die Verbindung und gibt für die Anforderung des Clients eine Fehlermeldung mit dem Statuscode 502 (Ungültiges Gateway) aus. Zudem enthält die HTTP-Statuszeile den Ausdruck „Invalid SSL Certificate“.

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Logik der Endpunktauswahl, wenn Dienste sichere und nicht sichere Endpunkte bereitstellen
Service Fabric unterstützt die Konfiguration mehrerer Endpunkte für einen Dienst. Informationen finden Sie unter [Angeben von Ressourcen in einem Dienstmanifest](service-fabric-service-manifest-resources.md).

Der Reverseproxy wählt einen der Endpunkte zum Weiterleiten der Anforderung basierend auf dem Abfrageparameter **ListenerName** aus. Wenn dieser nicht angegeben ist, kann er einen beliebigen Endpunkt aus der Liste der Endpunkte auswählen. Dabei kann es sich dann um einen HTTP- oder HTTPS-Endpunkt handeln. Möglicherweise möchten Sie festlegen, dass der Reverseproxy bei bestimmten Szenarien oder Anforderungen nur im „ausschließlich sicheren Modus“ ausgeführt wird, d.h., der sichere Reverseproxy soll keine Anforderungen an nicht sichere Endpunkte weiterleiten. Dies kann durch Angabe des Konfigurationseintrags **SecureOnlyMode** mit dem Wert **true** im Parameterabschnitt des ApplicationGateway/Http-Elements erreicht werden.   

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "SecureOnlyMode",
                "value": true
              }
            ]
          }
        ],
        ...
}
```

> 
> Wenn im **SecureOnlyMode** der Client einen **ListenerName** angegeben hat, der einem HTTP-Endpunkt (ungesichert) entspricht, gibt der Reverseproxy für die Anforderung eine Fehlermeldung mit dem HTTP-Statuscode 404 (Nicht gefunden) aus.

## <a name="setting-up-client-certificate-authentication-through-the-reverse-proxy"></a>Einrichten der Clientzertifikatauthentifizierung über den Reverseproxy
Die SSL-Beendigung erfolgt auf dem Reverseproxy, und alle Daten des Clientzertifikats gehen verloren. Damit die Dienste die Clientzertifikatauthentifizierung durchführen, legen Sie die Einstellung **ForwardClientCertificate** im Parameterabschnitt des ApplicationGateway/Http-Elements fest.

1. Wenn **ForwardClientCertificate** auf **false** festgelegt ist, fordert der Reverseproxy das Clientzertifikat während des SSL-Handshakes mit dem Client nicht an.
Dies ist das Standardverhalten.

2. Wenn **ForwardClientCertificate** auf **true** festgelegt ist, fordert der Reverseproxy das Clientzertifikat während des SSL-Handshakes mit dem Client an.
Dann leitet er die Daten des Clientzertifikats in einem benutzerdefinierten HTTP-Header mit dem Namen **X-Client-Certificate** weiter. Der Headerwert ist die Base64-codierte Zeichenfolge im PEM-Format des Clientzertifikats. Der Dienst kann die Anforderung nach dem Überprüfen der Zertifikatdaten mit dem entsprechenden Statuscode annehmen oder ablehnen.
Wenn der Client kein Zertifikat bereitstellt, leitet der Reverseproxy einen leeren Header weiter. Die weitere Verarbeitung erfolgt durch den Dienst.

> Der Reverseproxy führt lediglich die Weiterleitung durch. Er führt keine Überprüfung des Clientzertifikats durch.


## <a name="next-steps"></a>Nächste Schritte
* Beispiele für Azure Resource Manager-Vorlagen zum Konfigurieren eines sicheren Reverseproxys mit den unterschiedlichen Optionen für die Dienstzertifikatüberprüfung finden Sie unter [Configure reverse proxy to connect to secure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) (Konfigurieren des Reverseproxys für die Verbindung mit sicheren Diensten).
* Ein Beispiel für die HTTP-Kommunikation zwischen Diensten finden Sie im [Beispielprojekt auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Remoteprozeduraufrufe mit Reliable Services-Remoting](service-fabric-reliable-services-communication-remoting.md)
* [Web-API, die OWIN in Reliable Services verwendet](service-fabric-reliable-services-communication-webapi.md)
* [Verwalten von Clusterzertifikaten](service-fabric-cluster-security-update-certs-azure.md)

