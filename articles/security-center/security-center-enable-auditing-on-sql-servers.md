<properties
   pageTitle="Aktivieren der Überwachung für SQL-Server in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung „Aktivieren der Überwachung für SQL-Server“ umsetzen."
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

# Aktivieren der Überwachung für SQL-Server in Azure Security Center

Azure Security Center empfiehlt Ihnen, die Überwachung für alle Datenbanken auf Ihren Azure SQL-Servern zu aktivieren, sofern noch nicht erfolgt. Die Überwachung kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.

Nachdem Sie die Überwachung aktiviert haben, können Sie Einstellungen für die Bedrohungserkennung und E-Mails zum Empfangen von Sicherheitswarnungen konfigurieren. Die Bedrohungserkennung erkennt anormale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken für die Datenbank hindeuten. Dadurch können Sie potenzielle Bedrohungen erkennen und darauf reagieren, sobald sie auftreten.

Diese Empfehlung gilt für nur den Azure SQL-Dienst und nicht für auf Ihren virtuellen Computern in Azure Infrastructure Services (Azure IaaS) ausgeführte SQL-Server.

> [AZURE.NOTE] Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Überwachung für SQL-Server aktivieren** aus. Daraufhin wird das Blatt **Überwachung für SQL-Server aktivieren** geöffnet. ![Überwachung für SQL-Server aktivieren][1]

2. Wählen Sie einen SQL-Server aus, für den die Überwachung aktiviert werden soll. Daraufhin wird das Blatt **Überwachungseinstellungen** angezeigt. ![Überwachungseinstellungen][2]
3. Wählen Sie auf dem Blatt **Überwachungseinstellungen** unter **Überwachung** die Option **EIN** aus. ![Aktivieren der Überwachungseinstellungen][3]

4. Führen Sie die Schritte in [Erste Schritte bei der Überwachung von SQL-Datenbank](../sql-database/sql-database-auditing-get-started.md) zum Konfigurieren von Speicher aus, in dem Ihre Überwachungsprotokolle gespeichert werden sollen. Das Speicherkonto des Abonnements für die Datensammlung ist das Standardspeicherkonto.

5. Führen Sie die Schritte in [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](../sql-database/sql-database-threat-detection-get-started.md) aus, um die Bedrohungserkennung zu aktivieren und zu konfigurieren und die Liste der E-Mail-Empfänger einzurichten, die bei Erkennung ungewöhnlicher Aktivitäten Sicherheitswarnungen erhalten.

## Weitere Informationen

In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Aktivieren der Überwachung für SQL-Server“ umsetzen. Weitere Informationen zum Schützen Ihrer SQL-Datenbank finden Sie unter:

- [Sichern der SQL-Datenbank](../sql-database/sql-database-security.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md): Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie sicherheitsbezogene Neuigkeiten und Informationen zu Azure.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing.png
[3]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png

<!---HONumber=AcomDC_0727_2016-->