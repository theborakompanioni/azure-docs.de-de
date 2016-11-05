---
title: Azure RemoteApp-Netzwerkbandbreite – allgemeine Richtlinien | Microsoft Docs
description: Erläuterung einiger grundlegender Richtlinien zur Netzwerkbandbreite für Azure RemoteApp-Sammlungen und -Apps.
services: remoteapp
documentationcenter: ''
author: lizap
manager: mbaldwin

ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: elizapo

---
# Azure RemoteApp-Netzwerkbandbreite – allgemeine Richtlinien (wenn Sie Ihre eigenen nicht überprüfen können)
> [!IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Falls Sie nicht die Zeit oder die Möglichkeit haben, [Netzwerkbandbreiten-Tests](remoteapp-bandwidthtests.md) für Azure RemoteApp auszuführen, finden Sie in diesem Artikel einige relativ allgemeine Richtlinien, die Ihnen beim Abschätzen der Netzwerkbandbreite pro Benutzer helfen können.

Wenn auf Sie mehrere dieser Szenarios zutreffen, empfehlen wir 10 MBit/s als Mindest-Netzwerkbandbreite für moderne Apps mit Internetverbindung in einer Remoteumgebung. (Wie bereits erwähnt, garantiert dies jedoch keine überdurchschnittliche Anwendungsperformance.)

## PowerPoint komplex, PowerPoint einfach
Azure RemoteApp funktioniert am besten mit 100 MBit LAN. Verwenden Nutzer das Netzwerkprofil 10 MBit/s und dauert das Taktzittern (Jitter) in mehr als 5 % aller Fälle länger als 120 ms an, so fällt die Anwendungsperformance nur durchschnittlich aus. Bei 1 MBit/s ist der Unterschied eklatant: In einer Bildschirmpräsentation sieht der Benutzer möglicherweise überhaupt keine animierten Übergänge, da Frames übersprungen werden.

## Internet Explorer, PDF mit gemischten Inhalten, PDF, Text
Ein 10 MBit/s-Netzwerkprofil ähnelt einem LAN in vielerlei Hinsicht sehr. Bei 1 MBit/s ist die Anwendungsperformance in Ordnung, wobei beim Scrollen etwas Jitter auftreten kann, wenn Bilder auf dem Bildschirm angezeigt werden.

## Flash-Video (YouTube)
100 MBit/s-LAN bietet die beste Anwendungsperformance, während 10 MBit/s akzeptabel sind (d.h. wir halten mit der Bildfrequenz mit, Jitter nimmt jedoch zu). Bei 1 MBit/s ist Jitter sehr hoch und deutlich wahrzunehmen.

## Tippen in Word (Word-Remoteeingabe)
Dabei handelt es sich um ein Benutzerszenario mit geringer Bandbreite. Mit 256 KBit/s bieten wir eine ebenso gute Anwendungsperformance wie mit LAN.

## Weitere Informationen
* [Schätzen der Netzwerkbandbreiten-Nutzung von Azure RemoteApp](remoteapp-bandwidth.md)
* [Azure RemoteApp – wie wirken die Netzwerkbandbreite und die Qualität der Anwendungsperformance zusammen?](remoteapp-bandwidthexperience.md)
* [Azure RemoteApp – Festlegen Ihrer Netzwerkbandbreiten-Nutzung mit einigen häufigen Szenarios](remoteapp-bandwidthtests.md)

<!---HONumber=AcomDC_0817_2016-->