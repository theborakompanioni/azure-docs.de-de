<properties 
	pageTitle="Steigern der Leistung Ihres DocumentDB-S1-Kontos | Microsoft Azure" 
	description="Nehmen Sie einige einfache Änderungen im Azure-Portal vor, und profitieren Sie von einem höheren Durchsatz bei Ihrem DocumentDB-S1-Konto." 
	services="documentdb" 
	authors="mimig1" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/25/2016" 
	ms.author="mimig"/>

# Steigern der Leistung Ihres DocumentDB-Kontos

Führen Sie die hier beschriebenen Schritte durch, um den Durchsatz für Ihr Azure DocumentDB-S1-Konto zu steigern. Mit geringen bis gar keinen Zusatzkosten können Sie den Durchsatz Ihres vorhandenen S1-Kontos von 250 [RU/s](documentdb-request-units.md) auf 400 RU/s oder noch mehr erhöhen!

> [AZURE.VIDEO changedocumentdbcollectionperformance]

## Umstellen auf benutzerdefinierte Leistung im Azure-Portal

1. Navigieren Sie in Ihrem Browser zum [**Azure-Portal**](https://portal.azure.com).
2. Klicken Sie auf **Durchsuchen** > **DocumentDB (NoSQL)**, und wählen Sie das zu ändernde DocumentDB-Konto aus.
3. Wählen Sie im Fokus **Datenbanken** die zu ändernde Datenbank und anschließend auf dem Blatt **Datenbank** die Sammlung mit dem S1-Tarif aus.

      ![Screenshot des Blatts „Datenbank“ mit einer S1-Sammlung](./media/documentdb-supercharge-your-account/documentdb-change-performance-S1.png)

4. Klicken Sie auf dem Blatt **Sammlung** auf **Weitere** und dann auf **Einstellungen**.
5. Klicken Sie auf dem Blatt **Einstellungen** auf **Tarif**. Die voraussichtlichen monatlichen Kosten für die einzelnen Tarife werden angezeigt. Klicken Sie auf dem Blatt **Preisstufe auswählen** auf **Standard** und anschließend auf **Auswählen**, um die Änderung zu speichern.

      ![Screenshot der DocumentDB-Blätter „Einstellungen“ und „Preisstufe auswählen“](./media/documentdb-supercharge-your-account/documentdb-change-performance.png)

6. Auf dem Blatt **Einstellungen** wurde der **Tarif** in **Standard** geändert, und im Feld **Durchsatz (RU/s)** wird als Standardwert „400“ angezeigt. Klicken Sie zum Speichern der Änderungen auf **OK**.

    > [AZURE.NOTE] Der Durchsatz kann auf einen Wert zwischen 400 und 10.000 [Anforderungseinheiten](../articles/documentdb/documentdb-request-units.md)/Sekunde (RU/s) festgelegt werden. Die **Preiszusammenfassung** am unteren Rand der Seite wird automatisch aktualisiert, um eine Schätzung der monatlichen Kosten anzugeben.
    
	![Screenshot des Blatts „Einstellungen“, der veranschaulicht, wo Sie den Durchsatzwert ändern können](./media/documentdb-supercharge-your-account/documentdb-change-performance-set-thoughput.png)

8. Auf dem Blatt **Datenbank** können Sie den gesteigerten Durchsatz der Sammlung überprüfen.

	![Screenshot des Blatts „Datenbank“ mit geänderter Sammlung](./media/documentdb-supercharge-your-account/documentdb-change-performance-confirmation.png)

Weitere Informationen zu Änderungen in Verbindung mit benutzerdefiniertem und vordefiniertem Durchsatz finden Sie im Blogbeitrag [DocumentDB: Everything you need to know about using the new pricing options](https://azure.microsoft.com/blog/documentdb-use-the-new-pricing-options-on-your-existing-collections/) (DocumentDB: Alles Wissenswerte zur Verwendung der neuen Preisoptionen).

## Nächste Schritte

Wenn Sie feststellen, dass Sie einen höheren Durchsatz (größer als 10.000 RU/s) oder mehr Speicher (größer als 10 GB) benötigen, können Sie eine partitionierte Sammlung erstellen. Weitere Informationen zum Erstellen einer partitionierten Sammlung finden Sie unter [So erstellen Sie eine DocumentDB-Sammlung über das Azure-Portal](documentdb-create-collection.md).

<!---HONumber=AcomDC_0831_2016-->