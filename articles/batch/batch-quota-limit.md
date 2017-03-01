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
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6b6c548ca1001587e2b40bbe9ee2fcb298f40d72
ms.openlocfilehash: a0f47a19f7ef1832e64e9a0bdc4bda3434f77aa2
ms.lasthandoff: 02/28/2017


---
# <a name="batch-service-quotas-and-limits"></a>Batch-Dienst – Kontingente und Limits

Ebenso wie bei anderen Azure-Diensten gelten bei bestimmten Ressourcen in Verbindung mit dem Batch-Dienst Limits. Viele dieser Limits sind Standardkontingente, die von Azure auf Abonnement- oder Kontoebene angewendet werden. In diesem Artikel werden diese Standardwerte erläutert, und Sie erfahren, wie Sie Kontingenterhöhungen anfordern können.

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
Führen Sie die unten stehenden Schritte aus, um über das [Azure-Portal][portal] eine Kontingenterhöhung anzufordern.

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

