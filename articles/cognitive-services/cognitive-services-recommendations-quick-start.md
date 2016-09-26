<properties
	pageTitle="Schnellstartanleitung: Machine Learning-Empfehlungs-API | Microsoft Azure"
	description="Azure Machine Learning-Empfehlungen – Erste Schritte"
	services="cognitive-services"
	documentationCenter=""
	authors="luiscabrer"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="cognitive-services"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/22/2016"
	ms.author="luisca"/>

# Kurzanleitung für die Empfehlungs-API der kognitiven Dienste

Dieses Dokument beschreibt, wie Sie Ihren Dienst oder Ihre Anwendung integrieren können, um die [Empfehlungs-API](http://go.microsoft.com/fwlink/?LinkId=759710) zu verwenden. Weitere Informationen zur Empfehlungs-API und anderen kognitiven Diensten finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=759709). Die [Empfehlungs-API-Referenz](http://go.microsoft.com/fwlink/?LinkId=759348) kann Ihnen als praktische Ergänzung zu diesem Handbuch dienen.


<a name="Overview"></a>
## Allgemeine Übersicht

Dieses Dokument ist eine schrittweise Anleitung. Ziel ist es, Ihnen Schritt für Schritt die erforderlichen Schritte zum Trainieren eines Modells vorzustellen und Ihnen Ressourcen aufzuzeigen, mit denen Sie das Modell in Ihrer Produktionsumgebung umsetzen können.

Diese Übung dauert ungefähr 30 Minuten.

Zur Verwendung der [Empfehlungs-API](http://go.microsoft.com/fwlink/?LinkId=759710) müssen Sie die folgenden Schritte ausführen:

1. Erstellen eines Modells – ein Modell ist ein Container für Ihre Nutzungsdaten, Katalogdaten und das Empfehlungsmodell.
1. Importieren von Katalogdaten – Kataloge enthalten Metadateninformationen zu den Artikeln.
1. Importieren von Nutzungsdaten – Nutzungsdaten können auf zwei Arten (oder beide Arten) hochgeladen werden:
  -  Durch Hochladen einer Datei, die die Verwendung von Daten enthält.
  -  Senden von Ereignissen zur Datenerfassung. In der Regel laden Sie eine Nutzungsdatei hoch, damit Sie ein erstes Empfehlungsmodell (bootstrap) erstellen und dieses verwenden können, bis das System über das Datenerfassungsformat ausreichend Daten gesammelt hat.
1. Entwickeln eines Empfehlungsmodells – Dies ist ein asynchroner Vorgang, in dem das Empfehlungssystem anhand aller Nutzungsdaten ein Empfehlungsmodell erstellt. Dieser Vorgang dauert einige Minuten oder mehrere Stunden, je nach Größe der Daten und der Build-Konfigurationsparameter. Beim Auslösen des Builds erhalten Sie eine Build-ID. Überprüfen Sie anhand dieser ID, wann der Buildprozess beendet wurde, bevor Sie beginnen, die Empfehlungen zu nutzen.
1. Empfehlungen nutzen – Sie erhalten Empfehlungen für ein bestimmtes Element oder eine Liste von Elementen.

<a name="GetStarted"></a>
### Erste Schritte

Sie beginnen mit dem Erstellen eines Empfehlungenmodells. Anschließend zeigen wir Ihnen, wie Sie mit den vom Modell generierten Ergebnissen auf einer E-Commerce-Website Empfehlungen anbieten können.

<a name="Ex1Task1"></a>
#### Aufgabe 1 – Registrierung für die Empfehlungs-API ####

In dieser Aufgabe registrieren Sie sich für den Empfehlungs-API-Dienst und erstellen ein Empfehlungenmodell.

1. Rufen Sie das **Azure-Portal** unter [http://portal.azure.com/](http://portal.azure.com/) auf, und melden Sie sich bei Ihrem Azure-Konto an.

1.  Klicken Sie auf **+ Neu**.

1. Wählen Sie die Option **Intelligenz**.

1. Wählen Sie das Produkt **Kognitive Dienste-APIs**. Mit diesem Produkt können Sie ein Abonnement für die Cognitive Services-APIs (Face, Text Analytics, Computer Vision usw.) erstellen. Heute konzentrieren wir uns auf die Empfehlungs-API.

1. Geben Sie auf der Startseite der Cognitive Services-APIs den Kontonamen für Ihr Recommendations-Abonnement ein. (Beispiel: „MyRecommendations“). Dieser Name darf kein Leerzeichen enthalten.

1. Wählen Sie für **API-Typ** **Empfehlungen**.

1. Bei **Tarif** können Sie einen Plan auswählen. Sie können z. B. den **Kostenlosen** Tarif für 10.000 Transaktionen pro Monat** auswählen. Dies ist ein kostenloser Plan, also eine gute Möglichkeit für einen ersten Test des Systems. Sobald Sie in die Produktion gehen, sollten Sie Ihr Anforderungsvolumen abwägen und den Plantyp entsprechend ändern.

1. Wählen Sie eine **Ressourcengruppe**, oder erstellen Sie eine neue – wenn Sie nicht bereits eine besitzen.

1. Sie können andere Elemente im Dialogfeld „Erstellen“ ändern. Wir weisen darauf hin, dass der Ressourcenanbieter derzeit nur von Rechenzentren mit Sitz in den USA unterstützt wird.

1. Klicken Sie nach Abschluss der Auswahl auf **Erstellen**.

1. Warten Sie einige Minuten, bis die Ressource bereitgestellt wird. Sobald sie bereitgestellt ist, gelangen Sie zum Abschnitt **Schlüssel** des Blatts **Einstellungen**, wo Sie einen primären und sekundären Schlüssel zur Verwendung der API erhalten. Kopieren Sie den Primärschlüssel, da Sie ihn zum Erstellen Ihres ersten Modells benötigen.

<a name="Ex1Task2"></a>
#### Aufgabe 2 – Haben Sie Ihre Daten parat? ####

Die Empfehlungs-API lernt aus Ihrem Katalog und Ihren Transaktionen, um gute Produktempfehlungen bereitzustellen. Also müssen Sie sie mit guten Daten zu Ihren Produkten füttern (einer **Katalog**-Datei), und einer Reihe von Transaktionen, die groß genug ist, dass sie darin interessante Verbrauchsmuster findet (**Nutzung**).

1. In der Regel würden Sie Ihre Transaktionsdatenbank nach diesen Informationen abfragen. Nur für den Fall, dass Sie die Daten nicht parat haben, haben wir einige Beispieldaten basierend auf Microsoft Store-Transaktionsdaten für Sie bereitgestellt.

 Sie können die Daten [hier](http://aka.ms/RecoSampleData) herunterladen. Kopieren Sie die Datei „MsStoreData.Zip“ in einen Ordner Ihres lokalen Computers, und entpacken Sie sie dort.

 > **Hinweis:** Der Beispielcode, den Sie herunterladen und in Aufgabe 3 ausführen, enthält bereits eingebettete Beispieldaten. Diese Aufgabe ist also optional. Dies bedeutet, dass diese Aufgabe Ihnen ermöglicht, realistischere Datasets herunterzuladen und die Eingaben in die Empfehlungs-API besser zu verstehen.

1.	Nun werfen wir einen Blick in die Katalogdatei. Navigieren Sie zu dem Speicherort, an den Sie die Daten kopiert haben. Öffnen Sie die Katalogdatei im **Editor**.

 Sie werden feststellen, dass die Katalogdatei ziemlich einfach ist. Sie hat folgendes Format: `<itemid>,<item name>,<product category>`

 >  AAA-04294,OfficeLangPack 2013 32/64 E34 Online DwnLd,Office <br> AAA-04303,OfficeLangPack 2013 32/64 ET Online DwnLd,Office <br> C9F-00168,KRUSELL Kiruna Flip Cover for Nokia Lumia 635 - Camel,Accessories

 Eine Katalogdatei kann natürlich viel umfangreicher sein. So können Sie beispielsweise Metadaten zu den Produkten hinzufügen. (Diese werden von uns als *Artikelfeatures* bezeichnet.) Ausführlichere Informationen zum Katalogformat finden Sie in der API-Referenz im Abschnitt [Katalogformat](http://go.microsoft.com/fwlink/?LinkID=760716).

1. Mit den Nutzungsdaten verfahren wir gleichermaßen. Die Nutzungsdaten haben das Format `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase 0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase 0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase 00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase 0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase 00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

Beachten Sie, dass die ersten drei Elemente zwingend erforderlich sind. Der Ereignistyp ist optional. Weitere Informationen zu diesem Thema finden Sie im Abschnitt zum [Nutzungsformat](http://go.microsoft.com/fwlink/?LinkID=760712).

 > **Wie viele Daten benötigen Sie?**
 <p>
 Nun, das hängt wirklich von den Nutzungsdaten selbst ab. Das System lernt, wenn Benutzer unterschiedliche Artikel kaufen. FBT-Builds beispielsweise müssen wissen, welche Artikel in der gleichen Transaktion gekauft werden. (Wir nennen dies * gemeinsames Auftreten*). Nach einer guten Faustregel sollten die meisten Artikel in mindestens 20 Transaktionen vorkommen – wenn Ihr Katalog also 10.000 Artikel enthält, sollten Sie mindestens über das 20-fache an Transaktionen verfügen, also etwa 200.000. Wie gesagt, dies ist eine Faustregel. Sie müssen mit Ihren Daten experimentieren.
</p>

<a name="Ex1Task3"></a>
#### Aufgabe 3 – Erstellen eines Empfehlungenmodells ####

Nun, da Sie über ein Konto und über Daten verfügen, erstellen wir das erste Modell.

In dieser Aufgabe verwenden Sie die Beispielanwendung zum Erstellen Ihres ersten Modells.

1. Machen Sie sich zunächst mit der [Referenz zur Recommendations-API](http://go.microsoft.com/fwlink/?LinkId=759348) vertraut.

1. Laden Sie die [Beispielanwendung](http://go.microsoft.com/fwlink/?LinkID=759344) in einen lokalen Ordner herunter.

1. Öffnen Sie in Visual Studio die Projektmappe **RecommendationsSample.sln** (befindet sich im Ordner **C#**).

1. Öffnen Sie die Datei **SampleApp.cs**. Beachten Sie die folgenden Schritte in der Datei:
 + Modellerstellung
 + Hinzufügen einer Katalogdatei
 + Hinzufügen einer Nutzungsdatei
 + Auslösen eines Builds für das Modell
 + Abrufen einer Empfehlung basierend auf einem Paar von Artikeln
<p></p>

1. Ersetzen Sie den Wert im Feld **AccountKey** durch den Schlüssel aus Aufgabe 1.

1. Gehen Sie die Lösung durch, und Sie sehen, wie ein Modell erstellt wird.

1. Versuchen Sie, die Katalog- und Nutzungsdateien, die Sie gerade heruntergeladen haben, zu ersetzen, um ein neues Modell für den Microsoft Store oder Buchempfehlungen zu erstellen. Sie müssen ebenfalls den Namen des Modells ändern, sowie die Artikel, für die Sie Empfehlungen anfordern.

1. Wenn das Modell erstellt ist, notieren Sie sich die **Modell-ID**, da Sie sie beim Anfordern von Empfehlungen in Ihrer Produktionsumgebung benötigen.

>  Weitere Informationen zu Buildtypen und zur Bewertung der Qualität von Builds finden Sie [hier](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### Bringen Sie Ihr Modell in Produktion! ###

Da Sie nun ein Modell erstellen und Empfehlungen nutzen können, ist der nächste Schritt, es auf Ihrer Website bzw. mobilen Anwendung in Produktion zu bringen bzw. in Ihr CRM- oder ERP-System zu integrieren. Natürlich wäre jede dieser Implementierungen anders. Da die Empfehlungs-API als Webdienst angefordert wird, sollten Sie sie aus allen diesen verschiedenen Umgebungen einfach abrufen können.

**Wichtig:** Wenn Sie Empfehlungen von einem öffentlichen Client (z. B. Ihrer E-Commerce-Website) aus anzeigen möchten, sollten Sie einen Proxyserver zum Bereitstellen der Empfehlungen erstellen. Dies ist wichtig, damit Ihr API-Schlüssel nicht für externe (potenziell nicht vertrauenswürdige) Entitäten verfügbar wird.

Ihre Empfehlungen könnten Sie z. B. an folgenden Positionen verwenden:

### Produktseite

**Artikelempfehlungen**
<p>Wenn das Modell für Verkaufsdaten trainiert wurde, ermöglicht es Ihrem Kunden, *Produkte zu entdecken, die wahrscheinlich für Personen von Interesse sind, die den Quellartikel gekauft haben*.</p>
<p>Wenn das Modell für Klickdaten trainiert wurde, ermöglicht es Ihrem Kunden, *Produkte zu entdecken, die wahrscheinlich von Personen aufgerufen werden, die den Quellartikel aufgerufen haben*. Dieser Modelltyp kann ähnliche Artikel zurückgeben.</p>

**Empfehlungen im Bereich „Häufig zusammen gekauft“**
<p>Ein „Häufig zusammen gekauft“-Build könnte trainiert werden, damit Sie Gruppen von Artikeln abrufen können, die höchstwahrscheinlich mit diesem Artikel zusammen gekauft werden.</p>

### Auscheckseite

**Artikelempfehlungen**
<p>Ein Empfehlungenmodell könnte eine Artikelliste als Eingabe verwenden. Damit könnten Sie alle Artikel im Warenkorb als Eingabe verwenden, um Empfehlungen abzurufen. In diesem Fall wird das Modell Empfehlungen bereitstellen, die unter Berücksichtigung aller Artikel im Warenkorb von Interesse sind.
</p>

### Angebotsseite

**Benutzerempfehlungen**
<p>
Ein Empfehlungenmodell kann die Benutzer-ID als Eingabe verwenden. So werden dem Benutzer anhand des Verlaufs seiner Transaktionen personalisierte Empfehlungen geboten.
</p>

Lesen Sie die Dokumentation [Get recommendations based on items (I2I)](http://go.microsoft.com/fwlink/?LinkID=760719) (Abrufen von Empfehlungen auf Artikelbasis [I2I]).

<a name="Ex1Task6"></a>
### Wie geht es weiter?
Danke, dass Sie bis zum Schluss dabei waren! Weitere Informationen finden Sie in der vollständigen [Empfehlungen-API-Referenz](http://go.microsoft.com/fwlink/?LinkId=759348). Wenn Sie Fragen haben, zögern Sie nicht, sich unter mlapi@microsoft.com an uns zu wenden.

<!---HONumber=AcomDC_0914_2016-->