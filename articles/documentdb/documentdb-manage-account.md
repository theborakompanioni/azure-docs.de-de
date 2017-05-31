---
title: "Verwalten eines Azure Cosmos DB-Kontos über das Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihr Azure Cosmos DB-Konto über das Azure-Portal verwalten. Sie erhalten eine Anleitung, wie das Azure-Portal verwendet werden kann, um Konten anzuzeigen, zu kopieren, zu löschen und um auf Konten zuzugreifen."
keywords: Azure-Portal, DocumentDB, Azure, Microsoft Azure
services: cosmosdb
documentationcenter: 
author: kirillg
manager: jhubbard
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: kirillg
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 59c2a0fb05809233cf789feb81189a24a18484b9
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a>Verwalten eines Azure Cosmos DB-Kontos
Erfahren Sie, wie Sie globale Konsistenz festlegen, mit Schlüsseln arbeiten und ein Azure Cosmos DB-Konto im Azure-Portal löschen.

## <a id="consistency"></a>Verwalten von Konsistenzeinstellungen in Azure Cosmos DB
Die Auswahl der richtigen Konsistenzebene richtet sich nach der Semantik Ihrer Anwendung. Sie sollten sich mit den Konsistenzebenen in Azure Cosmos DB vertraut machen. Lesen Sie daher [Verwenden von Konsistenzebenen zum Maximieren der Verfügbarkeit und Leistung in Azure Cosmos DB][consistency]. Azure Cosmos DB bietet auf jeder für Ihr Datenbankkonto verfügbaren Konsistenzebene bestimmte Garantien für Konsistenz, Verfügbarkeit und Leistung. Wenn Sie Ihr Datenbankkonto mit der Konsistenzebene „Strong“ konfigurieren, sind Ihre Daten auf eine einzige Azure-Region beschränkt und dürfen nicht global verfügbar sein. Auf den gelockerten Konsistenzebenen dagegen – „Bounded Staleness“, „Session“ und „Eventual“ – können Sie Ihrem Datenbankkonto eine beliebige Anzahl von Azure-Regionen zuordnen. Die folgenden einfachen Schritte zeigen Ihnen, wie Sie die Standardkonsistenzebene für Ihr Datenbankkonto auswählen. 

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a>So legen Sie die Standardkonsistenz für ein Azure Cosmos DB-Konto fest
1. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf Ihr Azure Cosmos DB-Konto zu.
2. Klicken Sie auf dem Kontoblatt auf **Standardkonsistenz**.
3. Wählen Sie im Blatt **Standardkonsistenz** die neue Konsistenzebene aus und klicken Sie auf **Speichern**.
    ![Standardkonsistenz - Sitzung][5]

## <a id="keys"></a>Anzeigen, Kopieren und erneutes Generieren von Zugriffsschlüsseln
Wenn Sie ein Azure Cosmos DB-Konto erstellen, generiert der Dienst zwei Hauptzugriffsschlüssel, die für die Authentifizierung verwendet werden können, wenn der Zugriff auf das Azure Cosmos DB-Konto erfolgt. Durch Bereitstellen von zwei Zugriffsschlüsseln ermöglicht Azure Cosmos DB Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Zugriffs auf das Azure Cosmos DB-Konto. 

Greifen Sie im [Azure-Portal](https://portal.azure.com/) über das Ressourcenmenü im Blatt **Azure Cosmos DB-Konto** auf das Blatt **Schlüssel** zu, um die für den Zugriff auf Ihr Azure Cosmos DB-Konto verwendeten Schlüssel anzuzeigen, zu kopieren und neu zu generieren.

![Screenshot mit Azure-Portal, Blatt „Schlüssel“](./media/documentdb-manage-account/keys.png)

> [!NOTE]
> Das Blatt **Schlüssel** enthält auch die primären und sekundären Verbindungszeichenfolgen, die für die Verbindung des [Datenmigrationstools](documentdb-import-data.md)mit Ihrem Konto verwendet werden können.
> 
> 

Auf diesem Blatt stehen außerdem schreibgeschützte Schlüssel zur Verfügung. Lesevorgänge und Abfragen sind schreibgeschützte Vorgänge, während das Erstellen, Löschen und Ersetzen Schreib- und Lesevorgänge sind.

### <a name="copy-an-access-key-in-the-azure-portal"></a>Kopieren eines Zugriffsschlüssels im Azure-Portal
Klicken Sie auf dem Blatt **Schlüssel** rechts neben dem Schlüssel, den Sie kopieren möchten, auf die Schaltfläche **Kopieren**.

![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal, Blatt „Schlüssel“](./media/documentdb-manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a>Erneutes Generieren von Zugriffsschlüsseln
Sie sollten regelmäßig die Zugriffsschlüssel für Ihr Azure Cosmos DB-Konto ändern, um dafür zu sorgen, dass Ihre Verbindungen möglichst sicher sind. Zwei Zugriffsschlüssel werden zugewiesen, damit Sie Verbindungen zum Azure Cosmos DB-Konto mit einem Zugriffsschlüssel aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren.

> [!WARNING]
> Das erneute Generieren der Zugriffsschlüssel wirkt sich auf alle Anwendungen aus, die vom aktuellen Schlüssel abhängen. Alle Clients, die den Zugriffsschlüssel verwenden, um auf das Azure Cosmos DB-Konto zuzugreifen, müssen aktualisiert werden, um den neuen Schlüssel zu verwenden.
> 
> 

Falls Sie über Webanwendungen oder Cloud-Dienste verfügen, die das Azure Cosmos DB-Konto verwenden, verlieren Sie die Verbindungen beim erneuten Generieren von Schlüsseln – es sei denn, Sie führen einen Rollup für die Schlüssel aus. Die folgenden Schritte stellen den Prozess für das Rollup der Schlüssel dar.

1. Aktualisieren Sie den Zugriffsschlüssel im Anwendungscode, damit er auf den sekundären Zugriffsschlüssel des Azure Cosmos DB-Kontos verweist.
2. Generieren Sie den primären Zugriffsschlüssel für Ihr Azure Cosmos DB-Konto neu. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf Ihr Azure Cosmos DB-Konto zu.
3. Klicken Sie auf dem Blatt **Azure Cosmos DB-Konto** auf **Schlüssel**.
4. Klicken Sie auf dem Blatt **Schlüssel** auf die Regenerierungsschaltfläche, und klicken Sie dann auf **OK**, um das Generieren eines neuen Schlüssels zu bestätigen.
    ![Erneutes Generieren von Zugriffsschlüsseln](./media/documentdb-manage-account/regenerate-keys.png)
5. Sobald Sie sichergestellt haben, dass der neue Schlüssel verwendet werden kann (etwa 5 Minuten nach der erneuten Erzeugung), aktualisieren Sie den Zugriffsschlüssel im Anwendungscode, damit er auf den neuen primären Zugriffsschlüssel verweist.
6. Generieren Sie den sekundären Zugriffsschlüssel neu.
   
    ![Erneutes Generieren von Zugriffsschlüsseln](./media/documentdb-manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Die Bereitstellung eines neu generierten Schlüssels kann einige Minuten dauern, bevor Sie damit auf Ihr Azure Cosmos DB-Konto zugreifen können.
> 
> 

## <a name="get-the--connection-string"></a>Verbindungszeichenfolge abrufen
Führen Sie zum Abrufen der Verbindungszeichenfolge folgende Schritte aus: 

1. Greifen Sie im [Azure-Portal](https://portal.azure.com) auf Ihr Azure Cosmos DB-Konto zu.
2. Klicken Sie im Ressourcenmenü auf **Schlüssel**.
3. Klicken Sie neben dem Feld für die **primäre Verbindungszeichenfolge** oder **sekundäre Verbindungszeichenfolge** auf die Schaltfläche **Kopieren**. 

Bei Verwendung der Verbindungszeichenfolge im [Azure Cosmos DB-Datenbank-Migrationstool](documentdb-import-data.md) fügen Sie den Datenbanknamen am Ende der Verbindungszeichenfolge an. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a>: Löschen eines Azure Cosmos DB-Kontos
Klicken Sie zum Entfernen eines nicht mehr verwendeten Azure Cosmos DB-Kontos aus dem Azure-Portal mit der rechten Maustaste auf den Kontonamen, und klicken Sie dann auf **Konto löschen**.

![Löschen eines Azure Cosmos DB-Kontos im Azure-Portal](./media/documentdb-manage-account/deleteaccount.png)

1. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf das zu löschende Azure Cosmos DB-Konto zu.
2. Klicken Sie auf dem Blatt **Azure Cosmos DB-Konto** mit der rechten Maustaste auf das Konto, und klicken Sie dann auf **Konto löschen**. 
3. Geben Sie im daraufhin angezeigten Bestätigungsblatt den Namen des Azure Cosmos DB-Kontos ein, um zu bestätigen, dass Sie das Konto löschen möchten.
4. Klicken Sie auf die Schaltfläche **Löschen** .

![Löschen eines Azure Cosmos DB-Kontos im Azure-Portal](./media/documentdb-manage-account/delete-account-confirm.png)

## <a id="next"></a>Nächste Schritte
Erfahren Sie mehr auf der Seite [Erste Schritte mit dem Azure Cosmos DB-Konto](http://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[1]: ./media/documentdb-manage-account/documentdb_add_region-1.png
[2]: ./media/documentdb-manage-account/documentdb_add_region-2.png
[3]: ./media/documentdb-manage-account/documentdb_change_write_region-1.png
[4]: ./media/documentdb-manage-account/documentdb_change_write_region-2.png
[5]: ./media/documentdb-manage-account/documentdb_change_consistency-1.png
[6]: ./media/documentdb-manage-account/chooseandsaveconsistency.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/documentdb/

