<properties
   pageTitle="Aktivieren der SSL-Richtlinie und von End-to-End-SSL in Application Gateway | Microsoft Azure"
   description="Diese Seite bietet eine Übersicht über die Unterstützung von End-to-End-SSL in Application Gateway."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amsriva"/>


# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Aktivieren der SSL-Richtlinie und von End-to-End-SSL in Application Gateway

## <a name="overview"></a>Übersicht

Application Gateway unterstützt die SSL-Beendigung am Gateway, wonach der Datenverkehr in der Regel unverschlüsselt zu den Back-End-Servern gelangt. So können Webserver vom kostspieligen Verschlüsselungs-/Entschlüsselungsaufwand befreit werden. Für einige Kunden ist die unverschlüsselte Kommunikation mit den Back-End-Servern jedoch keine akzeptable Option. Der Grund hierfür können Sicherheits-/Kompatibilitätsanforderungen sein, oder die Anwendung akzeptiert möglicherweise nur sichere Verbindungen. Für solche Anwendungen unterstützt Application Gateway jetzt die End-to-End-SSL-Verschlüsselung.

End-to-End-SSL ermöglicht Ihnen die sichere, verschlüsselte Übertragung sensibler Daten an das Back-End und nutzt gleichzeitig die Vorteile der von Application Gateway bereitgestellten Funktionen des Lastenausgleichs der Ebene 7, z.B. Cookieaffinität, URL-basiertes Routing, Unterstützung für das standortbasierte Routing oder die Möglichkeit zum Einfügen von X-Forwarded-*-Headern.

Bei der Konfiguration mit dem End-to-End-SSL-Kommunikationsmodus beendet Application Gateway Benutzer-SSL-Sitzungen am Gateway und entschlüsselt den Benutzerdatenverkehr. Anschließend werden die konfigurierten Regeln angewendet, um eine geeignete Instanz des Back-End-Pools auszuwählen, an die der Datenverkehr geroutet werden soll. Application Gateway initiiert anschließend eine neue SSL-Verbindung mit dem Back-End-Server und verschlüsselt die Daten erneut mit dem öffentlichen Schlüssel des Back-End-Serverzertifikats, bevor die Anforderung an das Back-End übertragen wird. End-to-End-SSL wird aktiviert, indem Sie die Protokolleinstellung in „BackendHTTPSetting“ auf „Https“ festlegen. Diese Einstellung wird anschließend auf einen Back-End-Pool angewendet. Jeder Back-End-Server im Back-End-Pool mit aktiviertem End-to-End-SSL muss mit einem Zertifikat konfiguriert sein, um die sichere Kommunikation zu erlauben.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

In diesem Beispiel können Anforderungen für „https://contoso.com“ über HTTP an „ContosoServerPool“ geroutet werden, und „https://fabrikam.com“ wird mit End-to-End-SSL über HTTPS an „FabrikamServerPool“ geroutet.

## <a name="end-to-end-ssl-and-white-listing-of-certificates"></a>End-to-End-SSL und Whitelists von Zertifikaten

Application Gateway kommuniziert nur mit bekannten Back-End-Instanzen, deren Zertifikat sich auf der Whitelist des Anwendungsgateways befindet. Um die Whitelistfunktion für Zertifikate zu aktivieren, müssen Sie den öffentlichen Schlüssel der Back-End-Serverzertifikate auf das Anwendungsgateway hochladen. Anschließend sind nur Verbindungen mit bekannten und auf der Whitelist befindlichen Back-Ends zulässig, und die übrigen führen zu einem Gatewayfehler. Selbstsignierte Zertifikate dienen nur zu Testzwecken und werden für Produktionsworkloads nicht empfohlen. Solche Zertifikate müssen sich ebenfalls, wie oben beschrieben, auf der Whitelist des Anwendungsgateways befinden, damit sie verwendet werden können.

## <a name="application-gateway-ssl-policy"></a>Application Gateway-SSL-Richtlinie

Application Gateway unterstützt auch vom Benutzer konfigurierbare SSL-Aushandlungsrichtlinien, die Kunden eine detailliertere Steuerung der SSL-Verbindungen auf dem Anwendungsgateway ermöglichen.

1. SSL 2.0 und 3.0 sind für alle Anwendungsgateways zwangsweise deaktiviert. Sie sind in keiner Weise konfigurierbar.
2. Die SSL-Richtliniendefinition gibt Ihnen die Möglichkeit, die folgenden drei Protokolle beliebig zu deaktivieren: TLSv1_0, TLSv1_1, TLSv1_2.
3. Wenn keine SSL-Richtlinie definiert ist, sind alle drei (TLSv1_0, TLSv1_1, TLSv1_2) aktiviert.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich über End-to-End-SSL und SSL-Richtlinien informiert haben, finden Sie weitere Informationen zum Erstellen eines Anwendungsgateways mit der Möglichkeit zum Senden von verschlüsseltem Datenverkehr an das Back-End unter [Aktivieren von End-to-End-SSL auf Application Gateway](application-gateway-end-to-end-ssl-powershell.md) .



<!--HONumber=Oct16_HO2-->


