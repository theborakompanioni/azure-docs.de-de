---
title: "Erste Schritte in Azure AD v2 iOS – Verwendung | Microsoft-Dokumentation"
description: "Informationen, wie iOS (Swift)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 81d5ae3775cca7cea5abf1922a25be0663a8a949
ms.contentlocale: de-de


---

## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Verwenden der Microsoft Authentication Library (MSAL) zum Abrufen eines Tokens für die Microsoft Graph-API

Öffnen Sie `ViewController.swift`, und ersetzen Sie den Code mit:

```swift
import UIKit
import MSAL

class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {
    
    let kClientID = "Your_Application_Id_Here"
    let kAuthority = "https://login.microsoftonline.com/common/v2.0"

    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    
    var accessToken = String()
    var applicationContext = MSALPublicClientApplication.init()

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

     // This button will invoke the call to the Microsoft Graph API. It uses the
     // built in Swift libraries to create a connection.
    
    @IBAction func callGraphButton(_ sender: UIButton) {
        
        
        do {
            
            // We check to see if we have a current logged in user. If we don't, then we need to sign someone in.
            // We throw an interactionRequired so that we trigger the interactive signin.
            
            if  try self.applicationContext.users().isEmpty {
                throw NSError.init(domain: "MSALErrorDomain", code: MSALErrorCode.interactionRequired.rawValue, userInfo: nil)
            } else {
            
            // Acquire a token for an existing user silently
            
            try self.applicationContext.acquireTokenSilent(forScopes: self.kScopes, user: applicationContext.users().first) { (result, error) in
    
                    if error == nil {
                        self.accessToken = (result?.accessToken)!
                        self.loggingText.text = "Refreshing token silently)"
                        self.loggingText.text = "Refreshed Access token is \(self.accessToken)"
                        
                        self.signoutButton.isEnabled = true;
                        self.getContentWithToken()
    
                    } else {
                        self.loggingText.text = "Could not acquire token silently: \(error ?? "No error information" as! Error)"
    
                    }
                }
            }
        }  catch let error as NSError {
            
            // interactionRequired means we need to ask the user to sign-in. This usually happens
            // when the user's Refresh Token is expired or if the user has changed their password
            // among other possible reasons.
            
            if error.code == MSALErrorCode.interactionRequired.rawValue {
                
                self.applicationContext.acquireToken(forScopes: self.kScopes) { (result, error) in
                        if error == nil {
                            self.accessToken = (result?.accessToken)!
                            self.loggingText.text = "Access token is \(self.accessToken)"
                            self.signoutButton.isEnabled = true;
                            self.getContentWithToken()
                            
                        } else  {
                            self.loggingText.text = "Could not acquire token: \(error ?? "No error information" as! Error)"
                        }
                }
                
            }
            
        } catch {
            
            // This is the catch all error.
            
            self.loggingText.text = "Unable to acquire token. Got error: \(error)"
            
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        
        do {
             // Initialize a MSALPublicClientApplication with a given clientID and authority
            self.applicationContext = try MSALPublicClientApplication.init(clientId: kClientID, authority: kAuthority)
        } catch {
            self.loggingText.text = "Unable to create Application Context. Error: \(error)"
        }
    }


    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        
        if self.accessToken.isEmpty {
            signoutButton.isEnabled = false; 
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen
#### <a name="getting-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens
Das Aufrufen der `acquireToken`-Methode führt zum Öffnen eines Browserfensters, in dem der Benutzer aufgefordert wird, sich anzumelden. Anwendungen erfordern in der Regel, dass sich ein Benutzer interaktiv anmeldet, wenn er erstmals auf eine geschützte Ressource zugreifen muss oder ein automatischer Vorgang zum Beziehen eines Tokens misslingt (wenn z.B. das Kennwort des Benutzers abgelaufen ist).

#### <a name="getting-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens
Die Methode `acquireTokenSilent` verarbeitet das Beziehen und Verlängern von Token ohne Eingreifen des Benutzers. Nachdem `acquireToken` zum ersten Mal ausgeführt wurde, ist `acquireTokenSilent` die übliche Methode zum Abrufen von Token, die für den Zugriff auf geschützte Ressourcen bei nachfolgenden Aufrufen verwendet werden, da Aufrufe zum Anfordern oder Verlängern von Token automatisch erfolgen.

Irgendwann schlägt `acquireTokenSilent` fehl, z.B. wenn sich der Benutzer abgemeldet oder sein Kennwort auf einem anderen Gerät geändert hat. Wenn die MSAL feststellt, dass das Problem durch Anfordern einer interaktiven Aktion gelöst werden kann, löst sie eine `MSALErrorCode.interactionRequired`-Ausnahme aus. Ihre Anwendung kann diese Ausnahme auf zwei Arten handhaben:

1.  Sofortiges Aufrufen von `acquireToken`, was dazu führt, dass der Benutzer zur Anmeldung aufgefordert wird. Dieses Muster wird in der Regel in Onlineanwendungen verwendet, in denen kein Offline-Inhalt in der Anwendung für den Benutzer verfügbar ist. Die Beispielanwendung, die durch dieses Schritt-für-Schritt-Setup erstellt wird, verwendet dieses Muster: Sie sehen es beim ersten Ausführen der Anwendung in Aktion. Da noch kein Benutzer die Anwendung verwendet hat, enthält `applicationContext.users().first` einen Nullwert, und eine ` MSALErrorCode.interactionRequired `-Ausnahme wird ausgelöst. Der Code im Beispiel behandelt dann die Ausnahme durch Aufrufen von `acquireToken`, was dazu führt, dass der Benutzer zur Anmeldung aufgefordert wird.

2.  Anwendungen können dem Benutzer auch visuell zu verstehen geben, dass eine interaktive Anmeldung erforderlich ist, damit der Benutzer den richtigen Zeitpunkt zum Anmelden wählen oder die Anwendung `acquireTokenSilent` zu einem späteren Zeitpunkt wiederholen kann. Dies wird normalerweise verwendet, wenn der Benutzer andere Funktionen der Anwendung nutzen kann, ohne unterbrochen zu werden, z.B. wenn Offline-Inhalt in der Anwendung verfügbar ist. In diesem Fall kann der Benutzer entscheiden, wann er sich für den Zugriff auf die geschützte Ressource anmelden oder die veralteten Informationen aktualisieren möchte. Ihre Anwendung kann sich auch für einen Wiederholungsversuch von `acquireTokenSilent` entscheiden, wenn das Netzwerk wiederhergestellt wird, nachdem es vorübergehend nicht verfügbar war.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Aufrufen der Microsoft Graph-API mit dem zuvor bezogenen Token

Fügen Sie die nachstehende neue Methode zu `ViewController.swift` hinzu. Die Methode dient zum Stellen einer `GET`-Anforderung an die Microsoft Graph-API mithilfe eines *HTTP-Autorisierungsheaders*:

```swift
func getContentWithToken() {
    
    let sessionConfig = URLSessionConfiguration.default
    
    // Specify the Graph API endpoint
    let url = URL(string: kGraphURI)
    var request = URLRequest(url: url!)
    
    // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
    request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
    let urlSession = URLSession(configuration: sessionConfig, delegate: self, delegateQueue: OperationQueue.main)
    
    urlSession.dataTask(with: request) { data, response, error in
        let result = try? JSONSerialization.jsonObject(with: data!, options: [])
                    if result != nil {
                
                self.loggingText.text = result.debugDescription
            }
        }.resume()
}
```

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a>Ausführen eines REST-Aufrufs einer geschützten API

In dieser Beispielanwendung dient die `getContentWithToken()`-Methode zum Richten einer HTTP `GET`-Anforderung an eine geschützte Ressource, die ein Token erfordert, und anschließenden Zurückgeben des Inhalts an den Aufrufer. Diese Methode fügt das abgerufene Token in den *HTTP-Autorisierungsheader* ein. In diesem Beispiel ist die Ressource der Endpunkt *me* der Microsoft Graph-API, der die Profilinformationen des Benutzers anzeigt.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Einrichten der Abmeldung

Fügen Sie die folgende Methode zu `ViewController.swift` zum Abmelden des Benutzers hinzu:

```swift 
@IBAction func signoutButton(_ sender: UIButton) {

    do {
        
        // Removes all tokens from the cache for this application for the provided user
        // first parameter:   The user to remove from the cache
        
        try self.applicationContext.remove(self.applicationContext.users().first)
        self.signoutButton.isEnabled = false;
        
    } catch let error {
        self.loggingText.text = "Received error signing user out: \(error)"
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Weitere Informationen zum Abmelden

Die `signoutButton`-Methode entfernt den Benutzer aus dem MSAL-Benutzercache. Dadurch wird die MSAL effektiv angewiesen, den aktuellen Benutzer zu vergessen, sodass eine künftige Anforderung zum Abrufen eines Tokens nur erfolgreich ist, wenn diese interaktiv gestaltet wird.

Obwohl die Anwendung in diesem Beispiel einen einzelnen Benutzer unterstützt, werden von der MSAL Szenarios unterstützt, in denen mehrere Konten gleichzeitig angemeldet werden können. Ein Beispiel ist eine E-Mail-Anwendung, in der ein Benutzer mehrere Konten hat.
<!--end-collapse-->

## <a name="register-the-callback"></a>Registrieren des Rückrufs

Sobald der Benutzer die Authentifizierung durchgeführt hat, leitet der Browser den Benutzer wieder zurück zur Anwendung. Befolgen Sie den unten aufgeführten Schritte, um diesen Rückruf registrieren:

1.  Öffnen Sie `AppDelegate.swift`, und importieren Sie MSAL:

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Fügen Sie die folgende Methode zu Ihrer <code>AppDelegate</code>-Klasse hinzu, um Rückrufe zu verarbeiten:
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if the URL matches the redirect URI for a pending AppAuth
// authorization request and if so, will look for the code in the response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```


