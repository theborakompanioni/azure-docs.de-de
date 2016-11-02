<properties
   pageTitle="Aktivieren der Datensammlung in Azure Security Center | Microsoft Azure"
   description=" Hier erfahren Sie, wie Sie die Datensammlung in Azure Security Center aktivieren. "
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# Aktivieren der Datensammlung in Azure Security Center

Azure Security Center erfasst und verarbeitet Daten zu Ihren virtuellen Azure-Computern (einschließlich Konfigurationsinformationen, Metadaten, Ereignisprotokolle und mehr), um Kunden bei der Vermeidung, Erkennung und Behandlung von Bedrohungen zu unterstützen. Beim ersten Zugriff auf Security Center wird die Datensammlung für alle virtuellen Computer in Ihrem Abonnement aktiviert. Die Datensammlung wird zwar empfohlen, kann aber in der Security Center-Richtlinie deaktiviert werden (siehe [Deaktivieren der Datensammlung](#disabling-data-collection)). Wenn Sie die Datensammlung deaktivieren, empfiehlt Security Center, die Datensammlung in der Sicherheitsrichtlinie für dieses Abonnement zu aktivieren.

> [AZURE.NOTE] Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Security Center** die Kachel **Empfehlungen** aus. Das Blatt **Empfehlungen** wird geöffnet. ![Blatt „Security Center“][1]

2. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Enable data collection for subscriptions** (Datensammlung für Abonnements aktivieren) aus. Das Blatt **Turn on data collection** (Datensammlung aktivieren) wird geöffnet. ![Blatt „Empfehlungen“][2]

3. Wählen Sie auf dem Blatt **Turn on data collection** (Datensammlung aktivieren) Ihr Abonnement aus. Das Blatt **Sicherheitsrichtlinie** für das Abonnement wird geöffnet.

4. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** unter **Datensammlung** die Option **Ein** aus, um automatisch Protokolle zu erfassen. Durch Aktivieren der Datensammlung wird auch die Überwachungserweiterung auf allen aktuellen und neuen unterstützten VMs im Abonnement bereitgestellt. ![Blatt „Sicherheitsrichtlinie“][3]

5.	Wählen Sie **Speichern** aus.

6.	Wählen Sie **Ein Speicherkonto pro Region auswählen** aus. Wählen Sie für jede Region, in der Sie virtuelle Computer ausführen, ein Speicherkonto, in dem Daten dieser virtuellen Computer gespeichert werden. Wenn Sie kein Speicherkonto für die einzelnen Regionen auswählen, wird automatisch ein Speicherkonto für Sie erstellt. In diesem Beispiel wählen wir **newstoracct** aus. Sie können das Speicherkonto später ändern, indem Sie zur Sicherheitsrichtlinie für Ihr Abonnement zurückkehren und ein anderes Speicherkonto auswählen. ![Auswählen eines Speicherkontos][4]

7.	Klicken Sie auf **OK**.

> [AZURE.NOTE] Es wird empfohlen, dass Sie zunächst die Datensammlung aktivieren und ein Speicherkonto auf Abonnementebene auswählen. Sicherheitsrichtlinien können auf der Ebene von Azure-Abonnement und Ressourcengruppe festgelegt werden, die Konfiguration von Datensammlung und Speicherkonten erfolgt jedoch nur auf Abonnementebene.

## Nach Aktivierung der Datensammlung

Datensammlung wird über den Azure-Überwachungs-Agent und die Azure-Erweiterung für Sicherheitsüberwachung aktiviert. Die Azure-Erweiterung für die Sicherheitsüberwachung sucht nach verschiedenen sicherheitsrelevanten Konfigurationen und sendet diese in [ETW-Ablaufverfolgungen](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (Event Tracing for Windows, Ereignisablaufverfolgung für Windows). Außerdem erstellt das Betriebssystem Einträge im Ereignisprotokoll. Der Azure-Überwachungs-Agent liest Ereignisprotokolleinträge und ETW-Ablaufverfolgungen und kopiert diese zur Analyse in Ihr Speicherkonto. Darüber hinaus kopiert der Überwachungs-Agent Absturzabbilddateien in Ihr Speicherkonto. Dies ist das Speicherkonto, das Sie in der Sicherheitsrichtlinie konfiguriert haben.

## Deaktivieren der Datensammlung

Die Datensammlung kann jederzeit deaktiviert werden. Dadurch werden sämtliche Überwachungs-Agents entfernt, die zuvor von Security Center installiert wurden. Zum Deaktivieren der Datensammlung muss ein Abonnement ausgewählt werden.

> [AZURE.NOTE] Sicherheitsrichtlinien können auf der Ebene des Azure-Abonnements und der Ressourcengruppe festgelegt werden. Zum Deaktivieren der Datensammlung muss allerdings ein Abonnement ausgewählt werden.

1.	Kehren Sie zum Blatt **Security Center** zurück, und wählen Sie die Kachel **Richtlinie** aus. Das Blatt **Sicherheitsrichtlinie – Richtlinie pro Abonnement oder Ressourcengruppe definieren** wird geöffnet. ![Auswählen der Kachel „Richtlinie“][5]

2.	Wählen Sie auf dem Blatt **Sicherheitsrichtlinie – Richtlinie pro Abonnement oder Ressourcengruppe definieren** das Abonnement aus, für das Sie die Datensammlung deaktivieren möchten. ![Auswählen eines Abonnements zum Deaktivieren der Datensammlung][6]

3.	Das Blatt **Sicherheitsrichtlinie** für das Abonnement wird geöffnet. Wählen Sie unter „Datensammlung“ die Option **Aus** aus.

4.	Wählen Sie im oberen Menüband die Option **Speichern** aus.

5.	Wählen Sie im oberen Menüband die Option **Agents löschen** aus, um die Agents von vorhandenen virtuellen Computern zu entfernen.

## Weitere Informationen

In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Datensammlung aktivieren“ implementieren. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md): Hier erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie sicherheitsbezogene Neuigkeiten und Informationen zu Azure.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/security-center-blade.png
[2]: ./media/security-center-enable-data-collection/recommendations.png
[3]: ./media/security-center-enable-data-collection/data-collection.png
[4]: ./media/security-center-enable-data-collection/storage-account.png
[5]: ./media/security-center-enable-data-collection/policy.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png

<!---HONumber=AcomDC_0803_2016-->