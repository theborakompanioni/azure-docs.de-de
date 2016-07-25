<properties
	pageTitle="Azure AD v2.0: iOS-App | Microsoft Azure"
	description="Es wird beschrieben, wie Sie eine iOS-App erstellen, bei der sich Benutzer sowohl mit ihrem persönlichen Microsoft-Konto als auch mit ihrem Geschäfts-, Schul- oder Unikonto anmelden können."
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

# Hinzufügen der Anmeldung bei einer iOS-App mit dem v2.0-Endpunkt unter Verwendung einer Drittanbieterbibliothek mit Graph-API

Die Microsoft Identity-Plattform nutzt offene Standards, z.B. OAuth2 und OpenID Connect. Entwickler können jede gewünschte Bibliothek verwenden, um unsere Dienste zu integrieren. Um Entwickler bei der Nutzung unserer Plattform mit anderen Bibliotheken zu unterstützen, haben wir einige exemplarische Vorgehensweisen wie diese erstellt. Darin wird veranschaulicht, wie Sie Drittanbieterbibliotheken konfigurieren, um eine Verbindung mit der Microsoft-Identitätsplattform herzustellen. Die meisten Bibliotheken, die die [RFC6749 OAuth2-Spezifikation](https://tools.ietf.org/html/rfc6749) implementieren, können eine Verbindung mit der Microsoft-Identitätsplattform herstellen.

Mit der in dieser exemplarischen Vorgehensweise erstellten Anwendung können Benutzer sich bei ihrer Organisation anmelden und dann mithilfe der Graph-API in ihrer Organisation andere Benutzer suchen.

Wenn OAuth2 oder OpenID Connect neu für Sie sind, ergibt diese Beispielkonfiguration für Sie vielleicht noch keinen Sinn. Es empfiehlt sich, den Artikel [v2.0-Protokolle – OAuth 2.0-Autorisierungscodefluss](active-directory-v2-protocols-oauth-code.md) zu lesen, um Hintergrundinformationen zu erhalten.


> [AZURE.NOTE]
    Für einige Funktionen unserer Plattform, die über einen Ausdruck in den Standards OAuth2 oder OpenID Connect verfügen, z.B. für den bedingten Zugriff und die Intune-Richtlinienverwaltung, müssen Sie unsere Microsoft Azure-Open Source-Identitätsbibliotheken verwenden.

Der v2.0-Endpunkt unterstützt nicht alle Szenarien und Funktionen von Azure Active Directory.

> [AZURE.NOTE]
    Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.

## Code von GitHub herunterladen
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2) verwaltet. Um folgen zu können, können Sie [das App-Gerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) oder das Gerüst klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Sie können auch einfach das Beispiel herunterladen und direkt anfangen:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## Registrieren einer App
Erstellen Sie im [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com) eine neue App, oder führen Sie die detaillierten Schritte unter [Registrieren einer App mit dem v2.0-Endpunkt](active-directory-v2-app-registration.md) aus. Stellen Sie sicher, dass Sie:

- Kopieren Sie die Ihrer App zugewiesene **Anwendungs-ID**. Sie benötigen sie in Kürze.
- Fügen Sie die **Mobile**-Plattform Ihrer App hinzu.
- Kopieren Sie den **Umleitungs-URI** aus dem Portal. Sie müssen den Standardwert `urn:ietf:wg:oauth:2.0:oob` verwenden.


## Herunterladen der Drittanbieterbibliothek „NXOAuth2“ und Erstellen eines Arbeitsbereichs

Für diese exemplarische Vorgehensweise verwenden Sie den OAuth2Client von GitHub. Dies ist eine OAuth2-Bibliothek für Mac OS X und iOS (Cocoa und Cocoa Touch). Diese Bibliothek basiert auf Entwurf 10 der OAuth2-Spezifikation. Sie implementiert das native Anwendungsprofil und unterstützt den Autorisierungsendpunkt des Benutzers. Dies sind alle Voraussetzungen, die für die Integration in die Microsoft-Identitätsplattform erfüllt sein müssen.

### Hinzufügen der Bibliothek zu Ihrem Projekt mit CocoaPods

CocoaPods ist ein Abhängigkeits-Manager für Xcode-Projekte. Damit werden die obigen Installationsschritte automatisch durchgeführt.

```
$ vi Podfile
```
1. Fügen Sie diesem Podfile folgenden Code hinzu:

	```
	 platform :ios, '8.0'

	 target 'QuickStart' do

	 pod 'NXOAuth2Client'

	 end
	```

2. Laden Sie die Podfile-Datei mithilfe von CocoaPods. Dadurch wird ein neuer XCode-Arbeitsbereich erstellt, den Sie nun laden.

	```
	$ pod install
	...
	$ open QuickStart.xcworkspace
	```

## Erkunden der Struktur des Projekts

Im Gerüst ist die folgende Struktur für Ihr Projekt eingerichtet:

- Eine Masteransicht mit einer UPN-Suche
- Eine Detailansicht für die Daten zum ausgewählten Benutzer
- Eine Anmeldeansicht, in der Benutzer sich an der App anmelden können, um den Graphen abzufragen

Sie werden verschiedene Dateien im Gerüst öffnen, um die Authentifizierung hinzuzufügen. Andere Teile des Codes, z.B. der visuelle Code, sind für die Identität nicht von Belang, werden aber für Sie bereitgestellt.

## Einrichten der Datei „settings.plst“ in der Bibliothek

-	Öffnen Sie im QuickStart-Projekt die Datei `settings.plist`. Ersetzen Sie die Werte der Elemente in diesem Abschnitt durch die Werte, die Sie im Azure-Portal verwendet haben. Bei Verwendung der Active Directory Authentication Library verweist Ihr Code auf diese Werte.
    -	`clientId` ist die Client-ID der Anwendung, die Sie aus dem Portal kopiert haben.
    -	`redirectUri` ist die Umleitungs-URL, die über das Portal bereitgestellt wurde.

## Einrichten der NXOAuth2Client-Bibliothek im LoginViewController

Für die Einrichtung der NXOAuth2Client-Bibliothek sind einige Werte erforderlich. Nachdem Sie diese Aufgabe abgeschlossen haben, können Sie das abgerufene Token verwenden, um die Graph-API aufzurufen. Da `LoginView` bei jedem Authentifizierungsversuch aufgerufen wird, ist es sinnvoll, Konfigurationswerte in diese Datei einzugeben.

- Fügen Sie jetzt einige Werte zu `LoginViewController.m` hinzu, um den Kontext für die Authentifizierung und Autorisierung einzurichten. Im Anschluss an den Code finden Sie Details zu den Werten.

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

Sehen wir uns die Detail zum Code an.

Die erste Zeichenfolge ist für `scopes`. Der `User.ReadBasic.All`-Wert ermöglicht Ihnen das Lesen der Basisprofile aller Benutzer in Ihrem Verzeichnis.

Weitere Informationen zu allen verfügbaren Bereichen finden Sie unter [Microsoft Graph-Berechtigungsbereiche](https://graph.microsoft.io/docs/authorization/permission_scopes).

Für `authURL`, `loginURL`, `bhh` und `tokenURL` sollten Sie die zuvor angegebenen Werte verwenden. Wenn Sie die Microsoft Azure-Open Source-Identitätsbibliotheken verwenden, stellen wir diese Daten für Sie über unseren Metadatenendpunkt bereit. Wir haben Ihnen den Aufwand für das Extrahieren dieser Werte abgenommen.

Der Wert `keychain` ist der Container, den die NXOAuth2Client-Bibliothek verwendet, um einen Schlüsselbund zum Speichern Ihrer Token zu erstellen. Wenn Sie für eine größere Zahl von Apps das einmalige Anmelden (Single Sign-On, SSO) einrichten möchten, können Sie denselben Schlüsselbund in jeder Anwendung angeben und die Nutzung dieses Schlüsselbunds in Ihren Xcode-Berechtigungen anfordern. Dies wird in der Apple-Dokumentation erläutert.

Die restlichen Werte sind erforderlich, um die Bibliothek zu verwenden und Orte zu erstellen, über die Sie Werte in den Kontext übertragen können.

### Erstellen eines URL-Cache

In der Funktion `(void)viewDidLoad()`, die nach dem Laden der Sicht immer aufgerufen wird, weist der folgende Code einen Cache zur Verwendung zu.

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

### Erstellen einer Webansicht zum Anmelden

Eine Webansicht (WebView) kann Benutzer zur Angabe weiterer Faktoren auffordern, z.B. SMS (falls konfiguriert), oder Fehlermeldungen für Benutzer anzeigen. Hier richten Sie die Webansicht ein und schreiben dann später den Code zum Verarbeiten der Rückrufe, die in der Webansicht für die Identitätsdienste durchgeführt werden.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### Überschreiben der Webansichtmethoden zum Durchführen der Authentifizierung

Um der Webansicht mitzuteilen, was passiert, wenn ein Benutzer sich anmeldet (wie oben erläutert), können Sie folgenden Code einfügen.

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

### Schreiben von Code zum Behandeln des Ergebnisses der OAuth2-Anforderung

Der folgende Code verarbeitet die Umleitungs-URL, die von der Webansicht zurückgegeben wird. Wenn die Authentifizierung nicht erfolgreich war, versucht der Code es noch mal. In der Zwischenzeit stellt die Bibliothek den Fehler bereit, den Sie in der Konsole anzeigen oder asynchron verarbeiten können.

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

### Einrichten des OAuth-Kontexts (Kontospeicher)

Hier können Sie `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` im freigegebenen Kontospeicher für jeden Dienst aufrufen, auf den die Anwendung Zugriff haben soll. Der Kontotyp ist eine Zeichenfolge, die als Bezeichner für einen bestimmten Dienst verwendet wird. Da Sie auf die Graph-API zugreifen, verweist der Code darauf als `"myGraphService"`. Anschließend richten Sie einen Beobachter ein, der Sie darüber informiert, wenn beim Token etwas geändert wird. Nach dem Erhalt des Tokens geben Sie den Benutzer an `masterView` zurück.



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

Die Beschreibung einer MVC-App (Master-View-Controller), mit der die zurückgegebenen Daten im Raster angezeigt werden, würde den Rahmen dieser exemplarischen Vorgehensweise sprengen. Online sind viele Tutorials verfügbar, in denen die Erstellung erklärt wird. Der gesamte Code befindet sich in der Gerüstdatei. Sie müssen allerdings einige Schritte in dieser MVC-Anwendung ausführen:

* Eingreifen, wenn ein Benutzer einen Begriff in das Suchfeld eingibt
* Zurückgeben eines Datenobjekts an die Masteransicht (MasterView), damit die Ergebnisse im Raster angezeigt werden können

Dies führen wir weiter unten durch.

### Hinzufügen einer Prüfung, ob Sie angemeldet sind

Die Anwendung ist nicht sehr aktiv, wenn der Benutzer nicht angemeldet ist. Es ist also ratsam zu überprüfen, ob im Cache bereits ein Token vorhanden ist. Wenn nicht, richten Sie eine Umleitung an die Anmeldeansicht (LoginView) ein, damit der Benutzer sich anmelden kann. Vielleicht erinnern Sie sich: Die beste Möglichkeit zum Durchführen von Aktionen beim Laden einer Ansicht ist die Verwendung der von Apple bereitgestellten `viewDidLoad()`-Methode.

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

### Aktualisieren der Tabellenansicht nach dem Erhalt von Daten

Wenn die Graph-API Daten zurückgibt, müssen Sie diese anzeigen. Der Einfachheit halber ist hier der gesamte Code zum Aktualisieren der Tabelle angegeben. Sie können die richtigen Werte einfach in Ihre MVC-Codebausteine einfügen.

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

### Schaffen einer Möglichkeit zum Aufrufen der Graph-API bei einer Eingabe im Suchfeld

Wenn ein Benutzer einen Suchbegriff in das Feld eingibt, muss dieser an die Graph-API weitergeleitet werden. Die `GraphAPICaller`-Klasse, die Sie im folgenden Code erstellen, trennt die Lookupfunktionalität von der Darstellung. Jetzt schreiben Sie den Code, der Suchzeichen in die Graph-API eingibt. Hierzu stellen Sie eine Methode mit dem Namen `lookupInGraph` bereit, die die zu suchende Zeichenfolge enthält.

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

Dies ist das Kernstück unserer Anwendung. Während es bisher um das Einfügen von Code in das MVC-Standardmuster von Apple ging, schreiben Sie hier Code, um den Graphen abzufragen, während der Benutzer Text eingibt, und diese Daten dann zurückzugeben. Hier finden Sie den Code sowie eine ausführliche Erläuterung dazu.

### Erstellen einer neuen Objective-C-Headerdatei

Nennen Sie die Datei `GraphAPICaller.h`, und fügen Sie den folgenden Code hinzu.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Hier sehen Sie, dass eine angegebene Methode eine Zeichenfolge verwendet und ein completionBlock-Element zurückgibt. Dieses completionBlock-Element aktualisiert, wie Sie sich sicher denken können, die Tabelle, indem es in Echtzeit ein Objekt mit aufgefüllten Daten bereitstellt, während der Benutzer die Suche durchführt.


### Erstellen einer neuen Objective-C-Datei

Nennen Sie die Datei `GraphAPICaller.m`, und fügen Sie die folgende Methode hinzu.

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

Das Kernstück dieses Codes befindet sich in der `NXOAuth2Request`-Methode, die die Parameter verwendet, die Sie bereits in der Datei „settings.plist“ definiert haben.

Der erste Schritt ist die Erstellung des richtigen Graph-API-Aufrufs. Da Sie das `/users`-Element aufrufen, geben Sie es an, indem Sie es zusammen mit der Version an die Graph-API-Ressource anfügen. Es ist sinnvoll, diese Angaben in eine Datei mit externen Einstellungen einzufügen, da sie sich ändern können, wenn die API weiterentwickelt wird.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Als Nächstes müssen Sie Parameter angeben, die Sie ebenfalls für den Graph-API-Aufruf bereitstellen. Es ist *sehr wichtig*, dass Sie die Parameter nicht in den Ressourcenendpunkt einfügen, da zur Laufzeit alle nicht mit dem URI kompatiblen Zeichen bereinigt werden. Der gesamte Abfragecode muss in den Parametern bereitgestellt werden.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Sie haben vielleicht bemerkt, dass die `convertParamsToDictionary`-Methode aufgerufen wird, die Sie noch nicht geschrieben haben. Dies führen wir nun am Ende der Datei durch:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Als Nächstes verwenden Sie die `NXOAuth2Request`-Methode, um Daten im JSON-Format aus der API abzurufen.

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

Abschließend sehen wir uns an, wie Sie die Daten an den MasterViewController zurückgeben. Die Daten werden in serialisierter Form zurückgegeben. Sie müssen deserialisiert und in ein Objekt geladen werden, das vom MainViewController verwendet werden kann. Zu diesem Zweck verfügt das Gerüst über die Datei `User.m/h`, mit der ein User-Objekt erstellt wird. Sie füllen dieses User-Objekt mit Informationen aus dem Graphen auf.

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

Wenn Sie das Gerüst verwendet oder die Schritte der exemplarischen Vorgehensweise ausgeführt haben, sollte Ihre Anwendung nun ausgeführt werden können. Starten Sie den Simulator, und klicken Sie auf **Anmelden**, um die Anwendung zu verwenden.

## Abrufen von Sicherheitsupdates für unser Produkt

Wir empfehlen Ihnen, den Erhalt von Benachrichtigungen zu Sicherheitsvorfällen einzurichten. Rufen Sie dazu das [Security TechCenter](https://technet.microsoft.com/security/dd252948) auf, und abonnieren Sie die Sicherheitsempfehlungen.

<!---HONumber=AcomDC_0713_2016-->