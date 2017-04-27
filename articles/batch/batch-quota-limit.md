---
title: "Dienstkontingente und -grenzwerte für Azure Batch | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über Azure Batch-Standardkontingente, Limits und Einschränkungen sowie die Anforderung von Kontingentsteigerungen."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 4d2ba8d3a5efad1be3395aae732874e7a770f64b
ms.lasthandoff: 04/06/2017


---
# <a name="batch-service-quotas-and-limits"></a>Batch-Dienst – Kontingente und Limits

Ebenso wie bei anderen Azure-Diensten gelten bei bestimmten Ressourcen in Verbindung mit dem Batch-Dienst Limits. Viele dieser Limits sind Standardkontingente, die von Azure auf Abonnement- oder Kontoebene angewendet werden. In diesem Artikel werden diese Standardwerte erläutert, und Sie erfahren, wie Sie Kontingenterhöhungen anfordern können.

Bedenken Sie diese Kontingente beim Entwerfen und Skalieren Ihrer Batch-Workloads. Wenn Ihr Pool beispielsweise nicht die von Ihnen vorgegebene Anzahl von Computeknoten erreicht, haben Sie möglicherweise das Kernkontingentlimit für Ihr Batch-Konto erreicht.

Sie können mehrere Batch-Workloads in einem Batch-Konto ausführen oder Ihre Workloads auf Batch-Konten in demselben Abonnement, aber verschiedenen Azure-Regionen aufteilen.

Wenn Sie Produktionsworkloads in Batch ausführen möchten, müssen Sie möglicherweise ein oder mehrere Kontingente über den Standardwert erhöhen. Wenn Sie ein Kontingent erhöhen möchten, können Sie kostenlos eine [Anfrage an den Onlinekundensupport](#increase-a-quota) richten.

> [!NOTE]
> Bei einem Kontingent handelt es sich um ein Kreditlimit und keine Kapazitätsgarantie. Wenn Sie einen umfangreichen Kapazitätsbedarf haben, wenden Sie sich an den Azure-Support.
> 
> 

## <a name="resource-quotas"></a>Ressourcenkontingente
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Andere Limits
| **Ressource** | **Maximales Limit** |
| --- | --- |
| [Gleichzeitige Aufgaben](batch-parallel-node-tasks.md) pro Computeknoten |4 x Anzahl der Kerne des Knotens |
| [Anwendungen](batch-application-packages.md) pro Batch-Konto |20 |
| Anwendungspakete pro Anwendung. |40 |
| Anwendungspaketgröße (jeweils) |Ca. 195 GB<sup>1</sup> |

<sup>1</sup> Azure Storage-Limit für die maximale Blockblobgröße

## <a name="view-batch-quotas"></a>Anzeigen von Batch-Kontingenten
Sie können die Kontingente Ihres Batch-Kontos im [Azure-Portal][portal] anzeigen.

1. Wählen Sie **Batch-Konten** im Portal, und wählen Sie dann das Batch-Konto, das Sie interessiert.
2. Wählen Sie **Eigenschaften** im Menüblatt des Batch-Kontos.
3. Auf dem Blatt „Eigenschaften“ werden die **Kontingente** angezeigt, die derzeit für das Batch-Konto gelten.
   
    ![Batch-Kontokontingente][account_quotas]

## <a name="increase-a-quota"></a>Erhöhen eines Kontingents
Führen Sie diese Schritte aus, um eine Kontingenterhöhung über das [Azure-Portal][portal] anzufordern.

1. Wählen Sie auf Ihrem Portaldashboard die Kachel **Hilfe und Support** oder das Fragezeichen (**?**) in der oberen rechten Ecke des Portals.
2. Wählen Sie **Neue Supportanfrage** > **Grundlagen** aus.
3. Gehen Sie auf dem Blatt **Grundlagen** folgendermaßen vor:
   
    a. **Problemtyp** > **Kontingent**
   
    b. Wählen Sie Ihr Abonnement aus.
   
    c. **Kontingenttyp** > **Batch**
   
    d. **Supportplan** > **Kontingentsupport – inbegriffen**
   
    Klicken Sie auf **Weiter**.
4. Gehen Sie auf dem Blatt **Problem** folgendermaßen vor:
   
    a. Wählen Sie einen **Schweregrad** gemäß der [geschäftlichen Auswirkung][support_sev] aus.
   
    b. Geben Sie unter **Details**jedes Kontingent an, das Sie ändern möchten, sowie den Batch-Kontonamen und das neue Limit.
   
    Klicken Sie auf **Weiter**.
5. Gehen Sie auf dem Blatt **Kontaktinformationen** folgendermaßen vor:
   
    a. Wählen Sie eine **bevorzugte Kontaktmethode**aus.
   
    b. Überprüfen Sie die erforderlichen Kontaktdetails, und geben Sie sie ein.
   
    Klicken Sie auf **Erstellen** , um die Supportanfrage zu übermitteln.

Nachdem Sie die Supportanfrage übermittelt haben, wird sich der Azure-Support mit Ihnen in Verbindung setzen. Beachten Sie, dass die Bearbeitung der Anfrage bis zu zwei Werktage in Anspruch nehmen kann.

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen und Verwalten eines Azure Batch-Kontos im Azure-Portal](batch-account-create-portal.md)
* [Übersicht über Azure Batch-Features](batch-api-basics.md)
* [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG

