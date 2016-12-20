---
title: Leistungsebenen in DocumentDB | Microsoft Docs
description: "Erfahren Sie, wie Sie mithilfe von Leistungsebenen in DocumentDB den Durchsatz pro Sammlung reservieren können."
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: c2bdd3adc9f22005dbaca971472bdc65fead1a76
ms.openlocfilehash: a6e415f6b8f2203209c1cfe3de37193fe18ce2b4


---
# <a name="performance-levels-and-pricing-tiers-in-documentdb"></a>Leistungsstufen und Tarife in DocumentDB
Dieser Artikel bietet einen Überblick über die Leistungsebenen in [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/).

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten:  

* Was ist eine Leistungsebene?
* Wie lässt sich der Durchsatz für ein Datenbankkonto reservieren?
* Wie arbeite ich mit Leistungsebenen?
* Wie werden Leistungsebenen abgerechnet?

## <a name="introduction-to-performance-levels"></a>Einführung in Leistungsebenen
Jede DocumentDB-Sammlung, die in einem DocumentDB-Standardkonto erstellt wird, wird mit einer zugewiesenen Leistungsebene bereitgestellt. Jede Sammlung in einer Datenbank kann über individuelle Leistungsebenen verfügen. So können Sie häufig genutzten Sammlungen mehr Durchsatz und seltener genutzten Sammlungen weniger Durchsatz zuweisen. 

DocumentDB unterstützt sowohl **benutzerdefinierte** als auch **vordefinierte** Leistungsebenen, wie Sie der folgenden Tabelle entnehmen können.  Mit benutzerdefinierter Leistung haben Sie Zugang zu reserviertem Durchsatz in Einheiten von 100 RU/s und Zugriff auf unbegrenzten Speicher. Bei den drei vordefinierten Leistungsebenen stehen bestimmte Durchsatzoptionen und ein Speicherkontingent von 10 GB zur Verfügung. Die folgende Tabelle enthält eine Gegenüberstellung von **benutzerdefinierter** und **vordefinierter** Leistung.

|Leistungstyp|Details|Durchsatz|Speicher|Version|APIs|
|----------------|-------|----------|-------|-------|----|
|Benutzerdefinierte Leistung|Durchsatz in Einheiten von 100 RU/s (vom Benutzer festgelegt)|Unbegrenzt. <br><br>standardmäßig 400 - 250.000 RU/s (höher auf Anforderung)|Unbegrenzt. <br><br>standardmäßig 250 GB (höher auf Anforderung)|V2|API 2015-12-16 und neuer|
|Vordefinierte Leistung|10 GB reservierter Speicher.<br><br>S1 = 250 RU/s<br>S2 = 1000 RU/s<br>S3 = 2500 RU/s|2.500 RU/s|10 GB|V1|Beliebig|

Der Durchsatz ist für jede Sammlung reserviert und kann nur für diese Sammlung verwendet werden. Durchsatz wird in [Anforderungseinheiten (Request Unit, RU)](documentdb-request-units.md) gemessen, die den Ressourcenverbrauch verschiedener DocumentDB-Datenbankvorgänge bezeichnen.

> [!NOTE]
> Die Leistungsebene einer Sammlung kann über die [SDKs](documentdb-sdk-dotnet.md) oder das [Azure-Portal](https://portal.azure.com/) angepasst werden. Änderungen der Leistungsstufe sollten innerhalb von drei Minuten abgeschlossen sein.
> 
> 

## <a name="setting-performance-levels-for-collections"></a>Festlegen von Leistungsebenen für Sammlungen
Sobald eine Sammlung erstellt wurde, wird die vollständige Zuweisung von RUs basierend auf der angegebenen Leistungsebene für die Sammlung reserviert.

Beachten Sie, dass DocumentDB sowohl über benutzerdefinierte als auch über vordefinierte Leistungsebenen verfügt und basierend auf dem reservierten Durchsatz betrieben wird. Durch Erstellen einer Sammlung reserviert eine Anwendung Durchsatz, welcher entsprechend in Rechnung gestellt wird. Dies geschieht unabhängig davon, wie viel von diesem Durchsatz aktiv genutzt wird. Der Speicher wird bei benutzerdefinierten Leistungsebenen auf Basis des Verbrauchs gemessen. Bei vordefinierten Leistungsebenen hingegen werden im Moment der Erstellung der Sammlung 10 GB Speicherplatz reserviert.  

Nach dem Erstellen einer Sammlung können Sie die Leistungsebene und/oder den Durchsatz über die [SDKs](documentdb-sdk-dotnet.md) oder das [Azure-Portal](https://portal.azure.com/) anpassen.

> [!IMPORTANT]
> DocumentDB-Standardsammlungen werden auf Stundenbasis abgerechnet. Hierbei wird für jede erstellte Sammlung eine Verwendung von mindestens einer Stunde berechnet.
> 
> 

Wenn Sie die Leistungsebene einer Sammlung innerhalb einer Stunde anpassen, wird die höchste, während dieser Stunde festgelegte Leistungsebene berechnet. Beispiel: Wenn Sie die Leistungsebene für eine Sammlung um 8:53 Uhr erhöhen, wird die neue Ebene ab 8:00 Uhr berechnet. Wenn Sie die Leistungsebene um 8:53 Uhr verringern, gilt der neue Satz ab 9:00 Uhr.

Anforderungseinheiten werden für jede Sammlung basierend auf der Leistungsebene reserviert. Der Verbrauch von Anforderungseinheiten wird als Pro-Sekunde-Rate bemessen. Anwendungen, die die bereitgestellte Anforderungseinheitsrate (oder Leistungsebene) einer Sammlung überschreiten, werden gedrosselt, bis die Rate unter das für die Sammlung reservierte Niveau fällt. Wenn für Ihre Anwendung ein höherer Durchsatz erforderlich ist, können Sie die Leistungsebene für jede Sammlung erhöhen.

> [!NOTE]
> Wenn Ihre Anwendung die Leistungsebene für eine oder mehrere Sammlungen überschreitet, werden die Anforderungen pro Sammlung beschränkt. Dies bedeutet, dass einige Anforderungen erfolgreich sind, während andere aufgrund der Beschränkung fehlschlagen. Es wird empfohlen, bei einer Beschränkung eine kleine Anzahl von Wiederholungen hinzuzufügen, um die Spitzen im Datenverkehr zu bewältigen.
> 
> 

## <a name="working-with-performance-levels"></a>Arbeiten mit Leistungsebenen
Mit DocumentDB-Sammlungen können Sie Ihre Daten basierend auf den Abfragemustern und Leistungsanforderungen Ihrer Anwendung gruppieren. Aufgrund der DocumentDB-Unterstützung für automatische Indizierung und Abfragen ist es üblich, heterogene Dokumente in derselben Sammlung zusammenzustellen. Die wichtigsten Überlegungen bei der Entscheidung für oder gegen die Verwendung separater Sammlungen umfassen:

* Abfragen – eine Sammlung ist der Bereich für die Abfrageausführung. Wenn Sie eine Abfrage über eine Reihe von Dokumenten hinweg durchführen müssen, sind Lesevorgänge effizienter, wenn diese Dokumente in einer einzigen Sammlung zusammengestellt werden.
* Transaktionen – Alle Transaktionen sind auf den Inhalt einer Sammlung beschränkt. Wenn einige Ihrer Dokumente innerhalb einer einzelnen, gespeicherten Prozedur oder eines Triggers aktualisiert werden müssen, müssen sie in der gleichen Sammlung gespeichert werden. Genauer gesagt ist ein Partitionsschlüssel innerhalb einer Sammlung die Transaktionsgrenze. Detaillierte Ausführungen finden Sie unter [Partitionieren in DocumentDB](documentdb-partition-data.md) .
* Leistungsisolation – Eine Sammlung verfügt über eine zugewiesene Leistungsebene. So wird sichergestellt, dass jede Sammlung eine vorhersagbare Leistung durch reservierte RUs erbringt. Daten können verschiedenen Sammlungen mit unterschiedlichen Leistungsebenen (basierend auf der Zugriffshäufigkeit) zugeordnet werden.

> [!IMPORTANT]
> Es ist wichtig, zu verstehen, dass die Abrechnung zu vollständigen Standardsätzen basierend auf der Anzahl der von Ihrer Anwendung erstellten Sammlungen erfolgt.
> 
> 

Es wird empfohlen, dass die Anwendung eine kleine Anzahl von Sammlungen verwendet, sofern Sie nicht über große Speicher oder Durchsatzanforderungen verfügen. Stellen Sie sicher, dass Sie Anwendungsmuster für die Erstellung von neuen Sammlungen gut verstanden haben. Sie können die Sammlungserstellung als Verwaltungsaktion reservieren, die außerhalb Ihrer Anwendung durchgeführt wird. Ähnlich ändert sich der Stundensatz, über den die Sammlung abgerechnet wird, wenn Sie die Leistungsebene für eine Sammlung anpassen. Wenn Ihre Anwendung die Leistungsebenen dynamisch anpasst, sollten Sie diese überwachen.

## <a name="a-idchanging-performance-levels-using-the-azure-portalachange-from-s1-s2-s3-to-user-defined-performance"></a><a id="changing-performance-levels-using-the-azure-portal"></a>Wechseln von S1, S2, S3 zu benutzerdefinierter Leistung
Führen Sie die hier beschriebenen Schritte durch, um im Azure-Portal von vordefinierten Durchsatzstufen zu benutzerdefinierten Durchsatzstufen zu wechseln. Bei Verwendung benutzerdefinierter Durchsatzstufen können Sie den Durchsatz an Ihre Anforderungen anpassen. Wenn Sie immer noch ein S1-Konto verwenden, können Sie den Standarddurchsatz mit wenigen Klicks von 250 RU/s auf 400 RU/s erhöhen. Beachten Sie, dass Sie eine Sammlung nicht mehr zu S1, S2, oder S3 verschieben können, sobald Sie sie von S1, S2 oder S3 zu Standard (benutzerdefiniert) verschoben haben. Sie können allerdings den Durchsatz einer Standard-Sammlung jederzeit anpassen.

Weitere Informationen zu Preisänderungen in Verbindung mit benutzerdefiniertem und vordefiniertem Durchsatz finden Sie im Blogbeitrag [DocumentDB: Everything you need to know about using the new pricing options](https://azure.microsoft.com/blog/documentdb-use-the-new-pricing-options-on-your-existing-collections/)(DocumentDB: Alles Wissenswerte zur Verwendung der neuen Preisoptionen).

> [!VIDEO https://channel9.msdn.com/Blogs/AzureDocumentDB/ChangeDocumentDBCollectionPerformance/player]
> 
> 

1. Klicken Sie im [**Azure-Portal**](https://portal.azure.com) auf **NoSQL (DocumentDB)**, und wählen Sie das zu ändernde DocumentDB-Konto aus. 
 
    Wenn **NoSQL (DocumentDB)** in der Navigationsleiste nicht angezeigt wird, klicken Sie auf „>“, scrollen Sie zu **Datenbanken**, wählen Sie **NoSQL (DocumentDB)** und dann das DocumentDB-Konto aus.  

2. Klicken Sie im Ressourcenmenü unter **Sammlungen** auf **Skalieren**, wählen Sie in der Dropdownliste die zu ändernde Sammlung aus, und klicken Sie dann auf **Tarif**. Für Konten mit vordefiniertem Durchsatz gilt der Tarif S1, S2 oder S3.  Klicken Sie auf dem Blatt **Preisstufe auswählen** auf **Standard**, um den benutzerdefinierten Durchsatz zu ändern, und anschließend auf **Auswählen**, um die Änderung zu speichern.

    ![Screenshot des Blatts „Einstellungen“, der veranschaulicht, wo Sie den Durchsatzwert ändern können](./media/documentdb-performance-levels/documentdb-change-performance-set-thoughput.png)

3. Auf dem Blatt **Skalieren** wurde der **Tarif** in **Standard** geändert, und im Feld **Durchsatz (RU/s)** wird als Standardwert „400“ angezeigt. Legen Sie den Durchsatz auf einen Wert zwischen 400 und 10.000 [Anforderungseinheiten](documentdb-request-units.md)/Sekunde (RU/s) fest. Die **Geschätzte monatliche Rechnung** am unteren Rand der Seite wird automatisch aktualisiert, um eine Schätzung der monatlichen Kosten anzugeben. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

    Wenn Sie feststellen, dass Sie einen höheren Durchsatz (größer als 10.000 RU/s) oder mehr Speicher (größer als 10 GB) benötigen, können Sie eine partitionierte Sammlung erstellen. Weitere Informationen zum Erstellen einer partitionierten Sammlung finden Sie unter [So erstellen Sie eine DocumentDB-Sammlung über das Azure-Portal](documentdb-create-collection.md).

> [!NOTE]
> Das Ändern von Leistungsstufen einer Sammlung kann bis zu 2 Minuten dauern.
> 
> 

## <a name="changing-performance-levels-using-the-net-sdk"></a>Ändern von Leistungsstufen mit dem .NET SDK
Eine weitere Möglichkeit zum Ändern der Leistungsstufen Ihrer Sammlungen stellen unsere SDK dar. In diesem Abschnitt wird nur das Ändern einer Sammlung mit unserer [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) behandelt. Die Vorgehensweise bei unseren anderen [SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx) ist jedoch ähnlich. Wenn Sie noch keine Erfahrung mit unserer .NET SDK haben, besuchen Sie unser [Erste-Schritte-Tutorial](documentdb-get-started.md).

Nachstehend finden Sie einen Codeausschnitt zum Ändern des Angebotsdurchsatzes auf 50.000 Anforderungseinheiten pro Sekunde:

    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);

    // Set the throughput to S2
    offer = new Offer(offer);
    offer.OfferType = "S2";

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);



> [!NOTE]
> Sammlungen mit weniger als 10.000 Anforderungseinheiten pro Sekunde können zwischen Angeboten mit benutzerdefiniertem und vordefiniertem Durchsatz (S1, S2, S3) zu jedem beliebigen Zeitpunkt migriert werden. Sammlungen mit mehr als 10.000 Anforderungseinheiten pro Sekunde können nicht in vordefinierte Durchsatzebenen konvertiert werden.
> 
> 

Besuchen Sie [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) , um weitere Beispiele anzuzeigen und mehr über unsere Angebotsmethoden zu erfahren:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

## <a name="a-idchange-throughputachanging-the-throughput-of-a-collection"></a><a id="change-throughput"></a>Ändern des Durchsatzes einer Sammlung
Wenn Sie bereits benutzerdefinierte Leistung nutzen, können Sie den Durchsatz der Sammlung wie folgt ändern. Wenn Sie von einer Leistungsebene des Typs S1, S2 oder S3 (vordefinierte Leistung) zu benutzerdefinierter Leistung wechseln müssen, siehe [Wechsel von den Leistungsebenen S1, S2 und S3 zu benutzerdefinierter Leistung](#changing-performance-levels-using-the-azure-portal).

1. Klicken Sie im [**Azure-Portal**](https://portal.azure.com) auf **NoSQL (DocumentDB)**, und wählen Sie das zu ändernde DocumentDB-Konto aus.    
2. Klicken Sie im Ressourcenmenü unter **Sammlungen** auf **Skalieren**, und wählen Sie in der Dropdownliste die zu ändernde Sammlung aus.
3. Geben Sie im Feld **Durchsatz (RU/s)** den neuen Durchsatz ein. 
   
    Die **Geschätzte monatliche Rechnung** am unteren Rand der Seite wird automatisch aktualisiert, um eine Schätzung der monatlichen Kosten anzugeben. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

    Wenn Sie nicht sicher sind, um wie viel Sie den Durchsatz erhöhen sollten, siehe [Schätzen der Durchsatzanforderungen](documentdb-request-units.md#estimating-throughput-needs) und [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner).

## <a name="troubleshooting"></a>Problembehandlung

Wenn die Option zum Wechseln zwischen den Leistungsebenen S1, S2, und S3 auf dem Blatt **Wählen Sie Ihren Tarif** nicht angezeigt wird, klicken Sie auf **Alle anzeigen**, um die Leistungsebenen Standard, S1, S2 und S3 anzuzeigen. Wenn Sie den Tarif „Standard“ verwenden, stehen S1, S2 und S3 nicht zur Auswahl.

![Abbildung des Blattes „Wählen Sie Ihren Tarif“ mit hervorgehobener Option „Alle anzeigen“](./media/documentdb-performance-levels/azure-documentdb-database-view-all-performance-levels.png)

Sobald Sie eine Sammlung von S1, S2, oder S3 zu Standard verschieben, stehen S1, S2 und S3 nicht mehr zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Preisen und der Datenverwaltung mit Azure DocumentDB finden Sie in folgenden Ressourcen:

* [DocumentDB-Preise](https://azure.microsoft.com/pricing/details/documentdb/)
* [Verwalten der DocumentDB-Kapazität](documentdb-manage.md)
* [Modellieren von Daten in DocumentDB](documentdb-modeling-data.md)
* [Partitionieren von Daten in DocumentDB](documentdb-partition-data.md)
* [Anforderungseinheiten](http://go.microsoft.com/fwlink/?LinkId=735027)

Weitere Informationen zu DocumentDB finden Sie in der Azure DocumentDB- [Dokumentation](https://azure.microsoft.com/documentation/services/documentdb/).

Im Artikel [Leistungs- und Skalierungstests mit Azure DocumentDB](documentdb-performance-testing.md)finden Sie eine Einführung in Leistungs- und Skalierungstests mit DocumentDB.

[1]: ./media/documentdb-performance-levels/documentdb-change-collection-performance7-9.png
[2]: ./media/documentdb-performance-levels/documentdb-change-collection-performance10-11.png



<!--HONumber=Nov16_HO3-->


