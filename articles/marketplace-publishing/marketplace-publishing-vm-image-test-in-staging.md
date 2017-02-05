---
title: "Testen Ihres VM-Angebots für Marketplace | Microsoft Docs"
description: "Grundlegendes zum Testen Ihres VM-Images für Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: ad057a9129156c0ad5094a73c0da2899fc2a3812


---
# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Testen Ihres VM-Angebots für den Azure Marketplace in der Stagingumgebung
In der Stagingumgebung wird Ihr Angebot in einem privaten "Sandkasten" bereitgestellt, in dem Sie seine Funktionalität vor der Bereitstellung in Marketplace testen und überprüfen können. Die SKU wird in der Stagingumgebung genau wie bei einem Kunden angezeigt, der sie bereitgestellt hat. Ihr VM-Image muss für die Freigabe in der Stagingumgebung zertifiziert sein.

## <a name="step-1-push-your-offer-to-staging"></a>Schritt 1: Freigeben Ihres Angebot in der Stagingumgebung
1. Klicken Sie auf der Registerkarte **Veröffentlichen** auf **Für Stagingumgebung freigeben**.
   
    ![Abbildung](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. Sollten Sie vom Veröffentlichungsportal auf Fehler hingewiesen werden, beheben Sie sie.
3. Geben Sie im Dialogfeld für den Zugriff auf Ihr in der Stagingumgebung bereitgestelltes Angebot **** die Azure-Abonnements an, die Sie für die Vorschau Ihres Angebots im [Azure-Vorschauportal](https://portal.azure.com)verwenden möchten.
   
   > [!NOTE]
   > Erstellen Sie bei virtuellen Computern und Lösungsvorlagen bitte **nicht** eine Whitelist von Abonnements der Typen CSP, DreamSpark oder Azure in Open.
   > 
   > 

    > Wenn Sie bei virtuellen Computern auf die Schaltfläche **PUSH TO STAGING**(Für Stagingumgebung freigeben) klicken, werden die folgenden Schritte im Hintergrund ausgeführt. Sie können den Fortschritt der einzelnen Schritte auf der Registerkarte VERÖFFENTLICHEN im Veröffentlichungsportal verfolgen. Sie müssen diese Seite in regelmäßigen Intervallen (bis der Status BEREITGESTELLT angezeigt wird) auf Fehlerinformationen überprüfen, die Korrektur Ihrerseits erfordern.

    > - Ihre Staginganforderung wird zunächst an das Zertifizierungsteam geleitet, das die VHD-Datei überprüft. Wenn Ihre Anforderung allerdings nur einer Marketingänderung entspricht, wird der Zertifizierungsschritt übersprungen.
    > - Wenn die Zertifizierung abgeschlossen ist, startet die Replikation des Angebots in allen Azure-Rechenzentren. In der Regel dauert es 24-48 Stunden, bis die Replikation abgeschlossen hat, aber es kann je nach Größe der virtuellen Festplatte bis zu einer Woche dauern. Wenn Ihre Anforderung allerdings nur einer Marketingänderung entspricht, verläuft die Replikation schneller.
    > - Nach Abschluss der Replikation ist das Angebot im [Azure-Portal](http:/portal.azure.com)verfügbar. Zu diesem Zeitpunkt erhält es den Status BEREITGESTELLT im Veröffentlichungsportal. Ein bereitgestelltes Angebot ist nur dann im [Azure-Portal](http:/portal.azure.com) sichtbar, wenn die E-Mail-IDs verwendet werden, die mit dem Abonnement verknüpft sind, mit dem das Angebot bereitgestellt wird.

1. Melden Sie sich beim [Azure-Vorschauportal](https://portal.azure.com) mit einem der Azure-Abonnements aus dem vorherigen Schritt an.
2. Suchen Sie Ihr Angebot, und überprüfen Sie für Ihr VM-Image die folgenden Punkte:
   
   * Vergewissern Sie sich, dass Marketinginhalte im Marketplace korrekt angezeigt werden.
   * End-to-End-Bereitstellung des VM-Images
     
      ![img-map-portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> Ihr Angebot bleibt so lange in der Stagingphase, bis Sie Microsoft über das Veröffentlichungsportal [Registerkarte **Veröffentlichen** > auf Schaltfläche **Genehmigung für Freigabe in Produktionsumgebungen anfordern** klicken] informieren, dass Ihr Angebot für die Veröffentlichung bereit ist. Vor der Auflistung Ihres Angebots sollte das gesamte Team noch einmal alles überprüfen.
> 
> Die Stagingplattform dient zum Testen des Angebots durch den Herausgeber in einem Vorschaumodus. Wir raten dringend ab, diese Plattform zu kommerziellen Zwecken zu verwenden.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Ihr Angebot in die Stagingphase versetzt wurde und Sie die Funktionalität und die Marketinginhalte getestet haben, können Sie zur letzten Veröffentlichungsphase übergehen: **Schritt 4**: [Bereitstellen Ihres Angebots im Marketplace](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte: Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)




<!--HONumber=Nov16_HO3-->


