---
title: "Kommunizieren mit Endpunkten über HTTP – Azure Logic Apps | Microsoft-Dokumentation"
description: "Erstellen von Logik-Apps, die über HTTP mit einem beliebigen Endpunkt kommunizieren können"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan; LADocs
translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: d422a07a27ffa62a673bd2d471ae4fc837251dee
ms.lasthandoff: 04/11/2017


---
# <a name="get-started-with-the-http-action"></a>Erste Schritte mit der HTTP-Aktion

Mit der HTTP-Aktion können Sie Workflows für Ihre Organisation erweitern und mit einem beliebigen Endpunkt über HTTP kommunizieren.

Sie können:

* Erstellen Sie Logik-App-Workflows, die aktiviert (durch einen Trigger ausgelöst) werden, wenn eine von Ihnen verwaltete Website ausfällt.
* Kommunizieren Sie mit jedem beliebigen Endpunkt über HTTP, um Ihre Workflows auf andere Dienste auszuweiten.

Wenn Sie die HTTP-Aktion in einer Logik-App verwenden möchten, müssen Sie zunächst eine [Logik-App erstellen](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>Verwenden des HTTP-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md).

Im Anschluss finden Sie eine Beispielsequenz für die Einrichtung eines HTTP-Triggers im Logik-App-Designer.

1. Fügen Sie den HTTP-Trigger Ihrer Logik-App hinzu.
2. Geben Sie die Parameter für den abzurufenden HTTP-Endpunkt an.
3. Ändern Sie das Wiederholungsintervall, um die gewünschte Abrufhäufigkeit anzugeben.

   Die Logik-App wird nun mit den Inhalten ausgelöst, die bei den jeweiligen Überprüfungen zurückgegeben werden.

   ![HTTP-Trigger](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>Funktionsweise des HTTP-Triggers

Der HTTP-Trigger sendet in einem bestimmten Wiederholungsintervall einen Aufruf an einen HTTP-Endpunkt. Standardmäßig bewirkt jeder HTTP-Antwortcode, der niedriger als 300 ist, das Ausführen einer Logik-App. Um anzugeben, ob die Logik-App ausgelöst werden soll, können Sie die Logik-App in der Codeansicht bearbeiten und eine Bedingung hinzufügen, die nach dem HTTP-Aufruf ausgewertet wird. Im Anschluss finden Sie ein Beispiel für einen HTTP-Trigger, der ausgelöst wird, wenn der zurückgegebene Statuscode mindestens `400` beträgt.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Vollständige Informationen zu den Parametern der HTTP-Trigger sind im [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger)verfügbar.

## <a name="use-the-http-action"></a>Verwenden der HTTP-Aktion

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. 
Weitere Informationen zu Aktionen finden Sie [hier](connectors-overview.md).

1. Wählen Sie **Neuer Schritt** > **Aktion hinzufügen** aus.
3. Geben Sie im Aktionssuchfeld die Zeichenfolge **http** ein, um die HTTP-Aktionen aufzulisten.
   
    ![Wählen der HTTP-Aktion](./media/connectors-native-http/using-action-1.png)

4. Fügen Sie alle erforderlichen Parameter für den HTTP-Aufruf hinzu.
   
    ![Ausführen der HTTP-Aktion](./media/connectors-native-http/using-action-2.png)

5. Klicken Sie auf der Symbolleiste des Designers auf **Speichern**. Die Logik-App wird gespeichert und gleichzeitig veröffentlicht (aktiviert).

## <a name="http-trigger"></a>HTTP-Trigger
Hier finden Sie Details zu dem Trigger, den dieser Connector unterstützt. Der HTTP-Connector besitzt einen Trigger.

| Trigger | Beschreibung |
| --- | --- |
| http |Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück. |

## <a name="http-action"></a>HTTP-Aktion
Hier finden Sie Details zu der Aktion, die dieser Connector unterstützt. Der HTTP-Connector verfügt über eine mögliche Aktion.

| Aktion | Beschreibung |
| --- | --- |
| http |Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück. |

## <a name="http-details"></a>HTTP-Details
Die folgenden Tabellen beschreiben die erforderlichen und optionalen Eingabefelder für die Aktion und die entsprechenden Ausgabedetails, die der Verwendung dieser Aktion zugeordnet sind.

#### <a name="http-request"></a>HTTP-Anforderung
Im Folgenden werden die Eingabefelder für die Aktion angegeben, die eine ausgehende HTTP-Anforderung ausführt.
Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

| Anzeigename | Eigenschaftenname | Beschreibung |
| --- | --- | --- |
| Methode* |method |Zu verwendendes HTTP-Verb |
| URI* |uri |Der URI für die HTTP-Anforderung |
| Headers |Headers |Ein JSON-Objekt für die einzubeziehenden Header |
| Body |Body |Der HTTP-Anforderungstext |
| Authentifizierung |Authentifizierung |Details im Abschnitt [Authentifizierung](#authentication) |

<br>

#### <a name="output-details"></a>Ausgabedetails
Im Folgenden werden die Ausgabedetails für die HTTP-Antwort angegeben.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| headers |Objekt |Antwortheader |
| Body |Objekt |Antwortobjekt |
| Statuscode |int |HTTP-Statuscode |

## <a name="authentication"></a>Authentifizierung
Das Logic Apps-Feature ermöglicht die Verwendung verschiedener Arten von Authentifizierung bei HTTP-Endpunkten. Sie können diese Authentifizierung mit den Connectors **HTTP**, **[HTTP + Swagger](connectors-native-http-swagger.md)** und **[HTTP-Webhook](connectors-native-webhook.md)** nutzen. Die folgenden Arten der Authentifizierung können konfiguriert werden:

* [Standardauthentifizierung](#basic-authentication)
* [Clientzertifikatsauthentifizierung](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth-Authentifizierung](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Standardauthentifizierung

Das folgende Authentifizierungsobjekt ist für die Standardauthentifizierung erforderlich.
Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| Typ* |Typ |Authentifizierungstyp (muss für die Standardauthentifizierung `Basic` sein) |
| Benutzername* |username |Zu authentifizierender Benutzername |
| Kennwort* |password |Das zu authentifizierende Kennwort |

> [!TIP]
> Wenn Sie ein Kennwort verwenden möchten, das aus der Definition nicht abgerufen werden kann, verwenden Sie einen `securestring`-Parameter und die `@parameters()` 
> [-Funktion für die Workflowdefinition](http://aka.ms/logicappdocs).

Beispiel:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Clientzertifikatsauthentifizierung

Das folgende Authentifizierungsobjekt ist für die Clientzertifikatsauthentifizierung erforderlich. Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| Typ* |Typ |Der Typ der Authentifizierung (Für SSL-Clientzertifikate muss der Wert auf `ClientCertificate` festgelegt werden.) |
| PFX* |pfx |Der Base64-codierte Inhalt der Personal Information Exchange-Datei (PFX) |
| Kennwort* |password |Das Kennwort für den Zugriff auf die PFX-Datei |

> [!TIP]
> Wenn Sie einen Parameter nutzen möchten, der in der Definition nach dem Speichern der Logik-App nicht lesbar ist, können Sie einen `securestring`-Parameter und die `@parameters()` 
> [-Funktion für die Workflowdefinition verwenden](http://aka.ms/logicappdocs).

Beispiel:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD-OAuth-Authentifizierung
Das folgende Authentifizierungsobjekt ist für die Azure AD-OAuth-Authentifizierung erforderlich. Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| Typ* |Typ |Der Authentifizierungstyp (muss für Azure AD OAuth `ActiveDirectoryOAuth` sein) |
| Mandant* |Mandant |Die Mandanten-ID für den Azure AD-Mandanten |
| Zielgruppe* |audience |Die Ressource, für deren Verwendung Sie die Autorisierung anfordern. Beispiel: `https://management.core.windows.net/` |
| Client-ID* |clientId |Die Client-ID für die Azure AD-Anwendung |
| Geheimer Schlüssel* |secret |Der geheime Schlüssel des Clients, der das Token anfordert |

> [!TIP]
> Sie können einen `securestring`-Parameter und die `@parameters()`-[Funktion für die Workflowdefinition](http://aka.ms/logicappdocs) verwenden, um einen Parameter zu nutzen, der in der Definition nach dem Speichern nicht lesbar ist.
> 
> 

Beispiel:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Nächste Schritte
Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.


