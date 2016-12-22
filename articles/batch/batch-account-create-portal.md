---
title: Erstellen eines Azure Batch-Kontos | Microsoft Docs
description: "Erfahren Sie, wie Sie ein Azure Batch-Konto im Azure-Portal erstellen, um umfangreiche parallele Workloads in der Cloud auszuführen."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/21/2016
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: dfcf1e1d54a0c04cacffb50eca4afd39c6f6a1b1
ms.openlocfilehash: 8875c5e91a2a1ae848ebf725ca00bd43ec667dce


---
# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Erstellen eines Azure Batch-Kontos im Azure-Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
> 
> 

Hier erfahren Sie, wie Sie im [Azure-Portal][azure_portal] ein Azure Batch-Konto erstellen und wo Sie wichtige Kontoeigenschaften wie Zugriffsschlüssel und Konto-URLs finden. Außerdem werden die Batch-Preise erörtert und das Verknüpfen eines Azure-Speicherkontos mit dem Batch-Konto erläutert, sodass Sie [Anwendungspakete](batch-application-packages.md) verwenden und die [Ausgabe von Aufträgen und Tasks beibehalten](batch-task-output.md) können.

## <a name="create-a-batch-account"></a>Erstellen eines Batch-Kontos
1. Melden Sie sich beim [Azure-Portal][azure_portal] an.
2. Klicken Sie auf **Neu** > **Compute** > **Batch-Dienst**.
   
    ![Batch im Marketplace][marketplace_portal]
3. Das Blatt **Neues Batch-Konto** wird angezeigt. Beschreibungen der einzelnen Elemente auf dem Blatt finden Sie im Folgenden (Punkt *a* bis *e*).
   
    ![Erstellen eines Batch-Kontos][account_portal]
   
    a. **Kontoname:**Ein eindeutiger Name für das Batch-Konto. Dieser Name muss innerhalb der Azure-Region, in der das Konto erstellt wird, eindeutig sein (siehe *Standort* weiter unten). Er darf nur Kleinbuchstaben und Zahlen enthalten und muss 3 bis 24 Zeichen lang sein.
   
    b. **Abonnement:**Ein Abonnement, in dem das Batch-Konto erstellt werden soll. Wenn Sie nur über ein Abonnement verfügen, ist es standardmäßig ausgewählt.
   
    c. **Ressourcengruppe**: Eine vorhandene Ressourcengruppe für Ihr neues Batch-Konto. Optional können Sie auch eine neue Ressourcengruppe erstellen.
   
    d. **Standort:**Eine Azure-Region, in der das Batch-Konto erstellt werden soll. Nur die von Ihrem Abonnement und der Ressourcengruppe unterstützten Regionen werden als Optionen angezeigt.
   
    e. **Speicherkonto** (optional): Ein Speicherkonto vom Typ **Allgemein**, das Sie dem neuen Batch-Konto zuordnen bzw. damit verknüpfen. Weitere Details finden Sie unter [Verknüpftes Azure-Speicherkonto](#linked-azure-storage-account).
4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.
   
   Im Portal wird angezeigt, dass das Konto **bereitgestellt** wird, und nach Abschluss des Vorgangs wird unter *Benachrichtigungen* die Meldung **Die Bereitstellungen waren erfolgreich** angezeigt.

## <a name="view-batch-account-properties"></a>Anzeigen der Eigenschaften des Batch-Kontos
Nachdem das Konto erstellt wurde, können Sie das Blatt **Batch-Konto** öffnen, um auf die Einstellungen und Eigenschaften des Kontos zuzugreifen. Sie können über das linke Menü auf dem Blatt „Batch-Konto“ auf alle Kontoeinstellungen und -eigenschaften zugreifen.

![Blatt „Batch-Konto“ im Azure-Portal][account_blade]

* **Batch-Konto-URL**: Für Anwendungen, die Sie mit den [APIs für die Batch-Entwicklung](batch-technical-overview.md#batch-development-apis) erstellen, ist eine Konto-URL zum Verwalten von Ressourcen und Ausführen von Aufträgen im Konto erforderlich. Eine Batch-Konto-URL weist das folgende Format auf:
  
    `https://<account_name>.<region>.batch.azure.com`

![Batch-Konto-URL im Portal][account_url]

* **Zugriffsschlüssel**: Für Ihre Anwendungen ist zur Verwendung mit Ressourcen im Batch-Konto außerdem ein Zugriffsschlüssel erforderlich. Geben Sie zum Anzeigen oder erneuten Generieren der Zugriffsschlüssel Ihres Batch-Kontos auf dem Blatt „Batch-Konto“ `keys` in das Feld **Suche** im linken Menü ein, und wählen Sie **Schlüssel** aus.
  
    ![Batch-Kontoschlüssel im Azure-Portal][account_keys]

## <a name="pricing"></a>Preise
Batch-Konten werden nur mit einem „Free-Tarif“ angeboten. Dies bedeutet, dass Ihnen keine Kosten für das Batch-Konto selbst in Rechnung gestellt werden. Kosten fallen für die zugrunde liegenden Azure-Computeressourcen an, die von Ihren Batch-Lösungen genutzt werden, sowie für die Ressourcen, die bei der Ausführung Ihrer Workloads von anderen Diensten genutzt werden. Beispielsweise werden Ihnen Computeknoten in Ihren Pools berechnet und die Daten, die Sie in Azure Storage als Ein- oder Ausgabe für Ihre Aufgaben speichern. Wenn Sie das Feature [Anwendungspakete](batch-application-packages.md) von Batch verwenden, fallen zudem Kosten für die Azure Storage-Ressourcen an, die zum Speichern der Anwendungspakete verwendet werden. Weitere Informationen finden Sie unter [Batch-Preise][batch_pricing].

## <a name="linked-azure-storage-account"></a>Verknüpftes Azure-Speicherkonto
Wie zuvor erwähnt, können Sie optional ein Speicherkonto vom Typ **Allgemein** mit Ihrem Batch-Konto verknüpfen. Das Feature [Anwendungspakete](batch-application-packages.md) von Batch verwendet ebenso wie die .NET-Bibliothek [Batch-Dateikonventionen](batch-task-output.md) Blobspeicher in einem verknüpften allgemeinen Speicherkonto. Diese optionalen Features unterstützen Sie beim Bereitstellen der von Ihren Batch-Aufgaben ausgeführten Anwendungen und Beibehalten der von ihnen erzeugten Daten.

Batch unterstützt derzeit *nur* den Speicherkontotyp **Allgemein** (siehe Schritt 5, [Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account), unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md)). Verwenden Sie beim Verknüpfen eines Azure-Speicherkontos mit Ihrem Batch-Konto *ausschließlich* ein Speicherkonto vom Typ **Allgemein** .

![Erstellen eines Speicherkontos vom Typ „Allgemein“][storage_account]

Wir empfehlen die Erstellung eines Speicherkontos, das ausschließlich von Ihrem Batch-Konto verwendet wird.

> [!WARNING]
> Gehen Sie beim erneuten Generieren der Zugriffsschlüssel für ein verknüpftes Speicherkonto vorsichtig vor. Generieren Sie nur einen Speicherkontoschlüssel erneut, und klicken Sie auf dem Blatt des verknüpften Speicherkontos auf **Synchronisierungsschlüssel**. Warten Sie fünf Minuten, damit der Schlüssel an die Computeknoten in Ihren Pools verteilt werden kann. Anschließend können Sie ggf. den anderen Schlüssel erneut generieren und synchronisieren. Wenn Sie beide Schlüssel gleichzeitig generieren, können die Computeknoten keinen der Schlüssel synchronisieren und nicht mehr auf das Speicherkonto zugreifen.
> 
> 

  ![Erneutes Generieren der Speicherkontoschlüssel][4]

## <a name="batch-service-quotas-and-limits"></a>Batch-Dienst – Kontingente und Limits
Beachten Sie, dass wie bei Ihrem Azure-Abonnement und anderen Azure-Diensten bestimmte [Kontingente und Grenzwerte](batch-quota-limit.md) für Batch-Konten gelten. Die aktuellen Kontingente für ein Batch-Konto werden im Portal in den **Eigenschaften**des Kontos angezeigt.

![Batch-Kontokontingente im Azure-Portal][quotas]

Bedenken Sie diese Kontingente beim Entwerfen und Skalieren Ihrer Batch-Workloads. Wenn Ihr Pool beispielsweise nicht die von Ihnen vorgegebene Anzahl von Computeknoten erreicht, haben Sie möglicherweise das Kernkontingentlimit für Ihr Batch-Konto erreicht.

Beachten Sie außerdem, dass Sie nicht auf ein einzelnes Batch-Konto für Ihr Azure-Abonnement beschränkt sind. Sie können mehrere Batch-Workloads in einem Batch-Konto ausführen oder Ihre Workloads auf Batch-Konten in demselben Abonnement, aber verschiedenen Azure-Regionen aufteilen.

Viele dieser Kontingente können einfach mittels einer kostenlosen Produktsupportanfrage im Azure-Portal erhöht werden. Ausführliche Informationen zum Anfordern einer Kontingenterhöhung finden unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md) .

## <a name="other-batch-account-management-options"></a>Weitere Optionen für die Verwaltung von Batch-Konten
Neben der Verwendung des Azure-Portals stehen Ihnen zum Erstellen und Verwalten von Batch-Konten die folgenden Möglichkeiten zur Verfügung:

* [Batch-PowerShell-Cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den Konzepten und Features des Batch-Diensts finden Sie unter [Übersicht über Azure Batch-Features für Entwickler](batch-api-basics.md). In diesem Artikel werden die primären Batch-Ressourcen beschrieben, z.B. Pools, Computeknoten, Aufträge und Aufgaben. Außerdem enthält er eine Übersicht über die Features des Diensts, mit denen Sie Computeworkloads in großem Umfang ausführen können.
* Informieren Sie sich über die Grundlagen der Entwicklung einer Batch-fähigen Anwendung mit der [Batch-.NET-Clientbibliothek](batch-dotnet-get-started.md). Im [Einführungsartikel](batch-dotnet-get-started.md) werden Sie durch eine funktionierende Anwendung geführt, die den Batch-Dienst zum Ausführen einer Workload auf mehreren Computeknoten verwendet und Azure Storage zum Bereitstellen und Abrufen von Workloaddateien nutzt.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Erneutes Generieren der Speicherkontoschlüssel"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png



<!--HONumber=Dec16_HO2-->


