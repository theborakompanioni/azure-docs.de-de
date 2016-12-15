---
title: 'Azure Active Directory B2C: Aufrufen einer Web-API aus einer iOS-Anwendung mit Drittanbieterbibliotheken| Microsoft Docs'
description: "In diesem Artikel wird gezeigt, wie Sie eine iOS-App für Aufgabenlisten erstellen, die eine Node.js-Web-API mithilfe von OAuth 2.0-Bearertoken und einer Drittanbieterbibliothek aufruft."
services: active-directory-b2c
documentationcenter: ios
author: xerners
manager: mbaldwin
editor: 
ms.assetid: d818a634-42c2-4cbd-bf73-32fa0c8c69d3
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: objectivec
ms.topic: hero-article
ms.date: 07/26/2016
ms.author: brandwe
translationtype: Human Translation
ms.sourcegitcommit: 0175f4e83aace12d8e4607f2ad924893093c6734
ms.openlocfilehash: cc5e199816668a5a0f936019ab8096e93a7a2f5a


---
# <a name="azure-ad-b2c-call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD B2C: Aufrufen einer Web-API aus einer iOS-Anwendung mit einer Drittanbieterbibliothek
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Die Microsoft Identity-Plattform nutzt offene Standards, z.B. OAuth2 und OpenID Connect. Dadurch können Entwickler eine beliebige Bibliothek verwenden, um sie in unsere Dienste zu integrieren. Um Entwickler bei der Nutzung unserer Plattform mit anderen Bibliotheken zu unterstützen, haben wir einige exemplarische Vorgehensweisen wie diese erstellt. Darin wird veranschaulicht, wie Sie Drittanbieterbibliotheken konfigurieren, um eine Verbindung mit der Microsoft Identity-Plattform herzustellen. Die meisten Bibliotheken, die die [RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749)-Spezifikation implementieren, können eine Verbindung mit der Microsoft Identity-Plattform herstellen.

Wenn OAuth2 oder OpenID Connect neu für Sie ist, ergibt diese Beispielkonfiguration für Sie vielleicht noch nicht viel Sinn. Wir empfehlen Ihnen, sich eine kurze [Übersicht über das Protokoll](active-directory-b2c-reference-protocols.md)anzusehen.

> [!NOTE]
> Für einige Funktionen unserer Plattform, die über einen Ausdruck in diesen Standards verfügen, z.B. Bedingter Zugriff und Intune-Richtlinienverwaltung, müssen Sie unsere Open Source-Microsoft Azure-Identitätsbibliotheken verwenden.
>
>

Nicht alle Szenarien und Funktionen von Azure Active Directory werden von der B2C-Plattform unterstützt.  Informationen zum Treffen der Entscheidung, ob die Verwendung der B2C-Plattform sinnvoll ist, finden Sie im Artikel zu den [B2C-Einschränkungen](active-directory-b2c-limitations.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Erstellen eines Azure AD B2C-Verzeichnisses
Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für alle Ihre Benutzer, Apps, Gruppen usw. Erstellen Sie, sofern noch nicht geschehen, [ein B2C-Verzeichnis](active-directory-b2c-get-started.md) , bevor Sie fortfahren.

## <a name="create-an-application"></a>Erstellen einer Anwendung
Als Nächstes müssen Sie in Ihrem B2C-Verzeichnis eine App erstellen. Dadurch werden Azure AD die Informationen bereitgestellt, die für die sichere Kommunikation mit Ihrer App erforderlich sind. Die App und die Web-API werden in diesem Fall durch eine einzelne **Anwendungs-ID** dargestellt, da sie zusammen eine logische App bilden. Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-b2c-app-registration.md). Führen Sie folgende Schritte aus:

* Binden Sie ein **mobiles Gerät** in die Anwendung ein.
* Kopieren Sie die **Anwendungs-ID** , die Ihrer App zugewiesen ist. Diese benötigen sie später ebenfalls.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen der Richtlinien
In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese App enthält eine kombinierte Identitätsoberfläche für die Anmeldung und Registrierung. Sie müssen diese Richtlinie für jeden Typ erstellen, wie im [Artikel zu Richtlinienreferenzen](active-directory-b2c-reference-policies.md#create-a-sign-up-policy)beschrieben. Achten Sie beim Erstellen der Richtlinie auf Folgendes:

* Wählen Sie den **Anzeigenamen** und die Registrierungsattribute in der Richtlinie aus.
* Wählen Sie den **Anzeigenamen** und die **Objekt-ID** als Anwendungsansprüche in jeder Richtlinie aus. Sie können auch andere Ansprüche auswählen.
* Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie müssen das Präfix `b2c_1_`aufweisen.  Sie benötigen den Richtliniennamen später noch.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die Richtlinien erstellt haben, können Sie Ihre App erstellen.

## <a name="download-the-code"></a>Herunterladen des Codes
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)verwaltet.  Um den Ablauf nachzuverfolgen, können Sie [die App im ZIP-Format herunterladen](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)(/archive/master.zip) oder klonen:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Oder laden Sie einfach den fertigen Code herunter, um sofort zu beginnen:

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Herunterladen der Drittanbieterbibliothek „nxoauth2“ und Starten eines Arbeitsbereichs
Für diese exemplarische Vorgehensweise verwenden wir OAuth2Client von GitHub. Dies ist eine OAuth2-Bibliothek für Mac OS X und iOS (Cocoa und Cocoa Touch). Diese Bibliothek basiert auf Entwurf 10 der OAuth2-Spezifikation. Sie implementiert das native Anwendungsprofil und unterstützt den Endbenutzer-Autorisierungsendpunkt. Dies sind alle Voraussetzungen, die für die Integration in die Microsoft Identity-Plattform erfüllt sein müssen.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>Hinzufügen der Bibliothek zu Ihrem Projekt mit CocoaPods
CocoaPods ist ein Abhängigkeits-Manager für Xcode-Projekte. Damit werden die obigen Installationsschritte automatisch verwaltet.

```
$ vi Podfile
```
Fügen Sie diesem Podfile folgenden Code hinzu:

```
 platform :ios, '8.0'

 target 'SampleforB2C' do

 pod 'NXOAuth2Client'

 end
```

Laden Sie nun das Podfile mithilfe von Cocoapods. Dadurch wird ein neuer XCode-Arbeitsbereich erstellt, den Sie nun laden.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>Struktur des Projekts
Wir haben im Gerüst die folgende Struktur für unser Projekt eingerichtet:

* Eine **Masteransicht** mit einem Aufgabenbereich
* Eine **Ansicht zum Hinzufügen von Aufgaben** für die Daten zur ausgewählten Aufgabe
* Eine **Anmeldeansicht** die Benutzern das Anmelden an der App ermöglicht

Wir springen in verschiedene Dateien im Projekt, um die Authentifizierung hinzuzufügen. Andere Teile des Codes, z.B. der visuelle Code, sind für die Identität nicht von Belang und werden für Sie bereitgestellt.

## <a name="create-the-settingsplist-file-for-your-application"></a>Erstellen der Datei `settings.plist` für Ihre Anwendung
Es ist einfacher, die Anwendung zu konfigurieren, wenn ein zentraler Speicherort für unsere Konfigurationswerte vorhanden ist. Außerdem können Sie so besser verstehen, welchen Zweck die einzelnen Einstellungen in Ihrer Anwendung erfüllen. Wir nutzen die *Eigenschaftenliste* als Möglichkeit, um diese Werte für die Anwendung anzugeben.

* Erstellen bzw. Öffnen der Datei `settings.plist` unter `Supporting Files` im Anwendungsarbeitsbereich
* Eingeben der folgenden Werte (Erläuterung weiter unten)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Wir sehen uns nun die Details an.

Für `authURL`, `loginURL`, `bhh` und `tokenURL` werden Sie bemerken, dass Sie Ihren Mandantennamen einfügen müssen. Dies ist der Mandantenname des B2C-Mandanten, der Ihnen zugewiesen wurde. Ein Beispiel hierfür ist `kidventusb2c.onmicrosoft.com`. Wenn Sie unsere Open Source-Microsoft Azure-Identitätsbibliotheken verwenden, stellen wir diese Daten für Sie über unseren Metadatenendpunkt bereit. Wir haben Ihnen den Aufwand für das Extrahieren dieser Werte abgenommen.

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Der Wert `keychain` ist der Container, den die NXOAuth2Client-Bibliothek verwendet, um einen Schlüsselbund zum Speichern Ihrer Token zu erstellen. Wenn Sie für eine größere Zahl von Apps das einmalige Anmelden (SSO) einrichten möchten, können Sie denselben Schlüsselbund in jeder Anwendung angeben und die Nutzung dieses Schlüsselbunds in Ihren XCode-Berechtigungen anfordern. Dieser Vorgang ist in der Apple-Dokumentation beschrieben.

`<policy name>` am Ende einer URL ist der Ort, an dem Sie die oben erstellte Richtlinie einfügen. Die App ruft diese Richtlinien in Abhängigkeit des jeweiligen Ablaufs auf.

`taskAPI` ist der REST-Endpunkt, den wir mit Ihrem B2C-Token aufrufen, um Aufgaben hinzuzufügen oder vorhandene Aufgaben abzufragen. Dies wurde speziell für dieses Beispiel eingerichtet. Sie müssen keine Änderungen vornehmen, damit das Beispiel funktioniert.

Die restlichen Werte sind erforderlich, um die Bibliothek zu verwenden und einfach Orte zu schaffen, über die Sie Werte in den Kontext einbringen können.

Nachdem wir die Datei `settings.plist` erstellt haben, benötigen wir Code, um sie lesen zu können.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Einrichten einer AppData-Klasse zum Lesen der Einstellungen
Wir erstellen eine einfache Datei, mit der die oben erstellte Datei `settngs.plist` analysiert wird und die Einstellungen für jede Klasse bereitgestellt werden. Da wir nicht jedes Mal eine neue Kopie der Daten erstellen möchten, wenn diese von einer Klasse angefordert werden, verwenden wir ein Singleton-Muster und geben immer nur die gleiche Instanz zurück, die bei einer Anforderung der Einstellungen erstellt wird.

* Erstellen Sie eine Datei vom Typ `AppData.h`:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Erstellen Sie eine Datei vom Typ `AppData.m`:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Nun können wir einfach auf unsere Daten zugreifen, indem wir `  AppData *data = [AppData getInstance];` in einer beliebigen Klasse aufrufen, wie weiter unten gezeigt.

## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>Einrichten der NXOAuth2Client-Bibliothek in AppDelegate
Für die Einrichtung der NXOAuthClient-Bibliothek sind einige Werte erforderlich. Nach Abschluss dieses Vorgangs können Sie das Token verwenden, das zum Aufrufen der REST-API abgerufen wird. Da `AppDelegate` immer aufgerufen wird, wenn die Anwendung geladen wird, ergibt es Sinn, unsere Konfigurationswerte in diese Datei einzufügen.

* Öffnen Sie die Datei `AppDelegate.m` .
* Importieren Sie einige Headerdateien zur späteren Verwendung.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Fügen Sie die `setupOAuth2AccountStore` -Methode im AppDelegate-Element hinzu.

Wir müssen ein AccountStore-Element erstellen und dann die Daten einfügen, die wir gerade aus der Datei `settings.plist` eingelesen haben.

Sie sollten an diesem Punkt einige Aspekte in Bezug auf den B2C-Dienst beachten, um den Code verständlicher zu gestalten:

1. Azure AD B2C verwendet die von den Abfrageparametern bereitgestellte *Richtlinie* , um Ihre Anforderung zu verarbeiten. So kann Azure Active Directory als unabhängiger Dienst nur für Ihre Anwendung fungieren. Zur Bereitstellung dieser zusätzlichen Abfrageparameter müssen wir die `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` -Methode mit unseren benutzerdefinierten Richtlinienparametern angeben.
2. Azure AD B2C nutzt Bereiche (Scopes) auf ähnliche Weise wie andere OAuth2-Server. Da es bei der Verwendung von B2C genauso um die Authentifizierung eines Benutzers wie um den Zugriff auf Ressourcen geht, sind einige Bereiche dringend erforderlich, damit der richtige Fluss sichergestellt ist. Dies ist der Bereich `openid` . Unsere Microsoft Identity SDKs stellen den Bereich `openid` automatisch für Sie bereit, und er ist daher nicht Teil unserer SDK-Konfiguration. Da wir eine Drittanbieterbibliothek verwenden, müssen wir diesen Bereich aber angeben.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Rufen Sie ihn als Nächstes im AppDelegate-Element unter der `didFinishLaunchingWithOptions:` -Methode auf.

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Erstellen einer `LoginViewController`-Klasse zum Verarbeiten von Authentifizierungsanforderungen
Wir nutzen eine Webansicht für die Kontoanmeldung. Auf diese Weise können wir Benutzer zur Angabe weiterer Faktoren auffordern, z.B. SMS (falls konfiguriert), oder Fehlermeldungen für Benutzer anzeigen. Hier richten wir die Webansicht ein und schreiben dann später den Code für die Rückrufe, die in der Webansicht für den Microsoft Identity-Dienst durchgeführt werden.

* Erstellen einer `LoginViewController.h`-Klasse

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Wir erstellen diese Methoden weiter unten.

> [!NOTE]
> Vergewissern Sie sich, dass Sie `loginView` an die tatsächliche Webansicht binden, die sich innerhalb Ihres Storyboards befindet. Andernfalls verfügen Sie nicht über eine Webansicht, die als Popup eingeblendet werden kann, wenn die Authentifizierung durchgeführt werden muss.
>
>

* Erstellen einer `LoginViewController.m` -Klasse
* Hinzufügen von Variablen für den Zustand beim Authentifizieren

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE;
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Überschreiben der Webansichtmethoden zum Durchführen der Authentifizierung

Wir müssen für die Webansicht das Verhalten angeben, das gezeigt werden soll, wenn sich ein Benutzer wie oben beschrieben anmelden möchte. Sie können den Code unten einfach ausschneiden und einfügen.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Schreiben von Code zum Behandeln des Ergebnisses der OAuth2-Anforderung

Wir benötigen Code für die Behandlung der redirectURL, die wir von der Webansicht zurückerhalten. Wenn der Vorgang nicht erfolgreich ist, probieren wir es erneut. In der Zwischenzeit stellt die Bibliothek den Fehler bereit, den Sie in der Konsole anzeigen oder asynchron behandeln können.

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Richten Sie die Factorys für die Benachrichtigung ein.

Wir erstellen die gleiche Methode wie oben im `AppDelegate`-Element, fügen diesmal aber einige `NSNotification`-Elemente hinzu, um Informationen zu Vorgängen in unserem Dienst zu erhalten. Wir richten einen Beobachter ein, der uns darüber informiert, wenn sich für das Token Änderungen ergeben. Nach dem Erhalt des Tokens geben wir den Benutzer an das `masterView`-Element zurück.

```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Hinzufügen von Code zur Behandlung des Benutzers, wenn eine Anforderung für die Anmeldung initiiert wird

Wir erstellen eine Methode, die jeweils aufgerufen wird, wenn eine Anforderung zur Authentifizierung ansteht. Dies ist die eigentliche Methode zur Erstellung einer Webansicht.

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Abschließend rufen wir alle Methoden, die wir oben geschrieben haben, bei jedem Ladevorgang von `LoginViewController` auf. Hierfür fügen wir diese Methoden der `viewDidLoad` -Methode von Apple hinzu.

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Sie haben nun die Hauptschritte zur Interaktion mit der Anwendung für die Anmeldung ausgeführt. Nach der Anmeldung müssen wir unsere empfangenen Token verwenden. Hierfür erstellen wir Hilfscode, mit dem REST-APIs für uns über diese Bibliothek aufgerufen werden.

## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Erstellen einer `GraphAPICaller` -Klasse zum Verarbeiten unserer Anforderungen für eine REST-API
Bei jedem Laden der App wird eine Konfiguration geladen. Nachdem wir ein Token beschafft haben, müssen wir die Konfiguration nun nutzen.

* Erstellen der Datei `GraphAPICaller.h`

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

In diesem Code sehen Sie, dass wir zwei Methoden erstellen: eine zum Abrufen der Aufgaben aus einer API und eine zum Hinzufügen von Aufgaben zur API.

Nachdem wir die Schnittstelle nun eingerichtet haben, fügen wir die eigentliche Implementierung hinzu:

* Erstellen der Datei `GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

//
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

//
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App
Der letzte Schritt besteht im Erstellen und Ausführen der App in Xcode. Registrieren Sie sich bei der App, oder melden Sie sich bei der App an, und erstellen Sie Aufgaben für einen angemeldeten Benutzer. Melden Sie sich ab, melden Sie sich als ein anderer Benutzer wieder an, und erstellen Sie für diesen Benutzer Aufgaben.

Beachten Sie, wie die Aufgaben pro Benutzer in der API gespeichert werden, da die API die Benutzeridentität aus dem empfangenen Zugriffstoken extrahiert.

## <a name="next-steps"></a>Nächste Schritte
Sie können nun mit den B2C-Themen für fortgeschrittenere Benutzer fortfahren. Versuchen Sie Folgendes:

[Rufen Sie eine Node.js-Web-API aus einer Node.js-Web-App auf.]()

[Passen Sie die UX für eine B2C-Anwendung an.]()



<!--HONumber=Dec16_HO1-->


