---
title: Azure Service Bus-Authentifizierung mit Shared Access Signatures | Microsoft-Dokumentation
description: "Übersicht über Service Bus Authentication mithilfe von Shared Access Signatures, Informationen zur SAS-Authentifizierung mit Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: a2760072acb7c62204759f3ec0d3cb9899460f2d
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="service-bus-authentication-with-shared-access-signatures"></a>Service Bus-Authentifizierung mit Shared Access Signatures

*Shared Access Signatures* (SAS) sind die primären Sicherheitsmechanismen für Service Bus-Messaging. Dieser Artikel beschreibt SAS, ihre Funktionsweise und die plattformunabhängige Verwendung.

Die SAS-Authentifizierung ermöglicht Anwendungen die Authentifizierung bei Service Bus mithilfe eines Zugriffsschlüssels, der für den Namespace oder für die Messagingentität (Warteschlange oder Thema) konfiguriert wird, welcher bestimmte Rechte zugeordnet sind. Sie können diesen Schlüssel zum Generieren eines SAS-Tokens verwenden, das Clients wiederum für die Authentifizierung bei Service Bus verwenden können.

Die Unterstützung der SAS-Authentifizierung ist im Azure SDK, Version 2.0 oder höher, enthalten.

## <a name="overview-of-sas"></a>Übersicht über SAS

SAS (Shared Access Signatures) sind ein Authentifizierungsmechanismus, der auf sicheren Hashes (SHA-256) oder URIs basiert. SAS ist äußerst leistungsstark und wird von allen Service Bus-Diensten verwendet. In der Praxis bestehen SAS aus zwei Komponenten: einer *Richtlinie für den gemeinsamen Zugriff* und einer *Shared Access Signature* (häufig auch als *Token* bezeichnet).

Die SAS-Authentifizierung in Service Bus umfasst die Konfiguration eines kryptografischen Schlüssels mit den zugehörigen Rechten für eine Service Bus-Ressource. Clients beanspruchen Zugriff auf Service Bus-Ressourcen, indem sie ein SAS-Token bereitstellen. Dieses Token besteht aus dem Ressourcen-URI, auf den zugegriffen wird, und einer Ablaufangabe, die mit dem konfigurierten Schlüssel signiert wird.

Sie können Shared Access Signature-Autorisierungsregeln in [Relays](service-bus-fundamentals-hybrid-solutions.md#relays), [Warteschlangen](service-bus-fundamentals-hybrid-solutions.md#queues) und [Themen](service-bus-fundamentals-hybrid-solutions.md#topics) von Service Bus konfigurieren.

Die SAS-Authentifizierung verwendet die folgenden Elemente:

* [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule): ein primärer 256-Bit-Kryptografieschlüssel in Base64-Darstellung, ein optionaler sekundärer Schlüssel und ein Schlüsselname sowie zugehörige Rechte (eine Sammlung der Rechte *Listen* [Lauschen], *Send* [Senden] und *Manage* [Verwalten]).
* [SharedAccessSignature](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) -Token: Wird mithilfe des HMAC-SHA256-Codes einer Ressourcenzeichenfolge generiert und besteht aus dem URI der Ressource, auf die zugegriffen wird, sowie einer Ablaufangabe mit dem kryptografischen Schlüssel. Die Signatur und andere in den folgenden Abschnitten beschriebene Elemente werden als Zeichenfolge formatiert, um das SAS-Token zu bilden.

## <a name="shared-access-policy"></a>SAS-Richtlinie

Wichtig: Bei SAS beginnt alles mit einer Richtlinie. Jede Richtlinie erfordert drei Angaben: **Name**, **Bereich** und **Berechtigungen**. Der **Name** ist ein eindeutiger Name innerhalb des Bereichs. Der Bereich ist der URI der betreffenden Ressource. Für einen Service Bus-Namespace wird als Bereich der vollqualifizierte Domänenname (Fully Qualified Domain Name, FQDN) angegeben, z.B. `https://<yournamespace>.servicebus.windows.net/`.

Die verfügbaren Berechtigungen für eine Richtlinie sind größtenteils selbsterklärend:

* Senden
* Empfangen
* Verwalten

Nach Erstellung der Richtlinie werden ihr ein *Primärschlüssel* und ein *Sekundärschlüssel* zugewiesen. Hierbei handelt es sich um kryptografisch starke Schlüssel. Achten Sie darauf, dass sie Ihnen nicht abhanden kommen: Sie sind immer im [Azure-Portal][Azure portal] verfügbar. Sie können einen beliebigen der generierten Schlüssel verwenden und die Schlüssel jederzeit erneut generieren. Wenn Sie allerdings den Primärschlüssel neu generieren oder in der Richtlinie ändern, werden alle auf deren Grundlage erstellten SAS ungültig.

Wenn Sie einen Service Bus-Namespace erstellen, wird automatisch eine Richtlinie namens **RootManageSharedAccessKey**für den gesamten Namespace erstellt. Da Sie sich nicht als **Root** anmelden, verwenden Sie diese Richtlinie nur, wenn ein wirklich guter Grund dafür vorliegt. Da Sie sich nicht als **Root** anmelden, verwenden Sie diese Richtlinie nur, wenn ein wirklich guter Grund dafür vorliegt. Beachten Sie, dass pro Ebene der Service Bus-Struktur (Namespace, Warteschlange usw.) maximal 12 Richtlinien zugeordnet werden können.

## <a name="configuration-for-shared-access-signature-authentication"></a>Konfiguration für SAS-Authentifizierung (Shared Access Signature)
Sie können die Regel [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) für Service Bus-Namespaces, -Warteschlangen oder -Themen konfigurieren. Das Konfigurieren einer [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) für ein Service Bus-Abonnement wird zurzeit nicht unterstützt, aber Sie können Regeln, die für einen Namespace oder ein Thema konfiguriert wurden, auch zum Sichern des Zugriffs auf Abonnements verwenden. Ein funktionierendes Beispiel, das dieses Verfahren veranschaulicht, finden Sie unter [Using Shared Access Signature (SAS) authentication with Service Bus Subscriptions](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) (Verwenden der SAS-Authentifizierung mit Service Bus-Abonnements).

Maximal zwölf solcher Regeln können für einen Service Bus-Namespace, eine Service Bus-Warteschlange oder ein Service Bus-Thema konfiguriert werden. Regeln, die für einen Service Bus-Namespace konfiguriert werden, gelten für alle Entitäten in dem jeweiligen Namespace.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

In dieser Abbildung gelten die Autorisierungsregeln *manageRuleNS*, *sendRuleNS* und *listenRuleNS* sowohl für die Warteschlange Q1 als auch für das Thema T1. *listenRuleQ* und *sendRuleQ* gelten nur für Warteschlange Q1, und *sendRuleT* gilt nur für Thema T1.

Die folgenden Schlüsselparameter gelten für ein [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -Objekt:

| Parameter | Beschreibung |
| --- | --- |
| *KeyName* |Eine Zeichenfolge, die die Autorisierungsregel beschreibt. |
| *PrimaryKey* |Ein primärer Base64-codierter 256-Bit-Schlüssel zum Signieren und Überprüfen des SAS-Tokens. |
| *SecondaryKey* |Ein sekundärer Base64-codierter 256-Bit-Schlüssel zum Signieren und Überprüfen des SAS-Tokens. |
| *AccessRights* |Eine Liste der Zugriffsrechte, die von der Autorisierungsregel erteilt werden. Bei diesen Rechten kann es sich um eine beliebige Auflistung von Lausch-, Sende- und Verwaltungsrechten ("Listen", "Send" und "Manage") handeln. |

Wenn ein Service Bus-Namespace bereitgestellt wird, wird standardmäßig eine [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) erstellt, in der [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) auf **RootManageSharedAccessKey** festgelegt ist.

## <a name="generate-a-shared-access-signature-token"></a>Generieren einer Shared Access Signature (Token)

Die Richtlinie selbst ist nicht das Zugriffstoken für Service Bus. Sie ist vielmehr das Objekt, auf dessen Grundlage das Zugriffstoken unter Verwendung des Primär- oder Sekundärschlüssels generiert wird. Jeder Client, der Zugriff auf die in der SAS-Autorisierungsregel angegebenen Signaturschlüssel besitzt, kann das SAS-Token generieren. Zur Tokengenerierung wird eine Zeichenfolge im folgenden Format erstellt:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

`signature-string` ist der SHA-256-Hash des Tokenbereichs (**Bereich** im Sinne der Beschreibung aus dem vorherigen Abschnitt) mit angefügtem CRLF und einer Ablaufzeit (in Sekunden seit der folgenden Epoche: `00:00:00 UTC` am 1. Januar 1970). 

> [!NOTE]
> Zum Verhindern, dass das Token nach kurzer Zeit abläuft, wird empfohlen, den Ablaufzeitwert als mindestens eine 32-Bit-Ganzzahl ohne Vorzeichen oder vorzugsweise als eine lange (64-Bit) Ganzzahl zu codieren.  
> 
> 

Der Hash ähnelt dem folgenden Pseudocode und gibt 32 Bytes zurück.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Die Werte ohne Hash befinden sich in der Zeichenfolge **SharedAccessSignature**, sodass der Empfänger den Hash mit den gleichen Parametern berechnen und somit sicherstellen kann, dass das gleiche Ergebnis zurückgegeben wird. Der URI gibt den Bereich und der Schlüsselname die Richtlinie an, der bzw. die bei der Hashberechnung verwendet werden soll. Dies ist ein wichtiger Sicherheitsaspekt. Stimmt die Signatur nicht mit dem Berechnungsergebnis des Empfängers (Service Bus) überein, wird der Zugriff verweigert. An diesem Punkt können Sie sicher sein, dass der Absender Zugriff auf den Schlüssel hatte und ihm die in der Richtlinie angegebenen Rechte gewährt werden sollen.

Beachten Sie, dass Sie den codierten Ressourcen-URI für diesen Vorgang verwenden sollten. Der Ressourcen-URI ist der vollständige URI der Service Bus-Ressource, auf die der Zugriff beansprucht wird.  Beispiel: `http://<namespace>.servicebus.windows.net/<entityPath>` oder `sb://<namespace>.servicebus.windows.net/<entityPath>`, also `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`.

Die zum Signieren verwendete SAS-Autorisierungsregel muss für die durch diesen URI angegebene Entität oder eines seiner hierarchisch übergeordneten Elemente konfiguriert werden. Beispiel: `http://contoso.servicebus.windows.net/contosoTopics/T1` oder `http://contoso.servicebus.windows.net` im vorherigen Beispiel.

Ein SAS-Token ist für alle Ressourcen unter dem `<resourceURI>` gültig, der in der `signature-string` verwendet wird.

Der [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) im SAS-Token bezieht sich auf den **keyName** der SAS-Autorisierungsregel, die zum Generieren des Tokens verwendet wird.

*URL-encoded-resourceURI* muss mit dem URI identisch sein, der in der Zeichenfolge für die Signatur während der Berechnung der Signatur verwendet wird. Er sollte als [Prozentwert codiert](https://msdn.microsoft.com/library/4fkewx0t.aspx)sein.

Es wird empfohlen, die im [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -Objekt verwendeten Schlüssel in regelmäßigen Abständen neu zu generieren. Anwendungen sollten im Allgemeinen den [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) zum Generieren eines SAS-Tokens verwenden. Wenn die Schlüssel erneut generiert werden, sollten Sie den [SecondaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) durch den alten primären Schlüssel ersetzen und dann einen neuen Schlüssel als neuen primären Schlüssel generieren. Auf diese Weise können Sie Token weiterhin für die Autorisierung verwenden, die mit dem alten primären Schlüssel ausgestellt wurden und noch nicht abgelaufen sind.

Wenn ein Schlüssel gefährdet ist und Sie die Schlüssel widerrufen müssen, können Sie sowohl den [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) als auch den [SecondaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) eines [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)-Objekts neu generieren und so durch neue Schlüssel ersetzen. Durch dieses Verfahren werden alle Token, die mit den alten Schlüsseln signiert wurden, für ungültig erklärt.

## <a name="how-to-use-shared-access-signature-authentication-with-service-bus"></a>Verwenden der SAS-Authentifizierung mit Service Bus

Die folgenden Szenarien umfassen die Konfiguration von Autorisierungsregeln, das Generieren von SAS-Token und die Clientautorisierung.

Ein vollständiges praktisches Beispiel für eine Service Bus-Anwendung, die die Konfiguration veranschaulicht und die SAS-Autorisierung verwendet, finden Sie unter [SAS-Authentifizierung bei Service Bus](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Ein Beispiel, das die Verwendung von in Namespaces oder Themen konfigurierten SAS-Autorisierungsregeln zum Absichern von Service Bus-Abonnements veranschaulicht, finden Sie hier: [Using Shared Access Signature (SAS) authentication with Service Bus Subscriptions](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c)(in englischer Sprache).

## <a name="access-shared-access-authorization-rules-on-a-namespace"></a>Zugreifen auf SAS-Autorisierungsregeln für einen Namespace

Vorgänge für den Service Bus-Namespacestamm erfordern eine Zertifikatauthentifizierung. Sie müssen ein Verwaltungszertifikat für Ihr Azure-Abonnement hochladen. Führen Sie [diese](../cloud-services/cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate) Schritte aus, um ein Verwaltungszertifikat mithilfe des [Azure-Portals][Azure portal] hochzuladen. Weitere Informationen zu Azure-Verwaltungszertifikaten finden Sie in der [Übersicht über Azure-Zertifikate](../cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Der Endpunkt für den Zugriff auf SAS-Autorisierungsregeln für einen Service Bus-Namespace lautet wie folgt:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/
```

Um ein [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -Objekt für einen Service Bus-Namespace zu erstellen, führen Sie einen POST-Vorgang für diesen Endpunkt mit den Regelinformationen aus, die als JSON oder XML serialisiert wurden. Beispiel:

```csharp
// Base address for accessing authorization rules on a namespace
string baseAddress = @"https://management.core.windows.net/<subscriptionId>/services/ServiceBus/namespaces/<namespace>/AuthorizationRules/";

// Configure authorization rule with base64-encoded 256-bit key and Send rights
var sendRule = new SharedAccessAuthorizationRule("contosoSendAll",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send });

// Operations on the Service Bus namespace root require certificate authentication.
WebRequestHandler handler = new WebRequestHandler
{
    ClientCertificateOptions = ClientCertificateOption.Manual
};
// Access the management certificate by subject name
handler.ClientCertificates.Add(GetCertificate(<certificateSN>));

HttpClient httpClient = new HttpClient(handler)
{
    BaseAddress = new Uri(baseAddress)
};
httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
httpClient.DefaultRequestHeaders.Add("x-ms-version", "2015-01-01");

// Execute a POST operation on the baseAddress above to create an auth rule
var postResult = httpClient.PostAsJsonAsync("", sendRule).Result;
```

Verwenden Sie in ähnlicher Weise einen GET-Vorgang für den Endpunkt, um die für den Namespace konfigurierten Autorisierungsregeln zu lesen.

Zum Aktualisieren oder Löschen einer bestimmten Autorisierungsregel verwenden Sie den folgenden Endpunkt:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/{KeyName}
```

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Zugreifen auf SAS-Autorisierungsregeln für eine Entität

Sie können auf ein [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)-Objekt, das in einer Service Bus-Warteschlange oder einem Service Bus-Thema konfiguriert wurde, über die [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules)-Sammlung der entsprechenden [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription)- oder [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription)-Objekte zugreifen.

Der folgende Code zeigt, wie Sie Autorisierungsregeln für eine Warteschlange hinzufügen.

```csharp
// Create an instance of NamespaceManager for the operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it to the queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create the queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Verwenden der Shared Access Signature-Authentifizierung

Anwendungen, die das Azure .NET SDK mit den .NET-Bibliotheken von Service Bus nutzen, können die SAS-Autorisierung über die [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) -Klasse verwenden. Der folgende Code veranschaulicht die Verwendung des Tokenanbieters zum Senden von Nachrichten an eine Service Bus-Warteschlange.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message to queue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Anwendungen können SAS auch zur Authentifizierung verwenden, indem sie eine SAS-Verbindungszeichenfolge in Methoden einsetzen, die Verbindungszeichenfolgen akzeptieren.

Beachten Sie, dass Sie zum Verwenden der SAS-Autorisierung mit Service Bus Relays SAS-Schlüssel nutzen können, die für den Service Bus-Namespace konfiguriert sind. Wenn Sie ein Relay explizit im Namespaceobjekt ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) mit [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription)) erstellen, können Sie die SAS-Regeln nur für dieses jeweilige Relay festlegen. Zum Verwenden der SAS-Autorisierung mit Service Bus-Abonnements können Sie SAS-Schlüssel nutzen, die für einen Service Bus-Namespace oder ein Thema konfiguriert sind.

## <a name="use-the-shared-access-signature-at-http-level"></a>Verwenden einer Shared Access Signature (auf HTTP-Ebene)

Nachdem Sie nun mit der SAS-Erstellung für beliebige Entitäten in Service Bus vertraut sind, können Sie einen Vorgang vom Typ „HTTP POST“ ausführen:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Das funktioniert überall. Sie können SAS für eine Warteschlange, ein Thema oder ein Abonnement erstellen. 

Wenn Sie einem Absender oder Client ein SAS-Token zuweisen, verfügt er nicht direkt über den Schlüssel, und er kann den Hash nicht umkehren und so den Schlüssel ermitteln. Dadurch haben Sie die Kontrolle darüber, worauf er wie lange Zugriff hat. Vergessen Sie nicht: Wenn Sie den Primärschlüssel neu generieren oder in der Richtlinie ändern, werden alle auf deren Grundlage erstellten SAS ungültig.

## <a name="use-the-shared-access-signature-at-amqp-level"></a>Verwenden der Shared Access Signature (auf AMQP-Ebene)

Im vorherigen Abschnitt wurde gezeigt, wie Sie das SAS-Token mit einer HTTP POST-Anforderung zum Senden von Daten an den Service Bus verwenden. Wie Sie wissen, können Sie mit dem AMQP (Advanced Message Queue Protocol) auf Service Bus zugreifen, das in zahlreichen Szenarien aus Leistungsgründen bevorzugt verwendet wird. Die Verwendung des SAS-Tokens mit AMQP wird im folgenden Dokument beschrieben: [Anspruchsbasierte Sicherheit mit AMQP Version 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc). Dieses Dokument ist seit 2013 eine funktionierende Entwurfsversion, die jedoch umfassend von Azure unterstützt wird.

Bevor Sie beginnen, Daten an Service Bus zu senden, muss der Herausgeber das SAS-Token in einer AMQP-Nachricht an einen ordnungsgemäß definierten AMQP-Knoten mit dem Namen **$cbs** senden (dieser kann als spezielle Warteschlange betrachtet werden, die vom Dienst zum Abrufen und Überprüfen aller SAS-Token verwendet wird). Der Herausgeber muss das Feld **ReplyTo** in der AMQP-Nachricht angeben. Hierbei handelt es sich um den Knoten, den der Dienst für die Antwort an den Herausgeber mit dem Ergebnis der Tokenüberprüfung verwendet (einfaches Anforderungs-/Antwortmuster zwischen Herausgeber und Dienst). Dieser spontan erstellte Antwortknoten beschäftigt sich mit der dynamischen Erstellung von Remoteknoten (siehe AMQP 1.0-Spezifikation). Nachdem die Gültigkeit des SAS-Tokens überprüft wurde, kann der Verleger mit dem Senden von Daten an den Dienst beginnen.

Die folgenden Schritte zeigen, wie das SAS-Token mit AMQP anhand der [AMQP.Net Lite](https://github.com/Azure/amqpnetlite)-Bibliothek gesendet wird. Dies ist hilfreich, wenn Sie das offizielle Service Bus SDK bei der Entwicklung in C\# nicht verwenden können (z.B. bei WinRT, .NET Compact Framework, .NET Micro Framework und Mono). Dank dieser nützlichen Bibliothek können Sie nachvollziehen, wie die anspruchsbasierte Sicherheit auf AMQP-Ebene funktioniert. Wie sie auf HTTP-Ebene funktioniert, haben Sie bereits gesehen (HTTP POST-Anforderung und SAS-Token werden im Autorisierungsheader gesendet). Wenn Sie nicht so genau über AMQP Bescheid wissen müssen, können Sie für .NET Framework-Anwendungen das offizielle Service Bus-SDK, das die erforderlichen Schritte für Sie ausführt verwenden.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Die `PutCbsToken()`-Methode empfängt die *Verbindung* (von der [AMQP .NET Lite-Bibliothek](https://github.com/Azure/amqpnetlite) bereitgestellte AMQP-Verbindungsklasseninstanz), die die TCP-Verbindung mit dem Dienst darstellt, und den *sasToken*-Parameter, bei dem es sich um das zu sendende SAS-Token handelt. 

> [!NOTE]
> Beim Erstellen der Verbindung muss der **SASL-Authentifizierungsmechanismus auf EXTERNAL festgelegt** werden (nicht auf den Standardmechanismus PLAIN mit Benutzername und Kennwort – dieser wird verwendet, wenn Sie das SAS-Token nicht senden müssen).
> 
> 

Im nächsten Schritt erstellt der Herausgeber zwei AMQP-Links zum Senden des SAS-Tokens und zum Empfangen der Antwort (Ergebnis der Tokenüberprüfung) vom Dienst.

Die AMQP-Nachricht enthält eine Reihe von Eigenschaften und mehr Informationen als eine einfache Nachricht. Das SAS-Token ist der Text der Nachricht (mit dem entsprechenden Konstruktor). Als **ReplyTo** -Eigenschaft wird der Knotenname zum Empfangen des Überprüfungsergebnisses über den Empfängerlink festgelegt. (Sie können den Namen nach Bedarf ändern, der Link wird vom Dienst dynamisch erstellt.) Anhand der letzten drei Anwendungseigenschaften/benutzerdefinierten Eigenschaften gibt der Dienst an, welche Art von Vorgang ausgeführt werden soll. Wie in der CBS-Entwurfsspezifikation beschrieben, müssen hier der **Vorgangsname** („put-token“), der **Tokentyp** (in diesem Fall „servicebus.windows.net:sastoken“) und der **„Name“ der Zielgruppe** angegeben werden, für die das Token gilt (die gesamte Entität).

Nach dem Senden des SAS-Tokens über den Senderlink muss der Herausgeber die Antwort über den Empfängerlink lesen. Bei der Antwort handelt es sich um eine einfache AMQP-Nachricht mit einer Anwendungseigenschaft namens **status-code** , die dieselben Werte wie ein HTTP-Statuscode enthalten kann.

## <a name="rights-required-for-service-bus-operations"></a>Erforderliche Rechte für Service Bus-Vorgänge

Die folgende Tabelle zeigt die Zugriffsrechte, die für verschiedene Vorgänge für Service Bus-Ressourcen erforderlich sind.

| Vorgang | Erforderlicher Anspruch | Anspruchsbereich |
| --- | --- | --- |
| **Namespace** | | |
| Konfigurieren einer Autorisierungsregel für einen Namespace |Verwalten |Jede Namespaceadresse |
| **Dienstregistrierung** | | |
| Aufzählen privater Richtlinien |Verwalten |Jede Namespaceadresse |
| Starten des Lauschvorgangs an einem Namespace |Empfangen |Jede Namespaceadresse |
| Senden von Nachrichten an einen Listener in einem Namespace |Send |Jede Namespaceadresse |
| **Warteschlange** | | |
| Erstellen einer Warteschlange |Verwalten |Jede Namespaceadresse |
| Löschen einer Warteschlange |Verwalten |Beliebige gültige Warteschlangenadresse |
| Aufzählen von Warteschlangen |Verwalten |/$Resources/Queues |
| Abrufen der Warteschlangenbeschreibung |Verwalten |Beliebige gültige Warteschlangenadresse |
| Konfigurieren einer Autorisierungsregel für eine Warteschlange |Verwalten |Beliebige gültige Warteschlangenadresse |
| Senden in die Warteschlange |Send |Beliebige gültige Warteschlangenadresse |
| Empfangen von Nachrichten aus einer Warteschlange |Empfangen |Beliebige gültige Warteschlangenadresse |
| Verwerfen oder Abschließen von Nachrichten nach dem Empfang der Nachricht im Peek/Lock-Modus |Empfangen |Beliebige gültige Warteschlangenadresse |
| Zurückstellen einer Nachricht für den späteren Abruf |Empfangen |Beliebige gültige Warteschlangenadresse |
| Platzieren einer Nachricht in die Warteschlange für unzustellbare Nachrichten |Empfangen |Beliebige gültige Warteschlangenadresse |
| Abrufen des einer Nachrichtenwarteschlangensitzung zugeordneten Status |Empfangen |Beliebige gültige Warteschlangenadresse |
| Festlegen des einer Nachrichtenwarteschlangensitzung zugeordneten Status |Empfangen |Beliebige gültige Warteschlangenadresse |
| **Thema** | | |
| Erstellen eines Themas |Verwalten |Jede Namespaceadresse |
| Löschen eines Themas |Verwalten |Beliebige gültige Themenadresse |
| Auflisten von Themen |Verwalten |/$Resources/Topics |
| Abrufen der Themenbeschreibung |Verwalten |Beliebige gültige Themenadresse |
| Konfigurieren einer Autorisierungsregel für ein Thema |Verwalten |Beliebige gültige Themenadresse |
| Senden an das Thema |Send |Beliebige gültige Themenadresse |
| **Abonnement** | | |
| Erstellen eines Abonnements |Verwalten |Jede Namespaceadresse |
| Löschen eines Abonnements |Verwalten |../myTopic/Subscriptions/mySubscription |
| Aufzählen von Abonnements |Verwalten |../myTopic/Subscriptions |
| Abrufen der Abonnementbeschreibung |Verwalten |../myTopic/Subscriptions/mySubscription |
| Verwerfen oder Abschließen von Nachrichten nach dem Empfang der Nachricht im Peek/Lock-Modus |Empfangen |../myTopic/Subscriptions/mySubscription |
| Zurückstellen einer Nachricht für den späteren Abruf |Empfangen |../myTopic/Subscriptions/mySubscription |
| Platzieren einer Nachricht in die Warteschlange für unzustellbare Nachrichten |Empfangen |../myTopic/Subscriptions/mySubscription |
| Abrufen des einer Themensitzung zugeordneten Status |Empfangen |../myTopic/Subscriptions/mySubscription |
| Festlegen des einer Themensitzung zugeordneten Status |Empfangen |../myTopic/Subscriptions/mySubscription |
| **Regeln** | | |
| Erstellen einer Regel |Verwalten |../myTopic/Subscriptions/mySubscription |
| Löschen einer Regel |Verwalten |../myTopic/Subscriptions/mySubscription |
| Aufzählen von Regeln |Verwalten oder Lauschen |../myTopic/Subscriptions/mySubscription/Rules 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Service Bus Messaging finden Sie in folgenden Themen.

* [Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Verwenden von Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com
