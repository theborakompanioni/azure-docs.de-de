<properties
   pageTitle="Schnellstarthandbuch zu Azure Security Center | Microsoft Azure"
   description="Dieses Dokument unterstützt Sie beim Einstieg in Azure Security Center. Sie erhalten eine Einführung in die Sicherheitsüberwachungs- und Richtlinienverwaltungs-Komponenten und werden zu weiterführenden Schritten geleitet."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# Schnellstarthandbuch zu Azure Security Center

Dieses Dokument unterstützt Sie beim Einstieg in Azure Security Center. Sie erhalten eine Einführung in die Sicherheitsüberwachungs- und Richtlinienverwaltungs-Komponenten und werden zu weiterführenden Schritten geleitet.

> [AZURE.NOTE] Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Datensammlung

Security Center sammelt Daten von Ihren virtuellen Computern, um den Sicherheitsstatus zu bewerten, Sicherheitsempfehlungen bereitzustellen und vor Bedrohungen zu warnen. Beim ersten Zugriff auf Security Center wird die Datensammlung für alle virtuellen Computer in Ihrem Abonnement aktiviert. Die Datensammlung wird zwar empfohlen, kann aber in der Security Center-Richtlinie deaktiviert werden. In den folgenden Schritten wird gezeigt, wie Sie die Datensammlung deaktivieren.

## Voraussetzungen

Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Security Center wird mit Ihrem Abonnement aktiviert. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testabonnement](https://azure.microsoft.com/pricing/free-trial/) registrieren.

Der Zugriff auf Security Center erfolgt über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/). Weitere Informationen finden Sie in der [Dokumentation zum Portal](https://azure.microsoft.com/documentation/services/azure-portal/).


## Zugreifen auf Security Center

Führen Sie im Portal folgende Schritte aus, um auf Security Center zuzugreifen:

1. Wählen Sie **Durchsuchen** aus, und führen Sie dann einen Bildlauf zur Option **Security Center** durch. ![Auf Azure Security Center im Portal zugreifen][1]

2. Wählen Sie **Security Center** aus. Dadurch wird das Blatt **Security Center** geöffnet.
3. Um zukünftig einfacher auf das Blatt **Security Center** zugreifen zu können, wählen Sie die Option **Blatt an Dashboard anheften** (oben rechts) aus. ![Option „Blatt an Dashboard anheften“][2]

## Verwenden von Security Center

Sie können die Sicherheitsrichtlinien für Ihre Azure-Abonnements und Ressourcengruppen konfigurieren. Konfigurieren wir eine **Sicherheitsrichtlinie** für Ihre Abonnements:

1. Klicken Sie auf dem Blatt **Security Center** auf die Kachel **Richtlinie**. ![Security Center][3]

2. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie – Richtlinie pro Abonnement oder Ressourcengruppe definieren** ein Abonnement aus. ![Blatt „Sicherheitsrichtlinie“ in Azure Security Center][4]

3. Auf dem Blatt **Sicherheitsrichtlinie** ist die Option **Datensammlung** aktiviert, um automatisch Protokolle zu erfassen. Die Überwachungserweiterung wird auf allen aktuellen und neuen VMs im Abonnement bereitgestellt. (Sie können die Datensammlung auch deaktivieren, indem Sie die Option **Datensammlung** auf **Aus** festlegen. Security Center kann dann aber keine Sicherheitswarnungen und Empfehlungen mehr bereitstellen.)
4. Wählen Sie **Speicherkonto nach Region auswählen** aus. Wählen Sie für jede Region, in der Sie virtuelle Computer ausführen, ein Speicherkonto, in dem Daten dieser virtuellen Computer gespeichert werden. Wenn Sie kein Speicherkonto für die einzelnen Regionen auswählen, wird ein Speicherkonto für Sie erstellt. Die gesammelten Daten werden aus Sicherheitsgründen logisch von anderen Kundendaten getrennt.

     > [AZURE.NOTE] Es wird empfohlen, dass Sie zunächst die Datensammlung aktivieren und ein Speicherkonto auf Abonnementebene auswählen. Sicherheitsrichtlinien können auf der Ebene von Azure-Abonnement und Ressourcengruppe festgelegt werden, die Konfiguration von Datensammlung und Speicherkonten erfolgt jedoch nur auf Abonnementebene.

5. Aktivieren Sie die **Empfehlungen**, die Sie als Teil Ihrer Sicherheitsrichtlinie verwenden möchten. Beispiele:

 - Durch Aktivieren von **Systemupdates** werden alle unterstützten virtuellen Computer auf fehlende Betriebssystemupdates überprüft.
 - Durch Aktivieren von **Sicherheitsrisiken des Betriebssystems** werden alle unterstützten virtuellen Computer untersucht, um Betriebssystemkonfigurationen zu ermitteln, die den virtuellen Computer anfälliger für Angriffe machen können.

**Empfehlungen**:

1. Kehren Sie zum Blatt **Security Center** zurück, und klicken Sie auf die Kachel **Empfehlungen**. Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen. Wenn mögliche Sicherheitsrisiken festgestellt werden, wird hier eine Empfehlung angezeigt.
2.	Wählen Sie alle Empfehlungen aus, um weitere Informationen anzuzeigen oder Schritte zur Behebung des Problems auszuführen. ![Empfehlungen in Azure Security Center][5]

Anzeigen des Integritäts- und Sicherheitsstatus Ihrer Ressourcen über **Sicherheitsintegrität der Ressource**:

1.	Kehren Sie zum Blatt **Security Center** zurück.
2.	Die Kachel **Sicherheitsintegrität der Ressource** enthält Indikatoren, die den Sicherheitsstatus für **virtuelle Computer**, **Netzwerk**, **SQL** und **Anwendungen** anzeigen.
3.	Wählen Sie für weitere Informationen **Virtuelle Computer** aus.
4.	Auf dem Blatt **Virtuelle Computer** wird eine Statuszusammenfassung angezeigt. Sie enthält den Status von Antischadsoftware, Systemupdates, Neustarts und die Grundregeln Ihrer virtuellen Computer.
5.	Wählen Sie unter **EMPFEHLUNGEN FÜR VIRTUELLE COMPUTER** ein Element aus, um weitere Informationen anzuzeigen oder Maßnahmen zur Konfiguration der erforderlichen Steuerelemente zu ergreifen.
6.	Führen Sie einen Drilldown aus, um zusätzliche Informationen zu bestimmten virtuellen Computern anzuzeigen. ![Kachel „Ressourcenintegrität“ in Azure Security Center][6]

Behandeln von **Sicherheitswarnungen**:

1.	Kehren Sie zum Blatt **Security Center** zurück, und klicken Sie auf die Kachel **Sicherheitswarnungen**. Auf dem Blatt **Sicherheitswarnungen** wird eine Liste der Warnungen angezeigt. Die Warnungen werden bei der Security Center-Analyse Ihrer Sicherheitsprotokolle und der Netzwerkaktivität generiert. Die Liste enthält auch Warnungen integrierter Partnerlösungen. ![Sicherheitswarnungen in Azure Security Center][7]

2.	Wählen Sie eine Warnung aus, um weitere Informationen anzuzeigen. ![Details zu Sicherheitswarnungen in Azure Security Center][8]

Anzeigen der Integrität Ihrer **Partnerlösungen**:

1. Kehren Sie zum Blatt **Security Center** zurück. Über die Kachel **Partnerlösungen** können Sie auf einen Blick den Integritätsstatus Ihrer Partnerlösungen prüfen, die in Ihr Azure-Abonnement integriert sind.
2. Wählen Sie die Kachel **Partnerlösungen** aus. Ein Blatt mit einer Liste der Partnerlösungen, die mit dem Security Center verbunden sind, wird geöffnet. ![Partnerlösungen][9]

3. Wählen Sie eine Partnerlösung aus. In diesem Beispiel wählen wir die Lösung **F5-WAF2** aus. Auf einem Blatt werden der Status der Partnerlösung und die zugeordneten Ressourcen der Lösung angezeigt. Wählen Sie die Option **Lösungskonsole** aus, um die Verwaltungsoberfläche des Partners für diese Lösung zu öffnen. ![Details der Partnerlösung][10]

## Siehe auch
In diesem Dokument wurden die Sicherheitsüberwachungs- und Richtlinienverwaltungskomponenten in Security Center vorgestellt. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md): Hier erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-get-started/security-tile.png
[2]: ./media/security-center-get-started/pin-blade.png
[3]: ./media/security-center-get-started/security-center.png
[4]: ./media/security-center-get-started/security-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/partner-solutions-detail.png

<!---HONumber=AcomDC_0810_2016-->