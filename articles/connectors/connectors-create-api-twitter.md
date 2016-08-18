<properties
	pageTitle="Informationen zum Verwenden des Twitter-Connectors in Logik-Apps | Microsoft Azure"
	description="Übersicht über den Twitter-Connector mit REST-API-Parametern"
	services=""
	documentationCenter="" 
	authors="msftman"
	manager="erikre"
	editor=""
	tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/18/2016"
   ms.author="deonhe"/>


# Erste Schritte mit dem Twitter-Connector

Der Twitter-Connector ermöglicht Folgendes:

- Tweets posten und abrufen
- Auf Timelines, Freunde und Follower zugreifen
- Andere Aktionen und Trigger ausführen, die weiter unten beschrieben sind

Wenn Sie einen [Connector](./apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst eine Logik-App, wie [hier](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Verbinden mit Twitter

Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](./connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.

### Erstellen einer Verbindung mit Twitter

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit Twitter](../../includes/connectors-create-api-twitter.md)]

## Verwenden eines Twitter-Triggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

In diesem Beispiel zeige ich Ihnen, wie der Trigger **Wenn ein neuer Tweet gepostet wird** verwendet wird, um nach #Seattle zu suchen, und wie eine Datei in Dropbox mit dem Text aus dem Tweet aktualisiert wird, wenn #Seattle gefunden wird. In einem Unternehmensbeispiel könnten Sie nach dem Namen Ihres Unternehmens suchen und eine SQL-Datenbank mit dem Text aus dem Tweet aktualisieren.

1. Geben Sie *Twitter* in das Suchfeld im Designer für Logik-Apps ein, und wählen Sie dann den Trigger **Twitter - Wenn ein neuer Tweet gepostet wird** aus   
![Twitter-Trigger – Abbildung 1](./media/connectors-create-api-twitter/trigger-1.png)  
- Geben Sie *#Seattle* in das Steuerelement **Suchtext** ein.  
![Twitter-Trigger – Abbildung 2](./media/connectors-create-api-twitter/trigger-2.png)  

Jetzt haben Sie Ihre Logik-App mit einem Trigger konfiguriert, der eine Ausführung der anderen Trigger und Aktionen im Workflow startet.

>[AZURE.NOTE]Damit eine Logik-App funktionsfähig ist, muss sie mindestens einen Trigger und eine Aktion enthalten. Führen Sie die Schritte im nächsten Abschnitt aus, um eine Aktion hinzuzufügen.

## Bedingung hinzufügen
Da uns nur Tweets von Benutzern mit mehr als 50 Benutzern interessieren, muss zunächst eine Bedingung, die die Anzahl der Follower überprüft, zur Logik-App hinzugefügt werden.

1. Wählen Sie **+ Neuer Schritt** aus, um die Aktion hinzuzufügen, die ausgeführt werden soll, wenn #Seattle in einem neuen Tweet gefunden wird.  
![Twitter-Aktion – Abbildung 1](../../includes/media/connectors-create-api-twitter/action-1.png)  
- Wählen Sie den Link **Bedingung hinzufügen** aus.  
![Twitter-Bedingung – Abbildung 1](../../includes/media/connectors-create-api-twitter/condition-1.png)  
Daraufhin wird das Steuerelement **Bedingung** geöffnet, in dem Sie Bedingungen wie z.B. *ist gleich*, *ist kleiner als*, *ist größer als* und *enthält* auswählen können.  
![Twitter-Bedingung – Abbildung 2](../../includes/media/connectors-create-api-twitter/condition-2.png)  
- Wählen Sie das Steuerelement **Wert auswählen** aus. In diesem Steuerelement können Sie eine oder mehrere Eigenschaften von vorherigen Aktionen oder Triggern als den Wert auswählen, dessen Bedingung als wahr oder falsch ausgewertet wird.  
![Twitter-Bedingung – Abbildung 3](../../includes/media/connectors-create-api-twitter/condition-3.png)  
- Wählen Sie **...** aus, um die Liste der Eigenschaften zu erweitern, damit Sie alle verfügbaren Eigenschaften sehen können.  
![Twitter-Bedingung – Abbildung 4](../../includes/media/connectors-create-api-twitter/condition-4.png)  
- Wählen Sie die Eigenschaft **Anzahl von Followern** aus.  
![Twitter-Bedingung – Abbildung 5](../../includes/media/connectors-create-api-twitter/condition-5.png)  
- Beachten Sie, dass die Eigenschaft für die Anzahl von Followern jetzt in das Steuerelement für den Wert aufgenommen wurde.  
![Twitter-Bedingung – Abbildung 6](../../includes/media/connectors-create-api-twitter/condition-6.png)  
- Wählen Sie **ist größer als** aus der Liste der Operatoren aus.  
![Twitter-Bedingung – Abbildung 7](../../includes/media/connectors-create-api-twitter/condition-7.png)  
- Geben Sie „50“ als Operanden für den Operator *ist größer als* ein. Die Bedingung wird hinzugefügt. Speichern Sie Ihre Arbeit mit dem Link **Speichern** oben im Menü.  
![Twitter-Bedingung – Abbildung 8](../../includes/media/connectors-create-api-twitter/condition-8.png)  

## Verwenden einer Twitter-Aktion

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Nachdem Sie nun einen Trigger hinzugefügt haben, gehen Sie wie folgt vor, um eine Aktion hinzuzufügen, mit der ein neuer Tweet gepostet wird, der den Inhalt der vom Trigger gefundenen Tweets enthält. Im Rahmen dieser exemplarischen Vorgehensweise werden nur die Tweets von Benutzern mit mehr als 50 Followern gepostet.

Im nächsten Schritt fügen Sie eine Twitter-Aktion hinzu, die mit einigen der Eigenschaften der einzelnen Tweets, die von einem Benutzer mit mehr als 50 Followern gepostet wurden hat, einen Tweet postet.

1. Wählen Sie **Aktion hinzufügen** aus. Daraufhin wird das Steuerelement für die Suche geöffnet, in dem Sie nach anderen Aktionen und Triggern suchen können.  
![Twitter-Bedingung – Abbildung 9](../../includes/media/connectors-create-api-twitter/condition-9.png)  
- Geben Sie *Twitter* in das Suchfeld ein, und wählen Sie die Aktion **Twitter - Tweet posten** aus. Daraufhin wird das Steuerelement **Tweet posten** geöffnet, in das Sie alle Details für den zu postenden Tweet eingeben.  
![Twitter-Aktion – Abbildung 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)  
- Wählen Sie das Steuerelement **Tweettext** aus. Alle Ausgaben früherer Aktionen und Trigger in der Logik-App werden jetzt angezeigt. Sie können beliebige davon auswählen und als Teil des Tweettexts des neuen Tweets verwenden.  
![Twitter-Aktion – Abbildung 2](../../includes/media/connectors-create-api-twitter/action-2.png)  
- Wählen Sie **Benutzername** aus.
- Geben Sie in das Steuerelement für den Tweettext *sagt:* direkt nach dem Benutzernamen ein.
- Wählen Sie *Tweettext* aus.  
![Twitter-Aktion – Abbildung 3](../../includes/media/connectors-create-api-twitter/action-3.png)  
- Speichern Sie Ihre Arbeit, und senden Sie einen Tweet mit dem Hashtag #Seattle, um den Workflow zu aktivieren.

## Technische Details

Im Folgenden finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## Twitter-Trigger

Der Twitter-Connector verfügt über folgende Trigger:

|Trigger | Beschreibung|
|--- | ---|
|[Wenn ein neuer Tweet gepostet wird](connectors-create-api-twitter.md#when-a-new-tweet-is-posted)|Mit diesem Vorgang wird einen Datenfluss ausgelöst, wenn ein neuer Tweet gepostet wird, der einer bestimmten Suchabfrage entspricht.|


## Twitter-Aktionen

Der Twitter-Connector verfügt über folgende Aktionen:


|Aktion|Beschreibung|
|--- | ---|
|[Timeline des Benutzers abrufen](connectors-create-api-twitter.md#get-user-timeline)|Mit diesem Vorgang wird eine Liste der neuesten Tweets abgerufen, die von einem bestimmten Benutzer gepostet wurden.|
|[Eigene Timeline abrufen](connectors-create-api-twitter.md#get-home-timeline)|Mit diesem Vorgang werden die neuesten Tweets und Retweets abgerufen, die von mir und meinen Followern gepostet wurden.|
|[Tweets durchsuchen](connectors-create-api-twitter.md#search-tweets)|Mit diesem Vorgang wird eine Liste der relevanten Tweets abgerufen, die mit der Suchabfrage übereinstimmen.|
|[Follower abrufen](connectors-create-api-twitter.md#get-followers)|Mit diesem Vorgang wird die Liste der Benutzer abgerufen, die einem bestimmten Benutzer folgen.|
|[Meine Follower abrufen](connectors-create-api-twitter.md#get-my-followers)|Mit diesem Vorgang wird die Liste der Benutzer abgerufen, die mir folgen.|
|[Gefolge abrufen](connectors-create-api-twitter.md#get-following)|Mit diesem Vorgang wird die Liste der Personen abgerufen, denen ein bestimmter Benutzer folgt.|
|[Mein Gefolge abrufen](connectors-create-api-twitter.md#get-my-following)|Mit diesem Vorgang wird die Liste der Benutzer abgerufen, denen ich folge.|
|[Benutzer abrufen](connectors-create-api-twitter.md#get-user)|Mit diesem Vorgang werden die Profildetails für einen bestimmten Benutzer abgerufen, z.B. Benutzername, Beschreibung, Anzahl von Followern usw.|
|[Tweet posten](connectors-create-api-twitter.md#post-a-tweet)|Mit diesem Vorgang wird ein neuer Tweet gepostet.|
## Aktionsdetails

Im Folgenden finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten:



### Timeline des Benutzers abrufen
Mit diesem Vorgang wird eine Liste der neuesten Tweets abgerufen, die von einem bestimmten Benutzer gepostet wurden.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|userName*|Benutzername|Twitter-Handle für den Benutzer|
|maxResults|Maximale Anzahl von Ergebnissen|Die maximale Anzahl von Tweets, die zurückgegeben werden sollen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

TweetModel: Darstellung des Tweet-Objekts


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|TweetText|string|Textinhalt des Tweets|
|TweetId|string|ID des Tweets|
|CreatedAt|string|Zeitpunkt, zu dem der Tweet gepostet wurde|
|RetweetCount|integer|Gesamtanzahl von Retweets für den Tweet|
|TweetedBy|string|Name des Benutzers, der den Tweet gepostet hat|
|MediaUrls|array|URL der Medien, die mit dem Tweet gepostet wurden|
|TweetLanguageCode|string|Sprachcode des Tweets|
|TweetInReplyToUserId|string|Benutzer-ID des Autors des Tweets, auf den der aktuelle Tweet eine Antwort ist|
|Favorited|Boolescher Wert|Gibt an, ob der Tweet als Favorit markiert ist|
|UserMentions|array|Liste der im Tweet erwähnten Benutzer|
|OriginalTweet|nicht definiert|Ursprünglicher Tweet, aus dem der aktuelle Tweet als Retweet erstellt wurde|
|UserDetails|nicht definiert|Details zu dem Benutzer, der getweetet hat|




### Eigene Timeline abrufen
Mit diesem Vorgang werden die neuesten Tweets und Retweets abgerufen, die von mir und meinen Followern gepostet wurden.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|maxResults|Maximale Anzahl von Ergebnissen|Die maximale Anzahl von Tweets, die zurückgegeben werden sollen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

TweetModel: Darstellung des Tweet-Objekts


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|TweetText|string|Textinhalt des Tweets|
|TweetId|string|ID des Tweets|
|CreatedAt|string|Zeitpunkt, zu dem der Tweet gepostet wurde|
|RetweetCount|integer|Gesamtanzahl von Retweets für den Tweet|
|TweetedBy|string|Name des Benutzers, der den Tweet gepostet hat|
|MediaUrls|array|URL der Medien, die mit dem Tweet gepostet wurden|
|TweetLanguageCode|string|Sprachcode des Tweets|
|TweetInReplyToUserId|string|Benutzer-ID des Autors des Tweets, auf den der aktuelle Tweet eine Antwort ist|
|Favorited|Boolescher Wert|Gibt an, ob der Tweet als Favorit markiert ist|
|UserMentions|array|Liste der im Tweet erwähnten Benutzer|
|OriginalTweet|nicht definiert|Ursprünglicher Tweet, aus dem der aktuelle Tweet als Retweet erstellt wurde|
|UserDetails|nicht definiert|Details zu dem Benutzer, der getweetet hat|




### Tweets durchsuchen
Mit diesem Vorgang wird eine Liste der relevanten Tweets abgerufen, die mit der Suchabfrage übereinstimmen.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|searchQuery*|Suchtext|Suchbegriff wie „Happy Hour“, „#haiku“, „Liebe OR Hass“|
|maxResults|Maximale Anzahl von Ergebnissen|Die maximale Anzahl von Tweets, die zurückgegeben werden sollen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

TweetModel: Darstellung des Tweet-Objekts


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|TweetText|string|Textinhalt des Tweets|
|TweetId|string|ID des Tweets|
|CreatedAt|string|Zeitpunkt, zu dem der Tweet gepostet wurde|
|RetweetCount|integer|Gesamtanzahl von Retweets für den Tweet|
|TweetedBy|string|Name des Benutzers, der den Tweet gepostet hat|
|MediaUrls|array|URL der Medien, die mit dem Tweet gepostet wurden|
|TweetLanguageCode|string|Sprachcode des Tweets|
|TweetInReplyToUserId|string|Benutzer-ID des Autors des Tweets, auf den der aktuelle Tweet eine Antwort ist|
|Favorited|Boolescher Wert|Gibt an, ob der Tweet als Favorit markiert ist|
|UserMentions|array|Liste der im Tweet erwähnten Benutzer|
|OriginalTweet|nicht definiert|Ursprünglicher Tweet, aus dem der aktuelle Tweet als Retweet erstellt wurde|
|UserDetails|nicht definiert|Details zu dem Benutzer, der getweetet hat|




### Follower abrufen
Mit diesem Vorgang wird die Liste der Benutzer abgerufen, die einem bestimmten Benutzer folgen.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|userName*|Benutzername|Twitter-Handle für den Benutzer|
|maxResults|Maximale Anzahl von Ergebnissen|Maximale Anzahl zurückzugebender Benutzer|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

UserDetailsModel: Twitter-Benutzerdetails


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|FullName|string|Name des Benutzers|
|Standort|string|Standort des Benutzers|
|ID|integer|Twitter-ID des Benutzers|
|UserName|string|Bildschirmname des Benutzers|
|FollowersCount|integer|Anzahl von Followern|
|Beschreibung|string|Benutzerbeschreibung|
|StatusesCount|integer|Anzahl von Benutzerstatuswerten|
|FriendsCount|integer|Anzahl von Freunden|
|FavouritesCount|integer|Anzahl von Tweets, die der Benutzer als Favorit markiert hat|
|ProfileImageUrl|string|URL des Profilbilds|




### Meine Follower abrufen
Mit diesem Vorgang wird die Liste der Benutzer abgerufen, die mir folgen.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|maxResults|Maximale Anzahl von Ergebnissen|Maximale Anzahl abzurufender Benutzer|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

UserDetailsModel: Twitter-Benutzerdetails


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|FullName|string|Name des Benutzers|
|Standort|string|Standort des Benutzers|
|ID|integer|Twitter-ID des Benutzers|
|UserName|string|Bildschirmname des Benutzers|
|FollowersCount|integer|Anzahl von Followern|
|Beschreibung|string|Benutzerbeschreibung|
|StatusesCount|integer|Anzahl von Benutzerstatuswerten|
|FriendsCount|integer|Anzahl von Freunden|
|FavouritesCount|integer|Anzahl von Tweets, die der Benutzer als Favorit markiert hat|
|ProfileImageUrl|string|URL des Profilbilds|




### Gefolge abrufen
Mit diesem Vorgang wird die Liste der Personen abgerufen, denen ein bestimmter Benutzer folgt.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|userName*|Benutzername|Twitter-Handle für den Benutzer|
|maxResults|Maximale Anzahl von Ergebnissen|Maximale Anzahl zurückzugebender Benutzer|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

UserDetailsModel: Twitter-Benutzerdetails


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|FullName|string|Name des Benutzers|
|Standort|string|Standort des Benutzers|
|ID|integer|Twitter-ID des Benutzers|
|UserName|string|Bildschirmname des Benutzers|
|FollowersCount|integer|Anzahl von Followern|
|Beschreibung|string|Benutzerbeschreibung|
|StatusesCount|integer|Anzahl von Benutzerstatuswerten|
|FriendsCount|integer|Anzahl von Freunden|
|FavouritesCount|integer|Anzahl von Tweets, die der Benutzer als Favorit markiert hat|
|ProfileImageUrl|string|URL des Profilbilds|




### Mein Gefolge abrufen
Mit diesem Vorgang wird die Liste der Benutzer abgerufen, denen ich folge.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|maxResults|Maximale Anzahl von Ergebnissen|Maximale Anzahl zurückzugebender Benutzer|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

UserDetailsModel: Twitter-Benutzerdetails


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|FullName|string|Name des Benutzers|
|Standort|string|Standort des Benutzers|
|ID|integer|Twitter-ID des Benutzers|
|UserName|string|Bildschirmname des Benutzers|
|FollowersCount|integer|Anzahl von Followern|
|Beschreibung|string|Benutzerbeschreibung|
|StatusesCount|integer|Anzahl von Benutzerstatuswerten|
|FriendsCount|integer|Anzahl von Freunden|
|FavouritesCount|integer|Anzahl von Tweets, die der Benutzer als Favorit markiert hat|
|ProfileImageUrl|string|URL des Profilbilds|




### Benutzer abrufen
Mit diesem Vorgang werden die Profildetails für einen bestimmten Benutzer abgerufen, z.B. Benutzername, Beschreibung, Anzahl von Followern usw.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|userName*|Benutzername|Twitter-Handle für den Benutzer|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

UserDetailsModel: Twitter-Benutzerdetails


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|FullName|string|Name des Benutzers|
|Standort|string|Standort des Benutzers|
|ID|integer|Twitter-ID des Benutzers|
|UserName|string|Bildschirmname des Benutzers|
|FollowersCount|integer|Anzahl von Followern|
|Beschreibung|string|Benutzerbeschreibung|
|StatusesCount|integer|Anzahl von Benutzerstatuswerten|
|FriendsCount|integer|Anzahl von Freunden|
|FavouritesCount|integer|Anzahl von Tweets, die der Benutzer als Favorit markiert hat|
|ProfileImageUrl|string|URL des Profilbilds|




### Tweet posten
Mit diesem Vorgang wird ein neuer Tweet gepostet.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|tweetText|Tweettext|Text, der gepostet werden soll|
|body|Medien|Zu veröffentlichende Medien|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

TweetResponseModel: Modell zur Darstellung eines geposteten Tweets


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|TweetId|string|ID des abgerufenen Tweets|




### Wenn ein neuer Tweet gepostet wird
Mit diesem Vorgang wird einen Datenfluss ausgelöst, wenn ein neuer Tweet gepostet wird, der einer bestimmten Suchabfrage entspricht.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|searchQuery*|Suchtext|Suchbegriff wie „Happy Hour“, „#haiku“, „Liebe OR Hass“|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

TriggerBatchResponse[TweetModel]


| Eigenschaftenname | Datentyp |
|---|---|
|value|array|



## HTTP-Antworten

Von den oben angegebenen Aktionen und Triggern können folgende HTTP-Statuscodes zurückgegeben werden:

|Name|Beschreibung|
|---|---|
|200|OK|
|202|Zulässig|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler.|
|die Standardeinstellung|Fehler beim Vorgang.|









## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0727_2016-->
