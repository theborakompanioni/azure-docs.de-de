---
title: "Einschränken des Zugriffs auf Ihre Azure CND-Inhalte nach Ländern | Microsoft Docs"
description: "Erfahren Sie, wie Sie den Zugriff auf Ihre Azure CDN-Inhalte mithilfe der Geofilterung einschränken."
services: cdn
documentationcenter: 
author: lichard
manager: akucer
editor: 
ms.assetid: 20943ac8-9dce-4cf2-85ed-e8362750aa63
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2016
ms.author: rli
translationtype: Human Translation
ms.sourcegitcommit: 2ee72c31072fb5201e057f4c3567d33ce53e4fbc
ms.openlocfilehash: 1a1cf8cea1b321bb4e758e9ad06e42da59d3bcb4


---
# <a name="restrict-access-to-your-content-by-country---verizon"></a>Einschränken des Zugriffs auf Inhalte nach Ländern – Verizon
> [!div class="op_single_selector"]
> * [Verizon](cdn-restrict-access-by-country.md)
> * [Akamai-Standard](cdn-restrict-access-by-country-akamai.md)
> 
> 

## <a name="overview"></a>Übersicht
Wenn ein Benutzer Ihren Inhalt anfordert, wird unabhängig davon, wo der Benutzer diese Anforderung vorgenommen hat, der Inhalt angezeigt. In einigen Fällen empfiehlt es sich, den Zugriff auf Ihre Inhalte nach Ländern einzuschränken. In diesem Thema wird erläutert, wie Sie die **Geofilterung** einsetzen, um den Zugriff je nach Land zu genehmigen oder verweigern.

> [!IMPORTANT]
> Die Verizon und Akamai-Produkte bieten die gleichen Geo-Filter-Funktionen, aber die Benutzeroberfläche unterscheidet sich. Dieses Dokument beschreibt die Benutzeroberfläche für **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon**. Informationen zur Geofilterung mit **Azure CDN Standard/Premium von Akamai** finden Sie unter [Beschränken des Zugriffs auf Ihre Inhalte nach Ländern – Akamai](cdn-restrict-access-by-country-akamai.md).
> 
> 

Informationen zu Überlegungen, die Sie beim Konfigurieren dieser Art von Einschränkungen berücksichtigen sollten, finden Sie im Abschnitt [Überlegungen](cdn-restrict-access-by-country.md#considerations) am Ende dieses Themas.  

> [!NOTE]
> Nachdem die Konfiguration eingerichtet wurde, gilt sie für alle **Azure CDN von Verizon** -Endpunkte in diesem CDN-Profil.
> 
> 

![Länderfilter](./media/cdn-filtering/cdn-country-filtering.png)

## <a name="step-1-define-the-directory-path"></a>Schritt 1: Definieren des Verzeichnispfads
Wenn Sie einen Länderfilter konfigurieren, müssen Sie den relativen Pfad zum Speicherort angeben, an dem der Zugriff für Benutzer zugelassen oder verweigert wird. Sie können Länderfilter für alle Dateien (durch Eingabe von „/“) oder ausgewählte Ordner anwenden, indem Sie Verzeichnispfade angeben.

Beispiel für Verzeichnispfadfilter:

    /                                 
    /Photos/
    /Photos/Strasbourg

## <a name="step-2-define-the-action-block-or-allow"></a>Schritt 2: Definieren der Aktion: Blockieren oder Zulassen
**Blockieren:** Benutzern aus den angegebenen Ländern wird der von diesem rekursiven Pfad angeforderte Zugriff auf Ressourcen verweigert. Wenn keine anderen Länderfilteroptionen für diesen Standort konfiguriert wurden, wird der Zugriff für alle anderen Benutzer zugelassen.

**Zulassen:** Nur für Benutzer aus den angegebenen Ländern wird der von diesem rekursiven Pfad angeforderte Zugriff auf Ressourcen zugelassen.

## <a name="step-3-define-the-countries"></a>Schritt 3: Definieren der Länder
Wählen Sie die Länder aus, die Sie für den Pfad zulassen oder blockieren möchten. Weitere Informationen finden Sie unter [Azure CDN from Verizon Country Codes](https://msdn.microsoft.com/library/mt761717.aspx)(Landeskennzahlen für Azure CDN von Verizon).

Beispielsweise werden durch die Regel zum Blockieren von "/Photos/Strasbourg/" die folgenden Dateien gefiltert:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


## <a name="country-codes"></a>Landeskennzahlen
Die **Geofilterung** verwendet Landeskennzahlen, um die Länder zu definieren, von denen aus eine Anforderung für ein sicheres Verzeichnis zugelassen oder blockiert wird. Die Landeskennzahlen finden Sie unter [Azure CDN from Verizon Country Codes](https://msdn.microsoft.com/library/mt761717.aspx)(Landeskennzahlen für Azure CDN von Verizon). Wenn Sie "EU" (Europa) oder "AP" (Asien/Pazifik) angeben, wird eine Teilmenge der IP-Adressen, die aus einem Land in diesen Regionen stammen, blockiert oder zugelassen.

## <a name="a-idconsiderationsaconsiderations"></a><a id="considerations"></a>Überlegungen
* Es kann bis zu 90 Minuten dauern, bis die Änderungen an Ihrem Länderfilter wirksam werden.
* Diese Funktion unterstützt keine Platzhalterzeichen (z. B. "*").
* Das dem relativen Pfad zugeordnete Länderfilterkonfiguration wird rekursiv auf diesen Pfad angewendet.
* Pro relativen Pfad kann nur eine Regel angewendet werden (Sie können nicht mehrere Länderfilter erstellen, die auf den gleichen relativen Pfad verweisen). Jedoch können mehrere Länderfilter auf einen Ordner angewendet werden. Das liegt an der rekursiven Natur der Länderfilter. Anders ausgedrückt: Ein Unterordner eines zuvor konfigurierten Ordners kann einem anderen Länderfilter zugewiesen werden.




<!--HONumber=Nov16_HO3-->


