<properties 
	pageTitle="Verwalten von Streamingendpunkten mithilfe des Azure-Portals | Microsoft Azure" 
	description="Diese Thema zeigt, wie Sie Streamingendpunkte mithilfe des Azure-Portals verwalten." 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	writer="juliako" 
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


#Verwalten von Streamingendpunkten mithilfe des Azure-Portals

## Übersicht

> [AZURE.NOTE] Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/).

In Microsoft Azure Media Services stellt ein **Streamingendpunkt** einen Streamingdienst dar, der Inhalte zur weiteren Verteilung direkt in einer Client-Player-Anwendung oder einem Content Delivery Network (CDN) bereitstellen kann. Media Services bietet auch eine nahtlose Integration von Azure CDN. Der ausgehende Stream des StreamingEndpoint-Diensts kann ein Livestream oder ein Video on Demand-Medienobjekt in Ihrem Media Services-Konto sein.

Darüber hinaus können Sie die Kapazität des Streamingendpunktdiensts für die Verarbeitung wachsender Bandbreitenanforderungen steuern, indem Sie die Streamingeinheiten anpassen. Es wird empfohlen, für Anwendungen in der Produktionsumgebung eine oder mehrere Skalierungseinheiten zu reservieren. Streamingeinheiten stellen dedizierte Ausgangskapazitäten bereit, die in Schritten zu jeweils 200 Mbit/s erworben werden können, sowie zusätzliche Funktionen, die Folgendes umfassen: [dynamische Paketerstellung](media-services-dynamic-packaging-overview.md), CDN-Integration und erweiterte Konfiguration.

>[AZURE.NOTE]Abgerechnet werden nur ausgeführte Streamingendpunkte.

Dieses Thema bietet einen Überblick über die wichtigsten Funktionen, die von Streamingendpunkten bereitgestellt werden. Es wird zudem erläutert, wie Streamingendpunkte über das Azure-Portal verwaltet werden. Weitere Informationen zum Skalieren des Streamingendpunkts finden Sie in [diesem](media-services-portal-scale-streaming-endpoints.md) Thema.

## Starten der Verwaltung von Streamingendpunkten

Gehen Sie wie folgt vor, um mit dem Verwalten von Streamingendpunkten für Ihr Konto zu beginnen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im Fenster **Einstellungen** auf **Streamingendpunkte**.

	![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##Hinzufügen und Löschen eines Streamingendpunkts

Führen Sie folgende Schritte aus, um einen Streamingendpunkt mithilfe des Azure-Portals hinzuzufügen oder zu löschen:

1. Um einen Streamingendpunkt hinzuzufügen, klicken Sie oben auf der Seite auf **+ Endpunkt**.
2. Um einen Streamingendpunkt zu löschen, klicken Sie auf die Schaltfläche **Löschen**.

	Der standardmäßige Streamingendpunkt kann nicht gelöscht werden.
2. Klicken Sie auf die Schaltfläche **Starten**, um den Streamingendpunkt zu starten.

	![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

In der Standardeinstellung können Sie bis zu zwei Streamingendpunkte festlegen. Wenn Sie weitere anfordern möchten, finden Sie entsprechende Informationen unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).
	
##<a id="configure_streaming_endpoints"></a>Konfigurieren des Streamingendpunkts

Mit einem Streamingendpunkt können Sie die folgenden Eigenschaften konfigurieren, wenn Sie über mindestens eine Skalierungseinheit verfügen:

- Zugriffssteuerung
- Cachesteuerelement
- Websiteübergreifende Zugriffsrichtlinien

Ausführliche Informationen zu diesen Eigenschaften finden Sie unter [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx).

Sie können den Streamingendpunkt wie folgt konfigurieren:

1. Wählen Sie den Streamingendpunkt aus, den Sie konfigurieren möchten.
1. Klicken Sie auf **Einstellungen**.
  
Eine kurze Beschreibung der Felder folgt.

![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Maximale Cacherichtlinie: Dient zum Konfigurieren der Cachelebensdauer von Medienobjekten, die über diesen Streamingendpunkt bereitgestellt werden. Wenn kein Wert festgelegt ist, wird der Standardwert verwendet. Die Standardwerte können auch direkt im Azure-Speicher definiert werden. Wenn Azure CDN für den Streamingendpunkt aktiviert ist, darf die Cacherichtlinie nicht auf einen Wert unter 600 Sekunden festgelegt werden.

2. Zulässige IP-Adressen: Dient zum Angeben von IP-Adressen, die sich mit dem veröffentlichten Streamingendpunkt verbinden dürfen. Falls keine IP-Adressen angegeben sind, dürfen sich alle IP-Adressen verbinden. IP-Adressen können als einzelne IP-Adresse (z.B. 10.0.0.1), IP-Adressbereiche mit einer IP-Adresse und einer CIDR-Subnetzmaske (z.B. 10.0.0.1/22) oder IP-Adressbereiche mit einer IP-Adresse und einer Subnetzmaske in Punkt-Dezimalschreibweise (z.B. 10.0.0.1(255.255.255.0)) angegeben werden.

3. Konfiguration für die Unterschriftenkopfzeilen-Authentifizierung von Akamai: Dient zum Angeben, wie die Unterschriftenkopfzeilen-Authentifizierungsanforderung von Akamai-Servern konfiguriert ist. Die Ablaufzeit ist in UTC angegeben.



##<a id="enable_cdn"></a>Aktivieren der Azure CDN-Integration

Sie können angeben, dass die Azure CDN-Integration für einen Streamingendpunkt aktiviert wird (in der Standardeinstellung ist sie deaktiviert).

So setzen Sie die Azure CDN-Integration auf "True":

- Der Streamingendpunkt muss über mindestens eine Streamingeinheit verfügen. Wenn Sie später die Skalierungseinheiten auf "0" festlegen möchten, müssen Sie zuerst die CDN-Integration deaktivieren. Beim Erstellen eines neuen Streamingendpunkts wird standardmäßig eine Streamingeinheit automatisch festgelegt.

- Der Streamingendpunkt muss sich im angehaltenen Status befinden. Nach der Aktivierung der CDN-Integration können Sie den Streamingendpunkt starten.

Die Aktivierung der Azure CDN-Integration kann bis zu 90 Minuten dauern. Es dauert bis zu zwei Stunden, bis die Änderungen in allen CDN-POPs aktiv sind.

Die CDN-Integration ist in allen Azure-Rechenzentren aktiviert: USA, Westen; USA, Osten; Europa, Norden; Europa, Westen; Japan, Westen; Japan, Osten; Asien, Südosten und Asien, Osten.

Sobald aktiviert, wird die **Access Control**-Konfiguration deaktiviert.

![Streamingendpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] Die Azure Media Services-Integration in Azure CDN ist in **Azure CDN von Verizon** implementiert. Wenn Sie **Azure CDN von Akamai** für Azure Media Services verwenden möchten, müssen Sie [den Endpunkt manuell konfigurieren](../cdn/cdn-create-new-endpoint.md). Weitere Informationen zu Azure CDN-Features finden Sie in der [Übersicht über das Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md).

###Zusätzliche Überlegungen

- Wenn CDN für einen Streamingendpunkt aktiviert ist, können die Clients keine Inhalte direkt vom Ursprung anfordern. Wenn Sie Ihre Inhalte mit oder ohne CDN testen können möchten, können Sie einen anderen Streamingendpunkt erstellen, für den kein CDN aktiviert ist.
- Der Hostname für den Streamingendpunkt bleibt nach dem Aktivieren von CDN gleich. Sie müssen keine Änderungen an Ihrem Media Services-Workflow vornehmen, nachdem CDN aktiviert wurde. Wenn der Hostname des Streamingendpunkts z. B. "strasbourg.streaming.mediaservices.windows.net" lautet, wird nach der Aktivierung von CDN genau der gleiche Hostname verwendet.
- Für neue Streamingendpunkte können Sie CDN einfach aktivieren, indem Sie einen neuen Endpunkt erstellen. Für vorhandene Streamingendpunkte müssen Sie zuerst den Endpunkt beenden und dann das CDN aktivieren.
 

Weitere Informationen finden Sie unter [Ankündigung der Azure Media Services-Integration in Azure CDN (Content Delivery Network)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##Nächste Schritte

Überprüfen Sie die Media Services-Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 

<!---HONumber=AcomDC_0831_2016-->