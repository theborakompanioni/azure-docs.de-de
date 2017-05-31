---
title: "Verwenden von Azure-CDN zum Zugreifen auf Blobs mit benutzerdefinierten Domänen über HTTPS"
description: "In diesem Artikel wird erläutert, wie Azure CDN mit Blob-Speicherung integriert werden kann, um mit benutzerdefinierten Domänen über HTTPS auf Blobs zugreifen zu können"
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mihauss
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 1439198250346ae9484eae448489e8a5de4734b7
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017

---
# <a name="using-the-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>Verwenden von Azure-CDN zum Zugreifen auf Blobs mit benutzerdefinierten Domänen über HTTPS

Azure Content Delivery Network (CDN) unterstützt nun HTTPS für benutzerdefinierte Domänennamen.
Sie können diese Funktion für den Zugriff auf Speicher-Blobs verwenden, indem Sie Ihre benutzerdefinierte Domäne über HTTPS nutzen. Zu diesem Zweck müssen Sie zuerst Azure CDN auf Ihrem Blob-Endpunkt aktivieren und das CDN einem benutzerdefinierten Domänennamen zuordnen. Bei diesen Schritten wird die Aktivierung von HTTPS für Ihre benutzerdefinierte Domäne durch die Möglichkeit der Aktivierung mit nur einem Klick sowie durch eine umfassende Zertifikatverwaltung vereinfacht – alles ohne zusätzliche Kosten im Vergleich zum normalen CDN-Preis.

Diese Funktionalität ist wichtig, weil Sie damit den Schutz und die Datenintegrität Ihrer sensiblen Webanwendungsdaten während der Übertragung schützen können. Das SSL-Protokoll während der Übertragung über HTTPS gewährleistet, dass die Daten beim Internetversand verschlüsselt werden. Das HTTPS-Protokoll sorgt für Vertrauen und bietet Authentifizierung und Angriffsschutz für Ihre Webanwendungen.

> [!NOTE]
> Zusätzlich zur Bereitstellung von SSL-Unterstützung für benutzerdefinierte Domänennamen kann Sie Azure CDN bei der Skalierung Ihrer Applikation unterstützen, damit diese Inhalte mit hoher Bandbreite in die gesamte Welt liefern kann.
> Weitere Informationen finden Sie im [Überblick über das Azure CDN](../cdn/cdn-overview.md).
>
>

## <a name="quick-start"></a>Schnellstart

Dies sind die erforderlichen Schritte zum Aktivieren von HTTPS für Ihren benutzerdefinierten Blob-Speicher-Endpunkt:

1.  [Integrieren eines Azure-Speicherkontos in CDN](../cdn/cdn-create-a-storage-account-with-cdn.md).
    Dieser Artikel führt Sie durch das Erstellen eines Speicherkontos im Azure-Portal, sofern Sie dies nicht bereits erledigt haben.
2.  [Zuordnen von CDN-Inhalt (Content Delivery Network) zu einer benutzerdefinierten Domäne](../cdn/cdn-map-content-to-custom-domain.md).
3.  [Aktivieren von HTTPS für eine benutzerdefinierte Azure CDN-Domäne](../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Shared Access Signatures

Wenn der Blob-Speicherendpunkt so konfiguriert ist, dass anonyme Lesezugriffe unterbunden werden, müssen Sie bei jeder Anforderung an Ihre benutzerdefinierte Domäne ein [Shared Access Signature](storage-dotnet-shared-access-signature-part-1.md)-Token (SAS-Token) liefern. Die Blob-Speicherendpunkte lehnen anonyme Lesezugriffe standardmäßig ab. Weitere Informationen über Shared Access Signatures finden Sie unter [Verwalten des anonymen Lesezugriffs auf Container und Blobs](storage-manage-access-to-resources.md).

Zum SAS-Token hinzugefügte Einschränkungen werden von Azure CDN nicht beachtet. Beispielsweise verfügen alle SAS-Token über eine Ablaufzeit. Dies bedeutet das mit einem abgelaufenen SAS noch so lange auf den Inhalt zugegriffen werden kann, bis dieser aus den CDN-Edgeknoten entfernt wurde. Über die Einstellung im Cache-Antwortheader können Sie steuern, wie lange die Daten im CDN zwischengespeichert werden. Eine Anleitung hierzu finden Sie in [Verwalten des Ablaufs von Azure-Speicher-Blobs im Azure-CDN](../cdn/cdn-manage-expiration-of-blob-content.md).

Wenn Sie zu demselben Blob-Endpunkt mehrere SAS-URLs erstellen, empfehlen wir Ihnen, in Ihrem Azure CDN das Zwischenspeichern der Abfragezeichenfolge einzuschalten. Dadurch wird sichergestellt, dass jede URL als eindeutige Entität behandelt wird. Weitere Informationen finden Sie unter [Steuern des Azure CDN-Zwischenspeicherverhaltens mit Abfragezeichenfolgen](../cdn/cdn-query-string.md).

## <a name="http-to-https-redirection"></a>HTTP-HTTPS-Umleitung

Sie können HTTP-Datenverkehr an HTTPS umleiten. Dies erfordert die Verwendung des Azure CDN-Premiumangebots von Verizon. Sie müssen das [HTTP-Verhalten mithilfe des Azure-CDN-Regelmodul](../cdn/cdn-rules-engine.md) mit der folgenden Regel überschreiben:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

„Cdn-Endpunkt-Name“ bezieht sich auf den Namen, den Sie für Ihren CDN-Endpunkt konfiguriert haben. Diesen Wert können Sie aus der Dropdownliste auswählen. „Origin-Path“ bezieht sich auf den Pfad in Ihrem Ursprungsspeicherkonto, wo Ihr statischer Inhalt liegt.
Wenn Sie alle statischen Inhalte in einem einzelnen Container hosten, ersetzen Sie „origin-path“ durch den entsprechenden Containernamen.

Eine weiterführende Beschreibung der Regeln finden Sie in [Features des Azure CDN-Regelmoduls](../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Beim Zugriff auf Blobs über ein Azure CDN bezahlen Sie [Blob-Speichergebühren](https://azure.microsoft.com/pricing/details/storage/blobs/) für den Datenverkehr zwischen den Edgeknoten und dem Ursprung (Blob-Speicher). Weiterhin zahlen Sie [CDN-Gebühren](https://azure.microsoft.com/pricing/details/cdn/) für Daten, auf die von den Edgeknoten aus zugegriffen wird.

Nehmen wir an, dass Sie über ein Speicherkonto im Westen der USA verfügen, auf das über ein Azure CDN zugegriffen wird. Wenn nun eine Person im Vereinigten Königreich versucht, über das auf ein Blob in diesem Speicherkonto zuzugreifen, prüft Azure zunächst den am wenigsten weit von Großbritannien entfernten Edgeknoten für diesen Blob. Wird Azure fündig, greift es auf diese Blob-Kopie zu und wendet die CDN-Gebühren an, weil der Zugriff im CDN erfolgt. Wird Azure nicht fündig, wird der Blob in den Edgeknoten kopiert, was gemäß den angegebenen Blob-Speicherpreisen Gebühren für ausgehenden Datenverkehr und Transaktionsgebühren nach sich zieht. Dann greift Azure im Edgeknoten auf die Datei zu, was eine CDN-Abrechnung nach sich zieht.

Bei Betrachtung der [CDN-Preisseite](https://azure.microsoft.com/pricing/details/cdn/) beachten Sie bitte, dass HTTPS-Unterstützung für benutzerdefinierte Domänennamen nur für Azure CDN aus Verizon-Produkten (Standard und Premium) angeboten wird.

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines benutzerdefinierten Domänennamens für Ihren Blob-Speicherendpunkt](storage-custom-domain-name.md)

