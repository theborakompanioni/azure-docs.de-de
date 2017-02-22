---
title: "Einschränken von Azure CDN-Inhalten nach Ländern | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie den Zugriff auf Ihre Azure CDN-Inhalte mithilfe der Geofilterung einschränken."
services: cdn
documentationcenter: 
author: lichard
manager: akucer
editor: 
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
translationtype: Human Translation
ms.sourcegitcommit: dccb945e170bd3e3f23283359db25e574a2d4296
ms.openlocfilehash: 30160088d9c770400f342e67527e1cf1cabc4f6b


---
# <a name="restrict-azure-cdn-content-by-country"></a>Einschränken von Azure-CDN-Inhalten nach Land

## <a name="overview"></a>Übersicht
Wenn ein Benutzer Ihren Inhalt anfordert, wird unabhängig davon, wo der Benutzer diese Anforderung vorgenommen hat, der Inhalt angezeigt. In einigen Fällen empfiehlt es sich, den Zugriff auf Ihre Inhalte nach Ländern einzuschränken. In diesem Thema wird erläutert, wie Sie die **Geofilterung** einsetzen, um den Zugriff je nach Land zu genehmigen oder verweigern.

> [!IMPORTANT]
> Die Verizon und Akamai-Produkte bieten die gleichen Geo-Filter-Funktionen, die jeweils unterstützten Landeskennzahlen unterscheiden sich jedoch geringfügig. Ein Link zu den Unterschieden finden Sie unter Schritt 3.


Informationen zu Überlegungen, die Sie beim Konfigurieren dieser Art von Einschränkungen berücksichtigen sollten, finden Sie im Abschnitt [Überlegungen](cdn-restrict-access-by-country.md#considerations) am Ende dieses Themas.  

![Länderfilter](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-the-directory-path"></a>Schritt 1: Definieren des Verzeichnispfads
Wählen Sie den Endpunkt innerhalb des Portals, und suchen Sie die Registerkarte Geofilterung in der linken Navigationsleiste.

Wenn Sie einen Länderfilter konfigurieren, müssen Sie den relativen Pfad zum Speicherort angeben, an dem der Zugriff für Benutzer zugelassen oder verweigert wird. Sie können Geofilterung für alle Dateien durch Eingabe von „/“ oder ausgewählte Ordner anwenden, indem Sie Verzeichnispfade „/pictures/“ angeben. Sie können die Geofilterung auch auf eine einzige Datei anwenden, indem Sie die Datei angeben, und den nachgestellten Schrägstrich „/ pictures/city.png“ weglassen.

Beispiel für Verzeichnispfadfilter:

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-the-action-block-or-allow"></a>Schritt 2: Definieren der Aktion: Blockieren oder Zulassen
**Blockieren:** Benutzern aus den angegebenen Ländern wird der von diesem rekursiven Pfad angeforderte Zugriff auf Ressourcen verweigert. Wenn keine anderen Länderfilteroptionen für diesen Standort konfiguriert wurden, wird der Zugriff für alle anderen Benutzer zugelassen.

**Zulassen:** Nur für Benutzer aus den angegebenen Ländern wird der von diesem rekursiven Pfad angeforderte Zugriff auf Ressourcen zugelassen.

## <a name="step-3-define-the-countries"></a>Schritt 3: Definieren der Länder
Wählen Sie die Länder aus, die Sie für den Pfad zulassen oder blockieren möchten. 

Beispielsweise werden durch die Regel zum Blockieren von "/Photos/Strasbourg/" die folgenden Dateien gefiltert:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a>Landeskennzahlen
Die **Geofilterung** verwendet Landeskennzahlen, um die Länder zu definieren, von denen aus eine Anforderung für ein sicheres Verzeichnis zugelassen oder blockiert wird. Die Landeskennzahlen finden Sie unter [Landeskennzahlen für Azure CDN](https://msdn.microsoft.com/library/mt761717.aspx). 

## <a name="a-idconsiderationsaconsiderations"></a><a id="considerations"></a>Überlegungen
* Für Verizon kann es bis zu 90 Minuten und für Akamai einige Minuten dauern, bis die Änderungen an Ihrem Länderfilter wirksam werden.
* Diese Funktion unterstützt keine Platzhalterzeichen (z. B. "*").
* Die dem relativen Pfad zugeordnete Geofilterkonfiguration wird rekursiv auf diesen Pfad angewendet.
* Pro relativen Pfad kann nur eine Regel angewendet werden (Sie können nicht mehrere Länderfilter erstellen, die auf den gleichen relativen Pfad verweisen). Jedoch können mehrere Länderfilter auf einen Ordner angewendet werden. Das liegt an der rekursiven Natur der Länderfilter. Anders ausgedrückt: Ein Unterordner eines zuvor konfigurierten Ordners kann einem anderen Länderfilter zugewiesen werden.




<!--HONumber=Jan17_HO4-->


