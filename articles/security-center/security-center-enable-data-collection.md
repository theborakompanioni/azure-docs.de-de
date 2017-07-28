---
title: Aktivieren der Datensammlung in Azure Security Center | Microsoft Docs
description: " Hier erfahren Sie, wie Sie die Datensammlung in Azure Security Center aktivieren. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 7e9ad8cd8c77c57c37dc208b86b3727a4e1dc7b5
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="enable-data-collection-in-azure-security-center"></a>Aktivieren der Datensammlung in Azure Security Center

> [!NOTE]
> Ab Anfang Juni 2017 verwendet Security Center den Microsoft Monitoring Agent zum Sammeln und Speichern von Daten. Weitere Informationen finden Sie unter [Azure Security Center – Plattformmigration](security-center-platform-migration.md). Die Informationen in diesem Artikel stellen Security Center-Funktionen nach dem Umstieg auf den Microsoft Monitoring Agent vor.
>
>

Security Center sammelt Daten von Ihren virtuellen Computern (VMs), um den Sicherheitsstatus zu bewerten, Sicherheitsempfehlungen bereitzustellen und vor Bedrohungen zu warnen. Beim ersten Zugriff auf Security Center können Sie die Datensammlung für alle VMs in Ihrem Abonnement aktivieren. Wenn die Datensammlung nicht aktiviert wird, empfiehlt Security Center, die Datensammlung in der Sicherheitsrichtlinie für dieses Abonnement zu aktivieren.

Wenn die Datensammlung aktiviert ist, stellt Security Center den Microsoft Monitoring Agent auf allen vorhandenen unterstützten virtuellen Azure-Computern sowie auf allen neuen virtuellen Computern bereit, die erstellt werden. Der Microsoft Monitoring Agent sucht nach verschiedenen sicherheitsbezogenen Konfigurationen. Außerdem löst das Betriebssystem Ereignisprotokollereignisse aus. Beispiele für Daten dieser Art: Betriebssystemtyp und -version, Betriebssystemprotokolle (Windows-Ereignisprotokolle), ausgeführte Prozesse, Computername, IP-Adressen, angemeldeter Benutzer und Mandanten-ID. Der Microsoft Monitoring Agent liest Ereignisprotokolleinträge und -konfigurationen und kopiert die Daten zur Analyse in Ihren Arbeitsbereich. Darüber hinaus kopiert der Microsoft Monitoring Agent Absturzabbilddateien in Ihren Arbeitsbereich.

Wenn Sie den Free-Tarif von Security Center verwenden, können Sie die Datensammlung von virtuellen Computern durch Deaktivieren der Datensammlung in der Sicherheitsrichtlinie deaktivieren. Durch das Deaktivieren der Datensammlung werden die Sicherheitsbewertungen für Ihre virtuellen Computer eingeschränkt. Weitere Informationen finden Sie unter [Deaktivieren der Datensammlung](#disabling-data-collection). Momentaufnahmen von VM-Datenträgern sowie die Artefaktsammlung sind auch bei deaktivierter Datensammlung aktiviert. Die Datensammlung ist für Abonnements des Standard-Tarifs von Security Center erforderlich.

> [!NOTE]
> Weitere Informationen zu den Tarifen Free und Standard von Security Center erfahren Sie [hier](security-center-pricing.md).
>
>

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>
>

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Sammlung von Daten für Abonnements aktivieren** aus.  Das Blatt **Turn on data collection** (Datensammlung aktivieren) wird geöffnet.
   ![Blatt „Empfehlungen“][2]
2. Wählen Sie auf dem Blatt **Turn on data collection** (Datensammlung aktivieren) Ihr Abonnement aus. Das Blatt **Sicherheitsrichtlinie** für das Abonnement wird geöffnet.
3. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** unter **Datensammlung** die Option **Ein** aus, um automatisch Protokolle zu erfassen. Durch Aktivieren der Datensammlung wird die Überwachungserweiterung auf allen aktuellen und neuen unterstützten VMs im Abonnement bereitgestellt.
4. Wählen Sie **Speichern**aus.
5. Klicken Sie auf **OK**.

## <a name="disabling-data-collection"></a>Deaktivieren der Datensammlung
Wenn Sie den Free-Tarif von Security Center verwenden, können Sie die Datensammlung von virtuellen Computern jederzeit durch Deaktivieren der Datensammlung in der Sicherheitsrichtlinie deaktivieren. Die Datensammlung ist für Abonnements des Standard-Tarifs von Security Center erforderlich.

1. Kehren Sie zum Blatt **Security Center** zurück, und wählen Sie die Kachel **Richtlinie** aus. Das Blatt **Sicherheitsrichtlinie – Richtlinie pro Abonnement definieren** wird geöffnet.
   ![Auswählen der Kachel „Richtlinie“][5]
2. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie – Richtlinie pro Abonnement definieren** das Abonnement aus, für das Sie die Datensammlung deaktivieren möchten.
3. Das Blatt **Sicherheitsrichtlinie** für das Abonnement wird geöffnet.  Wählen Sie unter „Datensammlung“ die Option **Aus** aus.
4. Wählen Sie im oberen Menüband die Option **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Datensammlung aktivieren“ implementieren. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md) : Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md)Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Azure Security Center-Datensicherheit:](security-center-data-security.md) Hier erfahren Sie, wie Daten in Security Center verwaltet und geschützt werden.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md)Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)(Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[2]: ./media/security-center-enable-data-collection/recommendations.png
[3]: ./media/security-center-enable-data-collection/data-collection.png
[4]: ./media/security-center-enable-data-collection/storage-account.png
[5]: ./media/security-center-enable-data-collection/policy.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png

