<properties
   pageTitle="Registrieren von Daten aus dem Data Lake-Speicher in Azure Data Catalog | Azure"
   description="Registrieren von Daten aus dem Data Lake-Speicher in Azure Data Catalog"
   services="data-lake-store,data-catalog" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# Registrieren von Daten aus dem Data Lake-Speicher in Azure Data Catalog

In diesem Artikel erfahren Sie, wie der Azure Data Lake-Speicher in Azure Data Catalog integriert wird, um Ihre Daten innerhalb einer Organisation auffindbar zu machen, indem sie in Data Catalog integriert werden. Weitere Informationen zum Katalogisieren von Daten finden Sie unter [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md). Informationen zu den Szenarien, in denen Sie Data Catalog verwenden können, finden Sie unter [Häufige Szenarien mit Azure Data Catalog](../data-catalog/data-catalog-common-scenarios.md).

## Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

- **Aktivieren Sie Ihr Azure-Abonnement** für die öffentliche Vorschauversion von Data Lake Store. Weitere Informationen finden Sie in den [Anweisungen](data-lake-store-get-started-portal.md#signup).

- **Azure Data Lake-Speicherkonto**. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit dem Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md) aus. In diesem Tutorial erstellen wir ein Data Lake-Speicherkonto namens **datacatalogstore**.

	Nachdem Sie das Konto erstellt haben, laden Sie ein Beispieldataset in das Konto hoch. In diesem Tutorial laden wir alle CSV-Dateien im Ordner **AmbulanceData** im [Azure Data Lake-Git-Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/) hoch. Sie können verschiedene Clients verwenden, z.B. [Azure-Speicher-Explorer](http://storageexplorer.com/), um Daten in einen Blobcontainer hochzuladen.

- **Azure Data Catalog**. In Ihrer Organisation muss bereits ein Azure Data Catalog für Ihre Organisation erstellt worden sein. Nur ein Katalog ist für jede Organisation zulässig.

## Registrieren des Data Lake-Speichers als Quelle für Data Catalog

>[AZURE.VIDEO adcwithadl] 

1. Navigieren Sie zu `https://azure.microsoft.com/services/data-catalog`, und klicken Sie auf **Erste Schritte**.

2. Melden Sie sich beim Azure Data Catalog-Portal an, und klicken Sie auf **Daten veröffentlichen**.

	![Registrieren einer Datenquelle](./media/data-lake-store-with-data-catalog/register-data-source.png "Registrieren einer Datenquelle")

3. Klicken Sie auf der nächsten Seite auf **Anwendung starten**. Dadurch wird die Manifestdatei der Anwendung auf Ihren Computer heruntergeladen. Doppelklicken Sie auf die Manifestdatei, um die Anwendung zu starten.

4. Klicken Sie auf der Seite „Willkommen“ auf **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein.

	![Bildschirm „Willkommen“](./media/data-lake-store-with-data-catalog/welcome.screen.png "Bildschirm „Willkommen“")

5. Wählen Sie auf der Seite „Datenquelle auswählen“ die Option **Azure Data Lake** aus, und klicken Sie dann auf **Weiter**.

	![Auswählen einer Datenquelle](./media/data-lake-store-with-data-catalog/select-source.png "Auswählen einer Datenquelle")

6. Geben Sie auf der nächsten Seite den Namen des Data Lake-Speicherkontos ein, das Sie in Data Catalog registrieren möchten. Behalten Sie für die anderen Optionen die Standardwerte bei, und klicken Sie dann auf **Verbinden**.

	![Verbinden mit einer Datenquelle](./media/data-lake-store-with-data-catalog/connect-to-source.png "Verbinden mit einer Datenquelle")

7. Die nächste Seite kann in die folgenden Segmente aufgeteilt werden.

	a. Das Feld **Serverhierarchie** stellt die Ordnerstruktur des Data Lake-Speicherkontos dar. **$Root** stellt den Stamm des Data Lake-Speicherkontos dar, und **AmbulanceData** steht für den Ordner, der im Stamm des Data Lake-Speicherkontos erstellt wurde.

	b. Im Feld **Verfügbare Objekte** werden die Dateien und Ordner im Ordner **AmbulanceData** aufgeführt.

	c. Im Feld **Zu registrierende Objekte** werden die Dateien und Ordner aufgeführt, die Sie in Azure Data Catalog registrieren möchten.

	![Anzeigen der Datenstruktur](./media/data-lake-store-with-data-catalog/view-data-structure.png "Anzeigen der Datenstruktur")

8. Für dieses Tutorial sollten Sie alle Dateien im Verzeichnis registrieren. Klicken Sie auf die Schaltfläche ![Verschieben von Objekten](./media/data-lake-store-with-data-catalog/move-objects.png "Verschieben von Objekten"), um alle Dateien in das Feld **Zu registrierende Objekte** zu verschieben.

	Da die Daten in einem organisationsweiten Datenkatalog registriert werden, empfiehlt es sich, einige Metadaten hinzuzufügen, die Sie später verwenden können, um die Daten schnell zu finden. Sie können z.B. eine E-Mail-Adresse für den Besitzer der Daten (z.B. der, der die Daten hochlädt) oder ein Tag zum Identifizieren der Daten hinzufügen. Der Screenshot unten zeigt ein Tag, dass wir den Daten hinzufügen.

	![Anzeigen der Datenstruktur](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Anzeigen der Datenstruktur")

	Klicken Sie auf **Registrieren**.

8. Die folgende Screenshot gibt an, dass die Daten erfolgreich in Data Catalog registriert wurden.

	![Registrierung abgeschlossen](./media/data-lake-store-with-data-catalog/registration-complete.png "Anzeigen der Datenstruktur")

9. Klicken Sie auf **Portal anzeigen**, um zum Data Catalog-Portal zurückzukehren, und stellen Sie sicher, dass Sie jetzt über das Portal auf die registrierten Daten zugreifen können. Um die Daten zu suchen, können Sie das Tag verwenden, das Sie beim Registrieren der Daten angegeben haben.

	![Suchen von Daten im Katalog](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Suchen von Daten im Katalog")

10. Sie können nun Vorgänge wie Hinzufügen von Anmerkungen und Dokumenten zu den Daten ausführen. Weitere Informationen finden Sie unter den folgenden Links.
	* [Hinzufügen von Anmerkungen zu Datenquellen in Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
	* [Hinzufügen von Dokumenten zu Datenquellen in Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)

## Siehe auch

* [Hinzufügen von Anmerkungen zu Datenquellen in Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
* [Hinzufügen von Dokumenten zu Datenquellen in Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)
* [Integrieren des Data Lake-Speichers in andere Azure-Dienste](data-lake-store-integrate-with-other-services.md)

<!---HONumber=AcomDC_0914_2016-->