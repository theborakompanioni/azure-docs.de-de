<properties
	pageTitle="Azure AD v2.0: iOS-App | Microsoft Azure"
	description="Vorgehensweise beim Erstellen einer iOS-App, bei der sich Benutzer sowohl mit ihrem persönlichen Microsoft-Konto als auch ihrem Geschäfts- oder Schulkonto anmelden können, indem sie Drittanbieterbibliotheken verwenden."
	services="active-directory"
	documentationCenter=""
	authors="brandwe"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="mobile-ios"
	ms.devlang="objective-c"
	ms.topic="article"
	ms.date="06/28/2016"
	ms.author="brandwe"/>

# Hinzufügen der Anmeldung zu einer iOS-App mit einer Drittanbieterbibliothek per Graph-API und v2.0-Endpunkt

Die Microsoft Identity-Plattform nutzt offene Standards, z.B. OAuth2 und OpenID Connect. Dadurch können Entwickler eine beliebige Bibliothek verwenden, um sie in unsere Dienste zu integrieren. Um Entwickler bei der Nutzung unserer Plattform mit anderen Bibliotheken zu unterstützen, haben wir einige exemplarische Vorgehensweisen wie diese erstellt. Darin wird veranschaulicht, wie Sie Drittanbieterbibliotheken konfigurieren, um eine Verbindung mit der Microsoft Identity-Plattform herzustellen. Die meisten Bibliotheken, die die [RFC6749 OAuth2-Spezifikation](https://tools.ietf.org/html/rfc6749) implementieren, können eine Verbindung mit der Microsoft Identity-Plattform herstellen.

Diese Anwendung ermöglicht Benutzern das Anmelden bei ihrer Organisation und das Suchen nach anderen Personen in der Organisation mit der Graph-API.

Wenn OAuth2 oder OpenID Connect neu für Sie ist, ergibt diese Beispielkonfiguration für Sie vielleicht noch nicht viel Sinn. Wir empfehlen Ihnen, sich eine kurze [Übersicht über das Protokoll](active-directory-v2-protocols-oauth-code.md) anzusehen.


> [AZURE.NOTE]
    Für einige Funktionen unserer Plattform, die über einen Ausdruck in diesen Standards verfügen, z.B. Bedingter Zugriff und Intune-Richtlinienverwaltung, müssen Sie unsere Open Source-Microsoft Azure-Identitätsbibliotheken verwenden.

> [AZURE.NOTE] 
    Nicht alle Szenarien und Funktionen von Azure Active Directory werden vom v2.0-Endpunkt unterstützt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.

## Herunterladen
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2) verwaltet. Um folgen zu können, können Sie [das App-Gerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) oder das Gerüst klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Oder beginnen Sie sofort nach dem Durchführen des Downloads:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## Registrieren einer App
Erstellen Sie eine neue App unter [apps.dev.microsoft.com](https://apps.dev.microsoft.com), oder führen Sie die folgenden [ausführlichen Schritte](active-directory-v2-app-registration.md) aus. Stellen Sie sicher, dass Sie:

- die Ihrer App zugewiesene **Anwendungs-ID** kopieren. Sie benötigen Sie in Kürze.
- Fügen Sie die **Mobile**-Plattform Ihrer App hinzu.
- Kopieren Sie den **Umleitungs-URI** aus dem Portal. Sie müssen den Standardwert `urn:ietf:wg:oauth:2.0:oob` verwenden.


## Herunterladen der Drittanbieterbibliothek „nxoauth2“ und Starten eines Arbeitsbereichs

Für diese exemplarische Vorgehensweise verwenden wir OAuth2Client von GitHub. Dies ist eine OAuth2-Bibliothek für Mac OS X und iOS (Cocoa und Cocoa Touch). Diese Bibliothek basiert auf Entwurf 10 der OAuth2-Spezifikation. Sie implementiert das native Anwendungsprofil und unterstützt den Endbenutzer-Autorisierungsendpunkt. Dies sind alle Voraussetzungen, die für die Integration in die Microsoft Identity-Plattform erfüllt sein müssen.

### Hinzufügen der Bibliothek zu Ihrem Projekt mit CocoaPods

CocoaPods ist ein Abhängigkeits-Manager für Xcode-Projekte. Damit werden die obigen Installationsschritte automatisch verwaltet.

```
$ vi Podfile
```
Fügen Sie diesem Podfile folgenden Code hinzu:

```
 platform :ios, '8.0'
 
 target 'QuickStart' do
 
 pod 'NXOAuth2Client'
 
 end
```

Laden Sie nun das Podfile mithilfe von Cocoapods. Dadurch wird ein neuer XCode-Arbeitsbereich erstellt, den Sie nun laden.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

## Struktur des Projekts

Wir haben im Gerüst die folgende Struktur für unser Projekt eingerichtet:

* Eine Masteransicht mit einer UPN-Suche
* Eine Detailansicht für die Daten zum ausgewählten Benutzer
* Eine Anmeldeansicht, die Benutzern das Anmelden an der App zum Abfragen des Graphen ermöglicht

Wir springen in verschiedene Dateien im Gerüst, um die Authentifizierung hinzuzufügen. Andere Teile des Codes, z.B. der visuelle Code, sind für die Identität nicht von Belang und werden für Sie bereitgestellt.

## Einrichten der Datei „settings.plst“ in der Bibliothek

-	Öffnen Sie im Projekt "QuickStart" die PLIST-Datei `settings.plist`. Ersetzen Sie die Werte der Elemente in diesem Abschnitt durch die Werte, die Sie im Azure-Portal eingegeben haben. Sobald Ihr Code ADAL verwendet, verweist er auf diese Werte.
    -	`clientId` ist die Client-ID Ihrer Anwendung, die Sie aus dem Portal kopiert haben.
    -	`redirectUri` ist die Umleitungs-URL, die über das Portal bereitgestellt wurde.

## Einrichten der NXOAuth2Client-Bibliothek im LoginViewController

Für die Einrichtung der NXOAuthClient-Bibliothek sind einige Werte erforderlich. Nach Abschluss dieses Vorgangs können Sie das Token verwenden, das zum Aufrufen der Graph-API abgerufen wird. Da `LoginView` immer aufgerufen wird, wenn eine Authentifizierung erforderlich ist, ergibt es Sinn, unsere Konfigurationswerte in diese Datei einzufügen.
* Öffnen Sie die Datei `LoginViewController.m`.

* Wir fügen dem Code jetzt einige Werte hinzu, die den Kontext für die Authentifizierung und Autorisierung schaffen. Diese werden im Folgenden ausführlicher beschrieben.

```objc
NSString *scopes = @"offline_access User.ReadBasic.All";
NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
NSString *loginURL = @"https://login.microsoftonline.com/common/login";
NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
NSURL *myRequestedUrl;
NSURL *myLoadedUrl;
bool loginFlow = FALSE;
bool isRequestBusy;
NSURL *authcode;
```

Wir sehen uns nun die Details an.

Die erste Zeichenfolge, die wir festlegen, ist für `scopes` bestimmt. Die Bereiche, die wir für diese Anwendung anfordern, lauten `User.ReadBasic.All`. Dies ermöglicht es uns, das grundlegende Profil aller Benutzer in unserem Verzeichnis zu lesen. Weitere Informationen zu allen verfügbaren Bereichen für Microsoft Graph finden Sie [hier](https://graph.microsoft.io/docs/authorization/permission_scopes).

Für `authURL`, `loginURL`, `bhh` und `tokenURL` sollten Sie die oben angegebenen Werte verwenden. Wenn Sie unsere Open Source-Microsoft Azure-Identitätsbibliotheken verwenden, stellen wir diese Daten für Sie über unseren Metadatenendpunkt bereit. Wir haben Ihnen den Aufwand für das Extrahieren dieser Werte abgenommen.

Der Wert `keychain` ist der Container, der von der NXOAuth2Client-Bibliothek verwendet wird, um einen Schlüsselbund zum Speichern Ihrer Token zu erstellen. Wenn Sie für eine größere Zahl von Apps das einmalige Anmelden (SSO) einrichten möchten, können Sie denselben Schlüsselbund in jeder Anwendung angeben und die Nutzung dieses Schlüsselbunds in Ihren XCode-Berechtigungen anfordern. Dieser Vorgang ist in der Apple-Dokumentation beschrieben.

Die restlichen Werte sind erforderlich, um die Bibliothek zu verwenden und einfach Orte zu schaffen, über die Sie Werte in den Kontext einbringen können.

* Erstellen eines URL-Caches

Im Element `(void)viewDidLoad()`, das nach dem Laden der Ansicht immer aufgerufen wird, richten wir einen Cache für unsere Zwecke ein.

Fügen Sie den folgenden Code hinzu:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];
    
}
```

* Erstellen einer Webansicht (WebView), die wir zum Anmelden verwenden können

Wir nutzen eine Webansicht für die Kontoanmeldung. Auf diese Weise können wir Benutzer zur Angabe weiterer Faktoren auffordern, z.B. SMS (falls konfiguriert), oder Fehlermeldungen für Benutzer anzeigen. Hier richten wir die Webansicht ein und schreiben dann später den Code für die Rückrufe, die in der Webansicht für den Microsoft Identity-Dienst durchgeführt werden.

```objc
-(void)requestOAuth2Access {
    //in order to login to Mircosoft APIs using OAuth2 we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client
                                       
                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

* Überschreiben der Webansichtmethoden zum Durchführen der Authentifizierung

Wir müssen für die Webansicht das Verhalten angeben, das gezeigt werden soll, wenn sich ein Benutzer wie oben beschrieben anmelden möchte. Sie können den Code unten einfach ausschneiden und einfügen.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
    
    // We get the auth token from a redirect so we need to handle that in the webview.
    
    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }
    
    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];
    
    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {
    
    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);
    
    // The webview is where all the communication happens. Slightly complicated.
    
    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);
    
    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    
    return YES;

}
```

* Schreiben von Code zum Behandeln des Ergebnisses der OAuth2-Anforderung

Wir benötigen Code für die Behandlung der redirectURL, die wir von der Webansicht zurückerhalten. Wenn der Vorgang nicht erfolgreich ist, probieren wir es erneut. In der Zwischenzeit stellt die Bibliothek den Fehler bereit, den Sie in der Konsole anzeigen oder asynchron behandeln können.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {
    
    AppData* data = [AppData getInstance];
    
    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

* Einrichten des OAuth-Kontexts (Kontospeicher)

Hier können Sie `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` im freigegebenen Kontospeicher für jeden Dienst aufrufen, auf den Sie über Ihre Anwendung Zugriff haben möchten. Der Kontotyp ist eine Zeichenfolge, die als Bezeichner für einen bestimmten Dienst verwendet wird. Da wir auf die Graph-API zugreifen, vergeben wir den Namen `"myGraphService"`. Anschließend richten wir einen Beobachter ein, der uns darüber informiert, wenn sich für das Token Änderungen ergeben. Nach dem Erhalt des Tokens geben wir den Benutzer an das `masterView`-Element zurück.



```objc
- (void)setupOAuth2AccountStore {
    

        AppData* data = [AppData getInstance];
    
    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];
    
    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {
                                                              
                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];
    
    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## Einrichten der Masteransicht zum Durchsuchen und Anzeigen der Benutzer aus der Graph-API

Die Beschreibung einer MVC-App, mit der die zurückgegebenen Daten im Raster angezeigt werden, würde den Rahmen dieser exemplarischen Vorgehensweise sprengen. Online sind viele Tutorials verfügbar, in denen die Erstellung erklärt wird. Wir stellen diesen Code für Sie in der Gerüstdatei bereit. In dieser MVC-Anwendung müssen wir aber einige Schritte ausführen:

* Eingreifen, wenn ein Benutzer einen Begriff in das Suchfeld eingibt
* Zurückgeben eines Datenobjekts an die Masteransicht (MasterView), damit die Ergebnisse im Raster angezeigt werden können

Dies führen wir weiter unten durch.

* Hinzufügen einer Prüfung, ob wir angemeldet sind

Die Anwendung ist nicht sehr aktiv, wenn der Benutzer nicht angemeldet ist. Es ist also ratsam zu überprüfen, ob im Cache bereits ein Token vorhanden ist. Wenn nicht, führen wir eine Umleitung an die Anmeldeansicht (LoginView) durch, damit der Benutzer sich anmelden kann. Vielleicht erinnern Sie sich: Die beste Möglichkeit zum Durchführen von Aktionen beim Laden einer Ansicht ist die Verwendung der von Apple bereitgestellten `viewDidLoad()`-Methode.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    
    
    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    
        if (accounts.count == 0) {
        
        dispatch_async(dispatch_get_main_queue(),^ {
            
            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

* Aktualisieren der Tabellenansicht nach dem Erhalt von Daten

Wenn wir Daten von der Graph-API zurückerhalten, müssen wir diese Daten anzeigen. Der Einfachheit halber ist hier der gesamte Code zum Aktualisieren der Tabelle angegeben. Sie können die richtigen Werte einfach in Ihre MVC-Codebausteine einfügen.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];
    
    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }
    
    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];

    
    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];
    
    return cell;
}

```

* Schaffen einer Möglichkeit zum Aufrufen der Graph-API bei einer Eingabe im Suchfeld

Wenn ein Benutzer einen Suchbegriff in das Feld eingibt, müssen wir den Begriff an die Graph-API weiterleiten. Wir erstellen eine weitere Klasse mit dem Namen `GraphAPICaller`, um die Suchfunktion besser von der Darstellung zu trennen. Diese Erstellung ist unten beschrieben. Vorerst schreiben wir den Code, um die Weitergabe von Suchzeichen an die Graph-API zu ermöglichen. Hierzu stellen wir eine Methode mit dem Namen `lookupInGraph` bereit, für die die zu suchende Zeichenfolge verwendet wird.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {
        
    };
    

    
        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {
                
                
                upnArray = returnedUpns;
                
                
            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];
                
                [alertView setDelegate:self];
                
                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }
            
            
        }];
    
    
} 
```

## Schreiben einer Hilfsklasse zum Zugreifen auf die Graph-API

Dies ist das Kernstück unserer Anwendung. Während es bisher um das Einfügen von Code in das MVC-Standardmuster von Apple ging, schreiben wir hier Code zum Abfragen des Graphen, während der Benutzer Text eingibt, und zum Zurückgeben dieser Daten. Wir zeigen den Code und erläutern ihn dann ausführlich.

* Erstellen Sie eine neue Objective C-Headerdatei mit dem Namen `GraphAPICaller.h`, indem Sie den folgenden Code verwenden:

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Sie sehen, dass wir eine Methode angeben, bei der eine Zeichenfolge verwendet und ein completionBlock-Element zurückgegeben wird. Mit diesem completionBlock-Element wird wie erwartet die Tabelle aktualisiert, indem ein Objekt mit aufgefüllten Daten in Echtzeit bereitgestellt wird, während der Benutzer die Suche durchführt.


* Erstellen Sie eine neue Objective C-Datei mit dem Namen `GraphAPICaller.m`, und fügen Sie die folgende Methode hinzu:

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }
    
    AppData* data = [AppData getInstance];
    
    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
    
    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];
    
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);
                           
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
                           
                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)
                           
                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];
                           
                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];
                               
                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];
                               
                               
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

```

Wir gehen diese Methode nun ausführlich durch.

Das Kernstück dieses Codes befindet sich in der `NXOAuth2Request`-Methode, für die die Parameter verwendet werden, die wir zu Beginn in der Datei „settings.plist“ definiert haben. Der erste Schritt ist die Erstellung des richtigen Graph-API-Aufrufs. Da wir das `/users`-Element aufrufen, geben wir es an, indem wir das Element zusammen mit der Version an die Graph-API-Ressource anfügen. Es ist sinnvoll, diese Angaben in eine Datei mit externen Einstellungen einzufügen, da sie sich ändern können, wenn die API weiterentwickelt wird.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Als Nächstes müssen wir Parameter angeben, die wir ebenfalls für den Graph-API-Aufruf bereitstellen. Es ist *sehr wichtig*, dass Sie die Parameter nicht in den Ressourcenendpunkt einfügen, da zur Laufzeit für alle nicht mit dem URI kompatiblen Zeichen eine Bereinigung durchgeführt wird. Der gesamte Abfragecode muss in den Parametern bereitgestellt werden.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Vielleicht fällt Ihnen auf, dass die `convertParamsToDictionary`-Methode aufgerufen wird, die wir noch nicht geschrieben haben. Dies führen wir nun am Ende der Datei durch:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];
    
        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];
    
           [dictionary setValue:query forKey:@"$filter"];


    
    return dictionary;
}

```
Als Nächstes verwenden wir die `NXOAuth2Request`-Methode, um Daten im JSON-Format wieder von der API abzurufen.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);
                           
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Abschließend sehen wir uns an, wie wir die Daten an den MasterViewController zurückgeben. Wir erhalten die Daten in serialisierter Form zurück. Sie müssen deserialisiert und in ein Objekt geladen werden, das vom MainViewController verwendet werden kann. Zu diesem Zweck verfügt das Gerüst über die Datei `User.m/h`, mit der ein User-Objekt erstellt wird. Wir füllen dieses Benutzerobjekt mit Informationen aus dem Graphen auf.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
                           
                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)
                           
                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];
                           
                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];
                               
                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];
                               
                               
                               [Users addObject:s];
```


## Ausführen des Beispiels

Wenn Sie das Gerüst verwendet oder die Schritte der exemplarischen Vorgehensweise ausgeführt haben, sollte Ihre Anwendung nun ausgeführt werden können. Starten Sie den Simulator, und klicken Sie auf „Anmelden“, um die Anwendung zu verwenden.

## Abrufen von Sicherheitsupdates für unser Produkt

Wir empfehlen Ihnen, den Erhalt von Benachrichtigungen zu Sicherheitsvorfällen einzurichten. Rufen Sie dazu [diese Seite](https://technet.microsoft.com/security/dd252948) auf, und abonnieren Sie die Sicherheitsempfehlungen.

<!---HONumber=AcomDC_0629_2016-->