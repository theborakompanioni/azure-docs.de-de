---
title: Aktivieren der SSL-Richtlinie und von End-to-End-SSL in Application Gateway | Microsoft Docs
description: "Diese Seite bietet eine Übersicht über die Unterstützung von End-to-End-SSL in Application Gateway."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/10/2016
ms.author: amsriva
translationtype: Human Translation
ms.sourcegitcommit: ee8cfffdbf054b4251ed269745f6b9ee5a5e6c64
ms.openlocfilehash: f5f3e76711a93b9b8b0fadd08b051758a7c71619


---
# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Aktivieren der SSL-Richtlinie und von End-to-End-SSL in Application Gateway

## <a name="overview"></a>Übersicht

Application Gateway unterstützt die SSL-Beendigung am Gateway, wonach der Datenverkehr in der Regel unverschlüsselt zu den Back-End-Servern gelangt. So können Webserver vom kostspieligen Verschlüsselungs-/Entschlüsselungsaufwand befreit werden. Für einige Kunden ist die unverschlüsselte Kommunikation mit den Back-End-Servern jedoch keine akzeptable Option. Der Grund hierfür können Sicherheits-/Kompatibilitätsanforderungen sein, oder die Anwendung akzeptiert möglicherweise nur sichere Verbindungen. Für solche Anwendungen unterstützt Application Gateway jetzt die End-to-End-SSL-Verschlüsselung.

End-to-End-SSL ermöglicht Ihnen die sichere, verschlüsselte Übertragung sensibler Daten an das Back-End und nutzt gleichzeitig die Vorteile der von Application Gateway bereitgestellten Funktionen des Lastenausgleichs auf Schicht 7, z.B. Cookieaffinität, URL-basiertes Routing, Unterstützung für das standortbasierte Routing oder die Möglichkeit zum Einfügen von X-Forwarded-*-Headern.

Bei der Konfiguration mit dem End-to-End-SSL-Kommunikationsmodus beendet Application Gateway Benutzer-SSL-Sitzungen am Gateway und entschlüsselt den Benutzerdatenverkehr. Anschließend werden die konfigurierten Regeln angewendet, um eine geeignete Instanz des Back-End-Pools auszuwählen, an die der Datenverkehr geroutet werden soll. Application Gateway initiiert anschließend eine neue SSL-Verbindung mit dem Back-End-Server und verschlüsselt die Daten erneut mit dem öffentlichen Schlüssel des Back-End-Serverzertifikats, bevor die Anforderung an das Back-End übertragen wird. End-to-End-SSL wird aktiviert, indem Sie die Protokolleinstellung in „BackendHTTPSetting“ auf „Https“ festlegen. Diese Einstellung wird anschließend auf einen Back-End-Pool angewendet. Jeder Back-End-Server im Back-End-Pool mit aktiviertem End-to-End-SSL muss mit einem Zertifikat konfiguriert sein, um die sichere Kommunikation zu erlauben.

![End-to-End-SSL-Szenario][1]

In diesem Beispiel werden Anforderungen, für die TLS 1.2 verwendet wird, an Back-End-Server in Pool1 geleitet, indem End-to-End-SSL genutzt wird.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End-to-End-SSL und Whitelists für Zertifikate

Application Gateway kommuniziert nur mit bekannten Back-End-Instanzen, deren Zertifikat in der Whitelist des Anwendungsgateways enthalten ist. Um die Whitelistfunktion für Zertifikate zu aktivieren, müssen Sie den öffentlichen Schlüssel der Back-End-Serverzertifikate auf das Anwendungsgateway hochladen (nicht das Stammzertifikat). Es sind dann nur Verbindungen mit bekannten und in Whitelists enthaltenen Back-Ends zulässig. Die übrigen Back-Ends führen zu einem Gatewayfehler. Selbstsignierte Zertifikate dienen nur zu Testzwecken und werden für Produktionsworkloads nicht empfohlen. Solche Zertifikate müssen ebenfalls, wie unter den obigen Schritten beschrieben, in der Whitelist des Anwendungsgateways enthalten sein, damit sie verwendet werden können.

## <a name="application-gateway-ssl-policy"></a>Application Gateway-SSL-Richtlinie

Application Gateway unterstützt vom Benutzer konfigurierbare SSL-Aushandlungsrichtlinien, die Kunden eine bessere Steuerung der SSL-Verbindungen auf dem Anwendungsgateway ermöglichen.

1. SSL 2.0 und 3.0 ist für alle Anwendungsgateways standardmäßig deaktiviert. Sie sind in keiner Weise konfigurierbar.
2. Die SSL-Richtliniendefinition gibt Ihnen die Möglichkeit, die folgenden drei Protokolle beliebig zu deaktivieren: **TLSv1\_0**, **TLSv1\_1**, **TLSv1\_2**.
3. Wenn keine SSL-Richtlinie definiert ist, sind alle drei (TLSv1\_0, TLSv1\_1, TLSv1_2) aktiviert.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich über End-to-End-SSL und SSL-Richtlinien informiert haben, finden Sie weitere Informationen zum Erstellen eines Anwendungsgateways mit der Möglichkeit zum Senden von verschlüsseltem Datenverkehr an Back-Ends unter [Aktivieren von End-to-End-SSL auf Application Gateway](application-gateway-end-to-end-ssl-powershell.md).

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png



<!--HONumber=Nov16_HO3-->


