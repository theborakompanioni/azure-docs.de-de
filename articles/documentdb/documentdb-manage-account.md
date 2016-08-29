<properties
	pageTitle="Verwalten eines DocumentDB-Kontos über das Azure-Portal | Microsoft Azure"
	description="Erfahren Sie, wie Sie Ihr DocumentDB-Konto über das Azure-Portal verwalten. Sie erhalten eine Anleitung, wie das Azure-Portal verwendet werden kann, um Konten anzuzeigen, zu kopieren, zu löschen und um auf Konten zuzugreifen."
	keywords="Azure-Portal, DocumentDB, Azure, Microsoft Azure"
	services="documentdb"
	documentationCenter=""
	authors="AndrewHoh"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/15/2016"
	ms.author="anhoh"/>

# Verwalten eines DocumentDB-Kontos

Erfahren Sie, wie Sie globale Konsistenz festlegen, mit Schlüsseln arbeiten und ein DocumentDB-Konto im Azure-Portal löschen.

## <a id="consistency"></a>Verwalten von DocumentDB-Konsistenzeinstellungen

Die Auswahl der richtigen Konsistenzebene richtet sich nach der Semantik Ihrer Anwendung. Sie sollten sich mit den Konsistenzebenen in DocumentDB vertraut machen: [Verwenden von Konsistenzebenen zum Maximieren der Verfügbarkeit und Leistung in DocumentDB][consistency]. DocumentDB bietet auf jeder für Ihr Datenbankkonto verfügbare Konsistenzebene bestimmte Garantien für Konsistenz, Verfügbarkeit und Leistung. Wenn Sie Ihr Datenbankkonto mit der Konsistenzebene „Strong“ konfigurieren, sind Ihre Daten auf eine einzige Azure-Region beschränkt und dürfen nicht global verfügbar sein. Auf den gelockerten Konsistenzebenen dagegen – „Bounded Staleness“, „Session“ und „Eventual“ – können Sie Ihrem Datenbankkonto eine beliebige Anzahl von Azure-Regionen zuordnen. Die folgenden einfachen Schritte zeigen Ihnen, wie Sie die Standardkonsistenzebene für Ihr Datenbankkonto auswählen.

### So legen Sie die Standardkonsistenz für ein DocumentDB-Konto fest

1. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf Ihr DocumentDB-Konto zu.
2. Klicken Sie auf dem Kontoblatt auf **Standardkonsistenz**.
3. Wählen Sie auf dem Blatt **Standardkonsistenz** die neue Konsistenzebene aus, und klicken Sie auf **OK**. 
![Standardkonsistenz - Sitzung][5]

## <a id="keys"></a>Anzeigen, Kopieren und erneutes Generieren von Zugriffsschlüsseln
Wenn Sie ein DocumentDB-Konto erstellen, generiert der Dienst zwei Hauptzugriffsschlüssel, die für die Authentifizierung verwendet werden können, wenn der Zugriff auf das DocumentDB-Konto erfolgt. Durch Bereitstellen von zwei Zugriffsschlüsseln ermöglicht DocumentDB Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Zugriffs auf das DocumentDB-Konto.

Greifen Sie im [Microsoft Azure-Portal](https://portal.azure.com/) über das Blatt **DocumentDB-Konto** auf das Blatt **Schlüssel** zu, um die für den Zugriff auf Ihr DocumentDB-Konto verwendeten Schlüssel anzuzeigen, zu kopieren und neu zu generieren.

![Screenshot mit Azure-Portal, Blatt „Schlüssel“](./media/documentdb-manage-account/keys.png)

> [AZURE.NOTE] Das Blatt **Schlüssel** enthält auch die primären und sekundären Verbindungszeichenfolgen, die für die Verbindung des [Datenmigrationstools](documentdb-import-data.md) mit Ihrem Konto verwendet werden können.

Auf diesem Blatt stehen außerdem schreibgeschützte Schlüssel zur Verfügung. Lesevorgänge und Abfragen sind schreibgeschützte Vorgänge, während das Erstellen, Löschen und Ersetzen Schreib- und Lesevorgänge sind.

### Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal

Klicken Sie im Blatt **Schlüssel** rechts neben dem Schlüssel, den Sie kopieren möchten, auf die Schaltfläche **Kopieren**.

![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal, Blatt „Schlüssel“](./media/documentdb-manage-account/copykeys.png)

### Erneutes Generieren von Zugriffsschlüsseln

Sie sollten regelmäßig die Zugriffsschlüssel für Ihr DocumentDB-Konto ändern, um dafür zu sorgen, dass Ihre Verbindungen möglichst sicher sind. Zwei Zugriffsschlüssel werden zugewiesen, damit Sie Verbindungen zum DocumentDB-Konto mit einem Zugriffsschlüssel aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren.

> [AZURE.WARNING] Das erneute Generieren der Zugriffsschlüssel wirkt sich auf alle Anwendungen aus, die vom aktuellen Schlüssel abhängen. Alle Clients, die den Zugriffsschlüssel verwenden, um auf das DocumentDB-Konto zuzugreifen, müssen aktualisiert werden, um den neuen Schlüssel zu verwenden.

Falls Sie über Webanwendungen oder Cloud-Dienste verfügen, die das DocumentDB-Konto verwenden, verlieren Sie die Verbindungen beim erneuten Generieren von Schlüsseln – es sei denn, Sie führen einen Rollup für die Schlüssel aus. Die folgenden Schritte stellen den Prozess für das Rollup der Schlüssel dar.

1. Aktualisieren Sie den Zugriffsschlüssel im Anwendungscode, damit er auf den sekundären Zugriffsschlüssel des DocumentDB-Kontos verweist.
2. Generieren Sie den primären Zugriffsschlüssel für Ihr DocumentDB-Konto neu. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf Ihr DocumentDB-Konto zu.
3. Klicken Sie auf dem Blatt **DocumentDB-Konto** auf **Schlüssel**.
4. Klicken Sie auf dem Blatt **Schlüssel** auf den Befehl **Primären Zugriffsschlüssel neu generieren**, und klicken Sie dann auf **OK**, um das Generieren eines neuen Schlüssels zu bestätigen. ![Erneutes Generieren von Zugriffsschlüsseln](./media/documentdb-manage-account/regenerate-keys.png)

5. Sobald Sie sichergestellt haben, dass der neue Schlüssel verwendet werden kann (etwa 5 Minuten nach der erneuten Erzeugung), aktualisieren Sie den Zugriffsschlüssel im Anwendungscode, damit er auf den neuen primären Zugriffsschlüssel verweist.
6. Generieren Sie den sekundären Zugriffsschlüssel neu. 
![Erneutes Generieren von Zugriffsschlüsseln](./media/documentdb-manage-account/regenerate-secondary-key.png)


> [AZURE.NOTE] Die Bereitstellung eines neu generierten Schlüssels kann einige Minuten dauern, bevor Sie damit auf Ihr DocumentDB-Konto zugreifen können.

## <a id="delete"></a>Löschen eines DocumentDB-Kontos
Um ein nicht mehr verwendetes DocumentDB-Konto aus dem Azure-Portal zu entfernen, verwenden Sie den Befehl **Löschen** auf dem Blatt **DocumentDB-Konto**.

![So löschen Sie ein DocumentDB-Konto im Azure-Portal](./media/documentdb-manage-account/deleteaccount.png)


1. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf das zu löschende DocumentDB-Konto zu.
2. Klicken Sie auf dem Blatt **DocumentDB-Konto** auf **Mehr**, und klicken Sie dann auf **Konto löschen**. Alternativ können Sie mit der rechten Maustaste auf den Namen der Datenbank und dann auf **Konto löschen** klicken.
3. Geben Sie im daraufhin angezeigten Bestätigungsblatt den Namen des DocumentDB-Kontos ein, um zu bestätigen, dass Sie das Konto löschen möchten.
4. Klicken Sie auf die Schaltfläche **Löschen**.

![So löschen Sie ein DocumentDB-Konto im Azure-Portal](./media/documentdb-manage-account/delete-account-confirm.png)

## <a id="next"></a>Nächste Schritte

Erfahren Sie mehr auf der Seite [Erste Schritte mit dem DocumentDB .NET SDK](http://go.microsoft.com/fwlink/p/?LinkId=402364).

Weitere Informationen zu DocumentDB finden Sie in der Azure DocumentDB-Dokumentation auf [azure.com](http://go.microsoft.com/fwlink/?LinkID=402319&clcid=0x409).


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
[azureregions]: https://azure.microsoft.com/de-DE/regions/#services
[offers]: https://azure.microsoft.com/de-DE/pricing/details/documentdb/

<!---HONumber=AcomDC_0817_2016-->