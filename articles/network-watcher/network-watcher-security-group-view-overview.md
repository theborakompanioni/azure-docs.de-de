---
title: "Einführung in die Sicherheitsgruppenansicht in Azure Network Watcher | Microsoft Docs"
description: "Diese Seite enthält eine Übersicht über die Network Watcher-Funktion zur Sicherheitsgruppenansicht."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 3e7595baa26ba9eebfcb8a2fd5c2744d9c0fbfcb
ms.openlocfilehash: 350ba23dbc0e8557f9609f7fb3a15169d26c9b4a
ms.lasthandoff: 02/23/2017

---

# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a>Einführung in die Netzwerksicherheitsgruppen-Ansicht in Azure Network Watcher

Netzwerksicherheitsgruppen können auf Subnetzebene oder Netzwerkkartenebene zugeordnet werden. Bei einer Zuordnung auf Subnetzebene gilt sie für alle VM-Instanzen in dem Subnetz. Die Netzwerksicherheitsgruppen-Ansicht gibt alle konfigurierten NSGs und Regeln zurück, die auf Netzwerkkarten- und Subnetzebene zugeordnet wurden. Darüber hinaus werden die geltenden Sicherheitsregeln für jede Netzwerkkarte auf einem virtuellen Computer zurückgegeben. Mithilfe der Netzwerksicherheitsgruppen-Ansicht können Sie Netzwerksicherheitslücken auf einem virtuellen Computer, z.B. geöffnete Ports, beurteilen. Sie können auch basierend auf einem Vergleich zwischen den konfigurierten und angewendeten Sicherheitsregeln überprüfen, ob die Netzwerksicherheitsgruppe wie erwartet funktioniert.

Einen weiter gefassten Anwendungsfall stellt die Sicherheitskonformität und Überwachung dar. Sie können einen normativen Satz von Sicherheitsregeln als Modell für die Sicherheitsbestimmungen in Ihrer Organisation definieren. Eine regelmäßige Überwachung der Einhaltung kann programmgesteuert durch einen Vergleich der normativen Regeln mit den angewendeten Regeln für die einzelnen virtuellen Computer in Ihrem Netzwerk implementiert werden.

Im Portal werden die Regeln nach Gültigkeit, Subnetz, Netzwerkschnittstelle und Standard unterteilt. Dies bietet einen einfachen Einblick in die Regeln, die auf einen virtuellen Computer angewendet werden. Über eine Downloadschaltfläche können sämtliche Sicherheitsregeln unabhängig von der Registerkarte in eine CSV-Datei heruntergeladen werden.

> [!NOTE]
> Network Watcher befindet sich derzeit in der Vorschau. Um die Features von Network Watcher zu verwenden, muss das [Feature registriert werden](network-watcher-create.md#register-the-preview-capability).

![Sicherheitsgruppenansicht][1]

Es können Regeln ausgewählt werden. Damit wird dann ein neues Blatt geöffnet, auf dem die Netzwerksicherheitsgruppe und die Quell- und Zielpräfixe angezeigt werden. Sie können von diesem Blatt aus direkt zu der Ressource der Netzwerksicherheitsgruppe navigieren.

![Drilldown][2]

### <a name="next-steps"></a>Nächste Schritte

Erfahren Sie unter [Überwachen von Netzwerksicherheitsgruppen-Einstellungen mit PowerShell](network-watcher-nsg-auditing-powershell.md), wie Sie die Einstellungen Ihrer Netzwerksicherheitsgruppe überwachen.

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png










