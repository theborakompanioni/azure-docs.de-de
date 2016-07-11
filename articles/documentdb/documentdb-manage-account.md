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
	ms.date="06/14/2016"
	ms.author="anhoh"/>

# Verwalten eines DocumentDB-Kontos

Erfahren Sie, wie Sie für global verfügbare Daten die globale Konsistenz sicherstellen und mehrere Regionen verwalten. Erfahren Sie, wie Sie mit Schlüsseln arbeiten und ein Konto im Azure-Portal löschen.

## <a id="consistency"></a>Verwalten von DocumentDB-Konsistenzeinstellungen

Die Auswahl der richtigen Konsistenzebene richtet sich nach der Semantik Ihrer Anwendung. Sie sollten sich mit den Konsistenzebenen in DocumentDB vertraut machen: [Verwenden von Konsistenzebenen zum Maximieren der Verfügbarkeit und Leistung in DocumentDB][consistency]. DocumentDB bietet auf jeder für Ihr Datenbankkonto verfügbare Konsistenzebene bestimmte Garantien für Konsistenz, Verfügbarkeit und Leistung. Wenn Sie Ihr Datenbankkonto mit der Konsistenzebene „Strong“ konfigurieren, sind Ihre Daten auf eine einzige Azure-Region beschränkt und dürfen nicht global verfügbar sein. Auf den gelockerten Konsistenzebenen dagegen – „Bounded Staleness“, „Session“ und „Eventual“ – können Sie Ihrem Datenbankkonto eine beliebige Anzahl von Azure-Regionen zuordnen. Die folgenden einfachen Schritte zeigen Ihnen, wie Sie die Standardkonsistenzebene für Ihr Datenbankkonto auswählen.

### So legen Sie die Standardkonsistenz für ein DocumentDB-Konto fest

1. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf Ihr DocumentDB-Konto zu.
2. Falls auf dem Blatt „Konto“ das Blatt **Einstellungen** nicht bereits geöffnet ist, klicken Sie auf **Alle Einstellungen**. ![Standardkonsistenz - Sitzung][5]

3. Klicken Sie auf dem Blatt **Alle Einstellungen** auf den Eintrag **Standardkonsistenz** unter **Feature**. ![Standardkonsistenz - Sitzung][6]

4. Wählen Sie im Blatt **Standardkonsistenz** die neue Konsistenzebene aus und klicken Sie auf **Speichern**.
5. Sie können den Fortschritt des Vorgangs im Notification Hub des Azure-Portals überwachen.

> [AZURE.NOTE] Beachten Sie, dass es einige Minuten dauern kann, bis sich die Änderung der Standardkonsistenz auf Ihr DocumentDB-Konto auswirkt.

## <a id="addregion"></a>Hinzufügen von Regionen

DocumentDB ist in den meisten [Azure-Regionen][azureregions] verfügbar. Nachdem Sie die Standardkonsistenzebene für Ihr Datenbankkonto ausgewählt haben, können Sie dem Konto eine oder mehrere Regionen zuordnen (je nachdem, welche Konsistenzebene Sie ausgewählt haben und welche Anforderungen an eine globale Verteilung bestehen).

> [AZURE.NOTE] Zurzeit können neue Regionen zu neuen DocumentDB-Konten hinzugefügt werden, die am oder nach dem 13. Juni 2016 erstellt wurden. Wählen Sie im Marketplace die Option „Azure DocumentDB – Datenbankkonto für mehrere Regionen“, um ein Konto für mehrere Regionen zu erstellen. Vor dem 13. Juni erstellte Konten werden in naher Zukunft für die globale Verfügbarkeit aktiviert.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) in der Navigationsleiste auf **DocumentDB-Konten**.
2. Wählen Sie auf dem Blatt **DocumentDB-Konto** das zu ändernde Datenbankkonto aus.
3. Falls auf dem Blatt „Konto“ das Blatt **Alle Einstellungen** nicht bereits geöffnet ist, klicken Sie auf **Alle Einstellungen**.
4. Klicken Sie auf dem Blatt **Alle Einstellungen** auf **Regionen hinzufügen/entfernen**: ![Hinzufügen von Regionen im DocumentDB-Konto > Einstellungen > Regionen hinzufügen/entfernen][1]
5. Wählen Sie auf dem Blatt **Regionen hinzufügen/entfernen** die Regionen aus, die Sie hinzufügen oder entfernen möchten, und klicken Sie auf **OK**. Das Hinzufügen von Regionen ist mit Kosten verbunden – weitere Informationen hierzu finden Sie in der Preisübersicht.

    ![Hinzufügen oder Entfernen von Regionen per Klick auf die Regionen auf der Karte][2]

### Auswählen von Regionen

Wenn Sie zwei oder mehr Regionen konfigurieren, empfiehlt es sich, die Regionen basierend auf den Regionspaaren auszuwählen, die im Artikel [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare][bcdr] beschrieben werden.

Stellen Sie beim Konfigurieren von mehreren Regionen insbesondere sicher, dass Sie für jede Spalte mit einem Regionspaar die gleiche Anzahl von Regionen auswählen (+/-1 für eine ungerade/gerade Anzahl). Wenn Sie also z.B. Daten in vier Regionen in den USA bereitstellen möchten, wählen Sie zwei USA-Regionen aus der linken Spalte und zwei USA-Regionen aus der rechten Spalte. Folgende Auswahl wäre eine richtige Kombination: „USA, Westen“, „USA, Osten“, „USA, Norden-Mitte“, „USA, Süden-Mitte“.

Diese Anweisungen müssen unbedingt befolgt werden, wenn für Szenarien der Notfallwiederherstellung nur zwei Regionen konfiguriert werden. Wenn Sie mehr als zwei Regionen konfigurieren, empfiehlt es sich, diese Anweisungen zu befolgen, dies ist jedoch nicht kritisch, sofern einige der ausgewählten Regionen zum gleichen Regionspaar gehören.

## <a id="selectwriteregion"></a>Auswählen der Schreibregion

Alle Ihrem DocumentDB-Datenbankkonto zugeordneten Regionen können Lesevorgänge (sowohl für Einzelelemente als auch für paginierte Lesevorgänge mit mehreren Elementen) und Abfragevorgänge verarbeiten. Es kann jedoch nur eine Region aktiv Schreibvorgänge („insert“, „upsert“, „replace“, „delete“) empfangen. Um die aktive Schreibregion festzulegen, gehen Sie folgendermaßen vor:


1. Wählen Sie auf dem Blatt **DocumentDB-Konto** das zu ändernde Datenbankkonto aus.
2. Falls auf dem Blatt „Konto“ das Blatt **Alle Einstellungen** nicht bereits geöffnet ist, klicken Sie auf **Alle Einstellungen**.
3. Klicken Sie auf dem Blatt **Alle Einstellungen** auf **Priorität für Schreibregion**. ![Ändern der Schreibregion im DocumentDB-Konto > Einstellungen > Regionen hinzufügen/entfernen][3]
4. Klicken und ziehen Sie Regionen, um die Liste der Regionen zu sortieren. Die erste Region in der Regionsliste ist die aktive Schreibregion. ![Ändern der Schreibregion durch Neusortieren der Regionsliste im DocumentDB-Konto > Einstellungen > Schreibregion ändern][4]

## <a id="keys"></a>Anzeigen, Kopieren und erneutes Generieren von Zugriffsschlüsseln
Wenn Sie ein DocumentDB-Konto erstellen, generiert der Dienst zwei Hauptzugriffsschlüssel, die für die Authentifizierung verwendet werden können, wenn der Zugriff auf das DocumentDB-Konto erfolgt. Durch Bereitstellen von zwei Zugriffsschlüsseln ermöglicht DocumentDB Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Zugriffs auf das DocumentDB-Konto.

Greifen Sie im [Microsoft Azure-Portal](https://portal.azure.com/) auf das Blatt **Schlüssel** der Leiste **Essentials** des Blatts **DocumentDB-Konto** zu, um die für den Zugriff auf Ihr DocumentDB-Konto verwendeten Schlüssel anzuzeigen, zu kopieren und erneut zu generieren.

![Screenshot mit Azure-Portal, Blatt „Schlüssel“](./media/documentdb-manage-account/keys.png)

Alternativ dazu können Sie auf den Eintrag **Schlüssel** auf dem Blatt **Alle Einstellungen** zugreifen.

![Alle Einstellungen, Blatt „Schlüssel“](./media/documentdb-manage-account/allsettingskeys.png)

> [AZURE.NOTE] Das Blatt **Schlüssel** enthält auch die primären und sekundären Verbindungszeichenfolgen, die für die Verbindung des [Datenmigrationstools](documentdb-import-data.md) mit Ihrem Konto verwendet werden können.

Zudem enthält es Nur-Lese-Schlüssel, um Benutzern den schreibgeschützten Zugriff auf DocumentDB zu ermöglichen. Lesevorgänge und Abfragen sind schreibgeschützte Vorgänge, während das Erstellen, Löschen und Ersetzen Schreib- und Lesevorgänge sind.

### Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal

1. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf Ihr DocumentDB-Konto zu.
2. Klicken Sie auf der Leiste **Essentials** des Blatts **DocumentDB-Konto** auf **Schlüssel**.
3. Klicken Sie im Blatt **Schlüssel** rechts neben dem Schlüssel, den Sie kopieren möchten, auf die Schaltfläche **Kopieren**. ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal, Blatt „Schlüssel“](./media/documentdb-manage-account/copykeys.png)

### Erneutes Generieren von Zugriffsschlüsseln

Sie sollten regelmäßig die Zugriffsschlüssel für Ihr DocumentDB-Konto ändern, um dafür zu sorgen, dass Ihre Verbindungen möglichst sicher sind. Zwei Zugriffsschlüssel werden zugewiesen, damit Sie Verbindungen zum DocumentDB-Konto mit einem Zugriffsschlüssel aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren.

> [AZURE.WARNING] Das erneute Generieren der Zugriffsschlüssel wirkt sich auf alle Anwendungen aus, die vom aktuellen Schlüssel abhängen. Alle Clients, die den Zugriffsschlüssel verwenden, um auf das DocumentDB-Konto zuzugreifen, müssen aktualisiert werden, um den neuen Schlüssel zu verwenden.

Falls Sie über Webanwendungen oder Cloud-Dienste verfügen, die das DocumentDB-Konto verwenden, verlieren Sie die Verbindungen beim erneuten Generieren von Schlüsseln – es sei denn, Sie führen einen Rollup für die Schlüssel aus. Die folgenden Schritte stellen den Prozess für das Rollup der Schlüssel dar.

1. Aktualisieren Sie den Zugriffsschlüssel im Anwendungscode, damit er auf den sekundären Zugriffsschlüssel des DocumentDB-Kontos verweist.

2. Generieren Sie den primären Zugriffsschlüssel für Ihr DocumentDB-Konto neu. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf Ihr DocumentDB-Konto zu.

3. Klicken Sie auf der Leiste **Essentials** des Blatts **DocumentDB-Konto** auf **Schlüssel**.

4. Klicken Sie im Blatt **Schlüssel** auf den Befehl **Primären Zugriffsschlüssel neu generieren**, und klicken Sie dann auf **OK**, um das Generieren eines neuen Schlüssels zu bestätigen.

5. Sobald Sie sichergestellt haben, dass der neue Schlüssel verwendet werden kann (etwa 5 Minuten nach der erneuten Erzeugung), aktualisieren Sie den Zugriffsschlüssel im Anwendungscode, damit er auf den neuen primären Zugriffsschlüssel verweist.

6. Generieren Sie den sekundären Zugriffsschlüssel neu.


> [AZURE.NOTE] Die Bereitstellung eines neu generierten Schlüssels kann einige Minuten dauern, bevor Sie damit auf Ihr DocumentDB-Konto zugreifen können.

## <a id="delete"></a>Löschen eines DocumentDB-Kontos
Um ein nicht mehr verwendetes DocumentDB-Konto aus dem Azure-Portal zu entfernen, verwenden Sie den Befehl **Löschen** im Blatt **DocumentDB-Konto**.

![So löschen Sie ein DocumentDB-Konto im Azure-Portal](./media/documentdb-manage-account/deleteaccountconfirmation.png)


1. Greifen Sie im [Azure-Portal](https://portal.azure.com/) auf das zu löschende DocumentDB-Konto zu.
2. Klicken Sie auf dem Blatt **DocumentDB-Konto** auf **Konto löschen**.
3. Geben Sie im daraufhin angezeigten Bestätigungsblatt den Namen des DocumentDB-Kontos ein, um zu bestätigen, dass Sie das Konto löschen möchten.
4. Klicken Sie im Bestätigungsfenster auf die Schaltfläche **Löschen**.

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

<!---HONumber=AcomDC_0629_2016-->