---
title: "Angeleitetes Setup für Azure AD v2 JS SPA – Verwenden | Microsoft-Dokumentation"
description: "Informationen, wie JavaScript SPA-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: ceec3760648535aecdb1d9ed6a1d1750dc2d0334
ms.contentlocale: de-de

---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Verwenden der MSAL (Microsoft Authentication Library) für die Benutzeranmeldung

1.  Erstellen Sie eine Datei mit dem Namen `app.js`. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie Folgendes aus: `Add` > `New Item` > `JavaScript File`.
2.  Fügen Sie Ihrer Datei `app.js` den folgenden Code hinzu:

```javascript
// Graph API endpoint to show user profile
var graphApiEndpoint = "https://graph.microsoft.com/v1.0/me";

// Graph API scope used to obtain the access token to read user profile
var graphAPIScopes = ["https://graph.microsoft.com/user.read"];

// Initialize application
var userAgentApplication = new Msal.UserAgentApplication(msalconfig.clientID, null, loginCallback, {
    redirectUri: msalconfig.redirectUri
});

//Previous version of msal uses redirect url via a property
if (userAgentApplication.redirectUri) {
    userAgentApplication.redirectUri = msalconfig.redirectUri;
}

window.onload = function () {
    // If page is refreshed, continue to display user info
    if (!userAgentApplication.isCallback(window.location.hash) && window.parent === window && !window.opener) {
        var user = userAgentApplication.getUser();
        if (user) {
            callGraphApi();
        }
    }
}

/**
 * Call the Microsoft Graph API and display the results on the page. Sign the user in if necessary
 */
function callGraphApi() {
    var user = userAgentApplication.getUser();
    if (!user) {
        // If user is not signed in, then prompt user to sign in via loginRedirect.
        // This will redirect user to the Azure Active Directory v2 Endpoint
        userAgentApplication.loginRedirect(graphAPIScopes);
        // The call to loginRedirect above frontloads the consent to query Graph API during the sign-in.
        // If you want to use dynamic consent, just remove the graphAPIScopes from loginRedirect call.
        // As such, user will be prompted to give consent when requested access to a resource that 
        // he/she hasn't consented before. In the case of this application - 
        // the first time the Graph API call to obtain user's profile is executed.
    } else {
        // If user is already signed in, display the user info
        var userInfoElement = document.getElementById("userInfo");
        userInfoElement.parentElement.classList.remove("hidden");
        userInfoElement.innerHTML = JSON.stringify(user, null, 4);

        // Show Sign-Out button
        document.getElementById("signOutButton").classList.remove("hidden");

        // Now Call Graph API to show the user profile information:
        var graphCallResponseElement = document.getElementById("graphResponse");
        graphCallResponseElement.parentElement.classList.remove("hidden");
        graphCallResponseElement.innerText = "Calling Graph ...";

        // In order to call the Graph API, an access token needs to be acquired.
        // Try to acquire the token used to query Graph API silently first:
        userAgentApplication.acquireTokenSilent(graphAPIScopes)
            .then(function (token) {
                //After the access token is acquired, call the Web API, sending the acquired token
                callWebApiWithToken(graphApiEndpoint, token, graphCallResponseElement, document.getElementById("accessToken"));

            }, function (error) {
                // If the acquireTokenSilent() method fails, then acquire the token interactively via acquireTokenRedirect().
                // In this case, the browser will redirect user back to the Azure Active Directory v2 Endpoint so the user 
                // can reenter the current username/ password and/ or give consent to new permissions your application is requesting.
                // After authentication/ authorization completes, this page will be reloaded again and callGraphApi() will be executed on page load.
                // Then, acquireTokenSilent will then get the token silently, the Graph API call results will be made and results will be displayed in the page.
                if (error) {
                    userAgentApplication.acquireTokenRedirect(graphAPIScopes);
                }
            });

    }
}

/**
 * Callback method from sign-in: if no errors, call callGraphApi() to show results.
 * @param {string} errorDesc - If error occur, the error message
 * @param {object} token - The token received from login
 * @param {object} error - The error string
 * @param {string} tokenType - the token type: usually id_token
 */
function loginCallback(errorDesc, token, error, tokenType) {
    if (errorDesc) {
        showError(msal.authority, error, errorDesc);
    } else {
        callGraphApi();
    }
}

/**
 * Show an error message in the page
 * @param {string} endpoint - the endpoint used for the error message
 * @param {string} error - Error string
 * @param {string} errorDesc - Error description
 */
function showError(endpoint, error, errorDesc) {
    var formattedError = JSON.stringify(error, null, 4);
    if (formattedError.length < 3) {
        formattedError = error;
    }
    document.getElementById("errorMessage").innerHTML = "An error has occurred:<br/>Endpoint: " + endpoint + "<br/>Error: " + formattedError + "<br/>" + errorDesc;
    console.error(error);
}

```

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen

Wenn ein Benutzer das erste Mal auf die Schaltfläche *Microsoft Graph-API aufrufen* klickt, ruft die `callGraphApi`-Methode `loginRedirect` auf, um den Benutzer anzumelden. Durch diese Methode wird der Benutzer zum *Microsoft Azure Active Directory-v2-Endpunkt* umgeleitet und dort aufgefordert, seine Anmeldeinformationen anzugeben, die dann ebenfalls dort überprüft werden. Als Ergebnis einer erfolgreichen Anmeldung wird der Benutzer zurück zur ursprünglichen Seite *index.html* umgeleitet, und es wird ein Token empfangen, das von `msal.js` verarbeitet wird. Die im Token enthaltenen Informationen werden zwischengespeichert. Dieses Token ist das sogenannte *ID-Token*, das grundlegende Informationen zum Benutzer enthält, beispielsweise den Benutzeranzeigenamen. Wenn Sie von diesem Token bereitgestellte Daten nutzen möchten, müssen Sie sicherstellen, dass das Token durch Ihren Back-End-Server überprüft wird. So wird garantiert, dass das Token für einen gültigen Benutzer Ihrer Anwendung ausgestellt wurde.

Die in diesem Leitfaden generierte Einzelseitenanwendung macht keinen direkten Gebrauch vom ID-Token – stattdessen wird `acquireTokenSilent` und/oder `acquireTokenRedirect` aufgerufen, um ein *Zugriffstoken* abzurufen, mit dem die Microsoft Graph-API abgefragt wird. Wenn Sie ein Beispiel zum Überprüfen des ID-Tokens suchen, sehen Sie sich [diese](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 sample")-Beispielanwendung in GitHub an. Dieses Beispiel verwendet eine ASP.NET-Web-API für die Tokenüberprüfung.

#### <a name="getting-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens

Nach der ersten Anmeldung möchten Sie die Benutzer nicht jedes Mal, wenn sie ein Token für den Zugriff auf eine Ressource anfordern, zur erneuten Authentifizierung auffordern – deshalb sollte in den meisten Fällen für das Abrufen von Token *acquireTokenSilent* verwendet werden. Es gibt jedoch Situationen, in denen Sie Benutzer zur Interaktion mit dem Azure Active Directory-v2-Endpunkt auffordern müssen. Beispiele:
-   Benutzer müssen möglicherweise ihre Anmeldeinformationen erneut eingeben, da das Kennwort abgelaufen ist.
-   Ihre Anwendung fordert Zugriff auf eine Ressource, der der Benutzer zustimmen muss.
-   Die zweistufige Authentifizierung ist erforderlich.

Das Aufrufen von *acquireTokenRedirect(scope)* führt zum Umleiten von Benutzern zum Azure Active Directory-v2-Endpunkt (oder *acquireTokenPopup(scope)* zu einem Popupfenster). Dort müssen Benutzer ihre Anmeldeinformationen bestätigen, ihre Zustimmung für die angeforderte Ressource geben oder die zweistufige Authentifizierung abschließen.

#### <a name="getting-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens
Die Methode ` acquireTokenSilent` verwaltet das Abrufen und Erneuern von Token ohne Eingreifen des Benutzers. Nachdem `loginRedirect` (oder `loginPopup`) zum ersten Mal ausgeführt wurde, ist `acquireTokenSilent` die übliche Methode zum Abrufen von Token, die für den Zugriff auf geschützte Ressourcen bei nachfolgenden Aufrufen verwendet werden, da Aufrufe zum Anfordern oder Verlängern von Token automatisch erfolgen.
`acquireTokenSilent` führt in einigen Fällen möglicherweise zu Fehlern – z.B. wenn das Kennwort des Benutzers abgelaufen ist. Ihre Anwendung kann diese Ausnahme auf zwei Arten handhaben:

1.  Sofortiges Aufrufen von `acquireTokenRedirect`, was dazu führt, dass der Benutzer zur Anmeldung aufgefordert wird. Dieses Muster wird in der Regel in Onlineanwendungen verwendet, in denen kein nicht authentifizierter Inhalt in der Anwendung für den Benutzer verfügbar ist. Die in diesem Leitfaden generierte Beispielanwendung verwendet dieses Muster.

2. Anwendungen können dem Benutzer auch visuell zu verstehen geben, dass eine interaktive Anmeldung erforderlich ist, damit der Benutzer den richtigen Zeitpunkt zum Anmelden wählen oder die Anwendung `acquireTokenSilent` zu einem späteren Zeitpunkt wiederholen kann. Dies wird normalerweise verwendet, wenn der Benutzer andere Funktionen der Anwendung nutzen kann, ohne unterbrochen zu werden, z.B. wenn nicht authentifizierter Inhalt in der Anwendung verfügbar ist. In diesem Fall kann der Benutzer entscheiden, ob er sich für den Zugriff auf die geschützte Ressource anmelden oder die veralteten Informationen aktualisieren möchte.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Aufrufen der Microsoft Graph-API mit dem zuvor bezogenen Token

Fügen Sie Ihrer Datei `app.js` den folgenden Code hinzu:

```javascript
/**
 * Call a Web API using an access token.
 * @param {any} endpoint - Web API endpoint
 * @param {any} token - Access token
 * @param {object} responseElement - HTML element used to display the results
 * @param {object} showTokenElement = HTML element used to display the RAW access token
 */
function callWebApiWithToken(endpoint, token, responseElement, showTokenElement) {
    var headers = new Headers();
    var bearer = "Bearer " + token;
    headers.append("Authorization", bearer);
    var options = {
        method: "GET",
        headers: headers
    };

    fetch(endpoint, options)
        .then(function (response) {
            var contentType = response.headers.get("content-type");
            if (response.status === 200 && contentType && contentType.indexOf("application/json") !== -1) {
                response.json()
                    .then(function (data) {
                        // Display response in the page
                        console.log(data);
                        responseElement.innerHTML = JSON.stringify(data, null, 4);
                        if (showTokenElement) {
                            showTokenElement.parentElement.classList.remove("hidden");
                            showTokenElement.innerHTML = token;
                        }
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            } else {
                response.json()
                    .then(function (data) {
                        // Display response as error in the page
                        showError(endpoint, data);
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            }
        })
        .catch(function (error) {
            showError(endpoint, error);
        });
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Weitere Informationen zum Richten eines REST-Aufrufs an eine geschützte API

In der in diesem Leitfaden erstellten Beispielanwendung wird die `callWebApiWithToken()`-Methode dazu verwendet, um eine HTTP `GET`-Anforderung an eine geschützte Ressource zu senden, die ein Token erfordert, und anschließend den Inhalt an den Aufrufer zurückzugeben. Diese Methode fügt das abgerufene Token in den *HTTP-Autorisierungsheader* ein. In der über diesen Leitfaden erstellten Beispielanwendung ist die Ressource der Endpunkt *me* der Microsoft Graph-API, der die Profilinformationen des Benutzers anzeigt.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Hinzufügen einer Methode zum Abmelden des Benutzers

Fügen Sie Ihrer Datei `app.js` den folgenden Code hinzu:

```javascript
/**
 * Sign-out the user
 */
function signOut() {
    userAgentApplication.logout();
}
```

