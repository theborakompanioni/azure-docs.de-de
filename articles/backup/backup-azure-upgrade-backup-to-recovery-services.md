---
title: "Durchführen eines Upgrades für einen Sicherungstresor auf einen Recovery Services-Tresor (Vorschau) | Microsoft-Dokumentation"
description: "Befehle und Supportinformationen zur Durchführung eines Upgrades für einen Azure-Sicherungstresor auf einen Recovery Services-Tresor"
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
ms.assetid: 228fef19-2f6b-4067-acc3-fb6e501afb88
ms.service: required
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/22/2017
ms.author: markgal;arunak
ms.translationtype: Human Translation
ms.sourcegitcommit: d9ae8e8948d82b9695d7d144d458fe8180294084
ms.openlocfilehash: b8cf27bd7e50d6edd2534ed2d9f6bb8a7079fbbf
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017


---
# <a name="upgrade-a-backup-vault-to-a-recovery-services-vault"></a>Durchführen eines Upgrades für einen Sicherungstresor auf einen Recovery Services-Tresor

In diesem Artikel wird beschrieben, wie ein Upgrade für einen Sicherungstresor auf einen Recovery Services-Tresor durchgeführt wird. Der Upgradevorgang hat keinerlei Auswirkungen auf derzeit ausgeführte Sicherungsaufträge. Zudem gehen keine Sicherungsdaten verloren. Für die Durchführung eines Upgrades für einen Sicherungstresor auf einen Recovery Services-Tresor gibt es folgende primäre Gründe:
- Alle Funktionen eines Sicherungstresors werden in einem Recovery Services-Tresor beibehalten.
- Recovery Services-Tresore weisen mehr Funktionen als Sicherungstresore auf, z.B. höhere Sicherheit, integrierte Überwachung, schnellere Wiederherstellungen, Wiederherstellungen auf Elementebene, Verwaltung von Sicherungselementen über ein verbessertes, vereinfachtes Portal.
-  Neue Funktionen werden nur für Recovery Services-Tresore veröffentlicht.

## <a name="how-to-upgrade"></a>Wie werden Upgrades durchgeführt?

Es gibt wichtige Anforderung für ein Upgrade der Sicherungstresore auf Recovery Services-Tresore. Um diesen Prozess zu optimieren, überführen wir immer mehrere Kunden in die Upgradewarteschlange. Verwenden Sie den entsprechenden Link, um sich für die Upgradewarteschlange zu registrieren:
- [Sicherungstresore aktualisieren](https://www.surveymonkey.com/r/Y57BJQX)
- [Site Recovery-Tresore aktualisieren](https://www.surveymonkey.com/r/5HHPZQN)

## <a name="what-happens-after-i-sign-up"></a>Was passiert nach der Registrierung?
Sobald Ihr Abonnement für das Upgrade zugelassen wurde, kontaktiert Microsoft Sie, um mit dem Upgrade fortzufahren. Weitere Informationen zum Upgradevorgang finden Sie im [Blog](http://azure.microsoft.com/blog/upgrade-classic-backup-and-siterecovery-vault-to-arm-recovery-services-vault) zu Azure Backup.

