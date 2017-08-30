---
title: "Azure-Speicherdienstverschlüsselung mit vom Kunden verwalteten Schlüssel in Azure Key Vault | Microsoft-Dokumentation"
description: "Mit der Azure-Speicherdienstverschlüsselung können Sie Ihren Azure Blob Storage auf Dienstseite beim Speichern von Daten verschlüsseln und wieder entschlüsseln, wenn Daten mit vom Kunden verwalteten Schlüsseln abgerufen werden."
services: storage
documentationcenter: .net
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: lakasa
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 6d1e6752fb631114f5be06cb27a63e40547bf6ca
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Speicherdienstverschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Key Vault

Es ist uns ein Anliegen, dass Microsoft Azure Sie beim Schutz Ihrer Daten gemäß den Sicherheits- und Konformitätsanforderungen Ihrer Organisation unterstützt.  Eine Funktion, mit der Sie Ihre ruhenden Daten schützen können, ist die Speicherdienstverschlüsselung (Storage Service Encryption, SSE), die Ihre Daten automatisch verschlüsselt, während sie in den Speicher geschrieben werden, und sie beim Abrufen auch wieder entschlüsselt. Die Ver- und Entschlüsselung erfolgt automatisch und vollständig transparent. Außerdem wird eine 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) verwendet, eine der stärksten verfügbaren Blockchiffren.

Mit SSE können von Microsoft verwaltete oder eigene Verschlüsselungsschlüssel verwendet werden. In diesem Artikel werden die letztgenannten behandelt. Weitere Informationen zur Verwendung von von Microsoft verwalteten Schlüsseln oder SSE im Allgemeinen finden Sie unter [Azure-Speicherverschlüsselung für ruhende Daten](../storage-service-encryption.md).

Damit Sie Ihre eigenen Verschlüsselungsschlüssel verwenden können, wird SSE für Blob Storage in Azure Key Vault (AKV) integriert. So haben Sie die Möglichkeit, Ihre eigenen Verschlüsselungsschlüssel zu erstellen und in AKV zu speichern oder mit AKV-APIs Verschlüsselungsschlüssel zu generieren. AKV bietet Ihnen nicht nur die Möglichkeit, Ihre Schlüssel zu verwalten und zu steuern, sondern auch die Verwendung der Schlüssel zu überwachen. 

Warum sollten Sie Ihre eigenen Schlüssel erstellen? Sie erhalten dadurch mehr Flexibilität, sodass Sie Zugriffssteuerelemente erstellen, drehen, deaktivieren und definieren können. Außerdem können Sie die zum Schutz Ihrer Daten verwendeten Verschlüsselungsschlüssel überwachen.

## <a name="sse-with-customer-managed-keys-preview"></a>Vorschauversion von SSE mit vom Kunden verwalteten Schlüsseln

Diese Funktion steht derzeit als Vorschau zur Verfügung. Um sie nutzen zu können, müssen Sie ein neues Speicherkonto erstellen. Den Schlüsseltresor und Schlüssel können Sie entweder neu erstellen oder bereits vorhandene verwenden. Das Speicherkonto und der Schlüsseltresor müssen sich in derselben Region befinden, dürfen aber zu verschiedenen Abonnements gehören.

Um an der SSE-Vorschauversion teilzunehmen, wenden Sie sich an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com). Für die Teilnahme stellen wir Ihnen dann einen speziellen Link bereit.

Weitere Informationen finden Sie in den [häufig gestellten Fragen](#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).

> [!IMPORTANT]
> Bevor Sie die Schritte in diesem Artikel ausführen, müssen Sie sich für die Vorschauversion registrieren. Ohne Zugriff auf die Vorschau, werden Sie diese Funktion im Portal nicht aktivieren können.

## <a name="getting-started"></a>Erste Schritte
## <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Schritt 1: [Erstellen eines neuen Speicherkontos](../storage-create-storage-account.md)

## <a name="step-2-enable-encryption"></a>Schritt 2: Aktivieren der Verschlüsselung
Sie können die SSE über das [Azure-Portal](https://portal.azure.com) für das Speicherkonto aktivieren. Suchen Sie auf dem Blatt „Einstellungen“ für das Speicherkonto nach dem Abschnitt „Blob-Dienst“, wie in der nachstehenden Abbildung gezeigt, und klicken Sie auf „Verschlüsselung“.

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)
<br/>*Aktivieren von SSE für den Blob-Dienst*

Wenn Sie SSE programmgesteuert für ein Speicherkonto aktivieren oder deaktivieren möchten, können Sie die [REST-API des Azure Storage-Ressourcenanbieters](https://docs.microsoft.com/en-us/rest/api/storagerp/?redirectedfrom=MSDN), die [Clientbibliothek des Speicherressourcenanbieters für .NET](https://docs.microsoft.com/en-us/dotnet/api/?redirectedfrom=MSDN), [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-4.0.0) oder die [Azure-CLI](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli) verwenden.

Wenn Sie auf diesem Bildschirm das Kontrollkästchen „Eigenen Schlüssel verwenden“ nicht angezeigt wird, wurden Sie nicht für die Vorschau zugelassen. Senden Sie eine E-Mail an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com), und fordern Sie eine Genehmigung an.

![Screenshot des Portals mit der Verschlüsselungsvorschau](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

Standardmäßig verwendet SSE verwaltete Schlüssel von Microsoft. Aktivieren Sie das Kontrollkästchen, um Ihre eigenen Schlüssel zu verwenden. Anschließend können Sie entweder Ihren Schlüssel-URI angeben oder einen Schlüssel und Schlüsseltresor aus der Auswahl auswählen.

## <a name="step-3-select-your-key"></a>Schritt 3: Auswählen Ihres Schlüssels

![Screenshot des Portals: Verschlüsselungsoption „Eigenen Schlüssel verwenden“](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

![Screenshot des Portals: Verschlüsselungsoption „Schlüssel-URI eingeben“](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Wenn das Speicherkonto keinen Zugriff auf den Schlüsseltresor hat, können Sie den Speicherkonten mithilfe des folgenden Befehls in Azure PowerShell Zugriff auf den erforderlichen Schlüsseltresor gewähren.

![Screenshot des Portals: Zugriff auf den Schlüsseltresor verweigert](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Sie können den Zugriff auf das Speicherkonto auch über das Azure-Portal gewähren, indem Sie dort zu Azure Key Vault gehen.

## <a name="step-4-copy-data-to-storage-account"></a>Schritt 4: Kopieren von Daten in das Speicherkonto
Wenn Sie Daten in das neue Speicherkonto übertragen möchten, damit sie verschlüsselt werden, lesen Sie [Step 3: Copy data to storage account](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-3-copy-data-to-storage-account) (Schritt 3: Kopieren von Daten in das Speicherkonto) im Artikel „Azure Storage Service Encryption for Data at Rest“ (Azure-Speicherdienstverschlüsselung für ruhende Daten).

## <a name="step-5-query-the-status-of-the-encrypted-data"></a>Schritt 5: Abfragen des Status der verschlüsselten Daten
Weitere Informationen zum Abfragen des Status der verschlüsselten Daten finden Sie im Abschnitt [Step 4: Query the status of the encrypted data](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-4-query-the-status-of-the-encrypted-data) (Schritt 4: Abfragen des Status der verschlüsselten Daten) im Artikel „Azure Storage Service Encryption for Data at Rest“ (Azure-Speicherdienstverschlüsselung für ruhende Daten).

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Häufig gestellte Fragen zu Storage Service Encryption für ruhende Daten
**F: Ich verwende die Storage Premium. Kann ich SSE mit vom Kunden verwalteten Schlüsseln verwenden?**

A: Ja. SSE mit von Microsoft verwalteten und vom Kunden verwalteten Schlüsseln wird sowohl im Standardspeicher als auch in Storage Premium unterstützt. 

**F: Kann ich neue Speicherkonten, bei denen SSE und vom Kunden verwaltete Schlüssel aktiviert sind, mit Azure PowerShell und Azure-CLI erstellen?**

A: Ja.

**F: Wie viel teurer ist Azure Storage, wenn SSE mit vom Kunden verwalteten aktiviert ist?**

A: Die Verwendung von Azure Key Vault ist nicht kostenlos. Weitere Informationen finden Sie unter [Key Vault – Preise](https://azure.microsoft.com/en-us/pricing/details/key-vault/). Für SSE werden keine zusätzlichen Kosten in Rechnung gestellt.

**F: Kann ich den Zugriff auf die Verschlüsselungsschlüssel widerrufen?**

A: Ja, Sie können den Zugriff jederzeit widerrufen. Dazu stehen Ihnen mehrere Möglichkeiten zur Verfügung. Weitere Informationen finden Sie unter [Azure Key Vault – PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.0.0) und [Azure Key Vault – CLI](https://docs.microsoft.com/en-us/cli/azure/keyvault). Durch das Widerrufen des Zugriffs wird der Zugriff auf alle Blobs im Speicherkonto blockiert, da Azure Storage keinen Zugriff mehr auf den Kontoverschlüsselungsschlüssel hat.

**F: Kann ich ein Speicherkonto und einen Schlüssel in unterschiedlichen Regionen erstellen?**

A: Nein, das Speicherkonto und der Schlüsseltresor/Schlüssel müssen sich in der gleichen Region befinden. 

**F: Kann ich SSE mit vom Kunden verwalteten Schlüsseln aktivieren, während ich ein Speicherkonto erstelle?**

A: Nein. Wenn Sie SSE während der Erstellung des Speicherkontos aktivieren, können Sie nur von Microsoft verwaltete Schlüssel verwenden. Wenn Sie vom Kunden verwaltete Schlüssel verwenden möchten, müssen Sie die Speicherkontoeigenschaften aktualisieren. Sie können REST oder eine der Speicherclientbibliotheken verwenden, um Ihr Speicherkonto programmgesteuert zu aktualisieren, oder die Eigenschaften des Speicherkontos nach dem Erstellen mithilfe des Azure-Portals aktualisieren.

**F: Kann ich die Verschlüsselung während der Verwendung von SSE mit vom Kunden verwalteten Schlüsseln deaktivieren?**

A: Nein, die Verschlüsselung kann während der Verwendung von SSE mit vom Kunden verwalteten Schlüsseln nicht deaktiviert werden. Um die Verschlüsselung zu deaktivieren, müssen Sie von Microsoft verwaltete Schlüssel verwenden. Diesen Wechsel können Sie mithilfe des Azure-Portals oder mit PowerShell durchführen.

**F: Ist SSE standardmäßig aktiviert, wenn ich ein neues Speicherkonto erstelle?**

A: SSE ist nicht standardmäßig aktiviert. Sie können dieses Feature im Azure-Portal aktivieren. Außerdem können Sie dieses Feature programmgesteuert mit der Speicherressourcenanbieter-REST-API aktivieren. 

**F: Ich kann für mein Speicherkonto keine Verschlüsselung aktivieren.**

A: Handelt es sich um ein Resource Manager-Speicherkonto? Klassische Speicherkonten werden nicht unterstützt. SSE mit vom Kunden verwalteten Schlüsseln kann auch nur auf neu erstellten Resource Manager-Speicherkonten aktiviert werden.

**F: Ist SSE mit vom Kunden verwalteten Schlüsseln nur in bestimmten Regionen zulässig?**

A: SSE ist nur in bestimmten Regionen für Blob Storage für diese Vorschau verfügbar. Senden Sie eine E-Mail an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com), um Informationen über Verfügbarkeit und Details zur Vorschauversion zu erhalten. 

**F: Wohin wende ich mich, wenn Probleme auftreten oder ich Feedback geben möchte?**

A: Wenden Sie sich in allen Angelegenheiten, die Storage Service Encryption betreffen, an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com). 

## <a name="next-steps"></a>Nächste Schritte

*   Weitere Informationen zu allen Sicherheitsfunktionen, die Entwickler beim Erstellen sicherer Anwendungen unterstützen, finden Sie im [Azure Storage-Sicherheitsleitfaden](https://docs.microsoft.com/en-us/azure/storage/storage-security-guide).
*   Eine Übersicht über Azure Key Vault finden Sie unter [Was ist Azure Key Vault?](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis).
*   Informationen zu den ersten Schritten mit Azure Key Vault finden Sie unter [Erste Schritte mit Azure Key Vault](../../key-vault/key-vault-get-started.md).

