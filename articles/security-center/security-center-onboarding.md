---
title: "Integrieren von Azure Security Center Standard für erhöhte Sicherheit | Microsoft-Dokumentation"
description: " Hier erfahren Sie, wie Sie Azure Security Center Standard für erhöhte Sicherheit integrieren. "
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
ms.date: 09/14/2017
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: a6394b1b02ebfe518dc2f2b7f65eb677bb0ba5f2
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="onboarding-to-azure-security-center-standard-for-enhanced-security"></a>Integrieren von Azure Security Center Standard für erhöhte Sicherheit
Führen Sie ein Upgrade auf Security Center Standard durch, um von erhöhter Sicherheit und besserem Schutz vor Bedrohungen für Ihre Hybridcloud-Workloads zu profitieren.  Sie können die Standard-Edition 60 Tage lang kostenlos testen. Weitere Informationen finden Sie auf der Seite mit den [Preisen für Security Center](https://azure.microsoft.com/pricing/details/security-center/).

Security Center Standard umfasst Folgendes:

- **Hybridsicherheit:** Verschaffen Sie sich einen einheitlichen Überblick über die Sicherheit sämtlicher lokaler und cloudbasierter Workloads. Wenden Sie Sicherheitsrichtlinien an, und bewerten Sie kontinuierlich die Sicherheit Ihrer Hybridcloud-Workloads, um die Einhaltung von Sicherheitsstandards zu gewährleisten. Sammeln, durchsuchen und analysieren Sie Sicherheitsdaten aus verschiedensten Quellen (einschließlich Firewalls und Partnerlösungen).
- **Erweiterte Bedrohungserkennung:** Verwenden Sie erweiterte Analysefunktionen und den Microsoft Intelligent Security Graph, um der Entwicklung von Cyberangriffen immer einen Schritt voraus zu sein.  Nutzen Sie integrierte Verhaltensanalysen und Machine Learning, um Angriffe und Zero-Day-Exploits zu erkennen. Überwachen Sie Netzwerke, Computer und Clouddienste auf neue Angriffe und auf Aktivitäten nach einer Sicherheitsverletzung. Optimieren Sie die Untersuchung mit interaktiven Tools und kontextbezogenen Informationen zu Bedrohungen.
- **Zugriffs- und Anwendungskontrollen:** Blockieren Sie Schadsoftware und andere unerwünschten Anwendungen mithilfe von Whitelist-Empfehlungen, die auf Ihre spezifischen Workloads abgestimmt sind und durch Machine Learning unterstützt werden. Verkleinern Sie die Angriffsfläche des Netzwerks mit kontrolliertem Just-in-Time-Zugriff auf die Verwaltungsports virtueller Azure-Computer, um die Gefahr von Brute-Force- und anderen Netzwerkangriffen erheblich zu senken.

## <a name="detecting-unprotected-resources"></a>Erkennen ungeschützter Ressourcen     
Security Center erkennt automatisch alle Azure-Abonnements oder -Arbeitsbereiche, für die Security Center Standard nicht aktiviert ist. Dazu zählen unter anderem Azure-Abonnements mit Security Center Free sowie Arbeitsbereiche, für die die Sicherheitslösung nicht aktiviert ist.

Sie können ein gesamtes Azure-Abonnement auf den Standard-Tarif upgraden, da dies von allen Ressourcen innerhalb des Abonnements geerbt wird. Falls Sie hingegen nur ein Upgrade für eine bestimmte Ressourcengruppe durchführen möchten, können Sie auch eine eindeutige Richtlinie definieren. Sind die Richtlinieneinstellungen der Ressourcengruppe eindeutig, überschreibt Security Center die Preisrichtlinien nicht, wenn Sie das Abonnement auf den Standard-Tarif upgraden. Wenn Sie den Standard-Tarif auf ein Abonnement anwenden, betrifft dies nur die virtuellen Computer innerhalb des Abonnements, die Berichte an von Security Center erstellte Arbeitsbereiche senden. Wenn Sie den Standard-Tarif auf den Arbeitsbereich anwenden, betrifft dies alle Ressourcen, die Berichte an den Arbeitsbereich senden.

> [!NOTE]
> Möglicherweise möchten Sie Ihre Kosten verwalten und den Umfang der für eine Lösung gesammelten Daten begrenzen, indem Sie sie auf einen bestimmten Satz von Agents beschränken. Mit der [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md) können Sie einen Bereich auf die Lösung anwenden und eine Teilmenge von Computern im Arbeitsbereich als Ziel angeben.  Bei Verwendung der Zielgruppenadressierung für Lösungen führt Security Center den Arbeitsbereich als Arbeitsbereich ohne Lösung auf.
>
>

## <a name="upgrade-an-azure-subscription"></a>Upgraden eines Azure-Abonnements
So führen Sie ein Upgrade aller Abonnements auf Standard durch:
1. Klicken Sie im Hauptmenü von Security Center auf **Onboarding**.
2. Unter **Integration in erweiterte Sicherheit** werden für die Integration geeignete Abonnements aufgeführt. Durch Klicken auf **Standardplan anwenden** können Sie alle aufgeführten Abonnements upgraden.

  ![Upgraden aller Abonnements][1]

Wenn Sie nur ein bestimmtes Abonnement auf Standard upgraden möchten, klicken Sie für das gewünschte Abonnement unter **Onboarding** auf **Standardplan anwenden**. Wenn Sie eine Ressourcengruppe unter dem Abonnement auf Standard upgraden möchten, wählen Sie das Abonnement aus:
1. Wählen Sie ein Abonnement aus.  **Sicherheitsrichtlinie** liefert Informationen zur im Abonnement enthaltenen Ressourcengruppe.
2. Wählen Sie das Abonnement oder eine Ressourcengruppe aus.

  ![Upgraden aller Abonnements][2]

3. Wählen Sie **Standard** aus, um ein Upgrade von Free auf Standard durchzuführen.
4. Wählen Sie **Speichern** aus.

> [!NOTE]
> Wenn Sie ein Abonnement auf Standard upgraden, wird die [automatische Bereitstellung](security-center-enable-data-collection.md) aktiviert, sofern sie zuvor deaktiviert war. Wir empfehlen die automatische Bereitstellung von Überwachungs-Agents.
>
>

## <a name="upgrade-a-workspace"></a>Upgraden eines Arbeitsbereichs
Wenn Sie „Standard“ auf den Arbeitsbereich anwenden, betrifft dies alle Ressourcen, die Berichte an den Arbeitsbereich senden.

1. Kehren Sie zum Blatt **Onboarding** zurück.
2. Wählen Sie einen Arbeitsbereich aus.

  ![Upgraden eines Arbeitsbereichs][8]

3. Wählen Sie **Standard** aus, um das Upgrade durchzuführen.  
4. Wählen Sie **Speichern** aus.

   > [!NOTE]
   > Es gibt ein Szenario, in dem möglicherweise nicht „Free“ oder „Standard“ auf Ihren Arbeitsbereich angewendet ist. Wenn Sie „Free“ auswählen, werden die Free-Funktionen von Security Center nur auf Ihre virtuellen Azure-Computer angewendet. Die Free-Funktionen werden nicht auf Ihre Azure-fremden Computer angewendet. Wenn Sie „Standard“ auswählen, werden die Standard-Funktionen auf alle virtuellen Azure-Computer und Azure-fremden Computer angewendet, die Berichte an den Arbeitsbereich senden. Wir empfehlen, „Standard“ anzuwenden, um die Sicherheit Ihrer Azure-basierten und Azure-fremden Ressourcen zu verbessern.
   >
   >

## <a name="onboard-non-azure-computers"></a>Integrieren Azure-fremder Computer
Security Center kann den Sicherheitsstatus Ihrer Azure-fremden Computer überwachen. Dazu müssen die entsprechenden Ressourcen jedoch zunächst integriert werden. Azure-fremde Computer können über das Blatt **Onboarding** oder über das Blatt **Compute** hinzugefügt werden. Beide Methoden werden hier Schritt für Schritt erläutert.

### <a name="add-new-non-azure-computers-from-onboarding"></a>Hinzufügen neuer Azure-fremder Computer über „Onboarding“

1. Kehren Sie zu **Onboarding** zurück.   
2. Klicken Sie auf **Do you want to add new non-Azure computers?** (Möchten Sie neue Azure-fremde Computer hinzufügen?).

  ![Hinzufügen eines Azure-fremden Computers][3]

Gegebenenfalls bereits vorhandene Arbeitsbereiche werden unter **Add new Non-Azure computers** (Neue Azure-fremde Computer hinzufügen) aufgeführt. Sie können Computer einem vorhandenen Arbeitsbereich hinzufügen oder einen neuen Arbeitsbereich erstellen. Klicken Sie zum Erstellen eines neuen Arbeitsbereichs auf den Link** **zum Hinzufügen eines neuen Arbeitsbereichs.

Beide Methoden werden hier Schritt für Schritt erläutert:

- Erstellen eines neuen Arbeitsbereichs und Hinzufügen eines Computers
- Auswählen eines vorhandenen Arbeitsbereichs und Hinzufügen eines Computers

**Erstellen eines neuen Arbeitsbereichs und Hinzufügen eines Computers**

1. Klicken Sie unter **Add new non-Azure computers** (Neue Azure-fremde Computer hinzufügen) auf den Link** **zum Hinzufügen eines neuen Arbeitsbereichs.

   ![Hinzufügen eines neuen Arbeitsbereichs][4]

2. Klicken Sie unter **Sicherheit und Überwachung** auf **OMS-Arbeitsbereich**, um einen neuen Arbeitsbereich zu erstellen.
3. Geben Sie unter **OMS-Arbeitsbereich** die Informationen für Ihren Arbeitsbereich ein.
4. Klicken Sie unter **OMS-Arbeitsbereich** auf **OK**.  Nachdem Sie auf „OK“ geklickt haben, erhalten Sie einen Downloadlink für einen Windows- oder Linux-Agent sowie Schlüssel für Ihre Arbeitsbereichs-ID, die Sie zum Konfigurieren des Agents benötigen.
5. Klicken Sie unter **Sicherheit und Überwachung** auf **OK**.

**Auswählen eines vorhandenen Arbeitsbereichs und Hinzufügen eines Computers**

Sie können einen Computer mithilfe des Workflows unter **Onboarding** hinzufügen, wie weiter oben gezeigt. Sie können einen Computer aber auch mithilfe des Workflows unter **Compute** hinzufügen. In diesem Beispiel verwenden wir **Compute**.

1. Kehren Sie zum Hauptmenü von Security Center und zum Dashboard **Übersicht** zurück.

   ![Übersicht][5]

2. Klicken Sie auf die Kachel **Compute**.
3. Klicken Sie unter **Compute** auf **Computer hinzufügen**.

   ![Blatt „Compute“][6]

4. Wählen Sie unter **Add new non-Azure computers** (Neue Azure-fremde Computer hinzufügen) einen Arbeitsbereich aus, mit dem Sie Ihren Computer verbinden möchten, und klicken Sie anschließend auf **Computer hinzufügen**.

   ![Hinzufügen von Computern][7]

 Auf dem Blatt **Direkt-Agent** erhalten Sie einen Downloadlink für einen Windows- oder Linux-Agent sowie Schlüssel für Ihre Arbeitsbereichs-ID, die Sie zum Konfigurieren des Agents benötigen.   

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Azure-basierte und Azure-fremde Ressourcen integrieren, um von der erhöhten Sicherheit von Security Center zu profitieren.  Informationen zu weiteren Möglichkeiten mit Ihren integrierten Ressourcen finden Sie unter den folgenden Links:

- [Aktivieren der Datensammlung](security-center-enable-data-collection.md)
- [Bericht über Bedrohungen](security-center-threat-report.md)
- [Verwalten des Zugriffs auf virtuelle Computer mithilfe des Just-In-Time-Features](security-center-just-in-time.md)

<!--Image references-->
[1]: ./media/security-center-onboarding/onboard.png
[2]: ./media/security-center-onboarding/onboard-subscription.png
[3]: ./media/security-center-onboarding/add-non-azure-resource.png
[4]: ./media/security-center-onboarding/create-workspace.png
[5]: ./media/security-center-onboarding/overview.png
[6]: ./media/security-center-onboarding/compute-blade.png
[7]: ./media/security-center-onboarding/add-non-azure-computer.png
[8]: ./media/security-center-onboarding/onboard-workspace.png

