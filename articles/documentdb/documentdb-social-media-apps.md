<properties 
	pageTitle="DocumentDB-Entwurfsmuster: Social Media Apps | Microsoft Azure" 
	description="Erfahren Sie mehr über ein Entwurfsmuster für soziale Netzwerke durch die Nutzung des flexiblen Speichers von DocumentDB und anderen Azure-Diensten." 
	keywords="Social Media Apps"
	services="documentdb" 
	authors="ealsur" 
	manager="" 
	editor="" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/29/2016" 
	ms.author="mimig"/>

# Soziale Netzwerke mit DocumentDB

Da wir in einer hochgradig vernetzten Welt leben, werden wir irgendwann Teil **sozialer Netzwerke**. Wir verwenden diese Netzwerke, um mit Freunden, Kollegen und der Familie in Kontakt zu bleiben oder auch, um das, was uns bewegt, mit Menschen mit den gleichen Interessen zu teilen.

Als Ingenieure oder Entwickler, haben Sie sich möglicherweise gefragt, wie diese Netzwerke unsere Daten speichern und verknüpfen. Möglicherweise wurden Sie sogar bereits mit dem Entwurf und der Erstellung eines neuen sozialen Netzwerks für einen bestimmten Nischenmarkt beauftragt. Spätestens dann stellt sich die große Frage: Wie werden all diese Daten gespeichert?

Nehmen wir an, dass wir ein nagelneues soziales Netzwerk erschaffen, in dem unsere Benutzer Artikel mit den dazugehörigen Medien wie Bilder, Videos oder sogar Musik posten können. Die Benutzer sollen Beiträge darüber hinaus kommentieren und durch Punktvergabe bewerten können. Über einen Feed auf der Hauptseite sollen Benutzer Beiträge lesen und mit ihnen interagieren können. Das klingt (anfänglich) nicht unbedingt komplex, der Einfachheit halber hören wir aber an dieser Stelle auf. (Wir könnten uns weiter mit von Beziehungen beeinflussten, benutzerdefinierten Feeds beschäftigen, aber das geht über das Ziel dieses Artikels hinaus).

Wie und wo speichern wir also die Daten?

Viele von Ihnen haben möglicherweise bereits Erfahrungen mit SQL-Datenbanken gesammelt, oder kennen zumindest das Konzept der [relationalen Datenmodellierung](https://en.wikipedia.org/wiki/Relational_model) und könnten daher versucht sein, so etwas Ähnliches zu zeichnen:

![Diagramm zur Veranschaulichung eines relativen relationalen Modells](./media/documentdb-social-media-apps/social-media-apps-sql.png)

Eine vollkommen normale und ordentliche Datenstruktur – die nicht skalierbar ist.

Nicht falsch verstehen, ich habe mein ganzes Leben lang mit SQL-Datenbanken gearbeitet. Sie sind großartig, aber wie jedes Muster, jede Praxis und Softwareplattform eignen sie sich nicht uneingeschränkt für jedes Szenario.

Warum ist SQL in diesem Szenario nicht die beste Option? Sehen wir uns die Struktur eines einzelnen Beitrag näher an: Wenn wir diesen Beitrag auf einer Website oder in einer Anwendung zeigen möchten, müssten wir... 8 Tabellenjoins (!) abfragen, um einen einzigen Beitrag anzuzeigen. Stellen Sie sich nun einen Stream von Beiträgen vor, die dynamisch geladen werden und auf dem Bildschirm erscheinen. Ahnen Sie, worauf ich hinausmöchte?

Wir könnten für unsere Sache natürlich eine gigantische SQL-Instanz mit ausreichend Leistung, um Tausende von Abfragen mit so vielen Verknüpfungen zu bearbeiten, verwenden. Aber, offen gestanden, warum sollten wir, wenn wir eine einfachere Lösung zur Hand haben?

## Der Weg mit NoSQL

Es gibt spezielle Diagrammdatenbanken, die [in Azure](http://neo4j.com/developer/guide-cloud-deployment/#_windows_azure) ausgeführt werden können, aber wenig kostengünstig sind und darüber hinaus IaaS-Dienste (Infrastructure-as-a-Service, hauptsächlich Virtual Machines) und Wartung erfordern. Dieser Artikel soll eine kosteneffizientere Lösung darstellen, die in den meisten Szenarios funktioniert und auf der Azure-NoSQL-Datenbank [DocumentDB](https://azure.microsoft.com/services/documentdb/) ausgeführt wird. Mithilfe eines [NoSQL](https://en.wikipedia.org/wiki/NoSQL)-Ansatzes, der Datenspeicherung im JSON-Format und der [Denormalisierung](https://en.wikipedia.org/wiki/Denormalization) kann unser zuvor komplizierter Beitrag in ein einziges [Dokument](https://en.wikipedia.org/wiki/Document-oriented_database) transformiert werden:

    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"The first video"},
            {"url":"http://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

Und dies erfolgt mit einer einzigen Abfrage und keinerlei Joins. Diese Lösung ist sehr viel unkomplizierter und erfordert auch von Budgetseite weniger Ressourcen, um ein besseres Ergebnis zu erzielen.

Azure DocumentDB stellt sicher, dass alle Eigenschaften mit der [automatischen Indizierung](documentdb-indexing.md) indiziert werden. Diese kann sogar [benutzerdefiniert](documentdb-indexing-policies.md) angepasst werden. Der schemafreie Ansatz ermöglicht es uns, Dokumente mit verschiedenen und dynamischen Strukturen zu speichern. Wenn den Beiträgen morgen eine Liste von Kategorien oder Hashtags zugeordnet werden soll, wird DocumentDB die neuen Dokumente mit den hinzugefügten Attributen bearbeiten, ohne zusätzlichen Arbeitsaufwand von unserer Seite.

Kommentare zu einem Beitrag können schlicht als weitere Beiträge mit einer übergeordneten Eigenschaft behandelt werden (was unsere Objektzuordnung vereinfacht).

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":User2,
        "parent":"ew12-res2-234e-544f"
    }

    {
        "id":"asd2-fee4-23gc-jh67",
        "title":"Ditto!",
        "date":"2016-01-03",
        "createdBy":User3,
        "parent":"ew12-res2-234e-544f"
    }

Des Weiteren können alle sozialen Interaktionen in einem separaten Objekt als Zähler gespeichert werden:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

Die Erstellung von Feeds ist nunmehr bloß eine Frage der Erstellung von Dokumenten, die eine Liste von Post-IDs mit einer bestimmten Relevanzordnung aufnehmen können:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Wir könnten einen Stream mit den „neuesten Meldungen“ erstellen, in dem Beiträge nach Erstellungsdatum sortiert sind, oder einen Stream der „beliebtesten Beiträge“ mit den meisten Likes in den letzten 24 Stunden. Wir könnten sogar einen benutzerdefinierten Stream für jeden Benutzer einbauen, der auf einer Logik wie „Follower“ und „Interessen“ basiert und trotzdem immer noch eine Liste von Beiträgen wäre. Relevant ist an dieser Stelle, wie diese Listen erstellt werden. Die Leseleistung bleibt davon unberührt. Sobald wir eine dieser Listen erhalten, schicken wir eine einzelne Abfrage an DocumentDB mit dem [IN-Operator](documentdb-sql-query.md#where-clause), um Seiten von Beiträgen gleichzeitig abzurufen.

Die Feed-Datenströme könnten mithilfe von [Azure App Services](https://azure.microsoft.com/services/app-service/)-Hintergrundprozessen erstellt werden: [WebJobs](../app-service-web/web-sites-create-web-jobs.md). Nach der Erstellung eines Beitrags kann die Hintergrundverarbeitung mit [Azure Storage](https://azure.microsoft.com/services/storage/)-[Warteschlangen](../storage/storage-dotnet-how-to-use-queues.md) und WebJobs mithilfe des [Azure WebJobs-SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) ausgelöst werden. Diese Funktionen implementieren die Verteilung der Beiträge innerhalb der Streams, basierend auf unserer eigenen benutzerdefinierten Logik.

Punkte und Likes zu einem Beitrag können mithilfe dieser Technik verzögert verarbeitet werden, um eine letztendlich konsistente Umgebung zu erstellen.

Follower sind komplizierter. In DocumentDB gilt ein Grenzwert für die Dokumentgröße von 512 KB, weshalb Sie ggf. erwägen, Follower als ein Dokument mit dieser Struktur zu speichern:

    {
    	"id":"234d-sd23-rrf2-552d",
    	"followersOf": "dse4-qwe2-ert4-aad2",
    	"followers":[
    		"ewr5-232d-tyrg-iuo2",
    		"qejh-2345-sdf1-ytg5",
    		//...
    		"uie0-4tyg-3456-rwjh"
    	]
    }

Dies mag für einen Benutzer mit ein paar Tausend Followern funktionieren. Doch wenn ein Prominenter in unseren Ranglisten auftaucht, wird bei diesem Ansatz die Größenbeschränkung für Dokumente letztlich erreicht.

Um dieses Problem zu lösen, können wir einen kombinierten Ansatz wählen. Die Anzahl der Follower können wir als Teil des Dokuments mit der Benutzerstatistik speichern:

    {
    	"id":"234d-sd23-rrf2-552d",
    	"user": "dse4-qwe2-ert4-aad2",
    	"followers":55230,
    	"totalPosts":452,
    	"totalPoints":11342
    }

Und das Diagramm der Follower kann in Azure Storage-Tabellen mit einer [Erweiterung](https://github.com/richorama/AzureStorageExtensions#azuregraphstore) gespeichert werden, die ein einfaches Speichern und Abrufen gemäß dem Muster „A folgt B“ zulässt. Auf diese Weise können wir den Prozess des Abrufens der exakten Liste mit den Followern (wenn wir sie brauchen) an Azure Storage-Tabellen delegieren, doch für ein schnelles Nachschlagen von Werten verwenden wir weiter DocumentDB.

## Das „Leiter-Muster“ und Datenduplizierung

Wie Sie vielleicht bemerkt haben, kommt ein Benutzer in dem JSON-Dokument, das auf einen Beitrag verweist, mehrfach vor. Natürlich haben Sie mit der Annahme recht, dass die Informationen, die einen Benutzer darstellen, aufgrund der Denormalisierung an mehr als einem Ort vorhanden sein können.

Um schnellere Abfragen zu ermöglichen, verursachen wir also Datenduplizierung. Das Problem dieses Seiteneffekts besteht darin, dass eine Änderung der Benutzerdaten das Auffinden aller Aktivitäten, die dieser Benutzer je ausgeführt hat, und ihre Aktualisierung notwendig macht. Das klingt nicht besonders praktisch, nicht wahr?

Diagrammdatenbanken lösen das Problem auf eigene Weise. Wir werden es lösen, indem wir die Schlüsselattribute eines Benutzers identifizieren, die wir in unserer Anwendung für jede Aktivität anzeigen. Wenn bei der visuellen Darstellung eines Beitrags in unserer Anwendung nur der Name und das Bild des Erstellers angezeigt wird, warum sollten wir dann alle Daten des Benutzers im createdBy-Attribut speichern? Wenn bei jedem Kommentar allein das Bild des Benutzers angezeigt wird, benötigen wir seine restlichen Informationen eigentlich nicht. Hier kommt das ins Spiel, was ich „Leiter-Muster“ nenne.

Nehmen wir die Benutzerinformationen als Beispiel:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }
    
Anhand dieser Informationen können wir schnell erkennen, welche Informationen wichtig sind und welche nicht. Dadurch können wir sie „leiterhaft“ hierarchisieren:

![Diagramm eines Leiter-Musters](./media/documentdb-social-media-apps/social-media-apps-ladder.png)

Die oberste Ebene ist der sogenannte UserChunk, die minimale Information, die einen Benutzer identifiziert und die für die Datenduplizierung verwendet wird. Durch die Reduzierung der doppelt vorhandenen Daten auf ausschließlich jene Informationen, die wir auch „zeigen“, minimieren wir die Gefahr massiver Aktualisierungen.

Die mittlere Ebene heißt „der Benutzer“. Dabei handelt es sich um die gesamten Daten, die für die meisten leistungsabhängigen Abfragen an DocumentDB verwendet werden: die wichtigsten Daten also, auf die am häufigsten zugegriffen wird. Diese Informationen enthalten auch den UserChunk.

Die umfangreichste Ebene ist der „Erweiterte Benutzer“. Sie enthält alle wichtigen Benutzerinformationen und andere Daten, die nicht schnell gelesen werden müssen oder nur sporadisch verwendet werden (z.B. beim Anmeldeprozess). Diese Daten können außerhalb von DocumentDB, in Azure SQL-Datenbanken oder Azure Storage-Tabellen gespeichert werden.

Warum empfiehlt es sich, die Benutzerinformationen aufzuteilen und sogar an verschiedenen Orten zu speichern? Weil der Speicherplatz in DocumentDB [nicht unbegrenzt](documentdb-limits.md) ist und weil die Abfragen, von der Leistungsseite betrachtet, immer kostenaufwendiger werden, je größer die Dokumente sind. Achten Sie auf schlanke Dokumente, die die entscheidenden Informationen für all Ihre leistungsabhängigen Abfragen für Ihr soziales Netzwerk enthalten. Speichern Sie alle weiteren Informationen für mögliche Szenarios wie eine vollständige Profilbearbeitung, Anmeldungen, sogar für Data Mining für eine Nutzungsanalyse und Big Data-Initiativen. Dabei ist nicht relevant, ob die Datensammlung für das Data Mining langsamer ist, weil es auf einer Azure SQL-Datenbank ausgeführt wird. Was zählt, ist eine schnelle und schlanke Funktionsweise für unsere Benutzer. Ein in DocumentDB gespeicherter Benutzer würde wie folgt aussehen:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"@john"
    }

Und ein Beitrag würde wie folgt aussehen:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
        	"id":"dse4-qwe2-ert4-aad2",
    		"username":"johndoe"
        }
    }

Wenn eine Änderung für eines der Attribute der Chunks anfällt, sind die betroffenen Dokumente mithilfe von Abfragen, die auf die indizierten Attribute verweisen (SELECT * FROM posts p WHERE p.createdBy.id == “edited\_user\_id”), leicht auffindbar und die Chunks ebenso einfach zu aktualisieren.

## Das Suchfeld

Benutzer generieren (glücklicherweise) viel Inhalt. Sie sollten daher auch in der Lage sein, Inhalte zu suchen, die nicht direkt in Ihrem Stream erscheinen, da Sie vielleicht dem jeweiligen Ersteller nicht folgen oder einen eigenen, sechs Monate alten Post suchen.

Dank und aufgrund von Azure DocumentDB können wir mit [Azure Search](https://azure.microsoft.com/services/search/) mühelos binnen weniger Minuten und ohne eine einzige Zeile Code (bis auf den Suchprozess und die Benutzeroberfläche natürlich) eine Suchmaschine implementieren.

Warum ist das so einfach?

Azure Search implementiert sogenannte [Indexer](https://msdn.microsoft.com/library/azure/dn946891.aspx). Das sind Hintergrundprozesse, die sich in Ihre Datenrepositorys einklinken und Ihre Objekte automatisch zu den Indizes hinzufügen, sie aktualisieren oder daraus entfernen . Unterstützt werden [Azure SQL-Datenbank-Indexer](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure-Blobs-Indexer](../search/search-howto-indexing-azure-blob-storage.md) und auch [Azure DocumentDB-Indexer](../documentdb/documentdb-search-indexer.md). Die Übertragung von Informationen von DocumentDB an Azure Search ist unkompliziert, da Informationen in beiden Fällen im JSON-Format gespeichert werden. Wir müssen lediglich [unseren Index erstellen](../search/search-create-index-portal.md) und angeben, welche Attribute aus unseren Dokumenten indiziert werden sollen. Bereits wenige Minuten später (abhängig von der Datengröße) stehen alle unsere Inhalte für die Suche zur Verfügung, für die beste Search-as-a-Service-Lösung in der Cloudinfrastruktur.

Weitere Informationen zu Azure Search finden Sie im [Hitchhiker’s Guide to Search](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/) (Per Anhalter durch Azure Search).

## Die verborgenen Informationen

Nachdem all diese täglich wachsenden Inhalte gespeichert wurden, stellen Sie sich womöglich die Frage: Wie kann ich den Informationsstrom meiner Benutzer verwenden?

Die Antwort ist einfach: Lassen Sie ihn arbeiten und lernen Sie von ihm.

Doch, was können wir von ihm lernen? Einige einfache Beispiele sind die [Stimmmungsanalyse](https://en.wikipedia.org/wiki/Sentiment_analysis), die Empfehlung von Inhalten auf Grundlage der Interessen eines Benutzers oder sogar ein automatischer Inhaltsmoderator, der sicherstellt, dass alle in unserem sozialen Netzwerk veröffentlichten Inhalte familiengeeignet sind.

Nun, da ich Ihr Interesse geweckt habe, glauben Sie wahrscheinlich, dass Sie eine Promotion in Mathematik benötigen, um einfachen Datenbanken und Dateien diese Muster und Informationen zu entlocken. Aber da liegen Sie falsch.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), ein Teil der [Cortana Intelligence Suite](https://www.microsoft.com/en/server-cloud/cortana-analytics-suite/overview.aspx), ist ein vollständig verwalteter Clouddienst, mit dem Sie Workflows mithilfe von Algorithmen in einer einfachen Drag & Drop-Schnittstelle erstellen, Ihre eigenen Algorithmen in [R](https://en.wikipedia.org/wiki/R_(programming_language)) programmieren oder einige der bereits erstellten und einsatzbereiten APIs verwenden können (z.B. [Text Analytics](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [Content Moderator](https://www.microsoft.com/moderator) oder [Recommendations](https://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2)).

Um diese Machine Learning-Szenarien zu realisieren, können wir [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) zum Erfassen der Informationen aus verschiedenen Quellen und [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) verwenden, um die Informationen zu verarbeiten und eine Ausgabe zu generieren, die von Azure Machine Learning verarbeitet werden kann.

## Zusammenfassung

Dieser Artikel soll die alternative, vollständige Erstellung sozialer Netzwerke in Azure beleuchten. Diese bietet kostengünstige Dienste und hervorragende Ergebnisse, da sie die Verwendung einer mehrstufigen Speicherlösung und „Leiter“-Datenverteilung begünstigt.

![Interaktionsdiagramm zwischen Azure-Diensten für soziale Netzwerke](./media/documentdb-social-media-apps/social-media-apps-azure-solution.png)

In Wahrheit gibt es keine Patentlösung für diese Art von Szenarios. Vielmehr sind es die Synergieeffekte aus einer Kombination hervorragender Dienste, die zu einem großartigen Nutzererlebnis verhelfen: die Geschwindigkeit und die Freiheit von Azure DocumentDB, die eine hervorragende soziale Anwendung ermöglichen, die Intelligenz hinter einer erstklassigen Suchlösung wie Azure Search, die Flexibilität von Azure App Services, nicht nur sprachunabhängige Anwendungen zu hosten, sondern sogar leistungsfähige Hintergrundprozesse. Dazu kommen der erweiterbare Azure-Speicher, die Azure SQL-Datenbank zum Speichern riesiger Datenmengen und die analytische Leistungsfähigkeit von Azure Machine Learning zur Gewinnung von Wissen und Intelligenz, was als Feedback für unsere Prozesse dienen und uns helfen kann, den richtigen Benutzern die richtigen Inhalte zur Verfügung zu stellen.

## Nächste Schritte

Weitere Informationen zur Datenmodellierung finden Sie im Artikel [Modellieren von Daten in DocumentDB](documentdb-modeling-data.md). Wenn Sie an weiteren Anwendungsmöglichkeiten für DocumentDB interessiert sind, lesen Sie [Häufige Anwendungsfälle für DocumentDB](documentdb-use-cases.md).

Oder erfahren Sie mehr über DocumentDB anhand des [DocumentDB-Lernpfads](https://azure.microsoft.com/documentation/learning-paths/documentdb/).

<!---HONumber=AcomDC_0706_2016-->