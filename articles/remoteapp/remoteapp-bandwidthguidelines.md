---
title: "Azure RemoteApp-Netzwerkbandbreite – allgemeine Richtlinien | Microsoft Docs"
description: "Erläuterung einiger grundlegender Richtlinien zur Netzwerkbandbreite für Azure RemoteApp-Sammlungen und -Apps."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 529bf318-ae37-4c14-a11c-43fa703d68a3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b44fcb6cca30acbd5771a0301f72faa7d9105849


---
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp-Netzwerkbandbreite – allgemeine Richtlinien (wenn Sie Ihre eigenen nicht überprüfen können)
> [!IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Falls Sie nicht die Zeit oder die Möglichkeit haben, [Netzwerkbandbreiten-Tests](remoteapp-bandwidthtests.md) für Azure RemoteApp auszuführen, finden Sie in diesem Artikel einige relativ allgemeine Richtlinien, die Ihnen beim Abschätzen der Netzwerkbandbreite pro Benutzer helfen können.

Wenn auf Sie mehrere dieser Szenarios zutreffen, empfehlen wir 10 MBit/s als Mindest-Netzwerkbandbreite für moderne Apps mit Internetverbindung in einer Remoteumgebung. (Wie bereits erwähnt, garantiert dies jedoch keine überdurchschnittliche Anwendungsperformance.)

## <a name="complex-powerpoint-simple-powerpoint"></a>PowerPoint komplex, PowerPoint einfach
Azure RemoteApp funktioniert am besten mit 100 MBit LAN. Verwenden Nutzer das Netzwerkprofil 10 MBit/s und dauert das Taktzittern (Jitter) in mehr als 5 % aller Fälle länger als 120 ms an, so fällt die Anwendungsperformance nur durchschnittlich aus. Bei 1 MBit/s ist der Unterschied eklatant: In einer Bildschirmpräsentation sieht der Benutzer möglicherweise überhaupt keine animierten Übergänge, da Frames übersprungen werden.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer, PDF mit gemischten Inhalten, PDF, Text
Ein 10 MBit/s-Netzwerkprofil ähnelt einem LAN in vielerlei Hinsicht sehr. Bei 1 MBit/s ist die Anwendungsperformance in Ordnung, wobei beim Scrollen etwas Jitter auftreten kann, wenn Bilder auf dem Bildschirm angezeigt werden.

## <a name="flash-video-youtube"></a>Flash-Video (YouTube)
100 MBit/s-LAN bietet die beste Anwendungsperformance, während 10 MBit/s akzeptabel sind (d.h. wir halten mit der Bildfrequenz mit, Jitter nimmt jedoch zu). Bei 1 MBit/s ist Jitter sehr hoch und deutlich wahrzunehmen.

## <a name="word-typing-word-remote-input"></a>Tippen in Word (Word-Remoteeingabe)
Dabei handelt es sich um ein Benutzerszenario mit geringer Bandbreite. Mit 256 KBit/s bieten wir eine ebenso gute Anwendungsperformance wie mit LAN.

## <a name="learn-more"></a>Weitere Informationen
* [Schätzen der Netzwerkbandbreiten-Nutzung von Azure RemoteApp](remoteapp-bandwidth.md)
* [Azure RemoteApp – wie wirken die Netzwerkbandbreite und die Qualität der Anwendungsperformance zusammen?](remoteapp-bandwidthexperience.md)
* [Azure RemoteApp – Festlegen Ihrer Netzwerkbandbreiten-Nutzung mit einigen häufigen Szenarios](remoteapp-bandwidthtests.md)




<!--HONumber=Nov16_HO3-->


