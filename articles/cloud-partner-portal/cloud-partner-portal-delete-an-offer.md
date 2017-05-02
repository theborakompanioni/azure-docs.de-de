---
title: "Löschen eines Angebots im Azure Marketplace | Microsoft-Dokumentation"
description: "Dieser Artikel enthält ausführliche Informationen zum Löschen eines Angebots im Azure Marketplace."
services: cloud-partner-portal
documentationcenter: 
author: anuragdalmia
manager: hamidm
ms.service: cloud-partner-portal
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/22/2017
ms.author: andalmia
translationtype: Human Translation
ms.sourcegitcommit: 233d62b176303ca384aae9453c2e125c0c70fad2
ms.openlocfilehash: f09be39b75bd0f39094a2ec08609c0c7ff03bc8e
ms.lasthandoff: 04/12/2017


---
# <a name="delete-an-offersku-from-azure-marketplace"></a>Löschen eines Angebots oder einer SKU im Azure Marketplace

Möglicherweise möchten Sie aus unterschiedlichen Gründen Ihr Angebot aus dem Marketplace entfernen.  Durch das Entfernen eines Angebots wird sichergestellt, dass Ihr Angebot nicht mehr durch neue Kunden erworben oder bereitgestellt werden kann. Dies wirkt sich jedoch nicht auf bestehende Kunden aus. Durch Beenden des Angebots wird der Dienst und/oder der Lizenzvertrag zwischen Ihnen und Ihren bestehenden Kunden beendet.  Eine Anleitung und Richtlinien zur Entfernung und Beendigung von Angeboten finden Sie im [Microsoft Marketplace-Herausgebervertrag](http://go.microsoft.com/fwlink/?LinkID=699560) (siehe Abschnitt 7) und in den [Teilnahmerichtlinien](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) (siehe Abschnitt 6.2).   In diesem Artikel werden die unterschiedlichen unterstützten Szenarien zum Löschen und die dazu ausgeführten Schritte behandelt.

## <a name="delete-a-live-sku-from-azure-marketplace"></a>Löschen einer Live-SKU im Azure Marketplace

Mit den folgenden Schritten können Sie eine Live-SKU im Azure Marketplace löschen:

1.  Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.

2.  Wählen Sie auf der Registerkarte **Alle Angebote** Ihr Angebot aus.

3.  Wählen Sie im Bereich auf der linken Bildschirmseite die Registerkarte **SKUs** aus.

4.  Wählen Sie die SKU aus, die Sie löschen möchten, und klicken Sie auf die Schaltfläche „Löschen“ für diese SKU.

5.  [Veröffentlichen Sie das Angebot erneut](./Cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) im Azure Marketplace.

Nachdem das Angebot im Azure Marketplace live geschaltet wurde, wird die SKU im Azure Marketplace und Azure-Portal gelöscht.

## <a name="rollback-to-a-previous-sku-version"></a>Rollback zu einer vorherigen SKU-Version

Sie können die aktuelle Version einer Live-SKU mit den folgenden Schritten im Azure Marketplace löschen. Nachdem der Vorgang abgeschlossen ist, wird die SKU auf die vorherige Version zurückgesetzt.

1.  Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.

2.  Wählen Sie auf der Registerkarte **Alle Angebote** Ihr Angebot aus.

3.  Wählen Sie im Bereich auf der linken Bildschirmseite die Registerkarte **SKUs** aus.

4.  Löschen Sie die neueste **Datenträgerversion** in der Liste der veröffentlichten Datenträgerversionen.

5.  [Veröffentlichen Sie das Angebot erneut](./Cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) im Azure Marketplace.

Nachdem das Angebot im Azure Marketplace live geschaltet wurde, wird die aktuelle Version der aufgelisteten SKU im Azure Marketplace und Azure-Portal gelöscht. Die SKU wird auf die vorherige Version zurückgesetzt.

## <a name="delete-a-live-offer"></a>Löschen eines Liveangebots

Bei einer Anforderung zum Entfernen eines Liveangebots müssen verschiedene Aspekte berücksichtigt werden. Führen Sie die unten angegebenen Schritte aus, um vom Supportteam Hilfe beim Entfernen eines Liveangebots aus dem Azure Marketplace zu erhalten:

1.  Erstellen Sie über diesen [Link](https://go.microsoft.com/fwlink/?linkid=844975) ein Supportticket.

2.  Wählen Sie **Angebotsverwaltung** in der Liste **Problemtyp** und **Modifying an offer and/or SKU already in production** (Ändern eines Angebots und/oder einer SKU in der Produktion) in der Liste **Kategorie** aus.

3.  Senden Sie die Anforderung.

Das Supportteam führt Sie durch den Vorgang zum Löschen des Angebots.

Beachten Sie, dass das Löschen einer SKU oder eines Angebots sich nicht auf den aktuellen bereits erfolgten Erwerb der SKU bzw. des Angebots auswirkt. Diese sind weiterhin funktionstüchtig. Sie können lediglich zukünftig nicht mehr erworben werden.  
