---
title: Installieren des Azure AD-Anwendungsproxyconnectors im Hintergrund | Microsoft-Dokumentation
description: "Dieses Thema erläutert, wie Sie eine unbeaufsichtigte Installation des Azure AD-Anwendungsproxyconnectors durchführen, um sicheren Remotezugriff auf Ihre lokalen Apps zu gewähren."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 3aa1c7f2-fb2a-4693-abd5-95bb53700cbb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 081e45e0256134d692a2da7333ddbaafc7366eaa
ms.openlocfilehash: cf00d47efc613f7bdc152c1b5f0d0830fb44a785


---
# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Installieren des Azure AD-Anwendungsproxyconnectors im Hintergrund
Sie möchten ein Installationsskript an mehrere Windows-Server senden können oder an Windows-Server, auf denen keine Benutzeroberfläche aktiviert ist. In diesem Thema wird das Erstellen eines Windows PowerShell-Skripts erläutert, das eine unbeaufsichtigte Installation zum Installieren und Registrieren des Azure AD-Anwendungsproxyconnectors ermöglicht.

Diese Funktion ist in folgenden Fällen nützlich:

* Installieren des Connectors auf Computern ohne Benutzeroberflächenebene, oder wenn Sie auf dem Computer keine RDP-Verbindung herstellen können.
* Gleichzeitiges Installieren und Registrieren vieler Connectors.
* Integrieren der Connectorinstallation und Registrierung als Teil einer anderen Prozedur.
* Erstellen eines standardmäßigen Serverimages, das die Connectorbits enthält, aber nicht registriert ist.

## <a name="enabling-access"></a>Aktivieren des Zugriffs
Um den Anwendungsproxy nutzen zu können, müssen Sie einen als Connector bezeichneten schlanken Windows Server-Dienst in Ihrem Netzwerk installieren. Für das Funktionieren des Anwendungsproxyconnectors muss dieser im Azure AD-Verzeichnis durch einen globalen Administrator mit Kennwort registriert werden. Diese Informationen werden normalerweise während der Installation des Connectors in einem Popupdialogfeld eingegeben. Stattdessen können Sie mit Windows PowerShell ein Anmeldeinformationsobjekt erstellen, um die Registrierungsinformationen einzugeben. Sie können jedoch auch ein eigenes Token erstellen und zur Eingabe der Registrierungsinformationen verwenden.

## <a name="step-1--install-the-connector-without-registration"></a>Schritt 1: Installieren des Connectors ohne Registrierung
Installieren Sie die Connector-MSIs wie folgt, ohne den Connector zu registrieren:

1. Öffnen Sie eine Eingabeaufforderung.
2. Führen Sie den folgenden Befehl aus, in dem die Option „/q“ für die Installation im Hintergrund steht – bei der Installation werden Sie nicht aufgefordert, den Endbenutzer-Lizenzvertrag zu akzeptieren.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Schritt 2: Registrieren des Connectors in Azure Active Directory
Dies lässt sich mit einer folgenden Methoden bewirken:

* Registrieren des Connectors mit einem Windows PowerShell-Anmeldeinformationsobjekts
* Registrieren des Connectors mithilfe eines offline erstellten Tokens

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Registrieren des Connectors mit einem Windows PowerShell-Anmeldeinformationsobjekts
1. Erstellen Sie das Windows PowerShell-Anmeldeinformationsobjekt durch Ausführen des folgenden Befehls, wobei \<username\> und \<password\> durch den Benutzernamen und das Kennwort für das betreffende Verzeichnis ersetzt werden müssen:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Wechseln Sie zu **C:\Programme\Microsoft AAD App Proxy Connector**, und führen Sie das Skript mithilfe des zuvor erstellten PowerShell-Anmeldeinformationsobjekts aus, wobei „$cred“ für den Namen des erstellten PowerShell-Anmeldeinformationsobjekts steht:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-the-connector-using-a-token-created-offline"></a>Registrieren des Connectors mithilfe eines offline erstellten Tokens
1. Erstellen Sie mithilfe der AuthenticationContext-Klasse ein Offlinetoken, indem Sie die Werte in dem Codeausschnitt verwenden:

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }


2. Sobald das Token vorliegt, erstellen Sie SecureString mit diesem Token:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Führen Sie folgenden Windows PowerShell-Befehl aus, und ersetzen Sie \<tenant GUID\> durch die ID Ihres Verzeichnisses:

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a>Nächste Schritte 
* [Veröffentlichen von Anwendungen mit Ihrem eigenen Domänennamen](active-directory-application-proxy-custom-domains.md)
* [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)
* [Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md)





<!--HONumber=Feb17_HO1-->


