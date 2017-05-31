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
ms.date: 05/03/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: f4d72d4d11ee64e3431879f6ad1b5d8d091a0c87
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="silently-install-the-azure-ad-application-proxy-connector"></a>Installieren des Azure AD-Anwendungsproxyconnectors im Hintergrund
Sie möchten ein Installationsskript an mehrere Windows-Server senden können oder an Windows-Server, auf denen keine Benutzeroberfläche aktiviert ist. In diesem Thema wird das Erstellen eines Windows PowerShell-Skripts erläutert, das eine unbeaufsichtigte Installation und Registrierung des Azure AD-Anwendungsproxyconnectors ermöglicht.

Diese Funktion ist in folgenden Fällen nützlich:

* Installieren des Connectors auf Computern ohne Benutzeroberflächenebene, oder wenn Sie auf dem Computer keine RDP-Verbindung herstellen können.
* Gleichzeitiges Installieren und Registrieren vieler Connectors.
* Integrieren der Connectorinstallation und Registrierung als Teil einer anderen Prozedur.
* Erstellen eines standardmäßigen Serverimages, das die Connectorbits enthält, aber nicht registriert ist.

Um den Anwendungsproxy nutzen zu können, müssen Sie einen als Connector bezeichneten schlanken Windows Server-Dienst in Ihrem Netzwerk installieren. Für das Funktionieren des Anwendungsproxyconnectors muss dieser im Azure AD-Verzeichnis durch einen globalen Administrator mit Kennwort registriert werden. Diese Informationen werden normalerweise während der Installation des Connectors in einem Popupdialogfeld eingegeben. Sie können jedoch mit Windows PowerShell ein Anmeldeinformationsobjekt erstellen, um die Registrierungsinformationen einzugeben, oder auch ein eigenes Token erstellen und zur Eingabe der Registrierungsinformationen verwenden.

## <a name="install-the-connector"></a>Installieren des Connectors
Installieren Sie die Connector-MSIs wie folgt, ohne den Connector zu registrieren:

1. Öffnen Sie eine Eingabeaufforderung.
2. Führen Sie den folgenden Befehl aus, in dem die Option „/q“ für die Installation im Hintergrund steht – bei der Installation werden Sie nicht aufgefordert, den Endbenutzer-Lizenzvertrag zu akzeptieren.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>Registrieren des Connectors bei Azure AD
Es gibt zwei Methoden zum Registrieren des Connectors:

* Registrieren des Connectors mit einem Windows PowerShell-Anmeldeinformationsobjekt
* Registrieren des Connectors mithilfe eines offline erstellten Tokens

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Registrieren des Connectors mit einem Windows PowerShell-Anmeldeinformationsobjekt
1. Erstellen Sie das Windows PowerShell-Anmeldeinformationsobjekt, indem Sie den folgenden Befehl ausführen. Ersetzen Sie *\<Benutzername\>* und *\<Kennwort\>* durch den Benutzernamen und das Kennwort für Ihr Verzeichnis:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Wechseln Sie zu **C:\Programme\Microsoft AAD App Proxy Connector**, und führen Sie das Skript mithilfe des zuvor erstellten PowerShell-Anmeldeinformationsobjekts aus. Ersetzen Sie *$cred* durch den Namen des zuvor erstellten PowerShell-Anmeldeinformationsobjekts:
   
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



