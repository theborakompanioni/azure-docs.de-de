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
ms.date: 12/19/2016
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: 6f05aa3f06c8ec86d58cafd406c2752ddb2eecc1
ms.openlocfilehash: cf5b560028f3f80e3ba46ef96ae7a157dad7847d


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
   
    a. **Kontoname**: Der Name für das Batch-Konto. Der gewählte Name muss innerhalb der Azure-Region, in dem das neue Konto erstellt wird, eindeutig sein (siehe **Standort** unten). Der Kontoname darf nur Kleinbuchstaben und Zahlen enthalten und muss 3 bis 24 Zeichen lang sein.
   
    b. **Abonnement**: Das Abonnement, in dem das Batch-Konto erstellt werden soll. Wenn Sie nur über ein Abonnement verfügen, ist es standardmäßig ausgewählt.
   
    c. **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe für Ihr neues Batch-Konto aus, oder erstellen Sie eine neue.
   
    d. **Standort**: Die Azure-Region, in der das Batch-Konto erstellt werden soll. Nur die von Ihrem Abonnement und der Ressourcengruppe unterstützten Regionen werden als Optionen angezeigt.
   
    e. **Speicherkonto** (optional): Ein allgemeines Azure Storage-Konto, das Sie dem neuen Batch-Konto zuordnen. Weitere Details finden Sie unter [Verknüpftes Azure-Speicherkonto](#linked-azure-storage-account).

4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.
   
   Im Portal wird angezeigt, dass das Konto **bereitgestellt** wird, und nach Abschluss des Vorgangs wird unter *Benachrichtigungen* die Meldung **Die Bereitstellungen waren erfolgreich** angezeigt.

## <a name="view-batch-account-properties"></a>Anzeigen der Eigenschaften des Batch-Kontos
Nachdem das Konto erstellt wurde, können Sie das Blatt **Batch-Konto** öffnen, um auf die Einstellungen und Eigenschaften des Kontos zuzugreifen. Sie können über das linke Menü auf dem Blatt „Batch-Konto“ auf alle Kontoeinstellungen und -eigenschaften zugreifen.

![Blatt „Batch-Konto“ im Azure-Portal][account_blade]

* **Batch-Konto-URL**: Bei der Entwicklung einer Anwendung mit den [Batch-APIs](batch-technical-overview.md#batch-development-apis) benötigen Sie eine Konto-URL für den Zugriff auf Ihre Batch-Ressourcen. Eine Batch-Konto-URL weist das folgende Format auf:
  
    `https://<account_name>.<region>.batch.azure.com`

![Batch-Konto-URL im Portal][account_url]

* **Zugriffsschlüssel**: Zum Authentifizieren des Zugriffs auf das Batch-Konto über Ihre Anwendung benötigen Sie einen Kontozugriffsschlüssel. Geben Sie zum Anzeigen oder erneuten Generieren der Zugriffsschlüssel Ihres Batch-Kontos auf dem Blatt „Batch-Konto“ `keys` in das Feld **Suche** im linken Menü ein, und wählen Sie **Schlüssel** aus.
  
    ![Batch-Kontoschlüssel im Azure-Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Verknüpftes Azure-Speicherkonto

Wie zuvor erwähnt, können Sie optional ein allgemeines Azure Storage-Konto mit Ihrem Batch-Konto verknüpfen. Das Feature [Anwendungspakete](batch-application-packages.md) von Batch verwendet ebenso wie die .NET-Bibliothek [Batch-Dateikonventionen](batch-task-output.md) Azure-Blobspeicher. Diese optionalen Features unterstützen Sie beim Bereitstellen der von Ihren Batch-Aufgaben ausgeführten Anwendungen und Beibehalten der von ihnen erzeugten Daten.

Wir empfehlen die Erstellung eines neuen Speicherkontos, das ausschließlich von Ihrem Batch-Konto verwendet wird.

![Erstellen eines Speicherkontos vom Typ „Allgemein“][storage_account]

> [!NOTE] 
> Azure Batch unterstützt derzeit nur allgemeine Speicherkonten. Dieser Kontotyp wird in Schritt 5 [Speicherkonto erstellen] (../storage/storage-create-storage-account.md#create-a-storage-account) unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md) beschrieben.
>
>

> [!WARNING]
> Gehen Sie beim erneuten Generieren der Zugriffsschlüssel für ein verknüpftes Speicherkonto vorsichtig vor. Generieren Sie nur einen Speicherkontoschlüssel erneut, und klicken Sie auf dem Blatt des verknüpften Speicherkontos auf **Synchronisierungsschlüssel**. Warten Sie fünf Minuten, damit der Schlüssel an die Computeknoten in Ihren Pools verteilt werden kann. Anschließend können Sie ggf. den anderen Schlüssel erneut generieren und synchronisieren. Wenn Sie beide Schlüssel gleichzeitig generieren, können die Computeknoten keinen der Schlüssel synchronisieren und nicht mehr auf das Speicherkonto zugreifen.
> 
> 

![Erneutes Generieren der Speicherkontoschlüssel][4]

## <a name="batch-service-quotas-and-limits"></a>Batch-Dienst – Kontingente und Limits
Beachten Sie, dass wie bei Ihrem Azure-Abonnement und anderen Azure-Diensten bestimmte [Kontingente und Grenzwerte](batch-quota-limit.md) für Batch-Konten gelten. Die aktuellen Kontingente für ein Batch-Konto werden im Portal in den **Eigenschaften**des Kontos angezeigt.

![Batch-Kontokontingente im Azure-Portal][quotas]

Bedenken Sie diese Kontingente beim Entwerfen und Skalieren Ihrer Batch-Workloads. Wenn Ihr Pool beispielsweise nicht die von Ihnen vorgegebene Anzahl von Computeknoten erreicht, haben Sie möglicherweise das Kernkontingentlimit für Ihr Batch-Konto erreicht.

Das Kontingent für Batch-Konten gilt pro Region und Abonnement, sodass Sie standardmäßig mehrere Batch-Konten haben können, sofern sich diese in unterschiedlichen Regionen befinden. Sie können mehrere Batch-Workloads in einem Batch-Konto ausführen oder Ihre Workloads auf Batch-Konten in demselben Abonnement, aber verschiedenen Azure-Regionen aufteilen.

Zudem können viele dieser Kontingente einfach mittels einer kostenlosen Produktsupportanfrage im Azure-Portal erhöht werden. Ausführliche Informationen zum Anfordern einer Kontingenterhöhung finden unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md) .

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



<!--HONumber=Jan17_HO1-->


