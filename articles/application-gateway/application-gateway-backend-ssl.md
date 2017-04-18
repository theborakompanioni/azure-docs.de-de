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
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: amsriva
translationtype: Human Translation
ms.sourcegitcommit: 73ee330c276263a21931a7b9a16cc33f86c58a26
ms.openlocfilehash: 40368e31790a7ffa2d34a51a13e78d028cd0a1eb
ms.lasthandoff: 04/05/2017


---
# <a name="overview-of-end-to-end-ssl-and-ssl-policy-on-application-gateway"></a>Übersicht über End-to-End-SSL und SSL-Richtlinie in Application Gateway

Application Gateway unterstützt die SSL-Beendigung am Gateway, wonach der Datenverkehr in der Regel unverschlüsselt zu den Back-End-Servern gelangt. Mit diesem Feature können Webserver vom kostspieligen Verschlüsselungs- und Entschlüsselungsaufwand befreit werden. Für einige Kunden ist die unverschlüsselte Kommunikation mit den Back-End-Servern jedoch keine akzeptable Option. Der Grund für diese unverschlüsselte Kommunikation können Sicherheits- oder Konformitätsanforderungen sein, oder die Anwendung akzeptiert unter Umständen nur eine sichere Verbindung. Für Anwendungen dieser Art unterstützt Application Gateway die End-to-End-SSL-Verschlüsselung.

## <a name="overview"></a>Übersicht

End-to-End-SSL ermöglicht die sichere, verschlüsselte Übertragung sensibler Daten an das Back-End und nutzt gleichzeitig die Vorteile der von Application Gateway bereitgestellten Lastenausgleichsfeatures (Schicht 7). Zu diesen Features zählen unter anderem cookiebasierte Sitzungsaffinität, URL-basiertes Routing, Unterstützung von standortbasiertem Routing und die Möglichkeit zum Einfügen von X-Forwarded-*-Headern.

Bei der Konfiguration mit dem End-to-End-SSL-Kommunikationsmodus beendet Application Gateway die SSL-Sitzungen am Gateway und entschlüsselt den Benutzerdatenverkehr. Anschließend werden die konfigurierten Regeln angewendet, um eine geeignete Instanz des Back-End-Pools auszuwählen, an die der Datenverkehr geroutet werden soll. Application Gateway initiiert anschließend eine neue SSL-Verbindung mit dem Back-End-Server und verschlüsselt die Daten mit dem öffentlichen Schlüssel des Back-End-Serverzertifikats erneut, bevor die Anforderung an das Back-End übertragen wird. End-to-End-SSL wird aktiviert, indem Sie die Protokolleinstellung in „BackendHTTPSetting“ auf „HTTPS“ festlegen. Diese Einstellung wird anschließend auf einen Back-End-Pool angewendet. Jeder Back-End-Server im Back-End-Pool mit aktiviertem End-to-End-SSL muss mit einem Zertifikat konfiguriert sein, um die sichere Kommunikation zu erlauben.

![End-to-End-SSL-Szenario][1]

In diesem Beispiel werden Anforderungen, für die TLS 1.2 verwendet wird, an Back-End-Server in Pool1 geleitet, indem End-to-End-SSL genutzt wird.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End-to-End-SSL und Whitelists für Zertifikate

Application Gateway kommuniziert nur mit bekannten Back-End-Instanzen, deren Zertifikat in der Whitelist des Anwendungsgateways enthalten ist. Um die Whitelistfunktion für Zertifikate zu aktivieren, müssen Sie den öffentlichen Schlüssel der Back-End-Serverzertifikate auf das Anwendungsgateway hochladen (nicht das Stammzertifikat). Es sind dann nur Verbindungen mit bekannten und in Whitelists enthaltenen Back-Ends zulässig. Die übrigen Back-Ends führen zu einem Gatewayfehler. Selbstsignierte Zertifikate dienen nur zu Testzwecken und werden für Produktionsworkloads nicht empfohlen. Solche Zertifikate müssen wie in den obigen Schritten beschrieben in der Whitelist des Anwendungsgateways enthalten sein, damit sie verwendet werden können.

## <a name="application-gateway-ssl-policy"></a>Application Gateway-SSL-Richtlinie

Application Gateway unterstützt vom Benutzer konfigurierbare SSL-Aushandlungsrichtlinien, die Kunden eine bessere Steuerung der SSL-Verbindungen auf dem Anwendungsgateway ermöglichen.

1. SSL 2.0 und 3.0 ist für alle Anwendungsgateways standardmäßig deaktiviert. Diese Richtlinien sind nicht konfigurierbar.
2. Die SSL-Richtliniendefinition gibt Ihnen die Möglichkeit, die drei folgenden Protokolle zu deaktivieren: **TLSv1\_0**, **TLSv1\_1**, **TLSv1\_2**.
3. Wenn keine SSL-Richtlinie definiert ist, sind alle drei (TLSv1\_0, TLSv1\_1, TLSv1_2) aktiviert.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich über End-to-End-SSL und SSL-Richtlinien informiert haben, finden Sie weitere Informationen zum Erstellen eines Anwendungsgateways mit End-to-End-SSL unter [Aktivieren von End-to-End-SSL auf Application Gateway](application-gateway-end-to-end-ssl-powershell.md).

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png

