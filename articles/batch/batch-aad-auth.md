---
title: "Verwenden von Azure Active Directory zum Authentifizieren über Azure Batch | Microsoft-Dokumentation"
description: "Batch unterstützt für Azure AD die Authentifizierung über den Batch-Dienst und den Batch-Ressourcenanbieter."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 03/16/2017
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 28665625cde9c7b164986f7b2d9c845136543aba
ms.lasthandoff: 03/18/2017

---

# <a name="authenticate-from-batch-solutions-with-active-directory"></a>Authentifizieren über Batch-Lösungen mit Active Directory

Azure Batch unterstützt die Authentifizierung mit [Azure Active Directory][aad_about] (Azure AD) für den Batch-Dienst und den Batch-Verwaltungsdienst. Azure AD ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Azure selbst verwendet Azure AD für die Authentifizierung seiner Kunden, Dienstadministratoren und Organisationsbenutzer.

In diesem Artikel wird beschrieben, wie Azure AD zum Authentifizieren von Anwendungen verwendet werden kann, für die die Batch Management .NET-Bibliothek oder die Batch .NET-Bibliothek genutzt wird. Im Zusammenhang mit den Batch .NET-APIs wird veranschaulicht, wie Sie Azure AD zum Authentifizieren eines Administrators oder Co-Administrators eines Abonnements per integrierter Authentifizierung einsetzen können. Der authentifizierte Benutzer kann dann Anforderungen an Azure Batch senden.

Azure AD kann auch verwendet werden, um den Zugriff auf eine Anwendung zu authentifizieren, die unbeaufsichtigt ausgeführt wird. Hier konzentrieren wir uns auf die Verwendung der integrierten Azure AD-Authentifizierung und verweisen auf andere Ressourcen, um Informationen zur Authentifizierung von unbeaufsichtigten Anwendungen bereitzustellen.

## <a name="use-azure-ad-with-batch-management-solutions"></a>Verwenden von Azure AD mit Batch Management-Lösungen

Über die Batch Management .NET-Bibliothek werden Typen zur Verwendung von Batch-Konten, -Kontoschlüsseln, -Anwendungen und -Anwendungspaketen verfügbar gemacht. Die Batch Management .NET-Bibliothek ist ein Azure-Ressourcenanbieterclient, der zusammen mit [Azure Resource Manager][resman_overview] zum programmgesteuerten Verwalten dieser Ressourcen verwendet wird. 

Azure AD ist erforderlich, um Anforderungen über beliebige Azure-Ressourcenanbieterclients, einschließlich der Batch Management .NET-Bibliothek, und über [Azure Resource Manager][resman_overview] zu authentifizieren.

In diesem Abschnitt nutzen wir das [AccountManagment][acct_mgmt_sample]-Beispielprojekt auf GitHub, um die Schritte der Verwendung von Azure AD mit der Batch Management .NET-Bibliothek zu veranschaulichen. Das AccountManagement-Beispiel ist eine Konsolenanwendung, mit der programmgesteuert auf ein Abonnement zugegriffen wird, eine Ressourcengruppe und ein neues Batch-Konto erstellt werden und einige Vorgänge für das Konto durchgeführt werden. 

Weitere Informationen zur Nutzung der Batch Management .NET-Bibliothek und des AccountManagement-Beispiels finden Sie unter [Verwalten von Batch-Konten und -Kontingenten mit der Batch Management-Clientbibliothek für .NET](batch-management-dotnet.md).

### <a name="register-your-application-with-azure-ad"></a>Registrieren Ihrer Anwendung bei Azure AD

Die Azure [Active Directory Authentication Library][aad_adal] (ADAL) stellt eine programmgesteuerte Schnittstelle für Azure AD bereit, die Sie in Ihren Anwendungen einsetzen können. Zum Aufrufen von ADAL aus Ihrer Anwendung müssen Sie Ihre Anwendung in einem Azure AD-Mandanten registrieren. Beim Registrieren Ihrer Anwendung stellen Sie für Azure AD Informationen zu Ihrer Anwendung bereit, einschließlich eines entsprechenden Namens im Azure AD-Mandanten. Azure AD stellt dann eine Anwendungs-ID bereit, die Sie verwenden, um Ihre Anwendung zur Laufzeit Azure AD zuzuordnen. Weitere Informationen zur Anwendungs-ID finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).

Führen Sie die Schritte im Abschnitt [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) (Hinzufügen einer Anwendung) des Artikels [Integrating applications with Azure Active Directory][aad_integrate] (Integrieren von Anwendungen in Azure Active Directory) aus, um die AccountManagement-Beispielanwendung zu registrieren. Geben Sie als Typ der Anwendung **Systemeigene Clientanwendung** an. Für den **Umleitungs-URI** können Sie einen beliebigen gültigen URI angeben (z.B. `http://myaccountmanagementsample`), da dies kein echter Endpunkt sein muss:

![](./media/batch-aad-auth/app-registration-management-plane.png)

Nachdem Sie den Registrierungsprozess abgeschlossen haben, werden die Anwendungs-ID und die Objekt-ID (Dienstprinzipal) angezeigt, die für Ihre Anwendung aufgeführt sind.  

![](./media/batch-aad-auth/app-registration-client-id.png)

### <a name="update-your-code-to-reference-your-application-id"></a>Aktualisieren des Codes zum Verweisen auf Ihre Anwendungs-ID 

Ihre Clientanwendung verwendet die Anwendungs-ID (auch als Client-ID bezeichnet) zum Zugreifen auf Azure AD zur Laufzeit. Nachdem Sie die Anwendung im Azure-Portal registriert haben, können Sie Ihren Code aktualisieren, damit die Anwendungs-ID verwendet wird, die von Azure AD für Ihre registrierte Anwendung bereitgestellt wurde. Kopieren Sie Ihre Anwendungs-ID in der AccountManagement-Beispielanwendung aus dem Azure-Portal in die entsprechende Konstante:

```csharp
// Specify the unique identifier (the "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access the Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
Kopieren Sie außerdem den Umleitungs-URI, den Sie während des Registrierungsprozesses angegeben haben.

```csharp
// The URI to which Azure AD will redirect in response to an OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need to be a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

### <a name="grant-the-azure-resource-manager-api-access-to-your-application"></a>Gewähren des Zugriffs auf Ihre Anwendung für die Azure Resource Manager-API

Als Nächstes müssen Sie den Zugriff auf Ihre Anwendung an die Azure Resource Manager-API delegieren. Der Azure AD-Bezeichner für die Resource Manager-API lautet **Windows Azure-Service-Verwaltungs-API**.

Führen Sie im Azure-Portal die folgenden Schritte aus:

1. Wählen Sie im linken Navigationsbereich des Azure-Portals die Option **Weitere Dienste**, und klicken Sie auf **App-Registrierungen** und auf **Hinzufügen**.
2. Suchen Sie in der Liste mit den App-Registrierungen nach dem Namen Ihrer Anwendung:

    ![Suchen nach Ihrem Anwendungsnamen](./media/batch-aad-auth/search-app-registration.png)

3. Zeigen Sie das Blatt **Einstellungen** an. Wählen Sie im Abschnitt **API-Zugriff** die Option **Erforderliche Berechtigungen**.
4. Klicken Sie auf **Hinzufügen**, um eine neue erforderliche Berechtigung hinzuzufügen. 
5. Geben Sie in Schritt 1 **Windows Azure-Service-Verwaltungs-API** ein, wählen Sie diese API in der Liste mit den Ergebnissen aus, und klicken Sie auf die Schaltfläche **Auswählen**.
6. Aktivieren Sie in Schritt 2 das Kontrollkästchen neben **Access Azure classic deployment model as organization users** (Als Organisationsbenutzer auf das klassische Azure-Bereitstellungsmodell zugreifen), und klicken Sie auf die Schaltfläche **Auswählen**.
7. Klicken Sie auf die Schaltfläche **Fertig**.

Auf dem Blatt **Erforderliche Berechtigungen** wird jetzt angezeigt, dass Berechtigungen für Ihre Anwendung sowohl für die ADAL- als auch für die Resource Manager-APIs gewährt werden. Berechtigungen für ADAL werden standardmäßig gewährt, wenn Sie Ihre App zum ersten Mal mit Azure AD registrieren.

![Delegieren von Berechtigungen für die Azure Resource Manager-API](./media/batch-aad-auth/required-permissions-management-plane.png)


### <a name="acquire-an-azure-ad-authentication-token"></a>Abrufen eines Azure AD-Authentifizierungstokens

In der AccountManagement-Beispielanwendung werden Konstanten definiert, mit denen der Endpunkt für Azure AD und Azure Resource Manager bereitgestellt wird. Die Beispielanwendung nutzt diese Konstanten, um Abonnementinformationen für Azure AD abzufragen. Lassen Sie diese Konstanten unverändert:

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure service management resource 
private const string ResourceUri = "https://management.core.windows.net/";
```

Nach dem Registrieren des AccountManagement-Beispiels im Azure AD-Mandanten und Angeben der erforderlichen Werte im Quellcode des Beispiels ist das Beispiel für die Authentifizierung mit Azure AD bereit. Wenn Sie das Beispiel ausführen, wird für ADAL versucht, ein Authentifizierungstoken abzurufen. In diesem Schritt werden Sie zum Eingeben Ihrer Microsoft-Anmeldeinformationen aufgefordert: 

```csharp
// Obtain an access token using the "common" AAD resource. This allows the application
// to query AAD for information that lies outside the application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

Nach dem Angeben der Anmeldeinformationen kann die Beispielanwendung damit fortfahren, authentifizierte Anforderungen für den Batch Management-Dienst auszustellen. 

## <a name="use-azure-ad-with-batch-service-solutions"></a>Verwenden von Azure AD mit Batch-Dienstlösungen

Die Batch .NET-Bibliothek enthält Typen zum Erstellen von Workflows mit paralleler Verarbeitung mit dem Batch-Dienst. Der Batch-Dienst unterstützt sowohl die Authentifizierung per [gemeinsam verwendetem Schlüssel](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) als auch die Authentifizierung mit Azure AD. In diesem Abschnitt wird die Authentifizierung per Azure AD beschrieben.

>[!NOTE]
>Beim Erstellen eines Batch-Kontos können Sie angeben, ob Pools einem Abonnement, das mit Batch verwaltet wird, oder einem Benutzerabonnement zugeordnet werden sollen. Wenn Ihr Konto Pools in einem Benutzerabonnement zuordnet, müssen Sie Azure AD verwenden, um Anforderungen für Ressourcen in diesem Konto zu authentifizieren.
>
>

Die Authentifizierung von Batch .NET-Anwendungen per Azure AD ähnelt der Authentifizierung von Batch Management .NET-Anwendungen. Es gibt einige Unterschiede, die in diesem Abschnitt beschrieben werden.

### <a name="batch-service-endpoints"></a>Batch-Dienstendpunkte

Die Endpunkte des Batch-Diensts unterscheiden sich von den Endpunkten, die Sie mit Batch Management .NET verwenden.

Der Azure AD-Endpunkt für den Batch-Dienst lautet:

`https://login.microsoftonline.com/common`

Der Ressourcenendpunkt für den Batch-Dienst lautet:

`https://batch.core.windows.net/`

### <a name="grant-the-batch-service-api-access-to-your-application"></a>Gewähren des Zugriffs auf Ihre Anwendung für die Batch-Dienst-API

Bevor Sie sich über Ihre Batch-Anwendung per Azure AD authentifizieren können, müssen Sie die Anwendung bei Azure AD registrieren und Zugriff auf die Batch-Dienst-API gewähren. Der Azure AD-Bezeichner für die Batch-Dienst-API lautet **Microsoft Azure Batch (MicrosoftAzureBatch)**.

1. Führen Sie die Schritte im Abschnitt [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) (Hinzufügen einer Anwendung) des Artikels [Integrating applications with Azure Active Directory][aad_integrate] (Integrieren von Anwendungen in Azure Active Directory) aus, um Ihre Batch-Anwendung zu registrieren. Als **Umleitungs-URI** können Sie einen beliebigen gültigen URI angeben. Es muss sich nicht um einen echten Endpunkt handeln.

    Nachdem Sie Ihre Anwendung registriert haben, werden die Anwendungs-ID und Objekt-ID angezeigt:

    ![Registrieren Ihrer Batch-Anwendung bei Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

2. Zeigen Sie als Nächstes das Blatt **Einstellungen** an. Wählen Sie im Abschnitt **API-Zugriff** die Option **Erforderliche Berechtigungen**.
3. Klicken Sie auf dem Blatt **Erforderliche Berechtigungen** auf die Schaltfläche **Hinzufügen**.
4. Suchen Sie in Schritt 1 nach **MicrosoftAzureBatch**, wählen Sie **Microsoft Azure Batch (MicrosoftAzureBatch)** aus, und klicken Sie dann auf die Schaltfläche **Auswählen**.
5. Aktivieren Sie in Schritt 2 das Kontrollkästchen neben **Access Azure Batch Service** (Auf Azure Batch-Dienst zugreifen), und klicken Sie auf die Schaltfläche **Auswählen**.
6. Klicken Sie auf die Schaltfläche **Fertig**.

Auf dem Blatt **Erforderliche Berechtigungen** wird jetzt angezeigt, dass Ihre Azure AD-Anwendung sowohl Zugriff auf die Azure AD- als auch auf Azure Batch-APIs gewährt. 

![API-Berechtigungen](./media/batch-aad-auth/required-permissions-data-plane.png)

### <a name="authentication-for-batch-accounts-in-a-user-subscription"></a>Authentifizierung für Batch-Konten in einem Benutzerabonnement

Beim Erstellen eines neuen Batch-Kontos können Sie das Abonnement auswählen, unter dem Pools zugeordnet werden. Ihre Auswahl wirkt sich darauf aus, wie Sie Anforderungen für Ressourcen in diesem Konto authentifizieren.

Standardmäßig werden Batch-Pools in einem Abonnement des Batch-Diensts zugeordnet. Wenn Sie diese Option wählen, können Sie Anforderungen an Ressourcen in diesem Konto entweder mit einem gemeinsam verwendeten Schlüssel oder mit Azure AD authentifizieren.

Außerdem können Sie angeben, dass Batch-Pools in einem angegebenen Benutzerabonnement zugeordnet werden. Wenn Sie diese Option auswählen, müssen Sie die Authentifizierung mit Azure AD durchführen.

### <a name="best-practices-for-using-azure-ad-with-batch"></a>Bewährte Methoden für die Nutzung von Azure AD mit Batch

Ein Azure AD-Authentifizierungstoken läuft nach einer Stunde ab. Bei Verwendung eines langlebigen **BatchClient**-Objekts empfehlen wir Ihnen, bei jeder Anforderung ein Token aus ADAL abzurufen, um sicherzustellen, dass Sie immer über ein gültiges Token verfügen. 

Schreiben Sie in .NET hierfür eine Methode, mit der das Token aus Azure AD abgerufen wird, und übergeben Sie diese Methode als Delegat an ein **BatchTokenCredentials**-Objekt. Die Delegatmethode wird bei jeder Anforderung an den Batch-Dienst aufgerufen, um sicherzustellen, dass ein gültiges Token bereitgestellt wird. Standardmäßig werden Token von ADAL zwischengespeichert, sodass nur dann ein neues Token aus Azure AD abgerufen wird, wenn dies erforderlich ist. Ein Beispiel finden Sie unter [Codebeispiel: Verwenden von Azure AD mit Batch .NET](#code-example-using-azure-ad-with-batch-net) im nächsten Abschnitt. Weitere Informationen zu Token in Azure AD finden Sie unter [Authentifizierungsszenarien für Azure AD][aad_auth_scenarios].

### <a name="code-example-using-azure-ad-with-batch-net"></a>Codebeispiel: Verwenden von Azure AD mit Batch .NET

Nutzen Sie zum Schreiben von Batch .NET-Code, bei dem die Authentifizierung mit Azure AD durchgeführt wird, das [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/)-Paket und das [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)-Paket.

Fügen Sie die folgenden `using`-Anweisungen in den Code ein:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Verweisen Sie im Code auf den gemeinsamen Azure AD-Endpunkt und den Azure AD-Endpunkt für den Batch-Dienst:  

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/common";
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Verweisen Sie auf Ihren Endpunkt des Batch-Kontos:

```csharp
private const string BatchAccountEndpoint = "https://myaccount.westcentralus.batch.azure.com";
```

Geben Sie die Anwendungs-ID (Client-ID) für Ihre Anwendung an. Die Anwendungs-ID ist über Ihre App-Registrierung im Azure-Portal verfügbar. Sie finden sie im Abschnitt [Gewähren des Zugriffs auf Ihre Anwendung für die Batch-Dienst-API](#grant-the-batch-service-api-access-to-your-application). 

```csharp
private const string ClientId = "<application-id>";
```

Geben Sie außerdem einen Umleitungs-URI an. Dies kann ein beliebiger gültiger URI sein.

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

Schreiben Sie eine Callbackmethode, um das Authentifizierungstoken aus Azure AD abzurufen. Mit der **AcquireTokenAsync**-Methode werden die Anmeldeinformationen des Benutzers abgefragt und zum Beschaffen eines neuen Tokens verwendet.

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire the authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

Erstellen Sie ein **BatchTokenCredentials**-Objekt, in dem der Delegat als Parameter verwendet wird. Verwenden Sie diese Anmeldeinformationen, um ein **BatchClient**-Objekt zu öffnen. Sie können dieses **BatchClient**-Objekt dann für nachfolgende Vorgänge für den Batch-Dienst verwenden.

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountEndpoint, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

In der obigen **GetAuthenticationTokenAsync**-Callbackmethode wird Azure AD für die integrierte Authentifizierung eines Benutzers genutzt, der mit der Anwendung interagiert. Durch das Aufrufen der **AcquireTokenAsync**-Methode werden die Anmeldeinformationen des Benutzers abgefragt, und die Anwendung setzt den Vorgang fort, nachdem der Benutzer die Informationen angegeben hat. Sie können Azure AD auch verwenden, um eine unbeaufsichtigte Anwendung zu authentifizieren, indem Sie einen Azure AD-Dienstprinzipal verwenden. Weitere Informationen finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md) und [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../resource-group-create-service-principal-portal.md).  
 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ausführen der [AccountManagement-Beispielanwendung][acct_mgmt_sample] finden Sie unter [Verwalten von Batch-Konten und -Kontingenten mit der Batch Management-Clientbibliothek für .NET](batch-management-dotnet.md).

Weitere Informationen zu Azure AD finden Sie unter [Dokumentation zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Ausführliche Beispiele zur ADAL-Verwendung finden Sie in der Bibliothek mit den [Azure-Codebeispielen](https://azure.microsoft.com/resources/samples/?service=active-directory).


[aad_about]: ../active-directory/active-directory-whatis.md "Was ist Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentifizierungsszenarien für Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrieren von Anwendungen in Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

