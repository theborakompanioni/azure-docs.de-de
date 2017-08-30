---
title: "Aktivieren der Verschlüsselung für Speicherkonten in Azure Security Center | Microsoft Docs"
description: "In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlungen in **Aktivieren der Verschlüsselung für Azure Storage-Konten** umsetzen."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: b7b2e8a12cbab68da9c8fcc348e8e3c543607007
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Aktivieren der Verschlüsselung für Azure Storage-Konten in Azure Security Center
Azure Security Center empfiehlt eventuell die Aktivierung der Azure Storage Service Encryption für ruhende Daten.

Storage Service Encryption (SSE) verschlüsselt die Daten, wenn diese in Azure Storage geschrieben werden, und entschlüsselt sie vor dem Abrufen.  SSE steht zurzeit nur für den Azure Blob-Dienst zur Verfügung und kann für Blockblobs, Seitenblobs und Anfügeblobs verwendet werden.  Weitere Informationen finden Sie unter [Storage Service Encryption für ruhende Daten](../storage/common/storage-service-encryption.md).


> [!Note]
> Nach der Aktivierung der Verschlüsselung werden nur neue Daten verschlüsselt. Alle vorhandenen Blobs in Ihrem Speicherkonto bleiben unverschlüsselt. Weitere Informationen zum Verschlüsseln vorhandener Blobs finden Sie unter [Storage Service Encryption – häufig gestellte Fragen](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).
>
>

Storage Service Encryption wird nur für Resource Manager-Speicherkonten unterstützt. Klassische Speicherkonten werden zurzeit nicht unterstützt. Weitere Informationen zum klassischen Bereitstellungsmodell und zum Resource Manager-Bereitstellungsmodell finden Sie unter [Azure-Bereitstellungsmodellen](../azure-classic-rm.md).

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>
>

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Enable encryption for Azure Storage Account** (Verschlüsselung für Azure Storage-Konto aktivieren) aus.
   ![Aktivieren der Verschlüsselung für das Speicherkonto][1]
2. Das Blatt **Enable storage encryption** (Speicherverschlüsselung aktivieren) wird geöffnet. Auf diesem Blatt werden die Azure Storage-Konten aufgeführt, bei denen die Speicherverschlüsselung deaktiviert ist. Wählen Sie in diesem Beispiel **storageacct1** aus.
   ![Aktivieren der Speicherverschlüsselung][2]
3. Das Blatt **Verschlüsselung** für **storageacct1** wird geöffnet. Wählen Sie **Aktiviert**.
   ![Blatt „Verschlüsselung“][3]
4. Wählen Sie **Speichern**aus.

Sie haben nun die Speicherverschlüsselung für **storageacct1** aktiviert.


## <a name="see-also"></a>Weitere Informationen
In diesem Dokument wird erläutert, wie Sie die Security Center-Empfehlungen in „Aktivieren der Verschlüsselung für Azure Storage-Konten“ umsetzen. Weitere Informationen zu Azure Storage Service Encryption finden Sie unter folgenden Ressourcen:

* [Azure Storage Service Encryption für ruhende Daten](../storage/common/storage-service-encryption.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png

