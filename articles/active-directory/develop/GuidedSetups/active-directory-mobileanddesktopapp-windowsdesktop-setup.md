---
title: "Azure AD v2 – Windows-Desktop: Erste Schritte – Einrichtung | Microsoft-Dokumentation"
description: "Informationen, wie Windows Desktop .NET (XAML)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
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
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: ef74361c7a15b0eb7dad1f6ee03f8df707a7c05e
ms.openlocfilehash: be00cb8f117fcbd2d137197f7dbe9713540cea61
ms.contentlocale: de-de


---

<a id="set-up-your-project" class="xliff"></a>
## Einrichten des Projekts

Dieser Abschnitt enthält detaillierte Anleitungen zum Erstellen eines neuen Projekts, um zu veranschaulichen, wie Sie eine Windows Desktop .NET (XAML)-Anwendung mit *Mit Microsoft anmelden* so integrieren können, dass Sie Web-APIs abfragen kann, die ein Token erfordern.

Die in diesem Leitfaden erstellte Anwendung macht eine Schaltfläche für Graph verfügbar und zeigt Ergebnisse auf dem Bildschirm sowie eine Abmeldeschaltfläche an.

> Möchten Sie stattdessen das Visual Studio-Projekt dieses Beispiels herunterladen? [Laden Sie ein Projekt herunter](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip), und fahren Sie mit dem Schritt [Konfiguration](#create-an-application-express "Schritt „Konfiguration“") fort, um das Codebeispiel vor der Ausführung zu konfigurieren.


<a id="create-your-application" class="xliff"></a>
### Erstellen der Anwendung
1. In Visual Studio: `File` > `New` > `Project`<br/>
2. Wählen Sie  *unter* Vorlagen`Visual C#` aus.
3. Wählen Sie `WPF App` (oder je nach Ihrer Visual Studio-Version *WPF-Anwendung*) aus.

<a id="add-the-microsoft-authentication-library-msal-to-your-project" class="xliff"></a>
## Hinzufügen der MSAL (Microsoft Authentication Library) zu Ihrem Projekt
1. In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Kopieren Sie Folgendes, und fügen Sie es in das Fenster „Paket-Manager-Konsole“ ein:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> Das obige Paket installiert die Microsoft Authentication Library (MSAL). Die MSAL kümmert sich um das Erfassen, Zwischenspeichern und Aktualisieren von Benutzertoken für den Zugriff auf APIs, die von Azure Active Directory v2 geschützt werden.

<a id="add-the-code-to-initialize-msal" class="xliff"></a>
## Hinzufügen des Codes zum Initialisieren der MSAL
Dieser Schritt hilft Ihnen beim Erstellen einer Klasse zum Verarbeiten der Interaktion mit der MSAL, z.B. zum Verarbeiten von Token.

1. Öffnen Sie die Datei `App.xaml.cs`, und fügen Sie in den Verweis auf die MSAL-Bibliothek zur Klasse hinzu:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Aktualisieren Sie die „App“-Klasse wie folgt:
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is the clientId of your app registration. 
    //You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

<a id="create-your-applications-ui" class="xliff"></a>
## Erstellen der Benutzeroberfläche Ihrer Anwendung
Im folgenden Abschnitt wird gezeigt, wie eine Anwendung einen geschützten Back-End-Server wie Microsoft Graph abfragen kann. Die Datei „MainWindow.xaml“ sollte als Teil Ihrer Projektvorlage automatisch erstellt werden. Öffnen Sie diese Datei, und befolgen Sie dann die nachstehenden Anweisungen:

1.  Ersetzen Sie `<Grid>` Ihrer Anwendung durch Folgendes:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```

