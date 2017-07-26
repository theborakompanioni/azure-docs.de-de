---
title: Integrieren von Azure AD in eine iOS-Anwendung | Microsoft-Dokumentation
description: "In diesem Thema erfahren Sie, wie eine iOS-Anwendung erstellt wird, die sich für die Anmeldung in Azure AD integriert und über OAuth durch Azure AD geschützte APIs aufruft."
services: active-directory
documentationcenter: ios
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 57f465df99ac234466459b8031f61805d8334b59
ms.contentlocale: de-de
ms.lasthandoff: 06/01/2017


---
# <a name="integrate-azure-ad-into-an-ios-app"></a>Integrieren von Azure AD in eine iOS-Anwendung
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> Testen Sie die Vorschau unseres neuen [Entwicklerportals](https://identity.microsoft.com/Docs/iOS), mit dem Sie Azure Active Directory innerhalb weniger Minuten betriebsbereit machen.  Im Entwicklerportal werden Sie durch den Vorgang zum Registrieren einer App und die Integration von Azure AD in Ihren Code geleitet.  Nach dem Durchführen dieser Schritte verfügen Sie über eine einfache Anwendung zur Authentifizierung von Benutzern in Ihrem Mandanten sowie über ein Back-End, das Token akzeptieren und eine Überprüfung durchführen kann. 
> 
> 

Für iOS-Clients, die auf geschützte Ressourcen zugreifen müssen, bietet Azure Active Directory (Azure AD) die Active Directory-Authentifizierungsbibliothek (ADAL). ADAL vereinfacht den Prozess Ihrer App zum Abrufen von Zugriffstoken. Um Ihnen zu zeigen, wie einfach es geht, erstellen wir in diesem Artikel eine Objective C-Anwendung mit einer Aufgabenliste, mit der folgende Aktionen ausgeführt werden können:

* Abrufen der Zugriffstoken zum Aufrufen der Azure AD Graph-API mit dem [OAuth 2.0-Authentifizierungsprotokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)
* Durchsuchen eines Verzeichnisses nach Benutzern mit einem bestimmten Aliasnamen

Zur Entwicklung der vollständigen Arbeitsanwendung müssen Sie folgende Schritte ausführen:

1. Registrieren Ihrer Anwendung bei Azure AD
2. Installieren und Konfigurieren der ADAL
3. Verwenden von ADAL zum Abrufen von Tokens aus Azure AD

Beginnen Sie, indem Sie das [Anwendungsgerüst](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) oder [das vollständige Beispiel herunterladen](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). Außerdem benötigen Sie einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und Ihre Anwendung registrieren können. Wenn Sie über noch keinen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](active-directory-howto-tenant.md).


> [!TIP]
> Testen Sie die Vorschau unseres neuen [Entwicklerportals](https://identity.microsoft.com/Docs/iOS), mit dem Sie Azure AD innerhalb weniger Minuten betriebsbereit machen. Im Entwicklerportal werden Sie durch den Vorgang zum Registrieren einer App und die Integration von Azure AD in Ihren Code geleitet. Nach dem Durchführen dieser Schritte verfügen Sie über eine einfache Anwendung zur Authentifizierung von Benutzern in Ihrem Mandanten sowie über ein Back-End, das Token akzeptieren und eine Überprüfung durchführen kann. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. Festlegen des Umleitungs-URI für iOS
Um Ihre Anwendungen in bestimmten SSO-Szenarios sicher zu starten, müssen Sie einen *Umleitungs-URI* in einem bestimmten Format erstellen. Ein Umleitungs-URI wird verwendet, um sicherzustellen, dass die Tokens an die richtige Anwendung zurückgegeben werden, die sie angefordert hat.


Das iOS-Format für einen Umleitungs-URI lautet:

```
<app-scheme>://<bundle-id>
```

* **aap-scheme** – Dieses Schema ist in Ihrem XCode-Projekt registriert. So können Sie andere Anwendungen aufrufen. Sie finden dieses Schema unter "Info.plist -> URL types -> URL Identifier". Sie sollten ein Schema erstellen, sofern Sie nicht bereits ein oder mehrere Schemas konfiguriert haben.
* **bundle-id** – dies ist die Paket-ID, die sich unter "identity" in Ihren Projekteinstellungen in XCode befindet.

Beispiel für diesen QuickStart-Code: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>2. Registrieren der Anwendung DirectorySearcher
Damit Ihre Anwendung Token abrufen kann, müssen Sie sie zunächst bei Ihrem Azure AD-Mandanten registrieren und ihr die Berechtigung für den Zugriff auf die Azure AD Graph-API erteilen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf der oberen Leiste auf Ihr Konto. Wählen Sie in der **Verzeichnisliste** den Active Directory-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.
3. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, und wählen Sie anschließend **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen** aus.
5. Folgen Sie den Bildschirmaufforderungen, und erstellen Sie eine neue **native Clientanwendung**.
  * Am **Namen** der Anwendung sollten die Endbenutzer die Funktion der Anwendung ablesen können.
  * Die **Umleitungs-URI** ist eine Kombination aus einem Schema und einer Zeichenfolge, die Azure AD für die Rückgabe der Tokenantworten verwendet.  Geben Sie einen Wert ein, der spezifisch für Ihre Anwendung ist und auf den vorherigen Umleitungs-URI-Informationen basiert.
6. Nach Abschluss der Registrierung weist Azure AD Ihrer App eine eindeutige Anwendungs-ID zu.  Diesen Wert benötigen Sie in den nächsten Abschnitten. Daher sollten Sie ihn von der Registerkarte „Anwendung“ kopieren.
7. Wählen Sie auf der Seite **Einstellungen** zunächst **Erforderliche Berechtigungen** und dann **Hinzufügen** aus. Wählen Sie als API **Microsoft Graph** aus, und fügen Sie anschließend unter **Delegierte Berechtigungen** die Berechtigung **Verzeichnisdaten lesen** hinzu.  Mit dieser Berechtigung kann die Anwendung die Azure AD-Graph-API nach Benutzern abfragen.

## <a name="3-install-and-configure-adal"></a>3. Installieren und Konfigurieren der ADAL
Nachdem Sie nun eine Anwendung in Azure AD erstellt haben, können Sie ADAL installieren und Ihren identitätsbezogenen Code schreiben.  Damit ADAL mit Azure AD kommunizieren kann, müssen Sie einige Informationen zur App-Registrierung bereitstellen.

1. Fügen Sie dazu zunächst ADAL mithilfe von CocoaPods dem Projekt „DirectorySearcher“ hinzu.

    ```
    $ vi Podfile
    ```
2. Fügen Sie diesem Podfile folgenden Code hinzu:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. Laden Sie jetzt die Podfile-Datei mithilfe von CocoaPods. Dieser Schritt erstellt einen neuen XCode-Arbeitsbereich, den Sie laden.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. Öffnen Sie im Projekt "QuickStart" die PLIST-Datei `settings.plist`.  Ersetzen Sie die Werte der Elemente in diesem Abschnitt durch die Werte, die Sie im Azure-Portal eingegeben haben. Sobald Ihr Code die ADAL verwendet, verweist er auf diese Werte.
  * `tenant` ist die Domäne Ihres Azure AD-Mandanten, z.B. „contoso.onmicrosoft.com“.
  * `clientId` ist die Client-ID der Anwendung, die Sie aus dem Portal kopiert haben.
  * `redirectUri` ist die Umleitungs-URL, die Sie im Portal registriert haben.

## <a name="4----use-adal-to-get-tokens-from-azure-ad"></a>4.    Verwenden von ADAL zum Abrufen von Tokens aus Azure AD
Das Grundprinzip von ADAL ist wie folgt: Wann immer Ihre Anwendung ein Zugriffstoken benötigt, ruft sie einen completionBlock `+(void) getToken : ` auf, und ADAL erledigt alles Weitere.  

1. Öffnen Sie im Projekt `QuickStart` die Datei `GraphAPICaller.m`, und suchen Sie den Kommentar `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` oben in der Datei.  Dort übergeben Sie die zur Kommunikation mit Azure AD notwendigen Koordinaten über einen CompletionBlock an ADAL und weisen sie an, wie Token zwischengespeichert werden sollen.

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. Nun muss mithilfe dieses Tokens nach Benutzern im Graph gesucht werden. Suchen Sie den `// TODO: implement SearchUsersList`-Kommentar. Diese Methode übergibt der Azure AD Graph-API eine GET-Anforderung für die Suche nach Benutzern, deren UPNs mit dem angegebenen Suchbegriff beginnen.  Für die Abfrage der Azure ID Graph-API müssen Sie dem `Authorization`-Header der Anforderung ein access_token (Zugriffstoken) hinzufügen. An dieser Stelle tritt die ADAL auf den Plan.

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab the JSON node at the top to get our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by the key name being "value". It really is the name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

                             [Users addObject:s];
                         }

                         completionBlock(Users, nil);
                     }
                     else
                     {
                         completionBlock(nil, error);
                     }

                }];
             }
         }];

    }

    ```


3. Wenn Ihre App über einen `getToken(...)`-Aufruf ein Token anfordert, versucht die ADAL, ein Token zurückzugeben, ohne den Benutzer nach seinen Anmeldeinformationen zu fragen.  Stellt ADAL fest, dass sich der Benutzer zum Abrufen eines Tokens anmelden muss, zeigt es einen Anmeldedialog an, erfasst die Anmeldeinformationen des Benutzers und gibt nach erfolgreicher Authentifizierung ein Token zurück.  Wenn ADAL kein Token zurückgeben kann, löst es eine `AdalException` aus.

> [!Note] 
> Das Objekt `AuthenticationResult` enthält ein `tokenCacheStoreItem`-Objekt, mit dem von Ihrer Anwendung benötigte Informationen erfasst werden können. Im Projekt „QuickStart“ wird mithilfe von `tokenCacheStoreItem` ermittelt, ob die Authentifizierung bereits erfolgt ist.
>
>

## <a name="5-build-and-run-the-application"></a>5. Erstellen und Ausführen der Anwendung
Glückwunsch! Sie haben nun eine funktionierende iOS-Anwendung, die Benutzer authentifizieren, Web-APIs über OAuth 2.0 sicher aufrufen und grundlegende Benutzerinformationen abfragen kann.  Sofern nicht bereits geschehen, ist es nun an der Zeit, Ihren Mandanten mit Benutzern zu füllen.  Führen Sie die QuickStart-Anwendung aus, und melden Sie sich mit einem der Benutzernamen an.  Suchen Sie anhand des UPN nach anderen Benutzern.  Beenden Sie die Anwendung, und starten Sie sie erneut.  Wie Sie sehen, bleibt die Benutzersitzung erhalten.

ADAL erleichtert Ihnen die Integration all dieser allgemeinen Identitätsfunktionen in Ihrer Anwendung.  Sie übernimmt die unangenehmen Verwaltungsarbeiten für Sie, wie z.B. die Cacheverwaltung, Unterstützung des OAuth-Protokolls, Anzeige einer Anmeldeschnittstelle für den Benutzer und Aktualisierung abgelaufener Token.  Das Einzige, womit Sie sich noch beschäftigen müssen, ist der API-Aufruf `getToken`.

Als Referenz stellen wir das vollständige Beispiel (ohne Ihre Konfigurationswerte) auf [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip) bereit.  

## <a name="next-steps"></a>Nächste Schritte
Sie können sich nun weiteren Szenarien zuwenden.  Sie können beispielsweise Folgendes testen:

* [Schützen einer Node.JS-Web-API mit Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
* Lesen Sie [Vorgehensweise: Aktivieren von App-übergreifendem SSO unter iOS mit ADAL](active-directory-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


