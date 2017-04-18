---
title: Erstellen eines Batch-Kontos im Azure-Portal | Microsoft-Dokumentation
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
ms.date: 03/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 11f8c3f37e56e0b5c566c4abdb60697c5279e72a
ms.lasthandoff: 04/06/2017


---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Erstellen eines Batch-Kontos mit dem Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
> 
> 

Hier erfahren Sie, wie Sie im [Azure-Portal][azure_portal] ein Azure Batch-Konto erstellen und geeignete Kontoeigenschaften für Ihr Computeszenario auswählen. Außerdem erfahren Sie, wo Sie wichtige Kontoeigenschaften wie Zugriffsschlüssel und Konto-URLs finden. 

Hintergrundinformationen zu Batch-Konten und -Szenarien finden Sie in der [Funktionsübersicht](batch-api-basics.md).



## <a name="create-a-batch-account"></a>Erstellen eines Batch-Kontos

Verwenden Sie das Portal, um ein Batch-Konto in einem der beiden *Poolzuordnungsmodi* zu erstellen. Zur Auswahl stehen **Batch-Dienst** und der neuere, mit höherem Konfigurationsaufwand verbundene Modus **Benutzerabonnement**. Informationen zu den beiden Modi finden Sie in der [Funktionsübersicht](batch-api-basics.md#account). Informationen zu den Features des Benutzerabonnementmodus finden Sie auch in [diesem Blogbeitrag](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).

## <a name="batch-service-mode"></a>Modus „Batch-Dienst“



1. Melden Sie sich beim [Azure-Portal][azure_portal] an.
2. Klicken Sie auf **Neu** > **Compute** > **Batch-Dienst**.
   
    ![Batch im Marketplace][marketplace_portal]
3. Das Blatt **Neues Batch-Konto** wird angezeigt. Die einzelnen Elemente des Blatts werden im Anschluss näher beschrieben.
   
    ![Erstellen eines Batch-Kontos][account_portal]
   
    a. **Kontoname**: Der gewählte Batch-Kontoname muss innerhalb der Azure-Region, in der das Konto erstellt wird, eindeutig sein (siehe **Standort** weiter unten). Der Kontoname darf nur Kleinbuchstaben und Zahlen enthalten und muss 3 bis 24 Zeichen lang sein.
   
    b. **Abonnement**: Das Abonnement, in dem das Batch-Konto erstellt werden soll. Wenn Sie nur über ein Abonnement verfügen, ist es standardmäßig ausgewählt.

    c. **Poolzuordnungsmodus**: Wählen Sie **Batch-Dienst** aus.
   
    c. **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe für Ihr neues Batch-Konto aus, oder erstellen Sie eine neue.
   
    d. **Standort**: Die Azure-Region, in der das Batch-Konto erstellt werden soll. Nur die von Ihrem Abonnement und der Ressourcengruppe unterstützten Regionen werden als Optionen angezeigt.
   
    e. **Speicherkonto** (optional): Ein allgemeines Azure Storage-Konto, das Sie dem Batch-Konto zuordnen. Dies ist bei den meisten Batch-Konten empfehlenswert. Ausführlichere Informationen finden Sie weiter unten in diesem Artikel unter [Verknüpftes Azure-Speicherkonto](#linked-azure-storage-account).

4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.
   
   Das Portal gibt an, dass die Bereitstellung ausgeführt wird. Nach Abschluss des Vorgangs wird unter **Benachrichtigungen** die Benachrichtigung **Die Bereitstellungen waren erfolgreich.** angezeigt.
   
## <a name="user-subscription-mode"></a>Modus „Benutzerabonnement“

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Gewähren des Zugriffs auf Ihr Abonnement für Azure Batch (einmaliger Vorgang)
Wenn Sie Ihr erstes Batch-Konto im Modus „Benutzerabonnement“ erstellen, müssen Sie Ihr Abonnement wie folgt bei Batch registrieren. (Falls Sie diesen Schritt bereits ausgeführt haben, fahren Sie mit dem nächsten Abschnitt fort.)

1. Melden Sie sich beim [Azure-Portal][azure_portal] an.

2. Klicken Sie auf **Weitere Dienste** > **Abonnements** und anschließend auf das Abonnement, das Sie für das Batch-Konto verwenden möchten. 

3. Klicken Sie auf dem Blatt **Abonnement** auf **Zugriffssteuerung (IAM)** > **Hinzufügen**.

    ![Abonnementzugriffssteuerung][subscription_access]

4. Wählen Sie auf dem Blatt **Berechtigungen hinzufügen** die Rolle **Mitwirkender** aus, und suchen Sie nach **MicrosoftAzureBatch** (ohne Leerzeichen). Wählen Sie **MicrosoftAzureBatch** aus, und klicken Sie anschließend auf **Speichern**.

    ![Hinzufügen von Batch-Berechtigungen][add_permission]

### <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors
Im Modus „Benutzerabonnement“ wird ein Azure-Schlüsseltresor benötigt. Dieser muss der gleichen Ressourcengruppe angehören wie das zu erstellende Batch-Konto. Stellen Sie sicher, dass sich die Ressourcengruppe in einer Region befindet, in der Batch [verfügbar](https://azure.microsoft.com/regions/services/) ist und die von Ihrem Abonnement unterstützt wird.

1. Klicken Sie im [Azure-Portal][azure_portal] auf **Neu** > **Sicherheit und Identität** > **Key Vault**. 

2. Geben Sie auf dem Blatt **Schlüsseltresor erstellen** einen Namen für den Schlüsseltresor ein, und erstellen Sie eine Ressourcengruppe in der Region, die Sie für Ihr Batch-Konto verwenden möchten. Behalten Sie bei den übrigen Einstellungen die Standardwerte bei, und klicken Sie auf **Erstellen**.

### <a name="create-a-batch-account"></a>Erstellen eines Batch-Kontos

1. Klicken Sie im [Azure-Portal][azure_portal] auf **Neu** > **Compute** > **Batch-Dienst**.
   
    ![Batch im Marketplace][marketplace_portal]
3. Das Blatt **Neues Batch-Konto** wird angezeigt. Die einzelnen Elemente des Blatts werden im Anschluss näher beschrieben.
   
    ![Erstellen eines Batch-Kontos][account_portal_byos]
   
    a. **Kontoname**: Der gewählte Batch-Kontoname muss innerhalb der Azure-Region, in der das Konto erstellt wird, eindeutig sein (siehe **Standort** weiter unten). Der Kontoname darf nur Kleinbuchstaben und Zahlen enthalten und muss 3 bis 24 Zeichen lang sein.
   
    b. **Abonnement**: Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, das Sie beim Batch-Dienst registriert haben.

    c. **Poolzuordnungsmodus**: Wählen Sie **Benutzerabonnement** aus.

    d. **Schlüsseltresor**: Wählen Sie den Schlüsseltresor aus, den Sie im vorherigen Abschnitt für Ihr Batch-Konto erstellt haben. Sie können optional auch einen neuen Schlüsseltresor erstellen. Aktivieren Sie nach dem Auswählen des Tresors das Kontrollkästchen, um Azure Batch Zugriff auf den Schlüsseltresor zu gewähren.
   
    c. **Ressourcengruppe**: Wählen Sie die Ressourcengruppe aus, in der Sie den Schlüsseltresor erstellt haben.
   
    d. **Standort**: Die Azure-Region, in der Sie den Schlüsseltresor für das Batch-Konto erstellt haben. 
   
    e. **Speicherkonto** (optional): Ein allgemeines Azure Storage-Konto, das Sie dem Batch-Konto zuordnen. Dies ist bei den meisten Batch-Konten empfehlenswert. Weitere Details finden Sie unter [Verknüpftes Azure-Speicherkonto](#linked-azure-storage-account).

4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.
   
   Das Portal gibt an, dass die Bereitstellung ausgeführt wird. Nach Abschluss des Vorgangs wird unter **Benachrichtigungen** die Benachrichtigung **Die Bereitstellungen waren erfolgreich.** angezeigt.



## <a name="view-batch-account-properties"></a>Anzeigen der Eigenschaften des Batch-Kontos
Nachdem das Konto erstellt wurde, können Sie das Blatt **Batch-Konto** öffnen, um auf die Einstellungen und Eigenschaften des Kontos zuzugreifen. Sie können über das linke Menü auf dem Blatt „Batch-Konto“ auf alle Kontoeinstellungen und -eigenschaften zugreifen.

![Blatt „Batch-Konto“ im Azure-Portal][account_blade]

* **Batch-Konto-URL**: Bei der Entwicklung einer Anwendung mit den [Batch-APIs](batch-apis-tools.md#batch-development-apis) benötigen Sie eine Konto-URL für den Zugriff auf Ihre Batch-Ressourcen. Eine Batch-Konto-URL weist das folgende Format auf:
  
    `https://<account_name>.<region>.batch.azure.com`

![Batch-Konto-URL im Portal][account_url]

* **Zugriffsschlüssel** (Modus „Batch-Dienst“): Sie benötigen einen Kontozugriffsschlüssel, um den Zugriff auf Ihr Batch-Konto über Ihre Anwendung zu authentifizieren. (Diese Einstellung ist im Modus „Benutzerabonnement“ nicht verfügbar; in diesem Modus wird die Azure Active Directory-Authentifizierung verwendet.)

    Geben Sie zum Anzeigen oder erneuten Generieren der Zugriffsschlüssel Ihres Batch-Kontos auf dem Blatt „Batch-Konto“ `keys` in das Feld **Suche** im linken Menü ein, und wählen Sie **Schlüssel** aus. 
  
    ![Batch-Kontoschlüssel im Azure-Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Verknüpftes Azure-Speicherkonto

Sie können optional ein allgemeines Azure Storage-Konto mit Ihrem Batch-Konto verknüpfen. Das Feature [Anwendungspakete](batch-application-packages.md) von Batch verwendet ebenso wie die .NET-Bibliothek [Batch-Dateikonventionen](batch-task-output.md) Azure-Blobspeicher. Diese optionalen Features unterstützen Sie beim Bereitstellen der von Ihren Batch-Aufgaben ausgeführten Anwendungen und Beibehalten der von ihnen erzeugten Daten.

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



Zudem können viele dieser Kontingente einfach mittels einer kostenlosen Produktsupportanfrage im Azure-Portal erhöht werden. Ausführliche Informationen zum Anfordern einer Kontingenterhöhung finden unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md) .

## <a name="other-batch-account-management-options"></a>Weitere Optionen für die Verwaltung von Batch-Konten
Neben der Verwendung des Azure-Portals stehen Ihnen zum Erstellen und Verwalten von Batch-Konten die folgenden Möglichkeiten zur Verfügung:

* [Batch-PowerShell-Cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure-Befehlszeilenschnittstelle](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den Konzepten und Features des Batch-Diensts finden Sie in der [Funktionsübersicht für Batch](batch-api-basics.md). In diesem Artikel werden die primären Batch-Ressourcen beschrieben, z.B. Pools, Computeknoten, Aufträge und Aufgaben. Außerdem enthält er eine Übersicht über die Features des Diensts, mit denen Sie Computeworkloads in großem Umfang ausführen können.
* Informieren Sie sich über die Grundlagen der Entwicklung einer Batch-fähigen Anwendung mit der [Batch-.NET-Clientbibliothek](batch-dotnet-get-started.md) oder mit [Python](batch-python-tutorial.md). In diesen Einführungsartikeln werden Sie durch eine funktionierende Anwendung geführt, die den Batch-Dienst zum Ausführen einer Workload auf mehreren Computeknoten verwendet und Azure Storage zum Bereitstellen und Abrufen von Workloaddateien nutzt.

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
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
