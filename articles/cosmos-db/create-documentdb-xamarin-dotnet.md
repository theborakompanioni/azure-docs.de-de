---
title: 'Azure Cosmos DB: Erstellen einer Web-App mit Xamarin und Authentifizierung mit Facebook | Microsoft-Dokumentation'
description: "Veranschaulichung eines .NET-Codebeispiels, das Sie zur Verbindung mit und Abfrage von Azure Cosmos DB verwenden können"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: c8e2e688a7a8b99272c304df37ceff0f9ec52b19
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---
# <a name="azure-cosmos-db-build-a-web-app-with-net-xamarin-and-facebook-authentication"></a>Azure Cosmos DB: Erstellen einer Web-App mit .NET, Xamarin und Authentifizierung mit Facebook

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen. 

In diesem Schnellstart wird veranschaulicht, wie Sie ein Azure Cosmos DB-Konto, eine Dokumentendatenbank und eine Sammlung mithilfe des Azure-Portals erstellen. Anschließend erstellen Sie eine Web-App für To-Do-Listen mit der [.NET-API von DocumentDB](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/) und dem Authentifizierungsmodul von Azure Cosmos DB. Die Web-App implementiert ein Datenmuster pro Benutzer, mit dem sich Benutzer über die Authentifizierung mit Facebook anmelden und ihre eigenen To-Do-Elemente verwalten können.

## <a name="prerequisites"></a>Voraussetzungen

Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Hinzufügen einer Sammlung

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie nun eine DocumentDB-API-App aus GitHub, legen die Verbindungszeichenfolge fest und führen sie aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie ein Git-Terminalfenster, z.B. ein Git Bash, und `cd` in einem Arbeitsverzeichnis.  

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

    ```bash
    git clone https://github.com/Azure/azure-documentdb-dotnet.git
    ```

3. Öffnen Sie anschließend die Datei „DocumentDBTodo.sln“ aus dem Ordner „samples/xamarin/UserItems/xamarin.forms“ in Visual Studio. 

## <a name="review-the-code"></a>Überprüfen des Codes

Der Code im Xamarin-Ordner enthält Folgendes:

* Eine Xamarin-App. Die App speichert die To-Do-Elemente des Benutzers in einer partitionierten Sammlung mit dem Namen „UserItems“.
* Eine Ressourcentokenbroker-API. Eine einfache ASP.NET-Web-API, die Ressourcentoken von Azure Cosmos DB an in der App angemeldete Benutzer vermittelt. Ressourcentoken sind Zugriffstoken mit kurzer Lebensdauer, die der App den Zugriff auf die Daten des angemeldeten Benutzers ermöglichen.

Der Authentifizierungs- und Datenfluss wird im unten stehenden Diagramm veranschaulicht.

* Die Sammlung „UserItems“ wird mit dem Partitionsschlüssel „/userid“ erstellt. Durch die Angabe eines Partitionsschlüssels für eine Sammlung kann Azure Cosmos DB seine Größe unendlich an eine wachsende Zahl von Benutzern und Elementen anpassen.
* In der Xamarin-App können sich Benutzer mit Ihren Facebook-Anmeldeinformationen anmelden.
* Die Xamarin-App verwendet ein Zugriffstoken von Facebook, um sich bei der ResourceTokenBroker-API zu authentifizieren
* Die Ressourcentokenbroker-API authentifiziert die Anforderung mit der Funktion „App Service Auth“ (App Service-Authentifizierung) und fordert ein Ressourcentoken von Azure Cosmos DB mit Lese-/Schreibzugriff für alle Dokumente mit demselben Partitionsschlüssel eines authentifizierten Benutzers an.
* Der Ressourcentokenbroker gibt das Ressourcentoken an die Client-App zurück.
* Die App greift mit dem Ressourcentoken auf die To-Do-Elemente des Benutzers zu.

![To-Do-App mit Beispieldaten](./media/create-documentdb-xamarin-dotnet/tokenbroker.png)
    
## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) in Ihrem Azure Cosmos DB-Konto im linken Navigationsbereich auf **Schlüssel**, und klicken Sie anschließend auf **Lese-/Schreibschlüssel**. Kopieren Sie im nächsten Schritt mithilfe der Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms den URI und den Primärschlüssel in die Datei „web.config“.

    ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal auf dem Blatt „Schlüssel“](./media/create-documentdb-xamarin-dotnet/keys.png)

2. Öffnen Sie in Visual Studio 2017 die Datei „web.config“ aus dem Ordner „azure-documentdb-dotnet/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker“. 

3. Kopieren Sie den URI-Wert aus dem Portal (mithilfe der Schaltfläche zum Kopieren), und legen Sie ihn in „web.config“ als Wert von accountURL fest. 

    `<add key="accountUrl" value="{Azure Cosmos DB account URL}"/>`

4. Kopieren Sie anschließend den Wert für PRIMARY KEY aus dem Portal, und legen Sie ihn in „web.config“ als Wert von AccountKey fest. 

    `<add key="accountKey" value="{Azure Cosmos DB secret}"/>`

Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 

## <a name="build-and-deploy-the-web-app"></a>Erstellen und Bereitstellen der Web-App

1. Erstellen Sie im Azure-Portal eine App Service-Website zum Hosten der Ressourcentokenbroker-API.
2. Öffnen Sie im Azure-Portal das Blatt „App-Einstellungen“ der Website „Ressourcentokenbroker-API“. Legen Sie die folgenden App-Einstellungen fest:

    * accountURL: Die URL des Azure Cosmos DB-Kontos von der Registerkarte „Schlüssel“ in Ihrem Azure Cosmos DB-Konto
    * accountKey: Der Hauptschlüssel des Azure Cosmos DB-Kontos von der Registerkarte „Schlüssel“ in Ihrem Azure Cosmos DB-Konto
    * databaseId und collectionId der von Ihnen erstellten Datenbank und Sammlung

3. Veröffentlichen Sie die Projektmappe „ResourceTokenBroker“ auf der von Ihnen erstellten Website.

4. Öffnen Sie das Xamarin-Projekt, und navigieren Sie zu „TodoItemManager.cs“. Geben Sie Werte für accountURL, collectionId, databaseId sowie für resourceTokenBrokerURL als zugrundeliegende HTTPS-URL für die Ressourcentokenbroker-Website ein.

5. Führen Sie das Tutorial [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Facebook-Anmeldung](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) durch, um die Authentifizierung mit Facebook einzurichten und die ResourceTokenBroker-Website zu konfigurieren.

    Führen Sie die Xamarin-App aus.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese App nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen im Azure-Portal. Führen Sie dazu folgende Schritte durch: 

1. Klicken Sie im Menü links im Azure-Portal auf **Ressourcengruppen** und dann auf den Namen der von Ihnen gerade erstellten Ressource. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto sowie eine Sammlung mit dem Daten-Explorer erstellen und eine Xamarin-App erstellen und bereitstellen können. Jetzt können Sie zusätzliche Daten in Ihr Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Import data into Azure Cosmos DB (Importieren von Daten in Azure Cosmos DB)](import-data.md)

