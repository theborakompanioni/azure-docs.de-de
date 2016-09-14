<properties
	pageTitle="Anfordern von höheren DocumentDB-Kontokontingenten | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine Anpassung der DocumentDB-Datenbankkontingente anfordern, wie z.B. für Dokumentspeicher und Durchsatz pro Sammlung."
	services="documentdb"
	authors="AndrewHoh"
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
	ms.author="anhoh"/>

# Anfordern von erhöhten DocumentDB-Kontolimits

[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) verfügt über Standardkontingente, die Sie anpassen können, indem Sie sich an den Azure-Support wenden. In diesem Artikel wird veranschaulicht, wie Sie die Erhöhung eines Kontingents anfordern.

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

-	Welche DocumentDB-Datenbankkontingente können Sie mithilfe des Azure-Supports anpassen?
-	Wie kann ich die Anpassung eines DocumentDB-Kontokontingents anfordern?

##<a id="Quotas"></a> DocumentDB-Kontokontingente

In der folgenden Tabelle sind die DocumentDB-Kontingente beschrieben: Kontingente mit einem Sternchen (*) können angepasst werden. Wenden Sie sich dafür an den Azure-Support.

[AZURE.INCLUDE [azure-documentdb-limits](../../includes/azure-documentdb-limits.md)]


##<a id="RequestQuotaIncrease"></a>Anfordern einer Kontingentanpassung
Die folgenden Schritte zeigen, wie Sie eine Kontingentanpassung anfordern.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Weitere Dienste** und dann auf **Hilfe & Support**.

	![Screenshot: Start von Hilfe & Support](media/documentdb-increase-limits/helpsupport.png)

2. Klicken Sie auf dem Blatt **Hilfe und Support** auf **Neue Supportanfrage**.

	![Screenshot der Erstellung eines Supporttickets](media/documentdb-increase-limits/getsupport.png)

3. Klicken Sie im Blatt **Neue Supportanfrage** auf **Grundlagen**. Legen Sie anschließend **Problemtyp** als **Kontingent** fest, und wählen Sie **Abonnement** für Ihr Abonnement, das Ihr DocumentDB-Konto hostet. Legen Sie **Kontingenttyp** als **DocumentDB** und **Supportplan** als **Kontingentsupport – enthalten** fest. Klicken Sie dann auf **Weiter**.

	![Screenshot des Anforderungstyps für das Supportticket](media/documentdb-increase-limits/supportrequest1.png)

4. Wählen Sie auf dem Blatt **Problem** einen Schweregrad aus, und fügen Sie in **Details** Informationen zur Ihrer Kontigenterhöhung hinzu. Klicken Sie auf **Weiter**.

	![Screenshot der Abonnementauswahl für das Supportticket](media/documentdb-increase-limits/supportrequest2.png)

5. Geben Sie abschließend Ihre Kontaktinformationen auf dem Blatt **Kontaktinformationen** ein, und klicken Sie auf **Erstellen**.

Sobald das Supportticket erstellt wurde, sollten Sie per E-Mail eine Supportanforderungsnummer erhalten. Sie können die Supportanforderung auch anzeigen, indem Sie auf dem Blatt **Hilfe & Support** auf **Supportanforderungen verwalten** klicken.

![Screenshot des Blatts für Supportanforderungen](media/documentdb-increase-limits/supportrequest4.png)


##<a name="NextSteps"></a> Nächste Schritte
- Um weitere Informationen zu DocumentDB zu erhalten, klicken Sie [hier](http://azure.com/docdb).

<!---HONumber=AcomDC_0831_2016-->