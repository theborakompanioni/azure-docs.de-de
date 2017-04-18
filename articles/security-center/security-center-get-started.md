---
title: Schnellstarthandbuch zu Azure Security Center | Microsoft Docs
description: "Dieser Artikel enthält hilfreiche Informationen zum schnellen Einstieg in Azure Security Center. Sie erhalten eine Einführung in die Sicherheitsüberwachungs- und Richtlinienverwaltungs-Komponenten und werden zu weiterführenden Schritten geleitet."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/11/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: fcc43e5d98f75b34f2d65c9e1ce8eeba7762caaf
ms.lasthandoff: 04/12/2017


---
# <a name="azure-security-center-quick-start-guide"></a>Schnellstarthandbuch zu Azure Security Center
Dieser Artikel enthält hilfreiche Informationen zum schnellen Einstieg in Azure Security Center. Sie erhalten eine Einführung in die Sicherheitsüberwachungs- und Richtlinienverwaltungs-Komponenten.

> [!NOTE]
> Im Artikel wird der Dienst anhand einer Beispielbereitstellung vorgestellt. Es handelt sich nicht um eine Schritt-für-Schritt-Anleitung.
>
>

## <a name="prerequisites"></a>Voraussetzungen
Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

Der Free-Tarif von Security Center wird für Ihr Abonnement automatisch aktiviert und ermöglicht Einblicke in den Sicherheitszustand Ihrer Azure-Ressourcen. Er umfasst die grundlegende Verwaltung von Sicherheitsrichtlinien, Sicherheitsempfehlungen und die Integration in Sicherheitsprodukte und Dienste von Azure-Partnern.

Sie können über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)auf Security Center zugreifen. Weitere Informationen zum Azure-Portal finden Sie in der [Dokumentation zum Portal](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>Berechtigungen
In Security Center werden Ihnen nur dann Informationen zu einer Azure-Ressource angezeigt, wenn Ihnen für das Abonnement oder die Ressourcengruppe, der eine Ressource angehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugewiesen ist. Informationen zu Rollen und zulässigen Aktionen in Security Center finden Sie unter [Permissions in Azure Security Center](security-center-permissions.md) (Berechtigungen in Azure Security Center).

## <a name="data-collection"></a>Datensammlung
Security Center sammelt Daten von Ihren virtuellen Computern (VMs), um den Sicherheitsstatus zu bewerten, Sicherheitsempfehlungen bereitzustellen und vor Bedrohungen zu warnen. Beim ersten Zugriff auf Security Center wird die Datensammlung für alle VMs in Ihrem Abonnement aktiviert. Die Datensammlung wird zwar empfohlen, kann aber in der Security Center-Richtlinie deaktiviert werden.

In den folgenden Schritten wird beschrieben, wie Sie auf die Komponenten von Security Center zugreifen und diese verwenden. Die Schritte veranschaulichen, wie Sie die Datensammlung deaktivieren, wenn Sie sie nicht nutzen möchten.

## <a name="access-security-center"></a>Zugreifen auf Security Center
Führen Sie im Portal folgende Schritte aus, um auf Security Center zuzugreifen:

1. Wählen Sie im Menü **Microsoft Azure** die Option **Security Center**.

   ![Azure-Menü][1]
2. Wenn Sie zum ersten Mal auf Security Center zugreifen, wird das Blatt **Willkommen** geöffnet. Wählen Sie **Launch Security Center** (Security Center starten), um das Blatt **Security Center** zu öffnen und die Datensammlung zu aktivieren.
   ![Bildschirm „Willkommen“][10]
3. Nachdem Sie Security Center über das Blatt „Willkommen“ gestartet oder im Microsoft Azure-Menü die Option „Security Center“ gewählt haben, wird das Blatt **Security Center** geöffnet. Um zukünftig einfacher auf das Blatt **Security Center** zugreifen zu können, wählen Sie die Option **Blatt an Dashboard anheften** (oben rechts) aus.
   ![Option „Blatt an Dashboard anheften“][2]

## <a name="use-security-center"></a>Verwenden von Security Center
Sie können die Sicherheitsrichtlinien für Ihre Azure-Abonnements und Ressourcengruppen konfigurieren. Wir konfigurieren nun eine Sicherheitsrichtlinie für Ihr Abonnement:

1. Wählen Sie auf dem Blatt **Security Center** die Kachel **Richtlinie**.
   ![Sicherheitsrichtlinie][3]
2. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie – Richtlinie pro Abonnement oder Ressourcengruppe definieren** ein Abonnement aus.
3. Auf dem Blatt **Sicherheitsrichtlinie** ist die Option **Datensammlung** aktiviert, um automatisch Protokolle zu erfassen. Die Überwachungserweiterung wird auf allen aktuellen und neuen VMs im Abonnement bereitgestellt. (Sie können die Datensammlung auch deaktivieren, indem Sie die Option **Datensammlung** auf **Aus** festlegen. Security Center kann dann aber keine Sicherheitswarnungen und Empfehlungen mehr bereitstellen.)
4. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** die Option **Speicherkonto nach Region auswählen**. Wählen Sie für jede Region, in der Sie VMs ausführen, ein Speicherkonto, in dem Daten dieser VMs gespeichert werden. Wenn Sie nicht für jede Region ein Speicherkonto auswählen, wird ein Speicherkonto für Sie erstellt und in der securitydata-Ressourcengruppe platziert. Die gesammelten Daten werden aus Sicherheitsgründen logisch von anderen Kundendaten getrennt.

   > [!NOTE]
   > Es wird empfohlen, dass Sie zunächst die Datensammlung aktivieren und ein Speicherkonto auf Abonnementebene auswählen. Sicherheitsrichtlinien können auf der Ebene von Azure-Abonnement und Ressourcengruppe festgelegt werden, aber die Konfiguration von Datensammlung und Speicherkonten erfolgt nur auf Abonnementebene.
   >
   >
5. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** die Option **Präventionsrichtlinie**. Das Blatt **Präventionsrichtlinie** wird geöffnet.
   ![Präventionsrichtlinie][4]
6. Aktivieren Sie auf dem Blatt **Präventionsrichtlinie** die Empfehlungen, die im Rahmen der Sicherheitsrichtlinie verwendet werden sollen. Beispiele:

   * Durch Festlegen von **Systemupdates** auf **Ein** werden alle unterstützten virtuellen Computer auf fehlende Betriebssystemupdates überprüft.
   * Durch Festlegen von **Sicherheitsrisiken des Betriebssystems** auf **Ein** werden alle unterstützten virtuellen Computer untersucht, um Betriebssystemkonfigurationen zu ermitteln, die den virtuellen Computer anfälliger für Angriffe machen können.

### <a name="view-recommendations"></a>Anzeigen von Empfehlungen
1. Kehren Sie zum Blatt **Security Center** zurück, und klicken Sie auf die Kachel **Empfehlungen**. Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen. Wenn von Security Center potenzielle Sicherheitsrisiken identifiziert werden, werden die Empfehlungen auf dem Blatt **Empfehlungen** angezeigt.
   ![Empfehlungen in Azure Security Center][5]
2. Wählen Sie eine Empfehlung auf dem Blatt **Empfehlungen** aus, um weitere Informationen anzuzeigen und Maßnahmen zum Beheben des Problems zu ergreifen.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Anzeigen des Integritäts- und Sicherheitsstatus Ihrer Ressourcen
1. Kehren Sie zum Blatt **Security Center** zurück. Die Kachel **Sicherheitsintegrität der Ressource** enthält Indikatoren, die den Sicherheitsstatus für virtuelle Computer, Netzwerk, Daten und Anwendungen anzeigen.
2. Wählen Sie **Compute**, um weitere Informationen anzuzeigen. Das Blatt **Compute** wird mit drei Registerkarten geöffnet:

  - **Übersicht**: Enthält Empfehlungen zur Überwachung und zu virtuellen Computern.
  - **Virtuelle Computer**: Liste mit allen virtuellen Computern und dem jeweils aktuellen Sicherheitsstatus
  - **Clouddienste**: Liste mit den von Security Center überwachten Web- und Workerrollen

    ![Kachel „Ressourcenintegrität“ in Azure Security Center][6]

3. Wählen Sie auf der Registerkarte **Übersicht** unter **EMPFEHLUNGEN FÜR VIRTUELLE COMPUTER** eine Empfehlung aus, um weitere Informationen anzuzeigen und/oder Maßnahmen zur Konfiguration der erforderlichen Steuerelemente zu ergreifen.
4. Wählen Sie auf der Registerkarte **Virtuelle Computer** einen virtuellen Computer aus, um zusätzliche Details anzuzeigen.

### <a name="view-security-alerts"></a>Anzeigen von Sicherheitswarnungen
1. Kehren Sie zum Blatt **Security Center** zurück, und klicken Sie auf die Kachel **Sicherheitswarnungen**. Das Blatt **Sicherheitswarnungen** wird mit einer Liste angezeigt, in der die Warnungen aufgeführt sind. Die Warnungen werden bei der Security Center-Analyse Ihrer Sicherheitsprotokolle und der Netzwerkaktivität generiert. Die Liste enthält Warnungen integrierter Partnerlösungen.
   ![Sicherheitswarnungen in Azure Security Center][7]

   > [!NOTE]
   > Sicherheitswarnungen sind nur verfügbar, wenn der Standard-Tarif von Security Center aktiviert ist. Eine kostenlose 60-Tage-Testversion des Standard-Tarifs ist verfügbar. Informationen dazu, wie Sie den Standard-Tarif nutzen können, finden Sie unter [Nächste Schritte](#next-steps).
   >
   >
2. Wählen Sie eine Warnung aus, um weitere Informationen anzuzeigen. In diesem Beispiel wählen wir die Option **Erkennung der Änderung einer Systembinärdatei**. Es werden Blätter geöffnet, die weitere Details zur Warnung enthalten.
   ![Details zu Sicherheitswarnungen in Azure Security Center][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Anzeigen der Integrität Ihrer Partnerlösungen
1. Kehren Sie zum Blatt **Security Center** zurück. Mit der Kachel **Partnerlösungen** können Sie auf einen Blick den Integritätsstatus Ihrer Partnerlösungen prüfen, die in Ihr Azure-Abonnement integriert sind.
2. Wählen Sie die Kachel **Partnerlösungen** . Ein Blatt mit einer Liste der Partnerlösungen, die mit dem Security Center verbunden sind, wird geöffnet.
   ![Partnerlösungen][9]
3. Wählen Sie eine Partnerlösung aus. In diesem Beispiel wählen wir die Lösung **QualysVa1** aus.  Auf dem geöffneten Blatt werden der Status der Partnerlösung und die zugeordneten Ressourcen der Lösung angezeigt. Wählen Sie die Option **Lösungskonsole** , um die Verwaltungsoberfläche des Partners für diese Lösung zu öffnen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurden die Security Center-Komponenten für die Sicherheitsüberwachung und Richtlinienverwaltung vorgestellt. Nachdem Sie sich jetzt mit Security Center vertraut gemacht haben, können Sie die folgenden Schritte ausprobieren:

* Konfigurieren Sie eine Sicherheitsrichtlinie für Ihr Azure-Abonnement. Weitere Informationen hierzu finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md).
* Verwenden Sie die Empfehlungen in Security Center als Hilfe beim Schützen Ihrer Azure-Ressourcen. Weitere Informationen finden Sie unter [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).
* Überprüfen und verwalten Sie Ihre aktuellen Sicherheitswarnungen. Weitere Informationen finden Sie unter [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md).
* Informieren Sie sich über die [Funktionen der erweiterten Bedrohungserkennung](security-center-detection-capabilities.md), die Teil des [Standard-Tarifs](security-center-pricing.md) von Security Center sind. Der Standard-Tarif wird für die ersten 60 Tage kostenlos angeboten.
* Antworten auf Fragen zur Verwendung von Security Center finden Sie unter [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png

