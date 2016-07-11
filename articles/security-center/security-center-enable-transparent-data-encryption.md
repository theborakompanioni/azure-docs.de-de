<properties
   pageTitle="Aktivieren von Transparent Data Encryption in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung „Aktivieren von Transparent Data Encryption“ umsetzen."
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
   ms.date="06/27/2016"
   ms.author="terrylan"/>

# Aktivieren von Transparent Data Encryption in Azure Security Center

Azure Security Center empfiehlt das Aktivieren von Transparent Data Encryption (TDE) für SQL-Datenbanken, sofern noch nicht erfolgt. TDE schützt Ihre Daten und hilft Ihnen beim Erfüllen von Complianceanforderungen, indem Ihre Datenbank, dazugehörige Sicherungen und ruhende Transaktionsprotokolldateien verschlüsselt werden, ohne das Änderungen an Ihrer Anwendung erforderlich sind. Weitere Informationen finden Sie unter [Transparent Data Encryption mit Azure SQL-Datenbank](https://msdn.microsoft.com/library/dn948096).

Diese Empfehlung gilt für nur den Azure SQL-Dienst und nicht für auf Ihren virtuellen Computern ausgeführtes SQL Server.

> [AZURE.NOTE] Die Informationen in diesem Dokument gelten für die Vorschauversion von Azure Security Center. Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Transparent Data Encryption aktivieren** aus. ![][1]

2. Das Blatt **Transparent Data Encryption für SQL-Datenbanken aktivieren** wird geöffnet. Wählen Sie eine SQL-Datenbank aus, für die TDE aktiviert werden soll. ![][2]
3. Wählen Sie auf dem Blatt **Transparent Data Encryption** unter „Datenverschlüsselung“ **EIN** und dann auf dem oberen Menüband des Blatts **Speichern** aus. ![][3]

  Nach der Aktivierung von TDE für die ausgewählte SQL-Datenbank ändert sich **Verschlüsselungsstatus** in **Verschlüsselt**.

  ![][4]

## Nächste Schritte

In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Aktivieren von Transparent Data Encryption“ umsetzen. Weitere Informationen zu TDE für SQL finden Sie in folgenden Artikeln:

- [Transparent Data Encryption mit Azure SQL-Datenbank](https://msdn.microsoft.com/library/dn948096)
- [Erste Schritte mit Transparent Data Encryption (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md): Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie sicherheitsbezogene Neuigkeiten und Informationen zu Azure.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]: ./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png

<!---HONumber=AcomDC_0629_2016-->