<properties
	pageTitle=" Skalieren von Streamingendpunkten mithilfe des Azure-Portals | Microsoft Azure"
	description="Dieses Tutorial führt Sie durch die Schritte zur Skalierung von Streamingendpunkten mithilfe des Azure-Portals."
	services="media-services"
	documentationCenter=""
	authors="Juliako"
	manager="erikre"
	editor=""/>

<tags
	ms.service="media-services"
	ms.workload="media"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/29/2016"
	ms.author="juliako"/>


# Skalieren von Streamingendpunkten mithilfe des Azure-Portals

##Übersicht

> [AZURE.NOTE] Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/).

Wenn Sie mit Azure Media Services arbeiten, besteht eines der häufigsten Szenarien darin, Videos per Adaptive Bitrate Streaming an Ihre Clients zu übermitteln. Von Media Services werden die folgenden Streamingtechnologien mit adaptiver Bitrate unterstützt: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH und HDS (nur mit Adobe PrimeTime/Access-Lizenz).

Media Services bietet dynamische Paketerstellung für die Just-in-Time-Übermittlung von Daten vom Typ „MP4-codierte Inhalte mit adaptiver Bitrate“ in Streamingformaten, die von Media Services unterstützt werden (MPEG DASH, HLS, Smooth Streaming, HDS), ohne dass Sie jeweils vorab verpackte Versionen dieser Streamingformate speichern müssen.

Um die dynamische Paketerstellung nutzen zu können, müssen Sie folgende Schritte ausführen:

- Codieren Ihrer Zwischendatei (Quelle) in einen Satz von MP4-Dateien mit adaptiver Bitrate (die Codierungsschritte werden weiter unten in diesem Tutorial beschrieben)
- Erstellen von mindestens einer Streamingeinheit für den *Streamingendpunkt*, von dem aus Sie die Bereitstellung Ihrer Inhalte planen In den folgenden Schritten wird gezeigt, wie Sie die Anzahl von Streamingeinheiten ändern.

Mit der dynamischen Paketerstellung müssen Sie die Dateien nur in einem Speicherformat speichern und bezahlen. Media Services erstellt und verarbeitet die entsprechende Antwort basierend auf Anforderungen von einem Client.

Darüber hinaus können Sie die Kapazität des Streamingendpunktdiensts für die Verarbeitung wachsender Bandbreitenanforderungen steuern, indem Sie die Streamingeinheiten anpassen. Es wird empfohlen, für Anwendungen in der Produktionsumgebung eine oder mehrere Skalierungseinheiten zu reservieren. Streamingeinheiten stellen dedizierte Ausgangskapazitäten bereit, die in Schritten zu jeweils 200 Mbit/s erworben werden können, sowie zusätzliche Funktionen, die Folgendes umfassen: [dynamische Paketerstellung](media-services-dynamic-packaging-overview.md), CDN-Integration und erweiterte Konfiguration. Weitere Informationen finden Sie unter [Verwalten von Streamingendpunkten mithilfe des Azure-Portals](media-services-portal-manage-streaming-endpoints.md).

## Skalieren von Streamingendpunkten

Gehen Sie wie folgt vor, um die Anzahl von Einheiten zu erstellen und zu ändern, die für das Streaming reserviert sind:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im Fenster **Einstellungen** auf **Streamingendpunkte**.
3. Klicken Sie auf den Streamingendpunkt, den Sie skalieren möchten.
4. Verschieben Sie den Schieberegler, um die Anzahl der Streamingeinheiten anzugeben.
 
![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

Es gelten die folgenden Bedingungen:

- Die Zuordnung neuer Streamingeinheiten dauert ca. 20 Minuten.
- Aktuell kann das bedarfsgesteuerte Streaming für bis zu eine Stunde deaktiviert werden, wenn Sie einen positiven Wert für die Streaming-Einheiten zurück auf null setzen.
- Die höchste für den 24-Stunden-Zeitraum angegebene Anzahl an Einheiten wird zum Berechnen der Kosten verwendet. Informationen zu den Preisen finden Sie unter [Mediendienste – Preisübersicht](http://go.microsoft.com/fwlink/?LinkId=275107).

##Nächste Schritte

Überprüfen Sie die Media Services-Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0831_2016-->