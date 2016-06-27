<properties
	pageTitle="Erste Schritte mit der Azure-CDN-Bibliothek für .NET | Microsoft Azure"
	description="Erfahren Sie, wie Sie in Visual Studio .NET-Anwendungen zum Verwalten von Azure CDN schreiben."
	services="cdn"
	documentationCenter=".net"
	authors="camsoper"
	manager="erikre"
	editor=""/>

<tags
	ms.service="cdn"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/14/2016"
	ms.author="casoper"/>

# Erste Schritte mit der Azure-CDN-Bibliothek für .NET

Sie können die [Azure-CDN-Bibliothek für .NET](https://msdn.microsoft.com/library/mt657769.aspx) verwenden, um die Erstellung und Verwaltung von CDN-Profilen und -Endpunkten zu automatisieren. Dieses Tutorial führt Sie durch die Erstellung einer einfachen .NET-Konsolenanwendung, die einige der verfügbaren Vorgänge demonstriert. Dieses Tutorial dient nicht dazu, alle Aspekte der Azure-CDN-Bibliothek für .NET ausführlich zu beschreiben.

Für dieses Tutorial benötigen Sie Visual Studio 2015. [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) steht kostenlos zum Download zur Verfügung.

Ein abgeschlossenes Beispiel dieses Tutorials finden Sie [hier](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

## Vorbereitung

Bevor Sie Code für die CDN-Verwaltung schreiben können, müssen Sie einige Vorbereitungen treffen. Als Erstes erstellen Sie eine Ressourcengruppe, in der das in diesem Tutorial erstellte CDN-Profil enthalten sein soll. Danach richten Sie Azure Active Directory für die Authentifizierung Ihrer Anwendung ein. Anschließend wenden Sie Berechtigungen auf die Ressourcengruppe an, sodass nur autorisierte Benutzer aus Ihrem Azure AD-Mandanten mit dem CDN-Profil interagieren können.

### Erstellen der Ressourcengruppe

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie oben links auf die Schaltfläche **Neu**, und klicken Sie dann auf **Verwaltung** und **Ressourcengruppe**.
	
	![Erstellen einer neuen Ressourcengruppe](./media/cdn-app-dev-net/cdn-new-rg-1.png)

3. Nennen Sie die Ressourcengruppe *CdnConsoleTutorial*. Wählen Sie Ihr Abonnement aus, und wählen Sie einen Standort in Ihrer Nähe. Wenn Sie möchten, können Sie das Kontrollkästchen **An Dashboard anheften** aktivieren, um die Ressourcengruppe an das Dashboard im Portal anzuheften. Dadurch lässt sie sich später leichter wiederfinden. Nachdem Sie Ihre Auswahl getroffen haben, klicken Sie auf **Erstellen**.

	![Benennen der Ressourcengruppe](./media/cdn-app-dev-net/cdn-new-rg-2.png)

4. Wenn Sie die Ressourcengruppe nach dem Erstellen nicht an Ihr Dashboard angeheftet haben, können Sie sie suchen, indem Sie auf **Durchsuchen** und dann auf **Ressourcengruppen** klicken. Klicken Sie auf die Ressourcengruppe, um sie zu öffnen. Notieren Sie sich Ihre **Abonnement-ID**. Sie benötigen sie später.

	 ![Benennen der Ressourcengruppe](./media/cdn-app-dev-net/cdn-subscription-id.png)

### Erstellen der Azure AD-Anwendung

Es gibt zwei Methoden für die App-Authentifizierung mit Azure Active Directory: per einzelnem Benutzer oder per Dienstprinzipal. Ein Dienstprinzipal ähnelt einem Dienstkonto in Windows. Anstatt einen bestimmten Benutzer Berechtigungen für die Interaktion mit den CDN-Profilen zu gewähren, erteilen wir die Berechtigungen für den Dienstprinzipal. Dienstprinzipale werden im Allgemeinen für automatisierte, nicht interaktive Prozesse verwendet. Auch wenn in diesem Tutorial eine interaktive Konsolen-App erstellt wird, verwenden wir hier einen Dienstprinzipal.

Die Erstellung eines Dienstprinzipals umfasst mehrere Schritte, einschließlich der Erstellung einer Azure Active Directory-Anwendung. [Absolvieren Sie dieses Tutorial](../resource-group-create-service-principal-portal.md), um diese Schritte auszuführen.

> [AZURE.IMPORTANT] Stellen Sie sicher, dass Sie alle Schritte in [diesem Tutorial](../resource-group-create-service-principal-portal.md) ausführen. Es ist *von größter Bedeutung*, dass Sie die Schritte exakt wie beschrieben ausführen. Notieren Sie sich die folgenden Angaben, da Sie diese später benötigen werden: **Mandanten-ID**, **Domänenname des Mandanten** (üblicherweise eine *.onmicrosoft.com*-Domäne, sofern Sie keine benutzerdefinierte Domäne angegeben haben), **Client-ID** und **Clientauthentifizierungsschlüssel**. Schützen Sie die **Client-ID** und den **Clientauthentifizierungsschlüssel** sorgfältig, da diese Anmeldeinformationen von jeder Person verwendet werden können, um Vorgänge als Dienstprinzipal auszuführen.
> 	
> Wenn Sie zum Schritt [Mehrinstanzenfähige Anwendung konfigurieren](../resource-group-create-service-principal-portal.md#configure-multi-tenant-application) gelangen, wählen Sie **Nein**.
> 
> Im Schritt [Anwendung zu Rolle zuweisen](../resource-group-create-service-principal-portal.md#assign-application-to-role) verwenden Sie die zuvor erstellte Ressourcengruppe, *CdnConsoleTutorial*, weisen ihr jedoch nicht die Rolle **Leser**, sondern die Rolle **Mitwirkender von CDN-Profil** zu. Nachdem Sie die Anwendung in Ihrer Ressourcengruppe zur Rolle **Mitwirkender von CDN-Profil** zugewiesen haben, kehren Sie zu diesem Tutorial zurück.

Sobald Sie den Dienstprinzipal erstellt und die Rolle **Mitwirkender von CDN-Profil** zugewiesen haben, sollte das Blatt **Benutzer** für Ihre Ressourcengruppe in etwa wie folgt aussehen.

![Blatt „Benutzer“](./media/cdn-app-dev-net/cdn-service-principal.png)


### Interaktive Benutzerauthentifizierung

Wenn Sie statt eines Dienstprinzipals lieber eine interaktive individuelle Benutzerauthentifizierung einrichten möchten, ähnelt der Prozess dem der Erstellung eines Dienstprinzipals. Sie wenden im Grunde das gleiche Verfahren an, nehmen aber einige kleinere Änderungen vor.

>[AZURE.IMPORTANT] Führen Sie die nächsten Schritte nur aus, wenn Sie die individuelle Benutzerauthentifizierung anstelle eines Dienstprinzipals verwenden möchten.

1. Wählen Sie beim Erstellen der Anwendung statt **Web-App** die Option **Native Anwendung**. 
	
	![Native Anwendung](./media/cdn-app-dev-net/cdn-native-application.png)
	
2. Auf der nächsten Seite werden Sie zur Eingabe eines **Umleitungs-URIs** aufgefordert. Der URI wird nicht überprüft, merken Sie sich jedoch, was Sie eingegeben haben. Sie benötigen die Information später.

3. Sie müssen keinen **Clientauthentifizierungsschlüssel** erstellen.

4. Anstatt der Rolle **Mitwirkender von CDN-Profil** einen Dienstprinzipal zuzuweisen, weisen Sie einzelne Benutzer oder Gruppen zu. In diesem Beispiel sehen Sie, dass ich den Benutzer *CDN Demo User* der Rolle **CDN Profile Contributor** zugewiesen habe.
	
	![Individueller Benutzerzugriff](./media/cdn-app-dev-net/cdn-aad-user.png)


## Erstellen des Projekts und Hinzufügen von NuGet-Paketen

Nachdem Sie nun eine Ressourcengruppe für Ihre CDN-Profile erstellt und der Azure AD-Anwendung die Berechtigung zum Verwalten von CDN-Profilen und -Endpunkten innerhalb dieser Gruppe erteilt haben, können Sie damit beginnen, Ihre Anwendung zu erstellen.

Klicken Sie in Visual Studio 2015 auf **Datei**, **Neu**, **Projekt...**, um das Dialogfeld für neue Projekte zu öffnen. Erweitern Sie **Visual C#**, und wählen Sie im linken Bereich **Windows** aus. Klicken Sie im mittleren Bereich auf **Konsolenanwendung**. Benennen Sie Ihr Projekt, und klicken Sie auf **OK**.

![Neues Projekt](./media/cdn-app-dev-net/cdn-new-project.png)

Das Projekt soll einige Azure-Bibliotheken verwenden, die in NuGet-Paketen enthalten sind. Fügen Sie diese nun zum Projekt hinzu.

1. Klicken Sie auf das Menü **Tools**, dann auf **NuGet-Paket-Manager** und auf **Paket-Manager-Konsole**.

	![Verwalten von NuGet-Paketen](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. Führen Sie in der Paket-Manager-Konsole den folgenden Befehl aus, um die **Active Directory Authentication Library (ADAL)** zu installieren:

	`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. Führen Sie den folgenden Befehl aus, um die **Azure CDN Management Library** in installieren:

	`Install-Package Microsoft.Azure.Management.Cdn`

## Anweisungen, Konstanten, main-Methode und Hilfsmethoden

Schreiben Sie nun die Grundstruktur des Programms.

1. Ersetzen Sie auf der Registerkarte „Program.cs“ die `using`-Anweisungen oben durch Folgendes:

	```
	using System;
	using System.Collections.Generic;
	using Microsoft.Azure.Management.Cdn;
	using Microsoft.Azure.Management.Cdn.Models;
	using Microsoft.Azure.Management.Resources;
	using Microsoft.Azure.Management.Resources.Models;
	using Microsoft.IdentityModel.Clients.ActiveDirectory;
	using Microsoft.Rest;
	```

2. Sie müssen einige Konstanten definieren, die von den Methoden verwendet werden. Fügen Sie in der `Program`-Klasse vor der `Main`-Methode Folgendes hinzu. Stellen Sie sicher, dass Sie die Platzhalter, einschließlich der **&lt;spitzen Klammern&gt;**, nach Bedarf durch Ihre eigenen Werte ersetzen.

	```
	//Tenant app constants
	private const string clientID = "<YOUR CLIENT ID>";
	private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
	private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

	//Application constants
	private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
	private const string profileName = "CdnConsoleApp";
	private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
	private const string resourceGroupName = "CdnConsoleTutorial";
	private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
	```

3. Definieren Sie auf Ebene der Klasse diese beiden Variablen. Sie werden diese später verwenden, um zu ermitteln, ob Ihr Profil und Endpunkt bereits vorhanden sind.

	```
	static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
	```

4.  Ersetzen Sie die `Main`-Methode wie folgt:

	```
	static void Main(string[] args)
	{
		//Get a token
		AuthenticationResult authResult = GetAccessToken();

		// Create CDN client
		CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
			{ SubscriptionId = subscriptionId };

		ListProfilesAndEndpoints(cdn);

		// Create CDN Profile
		CreateCdnProfile(cdn);

		// Create CDN Endpoint
		CreateCdnEndpoint(cdn);
		
		Console.WriteLine();

		// Purge CDN Endpoint
		PromptPurgeCdnEndpoint(cdn);

		// Delete CDN Endpoint
		PromptDeleteCdnEndpoint(cdn);

		// Delete CDN Profile
		PromptDeleteCdnProfile(cdn);

		Console.WriteLine("Press Enter to end program.");
		Console.ReadLine();
	}
	```

5. Einige Ihrer Methoden werden die Benutzer auffordern, Fragen mit Ja oder Nein zu beantworten. Fügen Sie die folgende Methode hinzu, um diesen Vorgang etwas zu erleichtern:

	```
	private static bool PromptUser(string Question)
	{
		Console.Write(Question + " (Y/N): ");
		var response = Console.ReadKey();
		Console.WriteLine();
		if (response.Key == ConsoleKey.Y)
		{
			return true;
		}
		else if (response.Key == ConsoleKey.N)
		{
			return false;
		}
		else
		{
			// They're not pressing Y or N.  Let's ask them again.
			return PromptUser(Question);
		}
	}
	```

Die grundlegende Struktur des Programms ist geschrieben. Jetzt erstellen Sie die Methoden, die von der `Main`-Methode aufgerufen werden.

## Authentifizierung

Bevor Sie die Azure CDN Management Library verwenden können, müssen Sie den Dienstprinzipal authentifizieren und ein Authentifizierungstoken abrufen. Diese Methode verwendet die ADAL, um das Token abzurufen.

```
private static AuthenticationResult GetAccessToken()
{
	AuthenticationContext authContext = new AuthenticationContext(authority); 
	ClientCredential credential = new ClientCredential(clientID, clientSecret);
	AuthenticationResult authResult = 
		authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

	return authResult;
}
```

Wenn Sie die individuelle Benutzerauthentifizierung verwenden, sieht die `GetAccessToken`-Methode etwas anders aus.

>[AZURE.IMPORTANT] Verwenden Sie dieses Codebeispiel nur, wenn Sie die individuelle Benutzerauthentifizierung anstelle eines Dienstprinzipals verwenden möchten.

```
private static AuthenticationResult GetAccessToken()
{
	AuthenticationContext authContext = new AuthenticationContext(authority);
	AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
		clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

	return authResult;
}
```

Stellen Sie sicher, dass Sie `<redirect URI>` durch den Umleitungs-URI ersetzen, den Sie bei der Registrierung der Anwendung in Azure AD eingegeben haben.

## Auflisten von CDN-Profilen und -Endpunkten

Jetzt können Sie CDN-Vorgänge ausführen. Als Erstes listet die Methode alle Profile und Endpunkte in Ihrer Ressourcengruppe auf. Wenn eine Übereinstimmung für die in den Konstanten angegebenen Profil- und Endpunktnamen gefunden wird, vermerkt die Methode diese Namen für die spätere Verwendung, damit keine Duplikate erstellt werden.

```
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
	// List all the CDN profiles in this resource group
	var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
	foreach (Profile p in profileList)
	{
		Console.WriteLine("CDN profile {0}", p.Name);
		if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
		{
			// Hey, that's the name of the CDN profile we want to create!
			profileAlreadyExists = true;
		}

		//List all the CDN endpoints on this CDN profile
		Console.WriteLine("Endpoints:");
		var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
		foreach (Endpoint e in endpointList)
		{
			Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
			if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
			{
				// The unique endpoint name already exists.
				endpointAlreadyExists = true;
			}
		}
		Console.WriteLine();
	}
}
```

## Erstellen von CDN-Profilen und -Endpunkten

Als Nächstes erstellen Sie ein Profil.

```
private static void CreateCdnProfile(CdnManagementClient cdn)
{
	if (profileAlreadyExists)
	{
		Console.WriteLine("Profile {0} already exists.", profileName);
	}
	else
	{
		Console.WriteLine("Creating profile {0}.", profileName);
		ProfileCreateParameters profileParms =
			new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
		cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
	}
}
```

Danach erstellen Sie einen Endpunkt.

```
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
	if (endpointAlreadyExists)
	{
		Console.WriteLine("Profile {0} already exists.", profileName);
	}
	else
	{
		Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
		EndpointCreateParameters endpointParms =
			new EndpointCreateParameters()
			{
				Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
				IsHttpAllowed = true,
				IsHttpsAllowed = true,
				Location = resourceLocation
			};
		cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
	}
}
```

>[AZURE.NOTE] Das obige Beispiel weist den Endpunkt einem Ursprung namens *Contoso* mit dem Hostnamen `www.contoso.com` zu. Ändern Sie dies in den Hostnamen Ihres Ursprungs.

## Bereinigen eines Endpunkts

Eine häufige Aufgabe, die Sie auch in Ihr Programm aufnehmen sollten, ist das Löschen von Inhalten auf dem Endpunkt.

```
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
	if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
	{
		Console.WriteLine("Purging endpoint. Please wait...");
		cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
		Console.WriteLine("Done.");
		Console.WriteLine();
	}
}
```

>[AZURE.NOTE] Im obigen Beispiel gibt die Zeichenfolge `/*` an, dass ich sämtliche Daten aus dem Stammverzeichnis des Endpunktpfads löschen möchte. Dies entspricht der Option **Alles löschen** im Dialogfeld zur „Bereinigung“ im Azure-Portal. In der `CreateCdnProfile`-Methode habe ich das Profil durch Angabe des Codes `Sku = new Sku(SkuName.StandardVerizon)` als **Azure CDN from Verizon** angelegt, daher wird der Vorgang erfolgreich durchgeführt. **Azure CDN from Akamai**-Profile dagegen unterstützen **Alles löschen** nicht. Wenn ich also in diesem Tutorial ein Akamai-Profil verwendet hätte, müsste ich bestimmte Pfade zum Löschen angeben.

## Löschen von CDN-Profilen und -Endpunkten

Die letzten Methoden, die Sie in Ihren Code einschließen, löschen den Endpunkt und das Profil.

```
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
	if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
	{
		Console.WriteLine("Deleting endpoint. Please wait...");
		cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
		Console.WriteLine("Done.");
		Console.WriteLine();
	}
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
	if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
	{
		Console.WriteLine("Deleting profile. Please wait...");
		cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
		Console.WriteLine("Done.");
		Console.WriteLine();
	}
}
```

## Ausführen des Programms

Jetzt können Sie das Programm kompilieren und ausführen, indem Sie in Visual Studio auf die Schaltfläche **Starten** klicken.

![Ausgeführtes Programm](./media/cdn-app-dev-net/cdn-program-running-1.png)

Wenn das Programm die obige Eingabeaufforderung erreicht, wechseln Sie zurück zu Ihrer Ressourcengruppe im Azure-Portal. Sie sollten feststellen, dass das Profil erstellt wurde.

![Erfolg!](./media/cdn-app-dev-net/cdn-success.png)

Bestätigen Sie die Aufforderung, um den Rest des Programms auszuführen.

![Abgeschlossenes Programm](./media/cdn-app-dev-net/cdn-program-running-2.png)

## Nächste Schritte

Um sich das abgeschlossene Projekt dieser exemplarischen Vorgehensweise anzusehen, [laden Sie das Beispiel](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) herunter.

Weitere Dokumentation zur Azure CDN Management Library for .NET finden Sie in der [Referenz auf MSDN](https://msdn.microsoft.com/library/mt657769.aspx).

<!---HONumber=AcomDC_0615_2016-->