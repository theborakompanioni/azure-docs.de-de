<properties
pageTitle="MailChimp | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. MailChimp ist ein SaaS-Dienst, mit dem Unternehmen E-Mail-Marketingaktivitäten verwalten und automatisieren können. Dazu gehört auch das Senden von Marketing-E-Mails und automatisierten Nachrichten sowie das Durchführen zielgruppenspezifischer Kampagnen."
services="logic-apps"	
documentationCenter=".net,nodejs,java" 	
authors="msftman"	
manager="erikre"	
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem MailChimp-Connector

MailChimp ist ein SaaS-Dienst, mit dem Unternehmen E-Mail-Marketingaktivitäten verwalten und automatisieren können. Dazu gehört auch das Senden von Marketing-E-Mails und automatisierten Nachrichten sowie das Durchführen zielgruppenspezifischer Kampagnen.


>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Trigger und Aktionen

Der MailChimp-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der MailChimp-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### MailChimp-Aktionen
Sie können diese Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Erstellen einer neuen Kampagne auf Grundlage eines Kampagnentyps, einer Empfängerliste und der Kampagneneinstellungen (Betreffzeile, Titel, „from\_name“ und „reply\_to“)|
|[newlist](connectors-create-api-mailchimp.md#newlist)|Erstellen einer neuen Liste in Ihrem MailChimp-Konto|
|[addmember](connectors-create-api-mailchimp.md#addmember)|Hinzufügen eines Mitglieds zu einer Liste oder Aktualisieren eines Mitglieds|
|[removemember](connectors-create-api-mailchimp.md#removemember)|Löschen eines Mitglieds aus einer Liste|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Aktualisieren von Informationen für ein bestimmtes Listenmitglied|
### MailChimp-Trigger
Sie können auf diese Ereignisse lauschen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn einer Liste ein Mitglied hinzugefügt wurde|Löst einen Workflow aus, wenn einer Liste ein neues Mitglied hinzugefügt wurde.|
|Wenn eine neue Liste erstellt wird|Löst einen Workflow aus, wenn eine neue Liste erstellt wird.|


## Herstellen einer Verbindung mit MailChimp
Um Logik-Apps mit MailChimp zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Angeben der Anmeldeinformationen für MailChimp|

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] Sie können diese Verbindung in anderen Logik-Apps verwenden.

## Referenz für MailChimp
Gilt für Version 1.0.

## newcampaign
Neue Kampagne: Erstellen einer neuen Kampagne auf Grundlage eines Kampagnentyps, einer Empfängerliste und der Kampagneneinstellungen (Betreffzeile, Titel, „from\_name“ und „reply\_to“)

```POST: /campaigns```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|newCampaignRequest| |Ja|body|(Keine)|JSON-Objekt mit den Anforderungsparametern für die neue Kampagne, das im Text gesendet wird|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## newlist
Neue Liste: Erstellen einer neuen Liste in Ihrem MailChimp-Konto

```POST: /lists```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|newListRequest| |Ja|body|(Keine)|JSON-Objekt mit den Anforderungsparametern für die neue Kampagne, das im Text gesendet wird|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## addmember
Mitglied einer Liste hinzufügen: Hinzufügen eines Mitglieds zu einer Liste oder Aktualisieren eines Mitglieds

```POST: /lists/{list_id}/members```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list\_id|string|Ja|path|(Keine)|Die eindeutige ID für die Liste|
|newMemberInList| |Ja|body|(Keine)|JSON-Objekt mit den Informationen zum neuen Mitglied, das im Text gesendet wird|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## removemember
Mitglied aus der Liste entfernen: Löschen eines Mitglied aus der Liste

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list\_id|string|Ja|path|(Keine)|Die eindeutige ID für die Liste|
|member\_email|string|Ja|path|(Keine)|Die E-Mail-Adresse des zu löschenden Mitglieds|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## updatemember
Mitgliedsinformationen aktualisieren: Aktualisieren von Informationen für ein bestimmtes Listenmitglied

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list\_id|string|Ja|path|(Keine)|Die eindeutige ID für die Liste|
|member\_email|string|Ja|path|(Keine)|Die eindeutige E-Mail-Adresse des zu aktualisierenden Mitglieds|
|updateMemberInListRequest| |Ja|body|(Keine)|JSON-Objekt mit den Informationen zum aktualisierten Mitglied, das im Text gesendet wird|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## OnMemberSubscribed
Wenn einer Liste ein Mitglied hinzugefügt wurde: Löst einen Workflow aus, wenn einer Liste ein neues Mitglied hinzugefügt wurde

```GET: /trigger/lists/{list_id}/members```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list\_id|string|Ja|path|(Keine)|Die eindeutige ID für die Liste|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|202|Zulässig|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## OnCreateList
Wenn eine neue Liste erstellt wird: Löst einen Workflow aus, wenn eine neue Liste erstellt wird.

```GET: /trigger/lists```

Es gibt keine Parameter für diesen Aufruf
#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|202|Zulässig|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## Objektdefinitionen 

### NewCampaignRequest


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Typ|string|Ja |
|recipients|nicht definiert|Ja |
|settings|nicht definiert|Ja |
|variate\_settings|nicht definiert|Nein |
|tracking|nicht definiert|Nein |
|rss\_opts|nicht definiert|Nein |
|social\_card|nicht definiert|Nein |



### Recipient


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|list\_id|string|Ja |
|segment\_opts|nicht definiert|Nein |



### Einstellungen


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|subject\_line|string|Ja |
|title|string|Nein |
|from\_name|string|Ja |
|reply\_to|string|Ja |
|use\_conversation|Boolescher Wert|Nein |
|to\_name|string|Nein |
|folder\_id|integer|Nein |
|authenticate|Boolescher Wert|Nein |
|auto\_footer|Boolescher Wert|Nein |
|inline\_css|Boolescher Wert|Nein |
|auto\_tweet|Boolescher Wert|Nein |
|auto\_fb\_post|array|Nein |
|fb\_comments|Boolescher Wert|Nein |



### Variate\_Settings


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|winner\_criteria|string|Nein |
|wait\_time|integer|Nein |
|test\_size|integer|Nein |
|subject\_lines|array|Nein |
|send\_times|array|Nein |
|from\_names|array|Nein |
|reply\_to\_addresses|array|Nein |



### Tracking


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|opens|Boolescher Wert|Nein |
|html\_clicks|Boolescher Wert|Nein |
|text\_clicks|Boolescher Wert|Nein |
|goal\_tracking|Boolescher Wert|Nein |
|ecomm360|Boolescher Wert|Nein |
|google\_analytics|string|Nein |
|clicktale|string|Nein |
|salesforce|nicht definiert|Nein |
|highrise|nicht definiert|Nein |
|capsule|nicht definiert|Nein |



### RSS\_Opts


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|feed\_url|string|Nein |
|frequency|string|Nein |
|constrain\_rss\_img|string|Nein |
|schedule|nicht definiert|Nein |



### Social\_Card


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|image\_url|string|Nein |
|description|string|Nein |
|title|string|Nein |



### Segment\_Opts


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|saved\_segment\_id|integer|Nein |
|match|string|Nein |



### Salesforce


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|campaign|Boolescher Wert|Nein |
|notes|Boolescher Wert|Nein |



### Highrise


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|campaign|Boolescher Wert|Nein |
|notes|Boolescher Wert|Nein |



### Capsule


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|notes|Boolescher Wert|Nein |



### Schedule


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|hour|integer|Nein |
|daily\_send|nicht definiert|Nein |
|weekly\_send\_day|string|Nein |
|monthly\_send\_date|number|Nein |



### Daily\_Send


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|sunday|Boolescher Wert|Nein |
|monday|Boolescher Wert|Nein |
|tuesday|Boolescher Wert|Nein |
|wednesday|Boolescher Wert|Nein |
|thursday|Boolescher Wert|Nein |
|friday|Boolescher Wert|Nein |
|saturday|Boolescher Wert|Nein |



### CampaignResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|id|string|Nein |
|Typ|string|Nein |
|create\_time|string|Nein |
|archive\_url|string|Nein |
|status|string|Nein |
|emails\_sent|integer|Nein |
|send\_time|string|Nein |
|content\_type|string|Nein |
|recipient|array|Nein |
|settings|nicht definiert|Nein |
|variate\_settings|nicht definiert|Nein |
|tracking|nicht definiert|Nein |
|rss\_opts|nicht definiert|Nein |
|ab\_split\_opts|nicht definiert|Nein |
|social\_card|nicht definiert|Nein |
|report\_summary|nicht definiert|Nein |
|delivery\_status|nicht definiert|Nein |
|\_links|array|Nein |



### AB\_Split\_Opts


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|split\_test|string|Nein |
|pick\_winner|string|Nein |
|wait\_units|string|Nein |
|wait\_time|integer|Nein |
|split\_size|integer|Nein |
|from\_name\_a|string|Nein |
|from\_name\_b|string|Nein |
|reply\_email\_a|string|Nein |
|reply\_email\_b|string|Nein |
|subject\_a|string|Nein |
|subject\_b|string|Nein |
|send\_time\_a|string|Nein |
|send\_time\_b|string|Nein |
|send\_time\_winner|string|Nein |



### Report\_Summary


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|opens|integer|Nein |
|unique\_opens|integer|Nein |
|open\_rate|number|Nein |
|clicks|integer|Nein |
|subscriber\_clicks|number|Nein |
|click\_rate|number|Nein |



### Delivery\_Status


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|enabled|Boolescher Wert|Nein |
|can\_cancel|Boolescher Wert|Nein |
|status|string|Nein |
|emails\_sent|integer|Nein |
|emails\_canceled|integer|Nein |



### Link


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|rel|string|Nein |
|href|string|Nein |
|method|string|Nein |
|targetSchema|string|Nein |
|schema|string|Nein |



### NewListRequest


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|string|Ja |
|contact|nicht definiert|Ja |
|permission\_reminder|string|Ja |
|use\_archive\_bar|Boolescher Wert|Nein |
|campaign\_defaults|nicht definiert|Ja |
|notify\_on\_subscribe|string|Nein |
|notify\_on\_unsubscribe|string|Nein |
|email\_type\_option|Boolescher Wert|Ja |
|visibility|string|Nein |



### Contact


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|company|string|Ja |
|address1|string|Ja |
|address2|string|Nein |
|city|string|Ja |
|state|string|Ja |
|zip|string|Ja |
|country|string|Ja |
|phone|string|Ja |



### Campaign\_Defaults


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|from\_name|string|Ja |
|from\_email|string|Ja |
|subject|string|Nein |
|language|string|Ja |



### CreateNewListResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|id|string|Ja |
|Name|string|Ja |
|contact|nicht definiert|Ja |
|permission\_reminder|string|Ja |
|use\_archive\_bar|Boolescher Wert|Nein |
|campaign\_defaults|nicht definiert|Ja |
|notify\_on\_subscribe|string|Nein |
|notify\_on\_unsubscribe|string|Nein |
|date\_created|string|Nein |
|list\_rating|integer|Nein |
|email\_type\_option|Boolescher Wert|Ja |
|subscribe\_url\_short|string|Nein |
|subscribe\_url\_long|string|Nein |
|beamer\_address|string|Nein |
|visibility|string|Nein |
|modules|array|Nein |
|stats|nicht definiert|Nein |
|\_links|array|Nein |



### Stats


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|member\_count|integer|Nein |
|unsubscribe\_count|integer|Nein |
|cleaned\_count|integer|Nein |
|member\_count\_since\_send|integer|Nein |
|unsubscribe\_count\_since\_send|integer|Nein |
|cleaned\_count\_since\_send|integer|Nein |
|campaign\_count|integer|Nein |
|campaign\_last\_sent|integer|Nein |
|merge\_field\_count|integer|Nein |
|avg\_sub\_rate|number|Nein |
|avg\_unsub\_rate|number|Nein |
|target\_sub\_rate|number|Nein |
|open\_rate|number|Nein |
|click\_rate|number|Nein |
|last\_sub\_date|string|Nein |
|last\_unsub\_date|string|Nein |



### GetListsResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|lists|array|Nein |
|total\_items|integer|Nein |



### NewMemberInListRequest


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|email\_type|string|Nein |
|status|string|Ja |
|merge\_fields|nicht definiert|Nein |
|interests|string|Nein |
|language|string|Nein |
|VIP|Boolescher Wert|Nein |
|location|nicht definiert|Nein |
|email\_address|string|Ja |



### FirstAndLastName


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|FNAME|string|Nein |
|LNAME|string|Nein |



### Location


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|latitude|number|Nein |
|longitude|number|Nein |



### MemberResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|id|string|Nein |
|email\_address|string|Nein |
|unique\_email\_id|string|Nein |
|email\_type|string|Nein |
|status|string|Nein |
|merge\_fields|nicht definiert|Nein |
|interests|string|Nein |
|stats|nicht definiert|Nein |
|ip\_signup|string|Nein |
|timestamp\_signup|string|Nein |
|ip\_opt|string|Nein |
|timestamp\_opt|string|Nein |
|member\_rating|integer|Nein |
|last\_changed|string|Nein |
|language|string|Nein |
|VIP|Boolescher Wert|Nein |
|email\_client|string|Nein |
|location|nicht definiert|Nein |
|last\_note|nicht definiert|Nein |
|list\_id|string|Nein |
|\_links|array|Nein |



### Last\_Note


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|note\_id|integer|Nein |
|created\_at|string|Nein |
|created\_by|string|Nein |
|note|string|Nein |



### GetAllMembersResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|members|array|Nein |
|list\_id|string|Nein |
|total\_items|integer|Nein |



### Objekt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|



### UpdateMemberInListRequest


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|email\_address|string|Nein |
|email\_type|string|Nein |
|status|string|Ja |
|merge\_fields|nicht definiert|Nein |
|interests|string|Nein |
|language|string|Nein |
|VIP|Boolescher Wert|Nein |
|location|nicht definiert|Nein |



### GetMembersResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|members|array|Nein |
|list\_id|string|Nein |
|total\_items|integer|Nein |



### AddUserResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|id|string|Ja |
|email\_address|string|Ja |
|unique\_email\_id|string|Nein |
|email\_type|string|Nein |
|status|string|Nein |
|merge\_fields|nicht definiert|Ja |
|interests|string|Nein |
|stats|nicht definiert|Nein |
|ip\_signup|string|Nein |
|timestamp\_signup|string|Nein |
|ip\_opt|string|Nein |
|timestamp\_opt|string|Nein |
|member\_rating|integer|Nein |
|last\_changed|string|Nein |
|language|string|Nein |
|VIP|Boolescher Wert|Nein |
|email\_client|string|Nein |
|location|nicht definiert|Nein |
|last\_note|nicht definiert|Nein |
|list\_id|string|Nein |
|\_links|array|Nein |


## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->