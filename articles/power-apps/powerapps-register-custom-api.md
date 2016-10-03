<properties
	pageTitle="Gewusst wie: Verwenden von benutzerdefinierten APIs in logischen Abläufen und PowerApps | Microsoft Azure"
	description="Beschreibung von benutzerdefinierten APIs, Nutzung von OAuth-Anbietern und Verwendung von Swagger zum Hinzufügen von benutzerdefinierten APIs in PowerApps und logischen Abläufen."
	services=""
    suite="powerapps"
	documentationCenter="" 
	authors="sunaysv"
	manager="erikre"
	editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/12/2016"
   ms.author="mandia"/>

# Was sind benutzerdefinierte APIs?

Benutzerdefinierte APIs sind alle RESTful-APIs, die Sie einbinden und mit Ihren PowerApps und logischen Abläufen (Logic Flows) verwenden können. Diese APIs können überall gehostet werden, solange eine gut dokumentierte Spezifikation vorhanden ist, die den [OpenAPI][1]-Standard erfüllt.

>[AZURE.IMPORTANT] Dieses Thema finden Sie nun auf „powerapps.microsoft.com“ unter [Was sind benutzerdefinierte APIs?](https://powerapps.microsoft.com/tutorials/register-custom-api/) verschoben. Die aktuelle Version finden Sie in PowerApps. Dieser Azure-Link wird archiviert.

## Voraussetzungen

- Benutzerdefinierte APIs können von allen Personen verwendet werden, die PowerApps nutzen. PowerApps Enterprise ist nicht erforderlich.
- Benötigt werden eine Swagger-Datei (JSON) und ein Symbol für Ihre benutzerdefinierte API. Dieses Thema enthält mehrere Optionen zum Erstellen der Swagger-Datei. Als Symbol können Sie ein beliebiges Bild verwenden.


## Authentifizierung

Sie können einen der folgenden Authentifizierungsmechanismen verwenden:

- Standardauthentifizierung
- OAuth 2.0: Unten sind alle unterstützten OAuth 2.0-Anbieter aufgeführt, die Sie mit Ihrer benutzerdefinierten API verwenden können (die Unterstützung wird in Kürze erweitert):

	- Azure Active Directory
	- Box
	- Dropbox
	- Facebook
	- Google
	- Instagram
	- OneDrive
	- SalesForce
	- Puffer
	- Yammer

> [AZURE.NOTE] Die Unterstützung für die Authentifizierung per API-Schlüssel wird in Kürze hinzugefügt.


Weitere Informationen zum Angeben des Authentifizierungstyps finden Sie in Ihrem OpenAPI-Dokument (Swagger) unter [OpenAPI Specification][1] \(OpenAPI-Spezifikation).

Wenn Ihr API-Endpunkt den nicht authentifizierten Zugriff zulässt, sollten Sie das ```securityDefintions```-Objekt aus der OpenAPI-Datei (Swagger) entfernen. Entfernen Sie im folgenden Beispiel das ```securityDefintions```-Objekt vollständig:

```
  "securityDefinitions": {
    "AAD": {
      "type": "oauth2",
      "flow": "implicit",
      "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
      "scopes": []
    }
  },
```

> [AZURE.TIP] In JSON-Dateien können Sie keine Kommentare hinzufügen. Der gesamte Text wird als Teil der Daten angesehen.

### Beispiele für die Authentifizierung
* [Azure Resource Manager](powerapps-azure-resource-manager-tutorial.md) mit AAD-Authentifizierung
* [Azure WebApp](powerapps-web-api-tutorial.md) mit AAD-Authentifizierung

## Registrieren einer benutzerdefinierten API

### Schritt 1: Erstellen einer Swagger-Datei

Sie können aus **jedem** API-Endpunkt eine Swagger-Datei erstellen, z.B.:

- Jede API, die veröffentlicht wurde und öffentlich verfügbar ist. Beispiele hierfür sind [Spotify][2], [Uber][3], [Slack][4], [Rackspace][5] usw.
- Eine API, die Sie erstellen und für einen Clouddienst bereitstellen, in dem Sie Web-Apps bereitstellen können, z.B. Amazon Web Services (AWS), Heroku, Azure-Web-Apps, Google Cloud usw.
- Eine API, die in Ihrem Netzwerk oder auf Ihrem Computer bereitgestellt wird, solange die API im Internet öffentlich verfügbar ist.

Wenn Sie die Swagger-Datei erstellen, wird eine JSON-Datei erstellt. Halten Sie diese JSON-Datei bereit.

#### Erhalten von Hilfe zum Erstellen von Swagger-Dateien

- Wenn die Erstellung einer Swagger-Datei neu für Sie ist oder Sie noch nie eine Swagger-Datei erstellt haben, ist die Seite [Get started with Swagger][6] \(Erste Schritte mit Swagger) hilfreich.
 
- Um Ihre eigene API zu erstellen, stellen Sie sie in Azure bereit und erstellen basierend auf dieser neuen API eine Swagger-Datei. Anschließend können Sie bei Bedarf das [Web-API-Tutorial](powerapps-web-api-tutorial.md) durcharbeiten. In diesem Tutorial erhalten Sie eine funktionierende Swagger-Datei. Auf GitHub finden Sie außerdem ein [Hello World-Beispiel][7].

- Verwenden Sie zum Überprüfen Ihrer Swagger-Dateien den [Swagger-Editor][8]. Sie können Ihre JSON-Daten einfügen, und die Überprüfung wird dann automatisch durchgeführt.

- Informationen zum Anpassen Ihres Swagger-Dokuments für die Verwendung mit PowerApps und Logic Flows finden Sie unter [Anpassen der Swagger-Definition](powerapps-how-to-swagger.md).

### Schritt 2: Hinzufügen einer Verbindung mit der benutzerdefinierten API
Nachdem die Swagger-Datei (JSON-Datei) für die benutzerdefinierte API generiert wurde, fügen Sie die Verbindung mit PowerApps hinzu. Außerdem benötigen Sie das Symbol für Ihre benutzerdefinierte API.

1. Navigieren Sie zum PowerApps-[Webportal][9], und melden Sie sich mit Ihrem Geschäftskonto an.

	> [AZURE.NOTE] Derzeit können benutzerdefinierte APIs nur im PowerApps-Webportal verwendet werden. Die Verwendung im PowerApps-Client ist nicht möglich.

2. Wählen Sie **Verbindungen** und dann **Verbindung hinzufügen**:  
	![](./media/powerapps-register-custom-api/createnewconnection.png "Benutzerdefinierte API erstellen")

3. Wählen Sie **Benutzerdefinierte API hinzufügen**:  
	![](./media/powerapps-register-custom-api/connecttocustomapi.png "Benutzerdefinierte API erstellen")  
	Fügen Sie die Eigenschaften Ihrer API hinzu, einschließlich der JSON-Datei und Symboldatei. Wählen Sie anschließend **Weiter**:

	|Eigenschaft|Beschreibung|
|---|---|
|Name|Geben Sie den Namen Ihrer benutzerdefinierten API ein. Wenn dies die Beispiel-Web-API ist, können Sie ihr den Namen **MySampleWebAPI** geben.|
|Swagger-API-Definition|Navigieren Sie zu der von Swagger erstellten JSON-Datei.|
|Hochladen eines API-Symbols|Navigieren Sie zur Symboldatei.|
|Beschreibung|Geben Sie eine Beschreibung der benutzerdefinierten API ein. Wenn es sich um die Beispiel-Web-API handelt, können Sie **Beispiel-Web-API – Tutorial** eingeben.|

4. Geben Sie die Authentifizierungseigenschaften ein. Wenn für die JSON-Datei die OAuth2-Authentifizierung im ```securityDefintions```-Objekt verwendet wird, werden Sie zum Eingeben der folgenden Werte aufgefordert:

	|Eigenschaft|Beschreibung|
|---|---|
|Client-ID|Eine Client-ID wird bereitgestellt, indem einer der unterstützten OAuth-Identitätsanbieter (in diesen Thema aufgeführt) verwendet wird. Geben Sie diese Client-ID ein.
|Geheimer Clientschlüssel|Geben Sie den geheimen Clientschlüssel des gewählten Identitätsanbieters ein.|  

	In den **Authentifizierungsbeispielen** in diesem Thema sind Beispiele für diese OAuth-Eigenschaften angegeben.

	Wenn das ```securityDefintions```-Objekt nicht für die JSON-Datei verwendet wird, sind ggf. keine weiteren Werte erforderlich.

5. Klicken Sie auf **Erstellen**. Ihre benutzerdefinierte API wird jetzt unter **Verfügbare Verbindungen** angezeigt:

	![](./media/powerapps-register-custom-api/mycustomapi.png "Verfügbare Verbindungen")


> [AZURE.TIP] Falls die Swagger-Datei nicht überprüft werden kann, sind unter Umständen zusätzliche Zeichen vorhanden. Beispielsweise sollten die meisten Daten in Anführungszeichen stehen, z.B. auch Websites. Wenn `https://mywebapi.mywebsite.com` also nicht in Anführungszeichen gesetzt ist, kann die Datei nicht überprüft werden.


### Schritt 3: Hinzufügen der benutzerdefinierten API zu einem logischen Ablauf und zur PowerApp
Sie können die benutzerdefinierte API nun mit Ihrer PowerApp oder einem logischen Ablauf verwenden. In diesem Abschnitt verwenden wir eine benutzerdefinierte Wetter-API.

#### Hinzufügen der benutzerdefinierten API zu Ihrem logischen Ablauf
In diesem Schritt erstellen wir einen sehr einfachen logischen Ablauf, mit dem verdeutlicht wird, wie Sie Ihre benutzerdefinierte API hinzufügen. Ausführlicher behandelt wird dies auf der Seite mit den [ersten Schritten für logische Abläufe][10].

1. Wählen Sie im PowerApps-[Webportal][9] die Registerkarte **Home**.
2. Wählen Sie unter **Logischen Ablauf erstellen** die Option **Erste Schritte**.
3. In diesem Fenster sind bereits einige erstellte Vorlagen für den logischen Ablauf vorhanden, für die häufige Szenarien verwendet werden. Sie können sie verwenden und Ihre benutzerdefinierte API hinzufügen. Oder Sie können **Neu erstellen** wählen, um einen logischen Ablauf von Grund auf neu zu erstellen.

	Die schnellste Möglichkeit zum Hinzufügen Ihrer benutzerdefinierten API ist die Auswahl von **Neu erstellen**. Daraufhin wird der folgende logische Ablauf geöffnet:  
	![](./media/powerapps-register-custom-api/createfromblank.png "Beginn des logischen Ablaufs")

4. Wählen Sie **Wiederholung**, und legen Sie die Häufigkeit auf eine Minute fest:  
	![](./media/powerapps-register-custom-api/logicrecurrence.png "„Wiederholung“ auswählen")

5. Wählen Sie das Pluszeichen (![](./media/powerapps-register-custom-api/flowplussign.png)) und dann **Aktion hinzufügen**. Ihre benutzerdefinierte API wird in der Liste aufgeführt:  
![](./media/powerapps-register-custom-api/logicflow.png "Ihre benutzerdefinierte API")

Die nächsten Schritte richten sich danach, was mit Ihrer API möglich ist. Im Rahmen eines Wetterbeispiels ruft Ihre API ggf. die aktuelle Temperatur ab und sendet dann per Office 365 eine E-Mail:

![](./media/powerapps-register-custom-api/logicflowexample.png "Wetter-Beispiel")



#### Hinzufügen der benutzerdefinierten API zu Ihrer PowerApp
In diesem Schritt erstellen wir eine sehr einfache PowerApp, die verdeutlicht, wie Sie Ihre benutzerdefinierte API hinzufügen. Ausführlichere Informationen finden Sie unter [Create an app from Excel data][11] \(Erstellen einer App aus Excel-Daten).

> [AZURE.NOTE] Derzeit können benutzerdefinierte APIs nur im PowerApps-Webportal verwendet werden. Die Verwendung im PowerApps-Client ist nicht möglich.

1. Wählen Sie im PowerApps-[Webportal][9] die Option **Neue PowerApp**:  
	![](./media/powerapps-register-custom-api/newpowerapp.png "Neue PowerApp auswählen")
2. Im Browser wird eine neue Registerkarte geöffnet. Auf dieser neuen Registerkarte wird automatisch eine leere PowerApp erstellt. Wählen Sie **Mit Daten verbinden**:  
![](./media/powerapps-register-custom-api/blankpowerapp.png "„Mit Daten verbinden“ auswählen")
3. Wählen Sie auf der Registerkarte **Inhalt** die Option **Datenquellen**:  
![](./media/powerapps-register-custom-api/datasources.png "„Mit Daten verbinden“ auswählen")
4. Wählen Sie auf dem neuen Bildschirm unter **Meine Verbindungen** Ihre benutzerdefinierte API aus:  
![](./media/powerapps-register-custom-api/screencustomapi.png "Benutzerdefinierte API auswählen")
5. Wählen Sie die Option **Datenquelle hinzufügen**.

Nach dem Hinzufügen können Sie Ihre benutzerdefinierte API in der Funktionsleiste, einem Textfeld und an anderen Orten verwenden. In der Funktionsleiste können Sie beispielsweise mit der Eingabe von **MySampleWebAPI** beginnen, um die verfügbaren Funktionen anzuzeigen. [Show data from Office 365][12] \(Anzeigen von Daten aus Office 365) ist ein Beispiel für die Verwendung der Office 365-API.


## Freigeben einer benutzerdefinierten API
Benutzer können benutzerdefinierte APIs auch untereinander freigeben. Nachdem Sie eine benutzerdefinierte API hinzugefügt haben, wählen Sie die Registerkarte **Verbindungen** und dann **Benutzerdefinierte APIs**. Wählen Sie anschließend das Freigabesymbol:  

![](./media/powerapps-register-custom-api/sharecustomapi.png "Benutzerdefinierte API freigeben")

> [AZURE.NOTE] Sie können benutzerdefinierte APIs nur für andere Benutzer in Ihrer Organisation freigeben.

## Kontingent und Drosselung

- Sie können bis zu fünf benutzerdefinierte APIs in einem PowerApps-Konto erstellen. Benutzerdefinierte APIs, die für Sie freigegeben sind, werden nicht auf dieses Kontingent angerechnet.
- Für jede Verbindung, die auf einer benutzerdefinierten API erstellt wird, können Benutzer bis zu 500 Anforderungen pro Minute vornehmen.
- Beachten Sie, dass beim Löschen einer benutzerdefinierten API auch alle mit der API erstellten Verbindungen gelöscht werden.

Bei Fragen oder Kommentaren zu benutzerdefinierten APIs können Sie eine E-Mail an [customapishelp@microsoft.com](mailto:customapishelp@microsoft.com) senden.

<!--Reference links in article-->
[1]: https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#securityDefinitionsObject
[2]: https://developer.spotify.com/
[3]: https://developer.uber.com/
[4]: https://api.slack.com/
[5]: http://docs.rackspace.com/
[6]: http://swagger.io/getting-started/
[7]: https://github.com/OAI/OpenAPI-Specification/wiki/Hello-World-Sample
[8]: http://editor.swagger.io/#/
[9]: https://web.powerapps.com
[10]: https://powerapps.microsoft.com/tutorials/get-started-logic-flow/
[11]: https://powerapps.microsoft.com/tutorials/get-started-create-from-data/
[12]: https://powerapps.microsoft.com/tutorials/show-office-data/

<!---HONumber=AcomDC_0713_2016-->