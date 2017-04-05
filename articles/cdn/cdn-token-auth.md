---
title: "Schützen von Azure CDN-Assets mit Tokenauthentifizierung | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie die Tokenauthentifizierung zum Schützen des Zugriffs auf Ihre Azure CDN-Assets verwenden."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/11/2016
ms.author: mezha
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 42b182c314795b1ebf69639ec7ac5583208dc7c1
ms.lasthandoff: 03/28/2017


---


# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Schützen von Azure CDN-Assets mit Tokenauthentifizierung

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

##<a name="overview"></a>Übersicht

Die Tokenauthentifizierung ist ein Mechanismus, mit dem Sie verhindern können, dass Assets per Azure CDN für nicht autorisierte Clients bereitgestellt werden.  Dieser Ansatz wird normalerweise genutzt, um das „Hotlinking“ von Inhalten zu verhindern, wenn eine andere Website (häufig ein Diskussionsforum) Ihre Assets ohne Erlaubnis verwendet.  Dies kann sich auf Ihre Kosten für die Inhaltsbereitstellung auswirken. Wenn dieses Feature im CDN aktiviert wird, werden Anforderungen mit CDN-Edge-POPs authentifiziert, bevor der Inhalt bereitgestellt wird. 

## <a name="how-it-works"></a>So funktioniert's

Bei der Tokenauthentifizierung werden Anforderungen überprüft, die von einer vertrauenswürdigen Website generiert werden. Die Anforderungen müssen einen Tokenwert mit codierten Informationen zur anfordernden Person enthalten. Inhalte werden für die anfordernde Person nur bereitgestellt, wenn die codierten Informationen die erforderlichen Voraussetzungen erfüllen. Andernfalls werden die Anforderungen abgelehnt. Sie können diese Voraussetzungen einrichten, indem Sie die unten angegebenen Parameter verwenden.

- Land: Sie können Anforderungen aus bestimmten Ländern zulassen oder ablehnen.  [Liste mit gültigen Ländercodes](https://msdn.microsoft.com/library/mt761717.aspx) 
- URL: Sie können nur angegebene Assets oder bestimmte Pfade für Anforderungen zulassen.  
- Host: Sie können Anforderungen zulassen oder ablehnen, indem Sie die im Anforderungsheader angegebenen Hosts verwenden.
- Verweiser (Referrer): Sie können angegebene Verweiser für Anforderungen zulassen oder ablehnen.
- IP-Adresse: Sie können nur Anforderungen zulassen, die von einer bestimmten IP-Adresse oder aus einem bestimmten IP-Subnetz stammen.
- Protokoll: Sie können Anforderungen basierend auf dem Protokoll zulassen oder blockieren, das zum Anfordern des Inhalts verwendet wird.
- Ablaufzeit: Weisen Sie einen Datums- und Zeitbereich zu, um sicherzustellen, dass ein Link nur für eine begrenzte Zeit gültig bleibt.

Weitere Informationen finden Sie im ausführlichen Konfigurationsbeispiel für die einzelnen Parameter.

## <a name="reference-architecture"></a>Referenzarchitektur

Unten ist eine Referenzarchitektur für die Einrichtung der Tokenauthentifizierung im CDN für die Zusammenarbeit mit Ihrer Web-App dargestellt.

![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Tokenüberprüfungslogik auf dem CDN-Endpunkt
    
In diesem Diagramm wird beschrieben, wie Azure CDN eine Clientanforderung überprüft, wenn die Tokenauthentifizierung auf dem CDN-Endpunkt konfiguriert wird.

![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Einrichten der Tokenauthentifizierung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem CDN-Profil, und klicken Sie dann auf die Schaltfläche **Verwalten**, um das zusätzliche Portal zu starten.

    ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Zeigen Sie mit der Maus auf **HTTP Large**, und klicken Sie im Flyout dann auf **Token Auth**. Auf dieser Registerkarte richten Sie den Verschlüsselungsschlüssel und die Verschlüsselungsparameter ein.

    1. Geben Sie unter **Primärschlüssel** einen eindeutigen Verschlüsselungsschlüssel ein.  Geben Sie unter **Backup Key** (Sicherungsschlüssel) einen weiteren Schlüssel ein.

        ![CDN-Tokenauthentifizierung – Einrichten des Schlüssels](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Richten Sie die Verschlüsselungsparameter mit dem Verschlüsselungstool ein. (Sie können Anforderungen basierend auf Ablaufzeit, Land, Verweiser, Protokoll und Client-IP zulassen oder ablehnen. Hierbei ist eine beliebige Kombination möglich.)

        ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - ec-expire: Dient zum Zuweisen einer Ablaufzeit eines Tokens nach einem bestimmten Zeitraum. Anforderungen, die nach Ablauf des Zeitraums übermittelt werden, werden abgelehnt. Für diesen Parameter wird der Unix-Zeitstempel verwendet. (Basierend auf den Sekunden seit der Standardepoche „01.01.1970 00:00:00 GMT“. Sie können Onlinetools verwenden, um die Konvertierung zwischen der Standardzeit und der Unix-Zeit durchzuführen.)  Wenn Sie beispielsweise möchten, dass das Token zum Zeitpunkt „31.12.2016 12:00:00 GMT“ abläuft, können Sie wie hier dargestellt die Unix-Zeit „1483185600“ verwenden:
    
        ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - ec-url-allow: Dient zum Anpassen von Token an ein bestimmtes Asset oder einen Pfad. Der Zugriff wird auf Anforderungen beschränkt, deren URL mit einem bestimmten relativen Pfad beginnt. Sie können mehrere Pfade eingeben, indem Sie als Trennzeichen jeweils ein Komma verwenden. Bei URLs wird die Groß-/Kleinschreibung berücksichtigt. Je nach den Anforderungen können Sie einen anderen Wert angeben, um unterschiedliche Zugriffsebenen bereitzustellen. Unten sind einige Szenarien angegeben:
        
            Beispiel-URL: http://www.mydomain.com/pictures/city/strasbourg.png. Eingabewert („“) und entsprechende Zugriffsebene

            1. Eingabewert „/“: alle Anforderungen sind zulässig
            2. Eingabewert „/pictures“: alle folgenden Anforderungen sind zulässig
            
                - http://www.mydomain.com/pictures.png
                - http://www.mydomain.com/pictures/city/strasbourg.png
                - http://www.mydomain.com/picturesnew/city/strasbourgh.png
            3. Eingabewert „/pictures/“: nur Anforderungen für „/pictures/“ sind zulässig
            4. Eingabewert „/pictures/city/strasbourg.png“: nur Anforderungen für dieses Asset sind zulässig
    
        ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - ec-country-allow: Es sind nur Anforderungen zulässig, die aus den angegebenen Ländern stammen. Anforderungen aus allen anderen Ländern werden abgelehnt. Verwenden Sie den Ländercode, um die Parameter einzurichten, und fügen Sie bei mehreren Ländern jeweils ein Komma als Trennzeichen ein. Beispiel: Wenn Sie den Zugriff aus den USA und aus Frankreich zulassen möchten, geben Sie wie unten dargestellt „US, FR“ in die Spalte ein.  
        
        ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - ec-country-deny: Dient zum Ablehnen von Anforderungen, die aus einem oder mehreren angegebenen Ländern stammen. Anforderungen aus allen anderen Ländern werden zugelassen. Verwenden Sie den Ländercode, um die Parameter einzurichten, und fügen Sie bei mehreren Ländern jeweils ein Komma als Trennzeichen ein. Beispiel: Wenn Sie den Zugriff aus den USA und aus Frankreich ablehnen möchten, geben Sie „US, FR“ in die Spalte ein.
    
        - ec-ref-allow: Es werden nur Anforderungen vom angegebenen Verweiser zugelassen. Mit einem Verweiser wird die URL der Webseite identifiziert, die als Link zur angeforderten Ressource dient. Der Parameterwert des Verweisers sollte nicht das Protokoll enthalten. Sie können einen Hostnamen bzw. einen bestimmten Pfad für den Hostnamen eingeben. Außerdem können Sie mehrere Verweiser für einen Parameter hinzufügen, wenn Sie als Trennzeichen jeweils ein Komma verwenden. Wenn Sie einen Verweiserwert angegeben haben, die entsprechenden Informationen aufgrund der Browserkonfiguration aber nicht mit der Anforderung gesendet werden, werden diese Anforderungen standardmäßig abgelehnt. Sie können im Parameter „Missing“ oder einen leeren Wert zuweisen, um diese Anforderungen mit fehlenden Verweiserinformationen zuzulassen. Sie können auch „*.consoto.com“ verwenden, um alle Unterdomänen von „consoto.com“ zuzulassen.  Wenn Sie beispielsweise den Zugriff für Anforderungen von „www.consoto.com“, alle Unterdomänen unter „consoto2.com“ und Anforderungen mit leeren oder fehlenden Verweisern zulassen möchten, können Sie den folgenden Wert eingeben:
        
        ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - ec-ref-deny: Dient zum Ablehnen von Anforderungen über den angegebenen Verweiser. Unter dem Parameter „ec-ref-allow“ finden Sie Details und ein Beispiel hierzu.
         
        - ec-proto-allow: Es werden nur Anforderungen vom angegebenen Protokoll zugelassen. Beispiel: http oder https.
        
        ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - ec-proto-deny: Dient zum Ablehnen von Anforderungen vom angegebenen Protokoll. Beispiel: http oder https.
    
        - ec-clientip: Beschränkt den Zugriff auf die IP-Adresse der angegebenen anfordernden Person. IPv4 und IPv6 werden unterstützt. Sie können eine IP-Adresse oder ein IP-Subnetz für eine einzelne Anforderung angeben.
            
        ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Sie können Ihr Token mit dem Beschreibungstool testen.

    4. Außerdem können Sie den Typ der Antwort anpassen, die für den Benutzer zurückgegeben wird, wenn eine Anforderung abgelehnt wird. Standardmäßig wird 403 verwendet.

3. Klicken Sie als Nächstes unter **HTTP Large** auf die Registerkarte **Regelmodul**. Sie verwenden diese Registerkarte, um Pfade zum Anwenden der Funktion zu definieren und die Tokenauthentifizierung sowie weitere Funktionen zur Tokenauthentifizierung zu aktivieren.

    - Verwenden Sie die Spalte „IF“, um das Asset bzw. den Pfad zu definieren, für das bzw. den Sie die Tokenauthentifizierung anwenden möchten. 
    - Fügen Sie „Token Auth“ per Klick aus der Dropdownliste mit den Funktionen hinzu, um die Tokenauthentifizierung zu aktivieren.
        
    ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. Auf der Registerkarte **Regelmodul** können Sie noch weitere Funktionen aktivieren.
    
    - Token Auth Denial Code: Gibt den Typ der Antwort an, die für Benutzer zurückgegeben wird, wenn eine Anforderung abgelehnt wird. Mit den Regeln, die Sie hier einrichten, wird die Einstellung für den Ablehnungscode auf der Registerkarte „Token Auth“ überschrieben.
    - Token Auth Ignore: Legt fest, ob für die URL, die zum Überprüfen des Tokens verwendet wird, die Groß-/Kleinschreibung berücksichtigt wird.
    - Token Auth Parameter: Benennen Sie den Parameter für die Tokenauthentifizierung-Abfragezeichenfolge um, der für die angeforderte URL angezeigt wird. 
        
    ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Sie können das Token mit einer Anwendung anpassen, die Token für tokenbasierte Authentifizierungsfunktionen generiert. Auf den Quellcode können Sie auf [GitHub](https://github.com/VerizonDigital/ectoken) zugreifen.
Verfügbare Sprachen:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Preise für Azure CDN-Funktionen und -Anbieter

Informationen hierzu finden Sie im Thema [Übersicht über CDN](cdn-overview.md).

