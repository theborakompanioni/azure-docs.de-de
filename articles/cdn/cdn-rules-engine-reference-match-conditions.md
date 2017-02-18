---
title: "Übereinstimmungsbedingungen des Azure CDN-Regelmoduls | Microsoft-Dokumentation"
description: "Referenzdokumentation zu den Übereinstimmungsbedingungen und Features des Azure CDN-Regelmoduls."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
translationtype: Human Translation
ms.sourcegitcommit: dccb945e170bd3e3f23283359db25e574a2d4296
ms.openlocfilehash: f4886b1d78dfa87cf25737fb46c12b5963034f27


---

# <a name="azure-cdn-rules-engine-match-conditions"></a>Übereinstimmungsbedingungen des Azure CDN-Regelmoduls
Dieses Thema enthält ausführliche Beschreibungen der verfügbaren Übereinstimmungsbedingungen für das [Regelmodul](cdn-rules-engine.md) des Azure Content Delivery Network (CDN).

Der zweite Teil einer Regel ist die Übereinstimmungsbedingung. Eine Übereinstimmungsbedingung gibt bestimmte Typen von Anforderungen an, für die verschiedene sog. Features ausgeführt werden.

Sie kann beispielsweise verwendet werden, um Anforderungen von Inhalten an einem bestimmten Speicherort, von einer bestimmten IP-Adresse oder einem Land generierte Anforderungen oder anhand von Headerinformationen zu filtern.

## <a name="always"></a>Always

Die Übereinstimmungsbedingung „Always“ dient zum Anwenden einer Standardmenge von Features auf alle Anforderungen.

## <a name="device"></a>Device

Die Übereinstimmungsbedingung „Device“ bestimmt Anforderungen von einem Mobilgerät anhand seiner Eigenschaften.  Die Mobilgeräterkennung wird mithilfe von [WURFL](http://wurfl.sourceforge.net/) durchgeführt.  Die WURFL-Funktionen und die dazugehörigen Variablen des CDN-Regelmoduls sind unten aufgeführt.

> [!NOTE] 
> Die hier angegebenen Variablen werden für die Features **Modify Client Request Header** und **Modify Client Response Header** unterstützt.

Funktion | Variable | Beschreibung | Beispielwert(e)
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | Eine Zeichenfolge, die den Markennamen des Geräts angibt. | Samsung
Device OS | %{wurfl_cap_device_os} | Eine Zeichenfolge, die das auf dem Gerät installierte Betriebssystem angibt. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | Eine Zeichenfolge, die die Versionsnummer des auf dem Gerät installierten Betriebssystems angibt. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | Ein boolescher Wert, der angibt, ob das Gerät die duale Ausrichtung unterstützt. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | Eine Zeichenfolge, die die bevorzugte Dokumenttypdefinition (DTD) des mobilen Geräts für den HTML-Inhalt angibt. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | Ein boolescher Wert, der angibt, ob das Gerät Base64-codierte Images unterstützt. | false
Is Android | %{wurfl_vcap_is_android} | Ein boolescher Wert, der angibt, ob für das Gerät das Android-Betriebssystem verwendet wird. | true
Is IOS | %{wurfl_vcap_is_ios} | Ein boolescher Wert, der angibt, ob für das Gerät das iOS-Betriebssystem verwendet wird. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | Ein boolescher Wert, der angibt, ob es sich um ein Smart TV-Gerät handelt. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | Ein boolescher Wert, der angibt, ob es sich beim Gerät um ein Smartphone handelt. | true
Is Tablet | %{wurfl_cap_is_tablet} | Ein boolescher Wert, der angibt, ob es sich beim Gerät um ein Tablet handelt. Dies ist eine vom Betriebssystem unabhängige Beschreibung. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | Ein boolescher Wert, der angibt, ob das Gerät als drahtloses Gerät betrachtet wird. | true
Marketing Name | %{wurfl_cap_marketing_name} | Eine Zeichenfolge, die den Marketingnamen des Geräts angibt. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | Eine Zeichenfolge, die den Browser angibt, der zum Anfordern des Inhalts vom Gerät verwendet wird. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | Eine Zeichenfolge, die die Version des Browsers angibt, der zum Anfordern des Inhalts vom Gerät verwendet wird. | 31
Model Name | %{wurfl_cap_model_name} | Eine Zeichenfolge, die den Modellnamen des Geräts angibt. | s3
Progressive Download | %{wurfl_cap_progressive_download} | Ein boolescher Wert, der angibt, ob das Gerät die Wiedergabe von Audio/Video während des Herunterladens unterstützt. | true
Release Date | %{wurfl_cap_release_date} | Eine Zeichenfolge, die das Jahr und den Monat angibt, in dem das Gerät der WURFL-Datenbank hinzugefügt wurde.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | Eine ganze Zahl, die für das Gerät die Höhe in Pixel angibt. | 768
Resolution Width | %{wurfl_cap_resolution_width} | Eine ganze Zahl, die für das Gerät die Breite in Pixel angibt. | 1024


## <a name="location"></a>Location

Diese Übereinstimmungsbedingungen dienen zum Bestimmen von Anforderungen basierend auf dem Standort des Anfordernden.

Name | Zweck
-----|--------
AS Number | Bestimmt Anforderungen, die aus einem bestimmten Netzwerk stammen.
Country | Bestimmt Anforderungen, die aus den angegebenen Ländern stammen.


## <a name="origin"></a>Origin

Diese Übereinstimmungsbedingungen dienen zum Bestimmen von Anforderungen, die auf CDN-Speicher oder einen Kundenursprungsserver zeigen.

Name | Zweck
-----|--------
CDN Origin | Bestimmt Anforderungen von Inhalten, die in CDN-Speicher gespeichert sind.
Customer Origin | Bestimmt Anforderungen von Inhalten, die auf einem spezifischen Kundenursprungsserver gespeichert sind.


## <a name="request"></a>Request

Diese Übereinstimmungsbedingungen dienen zum Bestimmen von Anforderungen basierend auf ihren Eigenschaften.

Name | Zweck
-----|--------
Client IP Address | Bestimmt Anforderungen, die von einer bestimmten IP-Adresse stammen.
Cookie Parameter | Überprüft die Cookies, die jeder Anforderung für den angegebenen Wert zugeordnet sind.
Cookie Parameter Regex | Überprüft die Cookies, die jeder Anforderung zugeordnet sind, auf den angegebenen regulären Ausdruck.
Edge Cname | Bestimmt Anforderungen, die auf einen bestimmten Edge-CNAME zeigen.
Referring Domain | Bestimmt Anforderungen, auf die die angegebenen Hostnamen verwiesen haben.
Request Header Literal | Bestimmt Anforderungen, die den angegebenen auf einen bestimmten Wert festgelegten Header enthalten.
Request Header Regex | Bestimmt Anforderungen, die den angegebenen Header enthalten, der auf einen Wert festgelegt ist, der dem angegebenen regulären Ausdruck entspricht.
Request Header Wildcard | Bestimmt Anforderungen, die den angegebenen Header enthalten, der auf einen Wert festgelegt ist, der dem angegebenen Muster entspricht.
Request Method | Bestimmt Anforderungen anhand ihrer HTTP-Methode.
Request Scheme | Bestimmt Anforderungen anhand ihres HTTP-Protokolls.

## <a name="url"></a>URL

Diese Übereinstimmungsbedingungen dienen zum Bestimmen von Anforderungen basierend auf ihren URLs.

Name | Zweck
-----|--------
URL Path Directory | Bestimmt Anforderungen anhand ihres relativen Pfads.
URL Path Extension | Bestimmt Anforderungen anhand ihrer Dateinamenerweiterung.
URL Path Filename | Bestimmt Anforderungen anhand ihres Dateinamens.
URL Path Literal | Vergleicht den relativen Pfad einer Anforderung mit dem angegebenen Wert.
URL Path Regex | Vergleicht den relativen Pfad einer Anforderung mit dem angegebenen regulären Ausdruck.
URL Path Wildcard | Vergleicht den relativen Pfad einer Anforderung mit dem angegebenen Muster.
URL Query Literal | Vergleicht die Abfragezeichenfolge einer Anforderung mit dem angegebenen Wert.
URL Query Parameter | Bestimmt Anforderungen, die den angegebenen Abfragezeichenfolgen-Parameter enthalten, der auf einen Wert festgelegt ist, der einem angegebenen Muster entspricht.
URL Query Regex | Bestimmt Anforderungen, die den angegebenen Abfragezeichenfolgen-Parameter enthalten, der auf einen Wert festgelegt ist, der einem angegebenen regulären Ausdruck entspricht.
URL Query Wildcard | Vergleicht die angegebenen Werte mit der Abfragezeichenfolge der Anforderung.


## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über das Azure CDN](cdn-overview.md)
* [Rules Engine Reference](cdn-rules-engine-reference.md) (Regelmodul – Referenz)
* [Rules Engine Conditional Expressions](cdn-rules-engine-reference-conditional-expressions.md) (Regelmodul – Bedingte Ausdrücke)
* [Regelmodul – Features](cdn-rules-engine-reference-features.md)
* [Überschreiben des HTTP-Standardverhaltens mithilfe des Regelmoduls](cdn-rules-engine.md)




<!--HONumber=Jan17_HO4-->


