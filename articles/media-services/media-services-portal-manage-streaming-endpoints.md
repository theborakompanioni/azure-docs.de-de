---
title: Verwalten von Streamingendpunkten mithilfe des Azure-Portals | Microsoft Docs
description: Diese Thema zeigt, wie Sie Streamingendpunkte mithilfe des Azure-Portals verwalten.
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: 80ba024cd219b10bce8d88c9f11fd2d14d4ed34f
ms.openlocfilehash: 223fee8930b1aebff94eddaeb82b5c0e364e067c
ms.contentlocale: de-de
ms.lasthandoff: 01/11/2017

---


# <a name="manage-streaming-endpoints-with-the-azure-portal"></a>Verwalten von Streamingendpunkten mithilfe des Azure-Portals

In diesem Thema wird erläutert, wie Streamingendpunkte über das Azure-Portal verwaltet werden. 

>[!NOTE]
>Sehen Sie sich zunächst das Thema [Übersicht](media-services-streaming-endpoints-overview.md) an. 

Weitere Informationen zum Skalieren des Streamingendpunkts finden Sie in [diesem](media-services-portal-scale-streaming-endpoints.md) Thema.

## <a name="start-managing-streaming-endpoints"></a>Starten der Verwaltung von Streamingendpunkten 

Gehen Sie wie folgt vor, um mit dem Verwalten von Streamingendpunkten für Ihr Konto zu beginnen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Azure Media Services-Konto aus.
2. Klicken Sie auf dem Blatt **Einstellungen** auf **Streamingendpunkte**.
   
    ![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> Abgerechnet werden nur ausgeführte Streamingendpunkte.

## <a name="adddelete-a-streaming-endpoint"></a>Hinzufügen und Löschen eines Streamingendpunkts

>[!NOTE]
>Der standardmäßige Streamingendpunkt kann nicht gelöscht werden.

Führen Sie folgende Schritte aus, um einen Streamingendpunkt mithilfe des Azure-Portals hinzuzufügen oder zu löschen:

1. Um einen Streamingendpunkt hinzuzufügen, klicken Sie oben auf der Seite auf **+ Endpunkt** . 

    Es empfehlen sich ggf. mehrere Streamingendpunkte, wenn Sie planen, mit verschiedenen CDNs oder mit einem CDN und Direktzugriff zu arbeiten.

2. Um einen Streamingendpunkt zu löschen, klicken Sie auf die Schaltfläche **Löschen** .      
3. Klicken Sie auf die Schaltfläche **Starten** , um den Streamingendpunkt zu starten.
   
    ![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a id="configure_streaming_endpoints"></a>Konfigurieren des Streamingendpunkts
Mit einem Streamingendpunkt können Sie die folgenden Eigenschaften konfigurieren:

* Zugriffssteuerung
* Cachesteuerelement
* Websiteübergreifende Zugriffsrichtlinien

Ausführliche Informationen zu diesen Eigenschaften finden Sie unter [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

Sie können den Streamingendpunkt wie folgt konfigurieren:

1. Wählen Sie den Streamingendpunkt aus, den Sie konfigurieren möchten.
2. Klicken Sie auf **Einstellungen**.

Eine kurze Beschreibung der Felder folgt.

![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. Maximale Cacherichtlinie: Dient zum Konfigurieren der Cachelebensdauer von Medienobjekten, die über diesen Streamingendpunkt bereitgestellt werden. Wenn kein Wert festgelegt ist, wird der Standardwert verwendet. Die Standardwerte können auch direkt im Azure-Speicher definiert werden. Wenn Azure CDN für den Streamingendpunkt aktiviert ist, darf die Cacherichtlinie nicht auf einen Wert unter 600 Sekunden festgelegt werden.  
2. Zulässige IP-Adressen: Dient zum Angeben von IP-Adressen, die sich mit dem veröffentlichten Streamingendpunkt verbinden dürfen. Falls keine IP-Adressen angegeben sind, dürfen sich alle IP-Adressen verbinden. IP-Adressen können als einzelne IP-Adresse (z.B. 10.0.0.1), IP-Adressbereiche mit einer IP-Adresse und einer CIDR-Subnetzmaske (z.B. 10.0.0.1/22) oder IP-Adressbereiche mit einer IP-Adresse und einer Subnetzmaske in Punkt-Dezimalschreibweise (z.B. 10.0.0.1(255.255.255.0)) angegeben werden.
3. Konfiguration für die Unterschriftenkopfzeilen-Authentifizierung von Akamai: Dient zum Angeben, wie die Unterschriftenkopfzeilen-Authentifizierungsanforderung von Akamai-Servern konfiguriert ist. Die Ablaufzeit ist in UTC angegeben.

## <a name="scale-your-premium-streaming-endpoint"></a>Skalieren Sie Ihren Premium-Streamingendpunkt.

Weitere Informationen finden Sie in [diesem](media-services-portal-scale-streaming-endpoints.md) Thema.

## <a id="enable_cdn"></a>Aktivieren der Azure CDN-Integration

Wenn Sie ein neues Konto erstellen, ist die Streamingendpunkt-/Azure CDN-Integration standardmäßig aktiviert.

Wenn Sie das CDN später deaktivieren bzw. aktivieren möchten, muss Ihr Streamingendpunkt den Zustand **Beendet** haben. Es kann bis zu zwei Stunden dauern, bis die Azure CDN-Integration aktiviert ist und die Änderungen auf allen CDN-POPs aktiv sind. Sie können jedoch Ihren Streamingendpunkt starten und von ihm aus unterbrechungsfrei streamen. Sobald die Integration abgeschlossen ist, wird der Stream vom CDN übermittelt. Während der Bereitstellungsphase hat Ihr Streamingendpunkt den Zustand **Wird gestartet**, und seine Leistung ist ggf. eingeschränkt.

Die CDN-Integration ist in allen Azure-Rechenzentren mit Ausnahme der Regionen China und US-Regierung aktiviert.

Nach der Aktivierung wird die Konfiguration von **Zugriffssteuerung**, **Benutzerdefinierter Hostname** und **Akamai-Signaturauthentifizierung** deaktiviert.
 
> [!IMPORTANT]
> Die Azure Media Services-Integration in Azure CDN ist in **Azure CDN von Verizon**für Standard-Streamingendpunkte implementiert. Premium-Streamingendpunkte können mithilfe aller **Azure CDN-Tarife und -Anbieter** konfiguriert werden. Weitere Informationen zu Azure CDN-Features finden Sie in der [Übersicht über das Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md).
 
### <a name="additional-considerations"></a>Zusätzliche Überlegungen

* Wenn CDN für einen Streamingendpunkt aktiviert ist, können die Clients keine Inhalte direkt vom Ursprung anfordern. Wenn Sie Ihre Inhalte mit oder ohne CDN testen können möchten, können Sie einen anderen Streamingendpunkt erstellen, für den kein CDN aktiviert ist.
* Der Hostname für den Streamingendpunkt bleibt nach dem Aktivieren von CDN gleich. Sie müssen keine Änderungen an Ihrem Media Services-Workflow vornehmen, nachdem CDN aktiviert wurde. Wenn der Hostname des Streamingendpunkts z. B. "strasbourg.streaming.mediaservices.windows.net" lautet, wird nach der Aktivierung von CDN genau der gleiche Hostname verwendet.
* Für neue Streamingendpunkte können Sie CDN einfach aktivieren, indem Sie einen neuen Endpunkt erstellen. Für vorhandene Streamingendpunkte müssen Sie zuerst den Endpunkt beenden und dann das CDN aktivieren/deaktivieren.
* Standard-Streamingendpunkte können nur mithilfe des **Verizon Standard CDN-Anbieters** im Azure-Verwaltungsportal konfiguriert werden. Sie können jedoch andere Azure CDN-Anbieter mithilfe von REST-APIs aktivieren.

## <a name="configure-cdn-profile"></a>Konfigurieren des CDN-Profils

Sie können das CDN-Profil konfigurieren, indem Sie oben auf die Schaltfläche **CDN verwalten** klicken.

![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


