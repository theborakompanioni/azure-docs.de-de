---
title: "Anpassen der Benutzeroberfläche: Azure AD B2C | Microsoft-Dokumentation"
description: "Ein Thema über die Anpassungsfeatures für die Benutzeroberfläche (UI) in Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 99f5a391-5328-471d-a15c-a2fafafe233d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 122fa997ea11b369aae3c59edf0043ab19d21aea
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Anpassen der Azure AD B2C-Benutzeroberfläche (UI)

Benutzerfreundlichkeit ist in einer kundenorientierten Anwendung entscheidend.  Vergrößern Sie Ihren Kundenstamm, indem Sie zu Ihrem Unternehmen passende Benutzeroberflächen entwickeln. Azure Active Directory B2C (Azure AD B2C) ermöglicht Ihnen, die Seiten für Registrierung und Anmeldung, Profilbearbeitung und Kennwortzurücksetzung mit präziser Kontrolle anzupassen.

> [!NOTE]
> Das in diesem Artikel beschriebene Feature „Seite für die Benutzeroberflächenanpassung“ gilt nicht für die Anmeldungsrichtlinie, die dazugehörige Seite zum Zurücksetzen des Kennworts und Bestätigungs-E-Mails.  Für diese Features wird stattdessen das [Unternehmensbrandingfeature](../active-directory/active-directory-add-company-branding.md) verwendet.
>

In diesem Artikel werden die folgenden Themen behandelt:

* Das Feature „Seite für die Benutzeroberflächenanpassung“.
* Ein Tool zum Hochladen von HTML-Inhalt in Azure Blob Storage für die Verwendung mit dem Feature „Seite für die Benutzeroberflächenanpassung“.
* Die von Azure AD B2C verwendeten Benutzeroberflächenelemente, die Sie mithilfe von Cascading Stylesheets (CSS) anpassen können.
* Bewährte Methoden beim Üben des Umgangs mit diesem Feature.

## <a name="the-page-ui-customization-feature"></a>Das Feature „Seite für die Benutzeroberflächenanpassung“

Sie können das Aussehen und Verhalten der Seiten für die Registrierung, Anmeldung, Kennwortzurücksetzung und Profilbearbeitung von Kunden (durch Konfigurieren von [Richtlinien](active-directory-b2c-reference-policies.md)) anpassen. Beim Navigieren zwischen der Anwendung und den Seiten, die von Azure AD B2C bereitgestellt werden, wird Kunden eine nahtlose Benutzeroberfläche geboten.

Im Gegensatz zu anderen Diensten mit Benutzeroberflächenoptionen befolgt Azure AD B2C einen einfachen und modernen Ansatz zur Anpassung der Benutzeroberfläche.

So funktioniert es: Azure AD B2C führt den Code im Browser Ihres Kunden aus und verwendet einen modernen Ansatz namens [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/) (Ressourcenfreigabe zwischen verschiedenen Ursprüngen).  Zur Laufzeit wird Inhalt über eine URL geladen, die Sie in einer Richtlinie angeben. Sie können verschiedene URLs für unterschiedliche Seiten angeben. Nachdem der über die URL geladene Inhalt mit einem von Azure AD B2C eingefügten HTML-Fragment zusammengeführt wurde, wird die Seite Ihrem Kunden angezeigt. Sie müssen nur Folgendes durchführen:

1. Erstellen Sie an einer beliebigen Stelle in `<body>` wohlgeformten HTML5-Inhalt mit einem leeren `<div id="api"></div>`-Element. Dieses Element markiert die Stelle, an der der Azure AD B2C-Inhalt eingefügt wird.
1. Hosten Sie diese Inhalte auf einem HTTPS-Endpunkt (auf dem CORS zulässig ist). Beachten Sie, dass Sie bei der Konfiguration von CORS die Anforderungsmethoden GET und OPTIONS aktivieren müssen.
1. Formatieren Sie die Elemente der Benutzeroberfläche, die von Azure AD B2C einfügt werden, mithilfe von CSS.

### <a name="a-basic-example-of-customized-html"></a>Ein einfaches Beispiel benutzerdefinierter HTML

Das folgende Beispiel ist der einfachste HTML-Inhalt, den Sie zum Testen dieser Funktion verwenden können. Nutzen Sie das [Hilfsprogramm](active-directory-b2c-reference-ui-customization-helper-tool.md), um diesen Inhalt in Ihren Azure-Blobspeicher hochzuladen und zu konfigurieren. So können Sie sicherstellen, dass die grundlegenden unformatierten Schaltflächen und Formularfelder auf jeder Seite angezeigt werden und funktionieren.

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
    </body>
</html>
```

## <a name="test-out-the-ui-customization-feature"></a>Testen des Anpassungsfeatures für die Benutzeroberfläche (UI)

Möchten Sie mit unserem Beispiel-HTML- und CSS-Inhalt die Funktion zum Anpassen der Benutzeroberfläche testen?  Wir haben [ein Hilfsprogramm](active-directory-b2c-reference-ui-customization-helper-tool.md) bereitgestellt, das Beispielinhalte in Ihren Azure-Blobspeicher hochlädt und konfiguriert.

> [!NOTE]
> Sie können Ihre UI-Inhalte überall hosten: auf Webservern, CDNs, AWS S3, Dateifreigabesystemen usw. Solange die Inhalte auf einem öffentlich verfügbaren HTTPS-Endpunkt (mit aktiviertem CORS) gehostet werden, ist alles in Ordnung. Wir verwenden den Azure-Blobspeicher nur zur Veranschaulichung.
>

## <a name="the-ui-fragments-embedded-by-azure-ad-b2c"></a>Die von Azure AD B2C eingebetteten Benutzeroberflächenfragmente

In den folgenden Abschnitten finden Sie die HTML5-Fragmente, die Azure AD B2C im `<div id="api"></div>`-Element in Ihrem Inhalt zusammenführt. **Fügen Sie diese Fragmente nicht in Ihren HTML5-Inhalt ein.** Sie werden vom Azure AD B2C-Dienst zur Laufzeit eingefügt. Verwenden Sie diese Fragmente als Referenz beim Entwerfen Ihrer eigenen Cascading Stylesheets (CSS).

### <a name="fragment-inserted-into-the-identity-provider-selection-page"></a>Auf der „Auswahlseite für Identitätsanbieter“ eingefügtes Fragment

Diese Seite enthält eine Liste der Identitätsanbieter, aus denen der Benutzer bei der Registrierung oder Anmeldung auswählen kann. Bei diesen Schaltflächen handelt es sich um Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder um lokale Konten (basierend auf E-Mail-Adressen oder Benutzernamen).

```HTML
<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-local-account-sign-up-page"></a>Auf der „Registrierungsseite für lokales Konto“ eingefügtes Fragment

Diese Seite enthält ein Formular für die Registrierung eines lokalen Kontos, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert. Das Formular kann verschiedene Eingabesteuerelemente enthalten, z. B. Texteingabefelder, Kennworteingabefelder, Optionsfelder, Dropdownfelder mit einer Auswahlmöglichkeit und Kontrollkästchen mit mehreren Optionen.

```HTML
<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-social-account-sign-up-page"></a>Auf der „Registrierungsseite für Konten sozialer Netzwerke“ eingefügtes Fragment

Diese Seite kann angezeigt werden, wenn die Registrierung mit einem vorhandenen Konto bei einem sozialen Netzwerk wie Facebook oder Google+ als Identitätsanbieter erfolgt.  Sie wird verwendet, wenn zusätzliche Informationen zum Benutzer mithilfe eines Registrierungsformulars erfasst werden müssen. Diese Seite ähnelt der Registrierungsseite für lokale Konten (siehe Abschnitt oben), eine Ausnahme bilden die Kennworteingabefelder.

### <a name="fragment-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Auf der „Seite für einheitliche Registrierung oder Anmeldung“ eingefügtes Fragment

Diese Seite verarbeitet sowohl die Registrierung als auch die Anmeldung von Kunden. Diese können als Identitätsanbieter soziale Netzwerke wie z.B. Facebook oder Google+ oder lokale Konten verwenden.

```HTML
<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>
```

### <a name="fragment-inserted-into-the-multi-factor-authentication-page"></a>Auf der „Seite für die mehrstufige Authentifizierung“ eingefügtes Fragment

Auf dieser Seite können Benutzer während der Registrierung oder Anmeldung ihre Telefonnummern überprüfen (per SMS oder Sprachnachricht).

```HTML
<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-error-page"></a>Auf der „Fehlerseite“ eingefügtes Fragment

```HTML
<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>
```

## <a name="localizing-your-html-content"></a>Lokalisieren Ihres HTML-Inhalts

Sie können Ihren HTML-Inhalt lokalisieren, indem Sie die [Sprachanpassung](active-directory-b2c-reference-language-customization.md) aktivieren.  Wenn Sie dieses Feature aktivieren, kann Azure AD B2C den Open ID Connect-Parameter `ui-locales` an Ihren Endpunkt weiterleiten.  Ihr Inhaltsserver kann diesen Parameter verwenden, um benutzerdefinierte HTML-Seiten bereitzustellen, die sprachspezifisch sind.

## <a name="things-to-remember-when-building-your-own-content"></a>Wichtige Aspekte beim Erstellen eigener Inhalte

Wenn Sie das Anpassungsfeature für die Seiten-UI verwenden möchten, beachten Sie die folgenden bewährten Methoden:

* Kopieren und ändern Sie den Azure AD B2C-Standardinhalt nicht. Es wird empfohlen, eigene HTML5-Inhalte von Grund auf neu zu erstellen und den Standardinhalt als Referenz zu verwenden.
* Aus Gründen der Sicherheit ist es nicht zulässig, JavaScript in Ihren Inhalt aufzunehmen. Das meiste, was Sie benötigen, sollte im Programmumfang enthalten sein. Falls nicht, fordern Sie über [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) neue Funktionen an.
* Unterstützte Browserversionen:
  * Internet Explorer 11, 10, Edge
  * Eingeschränkte Unterstützung für Internet Explorer 9, 8
  * Google Chrome 42.0 und höher
  * Mozilla Firefox 38.0 und höher

