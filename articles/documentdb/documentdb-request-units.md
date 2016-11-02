<properties 
    pageTitle="Anforderungseinheiten in DocumentDB | Microsoft Azure" 
    description="Enthält Informationen zu den Grundlagen von Anforderungseinheiten in DocumentDB sowie zur Angabe und zur Schätzung des Bedarfs an Anforderungseinheiten." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>


#<a name="request-units-in-documentdb"></a>Anforderungseinheiten in DocumentDB
Jetzt verfügbar: [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner)in DocumentDB. Erfahren Sie mehr unter [Schätzen der Durchsatzanforderungen](documentdb-request-units.md#estimating-throughput-needs).

![Durchsatzrechner][5]

##<a name="introduction"></a>Einführung
Dieser Artikel bietet einen Überblick über Anforderungseinheiten in [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). 

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten:  

-   Was sind Anforderungseinheiten und Anforderungsgebühren?
-   Wie gebe ich die Kapazität der Anforderungseinheiten für eine Sammlung an?
-   Wie schätze ich die benötigten Anforderungseinheiten für meine Anwendung?
-   Was geschieht, wenn ich die Kapazität der Anforderungseinheiten für eine Sammlung überschreite?


##<a name="request-units-and-request-charges"></a>Anforderungseinheiten und Anforderungsgebühren
DocumentDB bietet eine schnelle, vorhersagbare Leistung durch die *Reservierung* von Ressourcen, die dem benötigten Durchsatz für Ihre Anwendung entsprechen.  Da sich Anwendungsauslastung und Zugriffsmuster mit der Zeit ändern, können Sie mit DocumentDB den Umfang des reservierten Durchsatzes, der Ihrer Anwendung zur Verfügung steht, ganz einfach erhöhen oder verringern.

Bei DocumentDB wird der reservierte Durchsatz in Anforderungseinheiten pro Sekunde angegeben.  Sie können sich Anforderungseinheiten als Währung für den Durchsatz vorstellen, wobei Sie eine Anzahl garantierter Anforderungseinheiten auf Sekundenbasis für Ihre Anwendung *reservieren* .  Jeder Vorgang in DocumentDB – das Schreiben eines Dokuments, das Durchführen einer Abfrage, das Aktualisieren eines Dokuments – beansprucht CPU, Arbeitsspeicher und IOPS.  Mit anderen Worten: Für jeden Vorgang fällt eine *Anforderungsgebühr* an, die in *Anforderungseinheiten* ausgedrückt wird.  Wenn Sie die Faktoren, die sich auf die berechneten Anforderungseinheiten auswirken, sowie die Durchsatzanforderungen Ihrer Anwendung genau kennen, können Sie die Kosten für Ihre Anwendung optimieren. 

##<a name="specifying-request-unit-capacity"></a>Angeben der Kapazität der Anforderungseinheiten
Wenn Sie eine DocumentDB-Sammlung erstellen, geben Sie die Anzahl von Anforderungseinheiten pro Sekunde (Request Units, RUs) an, die für die Sammlung reserviert werden sollen.  Sobald die Sammlung erstellt wurde, ist die vollständige Zuweisung von RUs für die Sammlung reserviert.  Für jede Sammlung werden dedizierte und isolierte Durchsatzmerkmale garantiert.  

Beachten Sie, dass DocumentDB mit einem Reservierungsmodell arbeitet. Ihnen wird also der für die Sammlung *reservierte* Durchsatz berechnet, unabhängig davon, wie viel von diesem Durchsatz aktiv *verwendet* wird.  Bedenken Sie jedoch, dass Sie die Menge reservierter RUs über DocumentDB-SDKs oder über das [Azure-Portal](https://portal.azure.com) ganz leicht zentral hoch- oder herunterskalieren können, wenn sich die Auslastung, die Daten und die Nutzungsmuster Ihrer Anwendung verändern.  Weitere Informationen zum zentralen Hoch- und Herunterskalieren des Durchsatzes finden Sie unter [Leistungsebenen in DocumentDB](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Aspekte zu Anforderungseinheiten
Beim Abschätzen der Anzahl von Anforderungseinheiten, die für Ihre DocumentDB-Sammlung reserviert werden soll, sollten Sie unbedingt die folgenden Variablen berücksichtigen:

- **Dokumentgröße**. Je größer ein Dokument, desto mehr Einheiten werden für das Lesen und Schreiben der Daten genutzt.
- **Anzahl der Dokumenteigenschaften**. Eine standardmäßige Indizierung aller Eigenschaften vorausgesetzt, werden mehr Einheiten für das Schreiben eines Dokuments genutzt, wenn die Anzahl der Eigenschaften steigt.
- **Datenkonsistenz**. Bei Verwendung der Datenkonsistenzebenen "Strong" oder "Bounded Staleness" werden zusätzliche Einheiten zum Lesen von Dokumenten genutzt.
- **Indizierte Eigenschaften**. Eine Indexrichtlinie für jede Sammlung gibt an, welche Eigenschaften standardmäßig indiziert werden. Sie können die Nutzung von Anforderungseinheiten reduzieren, indem Sie die Anzahl der indizierten Eigenschaften begrenzen oder die verzögerte Indizierung aktivieren.
- **Dokumentindizierung**. Standardmäßig wird jedes Dokument automatisch indiziert. Sie nutzen weniger Anforderungseinheiten, wenn Sie einige Dokumente nicht indizieren.
- **Abfragemuster**. Die Komplexität einer Abfrage wirkt sich darauf aus, wie viele Anforderungseinheiten für einen Vorgang verbraucht werden. Die Anzahl von Prädikaten, die Art der Prädikate, Projektionen, die Anzahl von UDFs und die Größe des Quelldatasets beeinflussen die Kosten von Abfragevorgängen.
- **Skriptnutzung**.  Wie bei Abfragen beanspruchen gespeicherte Prozeduren und Trigger Anforderungseinheiten basierend auf der Komplexität des ausgeführten Vorgangs. Untersuchen Sie während der Entwicklung Ihrer Anwendung den "x-ms-request-charge"-Header, um herauszufinden, wie viel Anforderungseinheiten die einzelnen Vorgänge verbrauchen.

##<a name="estimating-throughput-needs"></a>Schätzen der Durchsatzanforderungen
Eine Anforderungseinheit ist eine normalisierte Kennzahl für die Anforderungsverarbeitungskosten. Eine einzelne Anforderungseinheit stellt die Verarbeitungskapazität dar, die erforderlich ist, um ein einzelnes, aus 10 eindeutigen Eigenschaftswerten (außer Systemeigenschaften) bestehendes JSON-Dokument von 1 KB zu lesen (per „self link“ oder ID). Eine Anforderung zum Erstellen (Einfügen), Ersetzen oder Löschen des gleichen Dokuments verbraucht mehr Verarbeitungsleistung des Diensts und daher mehr Anforderungseinheiten.   

> [AZURE.NOTE] Die Baseline einer Anforderungseinheit für ein Dokument von 1 KB entspricht einem einfachen GET-Vorgang per „self link“ oder ID des Dokuments.

###<a name="use-the-request-unit-calculator"></a>Verwenden des Rechners für Anforderungseinheiten
Damit Kunden ihre Durchsatzschätzungen optimieren können, gibt es einen webbasierten [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner) , um den Bedarf an Anforderungseinheiten für normale Vorgänge zu schätzen, einschließlich:

- Erstellen von Dokumenten (Schreiben)
- Lesen von Dokumenten
- Löschen von Dokumenten
- Aktualisieren von Dokumenten

Das Tool unterstützt auch die Schätzung des Datenspeicherbedarfs auf der Grundlage der bereitgestellten Beispieldokumente.

Die Verwendung des Tools ist einfach:

1. Laden Sie mindestens ein repräsentatives JSON-Dokument hoch.

    ![Hochladen von Dokumenten in den Rechner für Anforderungseinheiten][2]

2. Geben Sie zum Schätzen des Speicherplatzbedarfs die Gesamtanzahl von Dokumenten ein, die Sie voraussichtlich speichern werden.

3. Geben Sie die pro Sekunde benötige Anzahl von Erstellungs-, Lese-, Aktualisierungs- und Löschvorgängen für Dokumente an. Laden Sie zum Ermitteln der bei Dokumentaktualisierungen voraussichtlich anfallenden Gebühren für Anforderungseinheiten eine Kopie des Beispieldokuments aus Schritt 1 mit typischen Feldaktualisierungen hoch.  Wenn bei Dokumentaktualisierungen also üblicherweise die beiden Eigenschaften „lastLogin“ und „userVisits“ geändert werden, kopieren Sie einfach das Beispieldokument, aktualisieren Sie die Werte für diese beiden Eigenschaften, und laden Sie das kopierte Dokument anschließend hoch.

    ![Eingeben der Durchsatzanforderungen in den Rechner für Anforderungseinheiten][3]

4. Klicken Sie auf „Berechnen“, und prüfen Sie die Ergebnisse.

    ![Ergebnisse des Rechners für Anforderungseinheiten][4]

>[AZURE.NOTE]Wenn sich die Dokumenttypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, laden Sie ein Beispiel für jeden *Typ* eines normalen Dokuments an das Tools hoch, und berechnen Sie dann die Ergebnisse.

###<a name="use-the-documentdb-request-charge-response-header"></a>Verwenden des DocumentDB-Antwortheaders „request-charge“
Jede Antwort des DocumentDB-Diensts enthält einen benutzerdefinierten Header (x-ms-request-charge), der die für die Anforderung verbrauchten Anforderungseinheiten enthält. Auf diesen Header kann auch über die DocumentDB-SDKs zugegriffen werden. Im .Net SDK ist „RequestCharge“ eine Eigenschaft des ResourceResponse-Objekts.  Für Abfragen stellt der DocumentDB-Abfrage-Explorer im Azure-Portal Informationen zu Anforderungsgebühren für ausgeführten Abfragen bereit.

![Untersuchen der berechneten RUs im Abfrage-Explorer][1]

Vor diesem Hintergrund besteht eine Methode zum Abschätzen des von der Anwendung benötigten Durchsatzes darin, typische Vorgänge mit einem repräsentativen, von Ihrer Anwendung verwendeten Dokument auszuführen, dabei die berechneten Anforderungseinheiten zu notieren und anschließend die Anzahl von Vorgängen zu schätzen, die erwartungsgemäß pro Sekunde ausgeführt werden.  Stellen Sie sicher, dass auch typische Abfragen und die Nutzung von DocumentDB-Skripts gemessen und berücksichtigt werden.

>[AZURE.NOTE]Wenn sich die Dokumenttypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, erfassen Sie für jeden typischen Dokumenttyp** jeweils die berechneten Anforderungseinheiten des jeweiligen Vorgangs.

Beispiel:

1. Notieren Sie die berechneten Anforderungseinheiten für das Erstellen (Einfügen) eines typischen Dokuments. 
2. Notieren Sie die berechneten Anforderungseinheiten für das Lesen eines typischen Dokuments.
3. Notieren Sie die berechneten Anforderungseinheiten für das Aktualisieren eines typischen Dokuments.
3. Notieren Sie die berechneten Anforderungseinheiten für typische, häufig ausgeführte Dokumentabfragen.
4. Notieren Sie die berechneten Anforderungseinheiten für benutzerdefinierte Skripts (gespeicherte Prozeduren, Trigger, benutzerdefinierte Funktionen), die von der Anwendung genutzt werden.
5. Berechnen Sie die erforderlichen Anforderungseinheiten anhand der geschätzten Anzahl von Vorgängen, die erwartungsgemäß pro Sekunde ausgeführt werden.

##<a name="a-request-unit-estimation-example"></a>Beispiel für die Schätzung von Anforderungseinheiten
Betrachten Sie das folgende Dokument von 1 KB:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Dokumente werden in DocumentDB minimiert, daher beträgt die vom System berechnete Größe des obigen Dokuments etwas weniger als 1 KB.


Die folgende Tabelle zeigt die ungefähre Anzahl berechneter Anforderungseinheiten für normale Vorgänge für dieses Dokument (bei der ungefähren Anzahl berechneter Anforderungseinheiten wird angenommen, dass die Kontokonsistenzebene auf „Session“ festgelegt ist und dass alle Dokumente automatisch indiziert werden):

Vorgang|Berechnete Anforderungseinheiten 
---|---
Dokument erstellen|~15 RUs 
Dokument lesen|~1 RU
Dokument abfragen nach ID|~2,5 RUs

Diese Tabelle zeigt darüber hinaus die ungefähre Anzahl berechneter Anforderungseinheiten für normale Abfragen, die in der Anwendung verwendet werden:

Abfrage|Berechnete Anforderungseinheiten|# zurückgegebener Dokumente
---|---|--- 
Nahrungsmittel nach ID auswählen|~2,5 RUs|1 
Nahrungsmittel nach Hersteller auswählen|~7 RUs|7
Nach Nahrungsmittelgruppe auswählen und nach Gewicht bestellen|~70 RUs|100
Die 10 beliebtesten Nahrungsmittel in einer Nahrungsmittelgruppe auswählen|~10 RUs|10

>[AZURE.NOTE]RU-Gebühren richten sich nach der Anzahl zurückgegebener Dokumente.

Mit diesen Informationen können wir den RU-Bedarf für diese Anwendung angesichts der Anzahl erwarteter Vorgänge und Abfragen pro Sekunde abschätzen:

Vorgang/Abfrage|Geschätzte Anzahl pro Sekunde|Erforderliche RUs 
---|---|--- 
Dokument erstellen|10|150 
Dokument lesen|100|100 
Nahrungsmittel nach Hersteller auswählen|25|175 
Nach Nahrungsmittelgruppe auswählen|10|700 
10 beliebteste auswählen|15|150 insgesamt|155|1275

In diesem Fall erwarten wir einen durchschnittlichen Durchsatzbedarf von 1,275 RU/s.  Wir runden auf den nächsten Hunderter auf und würden für die Sammlung dieser Anwendung 1.300 RU/s bereitstellen.

##<a name="<a-id="requestratetoolarge"></a>-exceeding-reserved-throughput-limits"></a><a id="RequestRateTooLarge"></a> Überschreiten von Grenzwerten für den reservierten Durchsatz
Der Verbrauch von Anforderungseinheiten wird als Rate pro Sekunde bemessen. Für Anwendungen, die die bereitgestellte Anforderungseinheitsrate für eine Sammlung überschreiten, werden Anforderungen an die Sammlung gedrosselt, bis die Rate unter das reservierte Niveau fällt. Bei einer Drosselung beendet der Server die Anforderung präemptiv mit „RequestRateTooLargeException“ (HTTP-Statuscode 429) und gibt den x-ms-retry-after-ms-Header zurück. Darin ist die Zeitspanne in Millisekunden angegeben, die der Benutzer abwarten muss, bevor ein neuer Anforderungsversuch unternommen werden kann.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Wenn Sie das .NET Client SDK und LINQ-Abfragen verwenden, werden Sie sich normalerweise nicht mit dieser Ausnahme beschäftigen müssen, da die aktuelle Version des .NET Client SDK diese Antwort implizit abfängt, den vom Server angegebenen Retry-After-Header beachtet und die Anforderung wiederholt. Wenn nicht mehrere Clients gleichzeitig auf Ihr Konto zugreifen, wird die nächste Wiederholung erfolgreich ausgeführt.

Wenn mehrere Clients kumulativ oberhalb der Anforderungsrate arbeiten, reicht das Standard-Wiederholungsverhalten möglicherweise nicht aus, und der Client löst für die Anwendung eine DocumentClientException mit dem Statuscode 429 aus. In diesen Fällen sollten Sie in Betracht ziehen, das Wiederholungsverhalten und die zugehörige Logik in die Anwendungsroutinen zur Fehlerbehandlung aufzunehmen oder den reservierten Durchsatz für die Sammlung zu erhöhen.

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum reservierten Durchsatz mit Azure DocumentDB-Datenbanken finden Sie in folgenden Ressourcen:
 
- [DocumentDB-Preise](https://azure.microsoft.com/pricing/details/documentdb/)
- [Verwalten der DocumentDB-Kapazität](documentdb-manage.md) 
- [Modellieren von Daten in DocumentDB](documentdb-modeling-data.md)
- [Leistungsebenen in DocumentDB](documentdb-partition-data.md)

Weitere Informationen zu DocumentDB finden Sie in der Azure DocumentDB- [Dokumentation](https://azure.microsoft.com/documentation/services/documentdb/). 

Im Artikel [Leistungs- und Skalierungstests mit Azure DocumentDB](documentdb-performance-testing.md)finden Sie eine Einführung in Leistungs- und Skalierungstests mit DocumentDB.


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png



<!--HONumber=Oct16_HO2-->


