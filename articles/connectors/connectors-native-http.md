---
title: Hinzufügen der HTTP-Aktion in Logik-Apps | Microsoft Docs
description: Übersicht über die HTTP-Aktion mit Eigenschaften
services: ''
documentationcenter: ''
author: jeffhollan
manager: erikre
editor: ''
tags: connectors

ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan

---
# Erste Schritte mit der HTTP-Aktion
Mit der HTTP-Aktion können Sie Workflows für Ihre Organisation erweitern und mit einem beliebigen Endpunkt über HTTP kommunizieren.

Sie können:

* Erstellen Sie Logik-App-Workflows, die aktiviert (durch einen Trigger ausgelöst) werden, wenn eine von Ihnen verwaltete Website ausfällt.
* Kommunizieren Sie mit jedem beliebigen Endpunkt über HTTP, um Ihre Workflows auf andere Dienste auszuweiten.

Wenn Sie die HTTP-Aktion in einer Logik-App verwenden möchten, müssen Sie zunächst eine [Logik-App erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Verwenden des HTTP-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md).

Im Anschluss finden Sie eine Beispielsequenz für die Einrichtung eines HTTP-Triggers im Logik-App-Designer.

1. Fügen Sie den HTTP-Trigger Ihrer Logik-App hinzu.
2. Geben Sie die Parameter für den abzurufenden HTTP-Endpunkt an.
3. Ändern Sie das Wiederholungsintervall, um die gewünschte Abrufhäufigkeit anzugeben.
4. Die Logik-App wird nun mit den Inhalten ausgelöst, die bei den jeweiligen Überprüfungen zurückgegeben werden.

![HTTP-Trigger](./media/connectors-native-http/using-trigger.png)

### Funktionsweise des HTTP-Triggers
Der HTTP-Trigger ruft in einem bestimmten Wiederholungsintervall einen HTTP-Endpunkt auf. Standardmäßig führt jeder HTTP-Antwortcode < 300 dazu, dass die Logik-App ausgeführt wird. In der Codeansicht können Sie eine Bedingung hinzufügen, um nach dem HTTP-Aufruf eine Überprüfung durchzuführen und zu ermitteln, ob die Logik-App ausgelöst werden soll. Im Anschluss finden Sie ein Beispiel für einen HTTP-Trigger, der ausgelöst wird, wenn der zurückgegebene Statuscode mindestens `400` beträgt.

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

Vollständige Informationen zu den Parametern der HTTP-Trigger sind im [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger) verfügbar.

## Verwenden der HTTP-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](connectors-overview.md).

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.
2. Wählen Sie **Aktion hinzufügen** aus.
3. Geben Sie im Aktionssuchfeld die Zeichenfolge **http** ein, um die HTTP-Aktion anzuzeigen.
   
    ![Wählen der HTTP-Aktion](./media/connectors-native-http/using-action-1.png)
4. Geben Sie alle Parameter an, die ggf. für den HTTP-Aufruf erforderlich sind.
   
    ![Ausführen der HTTP-Aktion](./media/connectors-native-http/using-action-2.png)
5. Klicken Sie zum Speichern auf die linke obere Ecke der Symbolleiste. Ihre Logik-App wird sowohl gespeichert als auch veröffentlicht (aktiviert).

## HTTP-Trigger
Hier finden Sie Details zu dem Trigger, den dieser Connector unterstützt. Der HTTP-Connector besitzt einen Trigger.

| Trigger | Beschreibung |
| --- | --- |
| HTTP |Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück. |

## HTTP-Aktion
Hier finden Sie Details zu der Aktion, die dieser Connector unterstützt. Der HTTP-Connector verfügt über eine mögliche Aktion.

| Aktion | Beschreibung |
| --- | --- |
| HTTP |Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück. |

## HTTP-Details
Die folgenden Tabellen beschreiben die erforderlichen und optionalen Eingabefelder für die Aktion und die entsprechenden Ausgabedetails, die der Verwendung dieser Aktion zugeordnet sind.

#### HTTP-Anforderung
Im Folgenden werden die Eingabefelder für die Aktion angegeben, die eine ausgehende HTTP-Anforderung ausführt. Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

| Anzeigename | Eigenschaftenname | Beschreibung |
| --- | --- | --- |
| Methode* |method |Zu verwendendes HTTP-Verb |
| URI* |uri |Der URI für die HTTP-Anforderung |
| Headers |headers |Ein JSON-Objekt für die einzubeziehenden Header |
| Body |body |Der HTTP-Anforderungstext |
| Authentifizierung |Authentifizierung |Details im Abschnitt [Authentifizierung](#authentication) |

<br>

#### Ausgabedetails
Im Folgenden werden die Ausgabedetails für die HTTP-Antwort angegeben.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| Headers |Objekt |Antwortheader |
| Body |Objekt |Antwortobjekt |
| Statuscode |int |HTTP-Statuscode |

## Authentifizierung
Mit dem Logik-Apps-Feature von Azure App Service können Sie verschiedene Authentifizierungstypen bei HTTP-Endpunkten verwenden. Sie können diese Authentifizierung mit den Connectors **HTTP**, **[HTTP + Swagger](connectors-native-http-swagger.md)** und **[HTTP-Webhook](connectors-native-webhook.md)** nutzen. Die folgenden Arten der Authentifizierung können konfiguriert werden:

* [Standardauthentifizierung](#basic-authentication)
* [Clientzertifikatsauthentifizierung](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth-Authentifizierung](#azure-active-directory-oauth-authentication)

#### Standardauthentifizierung
Das folgende Authentifizierungsobjekt ist für die Standardauthentifizierung erforderlich. Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| Typ* |Typ |Authentifizierungstyp (muss für die Standardauthentifizierung `Basic` sein) |
| Benutzername* |username |Zu authentifizierender Benutzername |
| Kennwort* |password |Das zu authentifizierende Kennwort |

> [!TIP]
> Wenn Sie ein Kennwort verwenden möchten, das aus der Definition nicht abgerufen werden kann, verwenden Sie einen `securestring`-Parameter und die `@parameters()`-[Funktion für die Workflowdefinition](http://aka.ms/logicappdocs).
> 
> 

Sie erstellen also ein Objekt wie das folgende im Authentifizierungsfeld:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### Clientzertifikatsauthentifizierung
Das folgende Authentifizierungsobjekt ist für die Clientzertifikatsauthentifizierung erforderlich. Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| Typ* |Typ |Der Typ der Authentifizierung (Für SSL-Clientzertifikate muss der Wert auf `ClientCertificate` festgelegt werden.) |
| PFX* |pfx |Der Base64-codierte Inhalt der Personal Information Exchange-Datei (PFX) |
| Kennwort* |password |Das Kennwort für den Zugriff auf die PFX-Datei |

> [!TIP]
> Sie können einen `securestring`-Parameter und die `@parameters()`-[Funktion für die Workflowdefinition](http://aka.ms/logicappdocs) verwenden, um einen Parameter zu nutzen, der in der Definition nach dem Speichern der Logik-App nicht lesbar ist.
> 
> 

Beispiel:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### Azure AD-OAuth-Authentifizierung
Das folgende Authentifizierungsobjekt ist für die Azure AD-OAuth-Authentifizierung erforderlich. Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| Typ* |Typ |Der Authentifizierungstyp (muss für Azure AD OAuth `ActiveDirectoryOAuth` sein) |
| Mandant* |Mandant |Die Mandanten-ID für den Azure AD-Mandanten |
| Zielgruppe* |audience |Legen Sie den Wert `https://management.core.windows.net/` fest. |
| Client-ID* |clientId |Die Client-ID für die Azure AD-Anwendung |
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

## Nächste Schritte
Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

<!---HONumber=AcomDC_0810_2016-->