<properties
	pageTitle="Gewusst wie: Erstellen eines DocumentDB-Kontos | Microsoft Azure"
	description="Erstellen Sie eine NoSQL-Datenbank mit Azure DocumentDB. Folgen Sie diesen Anweisungen, um ein DocumentDB-Konto zu erstellen und mit der Erstellung einer extrem schnellen, globalen NoSQL-Datenbank zu beginnen." 
	keywords="Datenbank erstellen"
	services="documentdb"
	documentationCenter=""
	authors="mimig1"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/16/2016"
	ms.author="mimig"/>

# Gewusst wie: Erstellen eines DocumentDB-Kontos über das Azure-Portal

> [AZURE.SELECTOR]
- [Azure-Portal](documentdb-create-account.md)
- [Azure-Befehlszeilenschnittstelle und ARM](documentdb-automation-resource-manager-cli.md)

Zum Erstellen einer Datenbank mit Microsoft Azure DocumentDB ist Folgendes erforderlich:

- Ein Azure-Konto. Sie können ein [kostenloses Azure-Konto](https://azure.microsoft.com/free) einrichten, falls Sie noch kein Konto besitzen. 
- Erstellen eines DocumentDB-Kontos.  

Sie können ein DocumentDB-Konto entweder mithilfe des Azure-Portals, der Azure Resource Manager-Vorlagen oder der Azure-CLI (Command-Line Interface, Befehlszeilenschnittstelle) erstellen. In diesem Artikel wird beschrieben, wie Sie ein Datenbankkonto über das Azure-Portal erstellen. Informationen zum Erstellen eines Kontos mit Azure-Ressourcen-Manager oder mit der Azure-Befehlszeilenschnittstelle finden Sie unter [Automatisieren der Erstellung von DocumentDB-Datenbankkonten](documentdb-automation-resource-manager-cli.md).

Arbeiten Sie zum ersten Mal mit DocumentDB? Sehen Sie sich [dieses](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) vier Minuten lange Video an, um zu erfahren, wie die häufigsten Aufgaben im Onlineportal ausgeführt werden.

[AZURE.INCLUDE [DocumentDB-Erstellen-DB-Account](../../includes/documentdb-create-dbaccount.md)]

## Nächste Schritte

Nachdem Sie nun über ein DocumentDB-Konto verfügen, besteht der nächste Schritt in der Erstellung der DocumentDB-Datenbank. Mit einer der folgenden Methoden können Sie eine Datenbank erstellen:

- Im Azure-Portal, wie unter [Erstellen einer DocumentDB-Datenbank über das Azure-Portal](documentdb-create-database.md) beschrieben.
- Mit den C# .NET-Beispielen im Projekt [DatabaseManagement](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/DatabaseManagement) des [azure-documentdb-net](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples)-GitHub-Repositorys.
- Die umfassenden Lernprogramme: [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) oder [Python](documentdb-python-application.md).
- Die [DocumentDB-SDKs](documentdb-sdk-dotnet.md). DocumentDB verfügt über SDKs für .NET, Java, Python, Node.js und JavaScript-API.


Nach dem Erstellen der Datenbank müssen Sie der Datenbank [mindestens eine Sammlung hinzufügen](documentdb-create-collection.md) und den Sammlungen dann [Dokumente hinzufügen](documentdb-view-json-document-explorer.md).

Wenn eine Sammlung Dokumente enthält, können Sie in [DocumentDB SQL](documentdb-sql-query.md) an den Dokumenten [Abfragen ausführen](documentdb-sql-query.md#executing-queries), indem Sie den [Abfrage-Explorer](documentdb-query-collections-query-explorer.md) im Portal, die [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder eines der [SDKs](documentdb-sdk-dotnet.md) verwenden.

Weitere Informationen zu DocumentDB finden Sie in folgenden Ressourcen:

-	[Lernpfad für DocumentDB](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
-	[Ressourcenmodell und Konzepte von DocumentDB](documentdb-resources.md)

<!---HONumber=AcomDC_0518_2016-->