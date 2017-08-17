---
title: "Optimieren der Inhaltsübermittlung von Azure für Ihr Szenario"
description: "Optimieren der Übermittlung Ihrer Inhalte für bestimmte Szenarien"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: 98941c49b057380b3ef9164515bcc2a63ccb56ce
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Optimieren der Inhaltsübermittlung von Azure für Ihr Szenario

Wenn Sie Inhalte an eine große globale Zielgruppe übermitteln, muss die Übermittlung optimiert werden. Das Azure Content Delivery Network kann die Übermittlung basierend auf der Art der zu übermittelnden Inhalte optimieren. Bei den Inhalten kann es sich um eine Website, einen Livestream, ein Video oder eine große Downloaddatei handeln. Beim Erstellen eines CDN-Endpunkts (Content Delivery Network) geben Sie in der Option **Optimiert für** ein Szenario an. Ihre Auswahl bestimmt, welche Optimierung auf die Inhalte angewendet wird, die von dem CDN-Endpunkt übermittelt werden.

Die Optimierungsoptionen basieren auf bewährten Maßnahmen zur Verbesserung der Übermittlungsleistung und zur Entlastung des Ursprungsservers. Die gewählten Szenariooptionen passen die Konfigurationen für teilweises Zwischenspeichern, Objektblockerstellung und die Wiederholungsrichtlinie für Ursprungsfehler an und beeinflussen dadurch die Leistung. 

Dieser Artikel enthält eine Übersicht über verschiedene Optimierungsfeatures sowie Informationen zu deren Verwendung. Weitere Informationen zu Features und Einschränkungen finden Sie in den entsprechenden Artikeln zum jeweiligen Optimierungstyp.

> [!NOTE]
> Die Optionen unter **Optimiert für** können sich abhängig vom gewählten Anbieter unterscheiden. CDN-Anbieter wenden Verbesserungen je nach Szenario auf unterschiedliche Weise an. 

## <a name="provider-options"></a>Anbieteroptionen

Das Azure Content Delivery Network von Akamai unterstützt Folgendes:

* Allgemeine Webbereitstellung 

* Allgemeines Medienstreaming

* Video on Demand-Medienstreaming

* Download großer Dateien

* Beschleunigung dynamischer Websites 

Das Azure Content Delivery Network von Verizon unterstützt nur die allgemeine Webbereitstellung. Es kann für Video on Demand sowie zum Herunterladen großer Dateien verwendet werden. Sie müssen keinen Optimierungstyp auswählen.

Es empfiehlt sich, die Leistungsunterschiede zwischen verschiedenen Anbietern zu testen, um den optimalen Anbieter für die Übermittlung zu finden.

## <a name="select-and-configure-optimization-types"></a>Auswählen und Konfigurieren von Optimierungstypen

Wählen Sie zum Erstellen eines neuen Endpunkts einen Optimierungstyp aus, der am besten zu Ihrem Szenario und zur Art der Inhalte passt, die Sie über den Endpunkt übermittelt möchten. Standardmäßig ist **Allgemeine Webbereitstellung** ausgewählt. Die Optimierungsoption kann für jeden beliebigen vorhandenen Akamai-Endpunkt jederzeit aktualisiert werden. Die Übermittlung über das CDN wird durch diese Änderung nicht beeinträchtigt. 

1. Wählen Sie einen Endpunkt innerhalb eines Akamai-Standardprofils aus.

    ![Endpunktauswahl ](./media/cdn-optimization-overview/01_Akamai.png)

2. Wählen Sie unter **EINSTELLUNGEN** die Option **Optimierung** aus. Wählen Sie anschließend in der Dropdownliste **Optimiert für** einen Typ aus.

    ![Auswählen von Optimierung und Typ](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optimierung für bestimmte Szenarien

Der CDN-Endpunkt kann für eines der folgenden Szenarien optimiert werden: 

### <a name="general-web-delivery"></a>Allgemeine Webbereitstellung

Allgemeine Webbereitstellung ist die am häufigsten verwendete Optimierungsoption. Sie wird für die Optimierung allgemeiner Webinhalte wie Webseiten und Webanwendungen verwendet. Die Optimierung eignet sich aber auch für Datei- und Videodownloads.

Eine typische Website enthält sowohl statische als auch dynamische Inhalte. Statische Inhalte sind unter anderem Bilder, JavaScript-Bibliotheken und Stylesheets, die zwischengespeichert und an verschiedene Benutzer übermittelt werden können. Dynamische Inhalte werden für den jeweiligen Benutzer personalisiert. Hierzu zählen beispielsweise speziell auf ein Benutzerprofil zugeschnittene Nachrichten. Dynamische Inhalte werden nicht zwischengespeichert, da es sich hierbei um benutzerspezifische Inhalte wie etwa den Inhalt eines Einkaufswagens handelt. Die allgemeine Webbereitstellung kann Ihre gesamte Website optimieren. 

> [!NOTE]
> Bei Verwendung des Azure Content Delivery Networks von Akamai können Sie diese Optimierung verwenden, wenn ihre Dateien im Schnitt kleiner als 10 MB sind. Wenn Ihre Dateien im Schnitt größer als 10 MB sind, wählen Sie in der Dropdownliste **Optimiert für** die Option **Download großer Dateien** aus.

### <a name="general-media-streaming"></a>Allgemeines Medienstreaming

Wenn Sie den Endpunkt für Livestreaming und Video on Demand-Streaming verwenden möchten, empfehlen wir die Option „Allgemeines Medienstreaming“.

Medienstreaming ist ein zeitkritischer Prozess, da das verspätete Eintreffen von Paketen beim Client häufiges Puffern des Videos und damit eine Beeinträchtigung der Wiedergabe zur Folge haben kann. Die Medienstreaming-Optimierung verringert die Wartezeit bei der Übermittlung von Medieninhalten und bietet dem Benutzer reibungsloses Streaming. 

Hierbei handelt es sich um ein gängiges Szenario für Azure Media Services-Kunden. Wenn Sie Azure Media Services verwenden, erhalten Sie einen Streamingendpunkt, der für Live- und On-Demand-Streaming verwendet werden kann. Dadurch müssen Kunden keinen anderen Endpunkt verwenden, wenn sie von Livestreaming zu On-Demand-Streaming wechseln. Dieses Szenario wird von der Optimierungsoption „Allgemeines Medienstreaming“ unterstützt.

Das Azure Content Delivery Network von Verizon verwendet zur Übermittlung von Streamingmedieninhalten den Optimierungstyp „Allgemeine Webbereitstellung“.

Weitere Informationen zur Medienstreaming-Optimierung finden Sie unter [Medienstreaming-Optimierung mit Azure CDN](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Video on Demand-Medienstreaming

Die Optimierung „Video on Demand-Medienstreaming“ verbessert das Streamen von Video on Demand-Inhalten. Diese Option empfiehlt sich, wenn Sie einen Endpunkt für Video on Demand-Streaming verwenden.

Das Azure Content Delivery Network von Verizon verwendet zur Übermittlung von Streamingmedieninhalten den Optimierungstyp „Allgemeine Webbereitstellung“.

Weitere Informationen zur Medienstreaming-Optimierung finden Sie unter [Medienstreaming-Optimierung mit Azure CDN](cdn-media-streaming-optimization.md).

> [!NOTE]
> Verwenden Sie diesen Optimierungstyp, wenn der Endpunkt in erster Linie Video on Demand-Inhalte bereitstellt. Der Hauptunterschied zwischen dieser Optimierung und der Optimierung „Allgemeines Medienstreaming“ ist das Timeout für wiederholte Verbindungsversuche. Für Livestreamingszenarien wird ein erheblich kürzeres Timeout verwendet.

### <a name="large-file-download"></a>Download großer Dateien

Bei Verwendung des Azure Content Delivery Networks von Akamai müssen Sie die Option „Download großer Dateien“ verwenden, wenn Sie Dateien mit einer Größe von mehr als 1,8 GB bereitstellen möchten. Beim Azure Content Delivery Network von Verizon ist die Dateidownloadgröße in der Optimierung „Allgemeine Webbereitstellung“ nicht begrenzt.

Bei Verwendung des Azure Content Delivery Networks von Akamai ist der Download großer Dateien für Inhalte mit einer Größe von mehr als 10 GB optimiert. Wenn Ihre Dateien im Schnitt kleiner als 10 MB sind, empfiehlt sich die Verwendung der Optimierung „Allgemeine Webbereitstellung“. Wenn Ihre Dateien durchweg größer als 10 MB sind, ist es unter Umständen effizienter, einen separaten Endpunkt für große Dateien zu erstellen. Firmware- oder Softwareupdates sind beispielsweise in der Regel große Dateien.

Das Azure Content Delivery Network von Verizon verwendet zur Übermittlung von Streamingmedieninhalten den Optimierungstyp „Allgemeine Webbereitstellung“.

Weitere Informationen zur Optimierung für große Dateien finden Sie unter [Optimieren großer Dateidownloads mit Azure CDN](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Beschleunigung dynamischer Websites

 Die Beschleunigung dynamischer Websites steht sowohl für Content Delivery Network-Profile von Akamai als auch für Content Delivery Network-Profile von Verizon zur Verfügung. Die Verwendung dieser Optimierung ist mit zusätzlichen Kosten verbunden. Weitere Informationen finden Sie in der Preisübersicht.

Die Beschleunigung dynamischer Websites umfasst verschiedene Maßnahmen zur Verbesserung der Wartezeit und der Leistung dynamischer Inhalte. Hierzu zählen unter anderem Routen- und Netzwerkoptimierungen sowie TCP-Optimierungen. 

Mit dieser Optimierung können Sie eine Web-App mit zahlreichen nicht zwischenspeicherbaren Antworten beschleunigen. Beispiele hierfür wären etwa Suchergebnisse, Auftragsabschlusstransaktionen oder Echtzeitdaten. Für statische Daten können Sie weiterhin die grundlegenden CDN-Cachefunktionen verwenden. 




