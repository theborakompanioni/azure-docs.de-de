---
title: Informationen zum sicheren Zugriff auf Daten in DocumentDB | Microsoft Docs
description: "Informationen zu Zugriffssteuerungskonzepten in DocumentDB, darunter Hauptschlüssel, Schlüssel mit Leseberechtigung, Benutzer und Berechtigungen."
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 8641225d-e839-4ba6-a6fd-d6314ae3a51c
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: a12d3a5c25decdc88df0c9f9b3216bfaae33d5a6
ms.lasthandoff: 03/10/2017


---
# <a name="securing-access-to-documentdb-data"></a>Sicherer Zugriff auf Daten in DocumentDB
Dieser Artikel bietet eine Übersicht über den sicheren Zugriff auf in [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)gespeicherte Daten.

DocumentDB verwendet zwei Arten von Schlüsseln, um Benutzer zu authentifizieren und den Zugriff auf Daten und Ressourcen zu ermöglichen. 

|Schlüsseltyp|Ressourcen|
|---|---|
|[Hauptschlüssel](#master-keys) |Wird für Verwaltungsressourcen verwendet: Datenbankkonten, Datenbanken, Benutzer und Berechtigungen|
|[Ressourcentoken](#resource-tokens)|Wird für Anwendungsressourcen verwendet: Auflistungen, Dokumente, Anlagen, gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen|

<a id="master-keys"></a>

## <a name="master-keys"></a>Hauptschlüssel 

Hauptschlüssel ermöglichen den Zugriff auf alle Verwaltungsressourcen für das Datenbankkonto. Hauptschlüssel:  
- Ermöglichen den Zugriff auf Konten, Datenbanken, Benutzer und Berechtigungen. 
- Können nicht zur präzisen Steuerung des Zugriffs auf Auflistungen und Dokumente verwendet werden.
- Werden im Zuge der Kontoerstellung erstellt.
- Können jederzeit neu generiert werden.

Jedes Konto umfasst zwei Hauptschlüssel: einen primären und einen sekundären Schlüssel. Dank der Verwendung von zwei Schlüsseln können Sie Schlüssel neu generieren oder ersetzen und trotzdem ohne Unterbrechung auf Ihr Konto und Ihre Daten zugreifen. 

Neben den beiden Hauptschlüsseln für das DocumentDB-Konto stehen noch zwei schreibgeschützte Schlüssel zur Verfügung. Diese schreibgeschützten Schlüssel ermöglichen ausschließlich Lesevorgänge für das Konto. Schreibgeschützte Schlüssel bieten keinen Zugriff auf Leseberechtigungsressourcen.

Primäre, sekundäre und schreibgeschützte Hauptschlüssel sowie Hauptschlüssel mit Lese-/Schreibzugriff können über das Azure-Portal abgerufen und neu generiert werden. Eine entsprechende Anleitung finden Sie unter [Anzeigen, Kopieren und erneutes Generieren von Zugriffsschlüsseln](documentdb-manage-account.md#a-idkeysaview-copy-and-regenerate-access-keys).

![Zugriffssteuerung (IAM) im Azure-Portal: Veranschaulichung der NoSQL-Datenbanksicherheit](./media/documentdb-secure-access-to-data/nosql-database-security-master-key-portal.png)

Der Hauptschlüssel kann ganz einfach gewechselt werden. Rufen Sie über das Azure-Portal Ihren sekundären Schlüssel ab, ersetzen Sie den primären Schlüssel in Ihrer Anwendung durch Ihren sekundären Schlüssel, und wechseln Sie anschließend den primären Schlüssel über das Azure-Portal.

![Wechsel des Hauptschlüssels im Azure-Portal: Veranschaulichung der NoSQL-Datenbanksicherheit](./media/documentdb-secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-to-use-a-master-key"></a>Codebeispiel für die Verwendung eines Hauptschlüssels

Das folgende Codebeispiel veranschaulicht, wie mit einem DocumentDB-Kontoendpunkt und einem Hauptschlüssel eine DocumentClient-Instanz instanziiert und eine Datenbank erstellt wird. 

```csharp
//Read the DocumentDB endpointUrl and authorization keys from config.
//These values are available from the Azure portal on the NOSQL (DocumentDB) account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access to your DocDB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly SecureString authorizationKey = ToSecureString(ConfigurationManager.AppSettings["AuthorizationKey"]);

client = new DocumentClient(new Uri(endpointUrl), authorizationKey);

// Create Database
Database database = await client.CreateDatabaseAsync(
    new Database
    {
        Id = databaseName
    });
```

<a id="resource-tokens"></a>

## <a name="resource-tokens"></a>Ressourcentoken

Ressourcentoken ermöglichen den Zugriff auf die Anwendungsressourcen in einer Datenbank. Ressourcentoken:
- Ermöglichen den Zugriff auf bestimmte Auflistungen, Dokumente, Anlagen, gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen.
- Werden erstellt, wenn einem [Benutzer](#users) [Berechtigungen](#permissions) für eine bestimmte Ressource gewährt werden.
- Werden neu erstellt, wenn durch einen POST-, GET- oder PUT-Aufruf eine Aktion für eine Berechtigungsressource ausgeführt wird.
- Verwenden ein Hashressourcentoken, das speziell für den Benutzer, die Ressource und die Berechtigung erstellt wird.
- Verfügen über einen anpassbaren Gültigkeitszeitraum. Die Gültigkeitsdauer beträgt standardmäßig eine Stunde. Die Tokengültigkeitsdauer kann aber explizit auf bis zu fünf Stunden festgelegt werden.
- Stellen eine sichere Alternative zur Weitergabe des Hauptschlüssels dar. 
- Ermöglichen Clients das Lesen, Schreiben und Löschen von Ressourcen im DocumentDB-Konto gemäß den gewährten Berechtigungen.

Mit einem Ressourcentoken (durch Erstellung von DocumentDB-Benutzern und -Berechtigungen) können Sie einem Client, dem Sie den Hauptschlüssel nicht anvertrauen können, Zugriff auf Ressourcen in Ihrem DocumentDB-Konto gewähren.  

DocumentDB-Ressourcentoken stellen eine sichere Alternative dar, um Clients das Lesen, Schreiben und Löschen von Ressourcen in einem DocumentDB-Konto gemäß den gewährten Berechtigungen und ohne Hauptschlüssel oder schreibgeschützten Schlüssel zu ermöglichen.

Hier sehen Sie ein typisches Design, bei dem Ressourcentoken angefordert, generiert und an Clients übermittelt werden können:

1. Ein Mid-Tier-Dienst dient zum Freigeben von Benutzerfotos für eine mobile Anwendung. 
2. Der Mid-Tier-Dienst verfügt über den Hauptschlüssel des DocumentDB-Kontos.
3. Die Foto-App ist auf mobilen Endbenutzergeräten installiert. 
4. Bei der Anmeldung richtet die Fotoapp die Identität des Benutzers mit dem Mid-Tier-Dienst ein. Dieser Mechanismus der Identitätseinrichtung hängt nur von der Anwendung ab.
5. Nachdem die Identität eingerichtet wurde, fordert der Mid-Tier-Dienst Berechtigungen auf Grundlage der Identität an.
6. Der Mid-Tier-Dienst sendet einen Ressourcentoken an die Phoneapp zurück.
7. Die Phoneapp kann weiterhin den Ressourcentoken für direkten Zugriff auf DocumentDB-Ressourcen mit den Berechtigungen verwenden, die durch das Ressourcentoken für einen bestimmten Zeitraum definiert sind. 
8. Wenn das Ressourcentoken abläuft, tritt bei anschließenden Anforderungen die Ausnahme 401 (nicht autorisierter Zugriff) auf.  An dieser Stelle richtet die Phoneapp die Identität erneut ein und fordert ein neues Ressourcentoken an.

    ![Workflow der DocumentDB-Ressourcentoken](./media/documentdb-secure-access-to-data/resourcekeyworkflow.png)

 Die Generierung und Verwaltung von Ressourcentoken wird von den nativen DocumentDB-Clientbibliotheken übernommen. Bei Verwendung von REST müssen Sie allerdings die Anforderungs-/Authentifizierungsheader erstellen. Weitere Informationen zum Erstellen von Authentifizierungsheadern für REST finden Sie unter [Access Control on DocumentDB Resources](https://docs.microsoft.com/en-us/rest/api/documentdb/access-control-on-documentdb-resources) (Zugriffssteuerung für DocumentDB-Ressourcen) oder im [Quellcode für unsere SDKs](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).
 
 Ein Beispiel für einen Dienst der mittleren Ebene, der zum Generieren oder Vermitteln von Ressourcentoken dient, finden Sie unter der [ResourceTokenBroker-App](https://github.com/kirillg/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

<a id="users"></a>

## <a name="users"></a>Benutzer
DocumentDB-Benutzer werden einer DocumentDB-Datenbank zugeordnet.  Jede Datenbank kann null oder mehr DocumentDB-Benutzer enthalten.  Das folgende Codebeispiel veranschaulicht das Erstellen einer DocumentDB-Benutzerressource.

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> Jeder DocumentDB-Benutzer verfügt über eine PermissionsLink-Eigenschaft, mit deren Hilfe die Liste mit den [Berechtigungen](#permissions) abgerufen werden kann, die dem Benutzer zugeordnet sind.
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a>Berechtigungen
Eine DocumentDB-Berechtigungsressource ist einem DocumentDB-Benutzer zugeordnet.  Jeder Benutzer kann null oder mehr DocumentDB-Berechtigungen enthalten.  Eine Berechtigungsressource ermöglicht den Zugriff auf ein Sicherheitstoken, das der Benutzer beim Zugriff auf eine bestimmte Anwendungsressource benötigt.
Von einer Berechtigungsressource können zwei Zugriffsebenen bereitgestellt werden:

* Alle: Der Benutzer verfügt über Vollzugriff auf die Ressource.
* Lesen: Der Benutzer kann die Inhalte der Ressource nur lesen und keine Schreib-, Aktualisierungs- oder Löschvorgänge für die Ressource vornehmen.

> [!NOTE]
> Zum Ausführen von gespeicherten DocumentDB-Prozeduren muss der Benutzer über uneingeschränkte Berechtigung für die Sammlung verfügen, in dem die gespeicherte Prozedur ausgeführt wird.
> 
> 

### <a name="code-sample-to-create-permission"></a>Codebeispiel für die Berechtigungserstellung

Das folgende Codebeispiel veranschaulicht das Erstellen einer Berechtigungsressource, das Lesen des Ressourcentokens der Berechtigungsressource sowie das Zuordnen der Berechtigungen zum oben erstellten [Benutzer](#users).

```csharp
// Create a permission.
Permission docPermission = new Permission
{
    PermissionMode = PermissionMode.Read,
    ResourceLink = documentCollection.SelfLink,
    Id = "readperm"
};
  
docPermission = await client.CreatePermissionAsync(UriFactory.CreateUserUri("db", "user"), docPermission);
Console.WriteLine(docPermission.Id + " has token of: " + docPermission.Token);
```

Falls Sie einen Partitionsschlüssel für Ihre Sammlung angegeben haben, müssen die Berechtigung für die Sammlung und die Dokument- und Anlagenressourcen neben „ResourceLink“ auch „ResourcePartitionKey“ beinhalten.

### <a name="code-sample-to-read-permissions-for-user"></a>Codebeispiel für das Lesen von Berechtigungen für einen Benutzer

Um alle einem bestimmten Benutzer zugeordneten Berechtigungsressourcen abzurufen, stellt DocumentDB für jedes Benutzerobjekt einen Berechtigungsfeed zur Verfügung.  Der folgende Codeausschnitt zeigt, wie die dem oben erstellten Benutzer zugeordnete Berechtigung abgerufen, eine Berechtigungsliste erstellt und ein neuer DocumentClient für den Benutzer instanziiert werden kann.

```csharp
//Read a permission feed.
FeedResponse<Permission> permFeed = await client.ReadPermissionFeedAsync(
  UriFactory.CreateUserUri("db", "myUser"));
 List<Permission> permList = new List<Permission>();

foreach (Permission perm in permFeed)
{
    permList.Add(perm);
}

DocumentClient userClient = new DocumentClient(new Uri(endpointUrl), permList);
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur DocumentDB-Datenbanksicherheit finden Sie unter [DocumentDB NoSQL database security](documentdb-nosql-database-security.md) (DocumentDB: NoSQL-Datenbanksicherheit).
* Informationen zum Verwalten von Hauptschlüsseln und schreibgeschützten Schlüsseln finden Sie unter [Verwalten eines DocumentDB-Kontos](documentdb-manage-account.md#a-idkeysaview-copy-and-regenerate-access-keys).
* Informationen zum Erstellen von DocumentDB-Autorisierungstoken finden Sie unter [Access Control on DocumentDB Resources](https://docs.microsoft.com/en-us/rest/api/documentdb/access-control-on-documentdb-resources) (Zugriffssteuerung für DocumentDB-Ressourcen).


