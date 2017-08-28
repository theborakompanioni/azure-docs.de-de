---
title: "Einrichten des Computers für die Media Services-Entwicklung mit .NET"
description: "Lernen Sie die Voraussetzungen für Media Services bei Verwendung des Media Services SDK für .NET kennen. Erfahren Sie zudem, wie Sie eine Visual Studio-App erstellen."
services: media-services
documentationcenter: 
author: juliako
manager: SyntaxC4
editor: 
ms.assetid: ec2804c7-c656-4fbf-b3e4-3f0f78599a7f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/16/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: ca5e537bd4347e17190ff4f66cc4d42a36870936
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---
# <a name="media-services-development-with-net"></a>Media Services-Entwicklung mit .NET
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

In diesem Thema wird das Entwickeln von Media Services-Anwendungen mit .NET erläutert.

Die Bibliothek für das **Azure Media Services .NET SDK** ermöglicht es Ihnen, unter Verwendung von .NET für Media Services zu programmieren. Um die Entwicklung mit .NET weiter zu vereinfachen, steht die Bibliothek für **Azure Media Services .NET SDK-Erweiterungen** zur Verfügung. Diese Bibliothek enthält eine Reihe von Erweiterungsmethoden und Hilfsfunktionen, die Ihren .NET-Code vereinfachen. Beide Bibliotheken sind über **NuGet** und **GitHub** verfügbar.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Mediendienstekonto in einem neuen oder existierenden Azure-Abonnement. Weitere Informationen finden Sie unter [Erstellen eines Media Services-Kontos](media-services-portal-create-account.md).
* Betriebssysteme: Windows 10, Windows 7, Windows 2008 R2 oder Windows 8.
* .NET Framework 4.5.
* Visual Studio.

## <a name="create-and-configure-a-visual-studio-project"></a>Erstellen und Konfigurieren eines Visual Studio-Projekts
Dieser Abschnitt beschreibt, wie Sie ein Projekt in Visual Studio erstellen und für die Mediendienste-Entwicklung einrichten.  Es handelt sich um eine C# Windows-Konsolenanwendung, allerdings gelten für andere Typen von Projekten für Media Services-Anwendungen (z.B. eine Windows Forms-Anwendung oder ASP.NET-Webanwendung) dieselben Schritte.

In diesem Abschnitt wird beschrieben, wie Sie mithilfe von **NuGet** Media Services .NET SDK-Erweiterungen und andere abhängige Bibliotheken hinzufügen.

Sie können auch die neuesten Media Services .NET SDK-Komponenten von GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) oder [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)) herunterladen, die Projektmappe erstellen und die Verweise dem Clientprojekt hinzufügen. Alle erforderlichen Abhängigkeiten werden automatisch heruntergeladen und extrahiert.

1. Erstellen Sie eine neue C#-Konsolenanwendung in Visual Studio. Geben Sie **Name**, **Speicherort** und **Projektmappenname** ein und klicken Sie auf „OK“.
2. Erstellen Sie die Projektmappe.
3. Verwenden Sie **NuGet**, um die **Azure Media Services .NET SDK-Erweiterungen** (**windowsazure.mediaservices.extensions**) zu installieren. Durch Installieren dieses Pakets werden auch das **Media Services .NET SDK** installiert und alle anderen erforderlichen Abhängigkeiten hinzugefügt.
   
    Stellen Sie sicher, dass Sie die neueste Version von NuGet installiert haben. Weitere Informationen und Installationsanweisungen finden Sie unter [NuGet](http://nuget.codeplex.com/).
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Namen des Projekts, und wählen Sie „NuGet-Pakete verwalten“ aus.
   
    Das Dialogfeld NuGet-Pakete verwalten wird geöffnet.
5. Suchen Sie im Onlinekatalog nach den Azure Media Services-Erweiterungen, wählen Sie die Azure Media Services .NET SDK-Erweiterungen aus, und klicken Sie dann auf die Schaltfläche „Installieren“.
   
    Das Projekt wird geändert, und Verweise auf die Media Services .NET SDK-Erweiterungen, das Media Services .NET-SDK und andere abhängige Assemblys werden hinzugefügt.
6. Um die Entwicklungsumgebung übersichtlicher zu halten, sollten Sie die Verwendung von NuGet Package Restore in Erwägung ziehen. Weitere Informationen finden Sie unter [NuGet Package Restore](http://docs.nuget.org/consume/package-restore).
7. Fügen Sie einen Verweis auf die Assembly **System.Configuration** hinzu. Diese Assembly enthält die **System.Configuration.ConfigurationManager**-Klasse, die zum Zugriff auf die Konfigurationsdateien (z.B. „App.config“) verwendet wird.
   
    Wenn Sie Verweise mit dem Dialogfeld zur Verweisverwaltung hinzufügen möchten, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen. Wählen Sie dann „Hinzufügen“ und „Verweise“.
   
    Das Dialogfeld „Verweise verwalten“ wird angezeigt.
8. Suchen Sie unter den .NET Framework-Assemblys die System.Configuration-Assembly, und wählen Sie sie aus. Klicken Sie anschließend auf „OK“.
9. Öffnen Sie die Datei „App.config“, und fügen Sie der Datei einen *appSettings*-Abschnitt hinzu.     
   
    Legen Sie die Werte fest, die zur Verbindung mit der Media Services-API erforderlich sind. Weitere Informationen finden Sie unter [Access the Azure Media Services API with Azure AD authentication (Zugriff auf die Azure Media Services-API mit der Azure AD-Authentifizierung)](media-services-use-aad-auth-to-access-ams-api.md). 

    Sollten Sie [Benutzerauthentifizierung](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) verwenden, weist Ihre Config-Datei vermutlich Werte für Ihre Azure AD-Mandanten-Domäne und den AMS-REST-API-Endpunkt auf.
    
    >[!Important]
    >Beispiele im Azure Media Services-Dokumentationssatz, verwenden Sie einen Benutzer-Authentifizierungstyp (interaktiv) für die Verbindung mit der AMS-API. Diese Authentifizierungsmethode eignet sich gut für die Verwaltung und Überwachung von nativen Apps: Mobile Apps, Windows-Apps und Konsolenanwendungen. Diese Authentifizierungsmethode eignet sich nicht für Server, Webdienste oder API-Anwendungen.  Weitere Informationen finden Sie unter [Access the AMS API with Azure AD authentication (Zugriff auf die AMS-API mit der Azure AD-Authentifizierung)](media-services-use-aad-auth-to-access-ams-api.md).

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. Überschreiben Sie die existierenden **using** -Anweisungen am Anfang der Datei "Program.cs" durch den folgenden Code.
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

Nun können Sie mit der Entwicklung einer Mediendienste-Anwendung beginnen.    

## <a name="example"></a>Beispiel

Hier ist ein kurzes Beispiel, bei dem eine Verbindung mit der AMS-API hergestellt wird und alle verfügbaren Medienprozessoren aufgelistet werden.
    
    class Program
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];
    
        private static CloudMediaContext _context = null;
        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
    
            // List all available Media Processors
            foreach (var mp in _context.MediaProcessors)
            {
                Console.WriteLine(mp.Name);
            }
    
        }

##<a name="next-steps"></a>Nächste Schritte

Jetzt [können Sie sich mit der API AMS verbinden](media-services-use-aad-auth-to-access-ams-api.md) und mit dem [Entwickeln](media-services-dotnet-get-started.md) beginnen.


## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


